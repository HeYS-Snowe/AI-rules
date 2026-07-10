# 项目清单 (Project List)

> HeYS-Snowe 名下所有项目的**导航索引**——项目名、根路径、一句话识别。
> 创建日期: 2026-07-10 | 维护人: HeYS-Snowe
> **最后状态扫描**: 2026-07-10（基于 git 提交历史与文件改动时间，快照性质）

---

## 定位与维护原则

本文件是**项目清单的单一事实来源**（Single Source of Truth），只存"导航级"信息：

- ✅ **存**：项目名、根路径、技术栈大类、一句话定位、活跃状态
- ❌ **不存**：具体版本号、依赖清单、包名、目录结构等技术细节

> **DRY 原则**：技术细节已存在于各项目内（`CLAUDE.md` / `pubspec.yaml` / `package.json` 等），本文件 NEVER 重复抄录 — 原因：两处事实来源迟早不一致。需要细节请直接读对应项目的 `CLAUDE.md`。

### 维护规则

- **新增项目**：在所属分组追加一行，根路径必须核实（NEVER 凭记忆写路径）
- **状态变更**：仅更新"状态"列；技术栈变化改项目内文件，不改本表
- **路径以实际磁盘为准**：项目搬迁后同步本表
- **状态列依据**：git 最后提交时间 > 文件改动时间；定期重新扫描刷新

---

## 一、Qore 产品线

> 所属组织 Qore（叩心），包名前缀 `com.qore`，身份见 `OrganizationAndUser.md`

| 项目 | 根路径 | 类型 | 定位 | 状态（扫描于 2026-07-10） |
|------|--------|------|------|---------------------------|
| 志极 Jeenith | `D:\Code\Project\Qore\Jeenith` | Flutter 全平台 | 叩问本心的卜算合集（小六壬、周易及更多） | 🟢 刚起步（今日开发，未入 git） |
| Loop | `D:\Code\Project\Loop` | Flutter (Android) | 移动 App（日计划/课表方向，Riverpod 3 + Drift） | 🟢 活跃 · 最后提交 7/02 |
| 校卫哨兵 Campus Sentinel | `D:\Code\Project\Campus_Sentinel` | Flutter + FastAPI(候选) | 校园安防预警 App（含图像识别预留） | ⚠️ git 已 init 但无提交历史，待核实 |

---

## 二、应用与产品

| 项目 | 根路径 | 类型 | 定位 | 状态（扫描于 2026-07-10） |
|------|--------|------|------|---------------------------|
| 灵幕 Lumina | `D:\Code\Project\Lumina` | Electron + Web (React/TS) | AI 驱动桌面+Web 双形态应用，一套 renderer | 🟡 近期 · 最后提交 6/25，2 文件未提交 |
| MindCare AI | `D:\Code\Project\MindCare_AI` | 全栈（FastAPI+Flutter+小程序+Admin） | 校园心理健康多模态预警系统（文本/语音/表情） | 🔴 停滞 7 周（5/21 仅文档），9 文件未提交 |
| SnoweShop | `D:\Code\Project\SnoweShop` | Flutter(GetX) + 待建后端 | 电商 App，正从第三方 API 迁移到自建后端 | 🔴 长期停滞 · 4 月+ 未动（非 git） |
| JobViz | `D:\Code\Project\Recruitment-Data-Visualization-Project` | 全栈 Web + 数据 | 招聘数据可视化平台 | 🔴 完赛停滞 · 3/30 赛前提交后仅文档 |
| Intelligent-Agent | `D:\Code\Project\Intelligent-Agent` | 全栈（Vue 3 + FastAPI） | AI 对话界面 | 🔴 停滞 7 周（5/21 仅文档） |
| 叩心 KouXin | `D:\Code\Project\创新创业小组作业` | 微信小程序 + Node.js | 校园互助小程序（拼车匹配 + 学习搭子匹配） | 🟢 活跃 · 本周仍在改（非 git） |

---

## 三、开发资源与比赛

| 项目 | 根路径 | 类型 | 定位 | 状态（扫描于 2026-07-10） |
|------|--------|------|------|---------------------------|
| Minecraft Mod | `D:\Code\Project\Minecraft_Mod` | Java / Gradle | Minecraft 模组合集（Fabric / Forge / NeoForge，含 CriticalCore、KillLine 等） | 🔴 停滞 7 周 · KillLine V2.0 后无动 |
| 麒麟 OS-Agent 比赛 | `D:\Code\Project\麒麟OS-Agent比赛` | 比赛 / 资料 | 麒麟软件 OS Agent 记忆优化及高效应用研究比赛（含 ISO、向量库 SDK、数据集等资源，暂无代码） | 🟡 资料期 · 7/05 仍在加资源（非 git） |

---

## 四、规则与知识库（基础设施，非业务项目）

> 这两个仓库与其他项目性质不同：它们是**供给所有项目使用的规则与知识底座**，不是被开发的产品。

| 仓库 | 路径 | 角色 |
|------|------|------|
| AI 编码规则体系 | `D:\Code\.Rules` | 本仓库。规则即模板的编码规范系统（common/ + stacks/），所有项目定制规则的来源 |
| 提示词与解决方案库 | `D:\Code\.prompt` | 提示词存储 + 问题解决方案沉淀，每次会话"会话前搜索、会话后沉淀"的目标库 |

**与业务项目的边界**：
- `.Rules` / `.prompt` 的变更影响所有下游项目，修改需谨慎评估
- 业务项目通过引用（`CLAUDE.md` 指向 `main.md`）消费这两个仓库，NEVER 将规则复制进业务项目

---

## 扫描备注（2026-07-10）

- **未纳入 git 的项目**（4 个）：Jeenith、SnoweShop、麒麟比赛、KouXin —— 建议尽早 `git init` 并做首次提交，避免成果遗失
- **MindCare_AI 有 9 个未提交文件**：停滞却挂着半成品改动，建议提交或清理
- **Loop 版本号不一致**：`CLAUDE.md` 标 `v1.3.0`，但 git 提交显示"v0.1.0 版本里程碑"——需区分文档版本与应用版本
- **Campus_Sentinel git 异常**：`.git` 存在但无提交历史、`status` 显示 0 文件，需人工核实（可能 init 后未 commit，或内容被 .gitignore 忽略）
- 5/21 有 4 个项目（MindCare_AI / Minecraft_Mod / JobViz / Intelligent-Agent）同步更新了文档，疑似统一接入 `.Rules` 规则体系，代码本身停滞更久

---

## 备注

- 各项目的**权威定制规则**为其根目录的 `CLAUDE.md`（麒麟比赛目录无代码故无 CLAUDE.md）
- 技术栈、版本、目录结构等细节 → 查对应项目 `CLAUDE.md`，NEVER 以本表为准
- 状态列为快照，精确进度以项目内文档与 git 历史为准；如本表状态过时，重新扫描后改本表

---

*本文件由 HeYS-Snowe 维护 | 身份信息见 `OrganizationAndUser.md`*
