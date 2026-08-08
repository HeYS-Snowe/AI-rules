# npm 私有包发布规范（GitHub Packages）

> 适用于通过 GitHub Packages (`npm.pkg.github.com`) 发布私有 npm 包的项目。
> 最后更新：2026-08-06

---

## 一、双 Token 策略

GitHub Packages 需要不同 scope 的 token 做不同的事：

| 用途 | Token scope | 存放位置 | 说明 |
|------|------------|---------|------|
| **下载** (`npm install` / `npm view`) | `read:packages` | `~/.npmrc` 日常状态 | 每天用，权限最小化 |
| **发布** (`npm publish`) | `write:packages` | 环境变量（不落盘到 .npmrc） | 仅发布时临时切换 |

### 安全原则

- **NEVER** 将 publish token 留在 `~/.npmrc` 的日常状态中
- **NEVER** 在命令行中直接传递 token（会出现在 shell history / 进程列表 / 工具日志中）
- **NEVER** 将 token 提交到 git
- 发布后**立即回退**到 download token，即使发布失败也要回退

---

## 二、环境变量

| 变量名 | scope | 设置方式 | 说明 |
|--------|-------|---------|------|
| `<PROJECT>_PUBLISH_TOKEN` | `write:packages` | `setx`（Windows）/ `export`（Unix） | 发布 token，持久化 |

### Windows 设置（Git Bash）

```bash
setx PROJECT_PUBLISH_TOKEN "ghp_your_publish_token_here"
```

> `setx` 只对**新开的终端**生效。设置后需重启终端/IDE。

### Unix 设置（~/.bashrc 或 ~/.zshrc）

```bash
export PROJECT_PUBLISH_TOKEN="ghp_your_publish_token_here"
```

### Token 创建

1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. 创建两个 token：
   - **Publish token**: 勾选 `write:packages`（自动包含 `read:packages`）
   - **Download token**: 仅勾选 `read:packages`
3. 将 download token 写入 `~/.npmrc`：
   ```
   //npm.pkg.github.com/:_authToken=ghp_your_download_token
   @YourScope:registry=https://npm.pkg.github.com/
   ```
4. 将 publish token 存入环境变量（上面的 `setx` / `export`）

---

## 三、发布流程（8 步）

```
1. Pre-checks    — typecheck + lint + test（全绿才继续）
2. Build         — bundle / esbuild 打包
3. Version bump  — npm version 或自定义 version.js
4. Pack tgz      — npm pack（生成本地 .tgz）
5. npm publish   — token 切换 → publish → token 回退
6. Git           — commit + tag vX.Y.Z + push
7. Distribute    — 生成分发包（可选）
8. Install + verify — 本地全局安装 + 版本验证
```

---

## 四、自动化脚本（可复用模板）

将以下脚本保存为项目根目录 `scripts/release.js`，按需修改标记了 `ADAPT` 的部分：

```javascript
/**
 * Full Release Script — GitHub Packages Private Publish
 *
 * Usage:
 *   node scripts/release.js [patch|minor|major] [--publish] [--skip-tests]
 *
 * Env:
 *   <PROJECT>_PUBLISH_TOKEN  GitHub Packages write:packages token
 */
import { spawnSync } from "node:child_process";
import { existsSync, readFileSync, writeFileSync } from "node:fs";
import { homedir } from "node:os";
import { join, dirname } from "node:path";
import { fileURLToPath } from "node:url";

const __dirname = dirname(fileURLToPath(import.meta.url));
const root = join(__dirname, "..");
const npmrcPath = join(homedir(), ".npmrc");
const AUTH_KEY = "//npm.pkg.github.com/:_authToken";

// ── ADAPT: 项目特定配置 ──────────────────────────────────────────────────────
const PACKAGES_DIR = "packages/cli";                    // ADAPT: 发布的子包目录
const DIST_BIN = "your-cli-command";                     // ADAPT: bin 命令名
const ENV_VAR = "PROJECT_PUBLISH_TOKEN";                 // ADAPT: 环境变量名
// ──────────────────────────────────────────────────────────────────────────────

function log(msg) { console.log(msg); }
function ok(msg) { console.log(`  OK  ${msg}`); }
function fail(msg) { console.error(`\n  FAIL  ${msg}`); process.exit(1); }

function run(cmd, args, opts = {}) {
  const result = spawnSync(cmd, args, {
    stdio: opts.silent ? "pipe" : "inherit",
    cwd: opts.cwd ?? root,
    shell: true,
    encoding: "utf8",
  });
  if (result.status !== 0) fail(`Command failed: ${cmd} ${args.join(" ")}`);
  return result;
}

function readNpmrcToken() {
  if (!existsSync(npmrcPath)) return null;
  const content = readFileSync(npmrcPath, "utf8");
  const match = content.match(/^\/\/npm\.pkg\.github\.com\/:_authToken=(.+)$/m);
  return match ? match[1].trim() : null;
}

function writeNpmrcToken(token) {
  if (!existsSync(npmrcPath)) return;
  let content = readFileSync(npmrcPath, "utf8");
  const regex = /^\/\/npm\.pkg\.github\.com\/:_authToken=.+$/m;
  if (regex.test(content)) {
    content = content.replace(regex, `${AUTH_KEY}=${token}`);
  } else {
    content = content.trimEnd() + `\n${AUTH_KEY}=${token}\n`;
  }
  writeFileSync(npmrcPath, content, "utf8");
}

// ── Args ─────────────────────────────────────────────────────────────────────
const args = process.argv.slice(2);
const bumpType = args.find((a) => ["patch", "minor", "major"].includes(a)) ?? "patch";
const doPublish = args.includes("--publish");
const skipTests = args.includes("--skip-tests");

const publishToken = process.env[ENV_VAR];
if (doPublish && !publishToken) {
  fail(`--publish requires ${ENV_VAR} environment variable.`);
}

const pkgPath = join(root, PACKAGES_DIR, "package.json");
const getVer = () => JSON.parse(readFileSync(pkgPath, "utf8")).version;

// ── Step 1: Pre-checks ───────────────────────────────────────────────────────
log("\n=== Step 1/8: Pre-checks ===");
if (!skipTests) {
  run("npm", ["run", "typecheck"]);
  run("npm", ["run", "lint"]);
  run("npm", ["test"]);
  ok("typecheck + lint + test");
} else {
  log("  SKIPPED (--skip-tests)");
}

// ── Step 2: Build ────────────────────────────────────────────────────────────
log("\n=== Step 2/8: Build ===");
run("npm", ["run", "bundle"]);  // ADAPT: 改为你的 build 命令
ok("built");

// ── Step 3: Version bump ─────────────────────────────────────────────────────
log(`\n=== Step 3/8: Version bump (${bumpType}) ===`);
const oldVer = getVer();
// ADAPT: monorepo 用自定义 version.js；单包用 npm version
run("npm", ["version", bumpType, "--no-git-tag-version"], { cwd: join(root, PACKAGES_DIR) });
const newVer = getVer();
ok(`${oldVer} -> ${newVer}`);

// ── Step 4: Pack tgz ─────────────────────────────────────────────────────────
log("\n=== Step 4/8: Pack tgz ===");
const pkgName = JSON.parse(readFileSync(pkgPath, "utf8")).name
  .replace("@", "").replace("/", "-");
const tgzName = `${pkgName}-${newVer}.tgz`;
run("npm", ["pack"], { cwd: join(root, PACKAGES_DIR), silent: true });
ok(tgzName);

// ── Step 5: npm publish ──────────────────────────────────────────────────────
if (doPublish) {
  log("\n=== Step 5/8: npm publish ===");
  const dlToken = readNpmrcToken();
  if (!dlToken) fail("Could not read download token from ~/.npmrc");

  writeNpmrcToken(publishToken);
  try {
    run("npm", ["publish"], { cwd: join(root, PACKAGES_DIR) });
    ok(`published v${newVer}`);
  } finally {
    writeNpmrcToken(dlToken);  // ALWAYS revert
    ok("token reverted");
  }
} else {
  log("\n=== Step 5/8: npm publish (SKIPPED) ===");
}

// ── Step 6: Git ──────────────────────────────────────────────────────────────
log("\n=== Step 6/8: Git ===");
run("git", ["add", "-u"]);
run("git", ["commit", "-m", `chore(release): v${newVer}`]);
run("git", ["tag", `v${newVer}`]);
const pushR = spawnSync("git", ["push", "origin", "main"], { cwd: root, stdio: "pipe", encoding: "utf8" });
if (pushR.status === 0) {
  spawnSync("git", ["push", "origin", `v${newVer}`], { cwd: root, stdio: "inherit" });
  ok("pushed main + tag");
} else {
  log("  WARNING: push failed — push manually");
}

// ── Step 7: Local install + verify ───────────────────────────────────────────
log("\n=== Step 7/8: Install + verify ===");
run("npm", ["install", "-g", join(root, PACKAGES_DIR, tgzName)]);
const verR = run("npx", [DIST_BIN, "--version"], { silent: true });
ok(`${DIST_BIN} --version -> ${verR.stdout.trim()}`);

// ── Done ─────────────────────────────────────────────────────────────────────
log(`\n=== v${newVer} complete! ===`);
```

### 标记说明

脚本中所有 `ADAPT` 注释处需要按项目修改：

| 标记 | 说明 | 示例 |
|------|------|------|
| `PACKAGES_DIR` | 发布的子包目录 | `packages/cli` 或 `.`（根包） |
| `DIST_BIN` | bin 命令名 | `deepcode-dev`、`my-cli` |
| `ENV_VAR` | 环境变量名 | `DEEPCODE_PUBLISH_TOKEN` |
| build 命令 | 打包命令 | `npm run bundle` / `npm run build` |
| version 命令 | 版本自增方式 | monorepo 用自定义 `version.js`；单包用 `npm version` |

---

## 五、常见问题

### E403: Permission denied

token scope 不对。检查：
- publish token 有 `write:packages`
- download token 有 `read:packages`
- `~/.npmrc` 的 registry 指向 `https://npm.pkg.github.com/`

### ENOWORKSPACES: This command does not support workspaces

在 monorepo 根目录运行 `npm config set` 或 `npm publish` 时触发。解决：
- `npm config set` 必须在**非 workspace 目录**运行（如 `cd ~`）
- `npm publish` 用 `--workspace <package>` 标志：`npm publish --workspace packages/cli`

### npm publish 后 bin 命令不存在

`package.json` 的 `bin` 路径格式问题。`"./dist/cli.js"` 在新版 npm 中无效，改为 `"dist/cli.js"`（去掉 `./` 前缀）。运行 `npm pkg fix` 自动修正。

### 误发布版本

GitHub Packages **不支持**命令行删除版本（返回 405）。解决：
1. 网页端删除：GitHub → Packages → 包名 → 版本 → Delete
2. 删除后 `latest` tag 自动指向上一个版本

### prepublishOnly 触发了重新构建

`npm publish` 会触发 `prepublishOnly` hook（如果定义了），可能导致 dist 被重新构建。这是正常行为——确保 `prepublishOnly` 中的 build 命令产出正确的产物。

---

## 六、.npmrc 模板

```ini
# 日常状态（download token）
//npm.pkg.github.com/:_authToken=ghp_your_download_token
@YourScope:registry=https://npm.pkg.github.com/
```

发布时脚本会临时将 `_authToken` 替换为 publish token，发布后立即换回。

---

## 七、参考

- 实际使用案例：`D:\Code\Project\deepcode-cli\scripts\release.js`
- npm workspaces 发布：`npm publish --workspace packages/<name>`
- GitHub Packages 文档：https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry
