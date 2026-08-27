# 组织与开发者信息 (Organization & Developer Identity)

> 本文件是组织与开发者身份的**唯一事实来源**（Single Source of Truth）。
> 包名、命名空间、版权、署名等需引用身份信息的场景，统一从本文件取值，NEVER 在各项目内硬编码不一致的身份信息。
> 创建日期: 2026-07-05 | 维护人: HeYS-Snowe

---

## 一、组织信息

| 属性 | 值 |
|------|-----|
| 组织名称（全称） | Qore Origins |
| 组织名称（简称） | Qore |
| 组织名（中文） | 叩心 |
| 名称由来 | 简称 Qore 取自 "QoCore"，"Qo" 音译自"叩"；全称 Qore Origins |
| 口号·一（中文） | 叩问本心，不忘初心 |
| 口号·一（英文·完整） | Question the core. Return to origins. |
| 口号·一（英文·凝练） | To the core, to the start. |
| 口号·二（中文） | 未雨绸缪，兜底必达 |
| 口号·二（英文·完整） | Plan ahead. Fallback guaranteed. |
| 口号·二（英文·凝练） | To plan, to guarantee. |

```yaml
组织标识:
  全称: Qore Origins
  简称: Qore
  中文: 叩心
  名称由来:
    简称 Qore: 取自 "QoCore"
    Qo: 音译自"叩"
    Core: 字面意为"核心"，呼应口号中的"本心"
    Origins: 字面意为"起源/本源"，呼应口号"不忘初心"
  slogan:
    一:                                    # 初心之本
      中文: 叩问本心，不忘初心
      英文:
        完整: Question the core. Return to origins.
        凝练: To the core, to the start.
    二:                                    # 做事之道：两手准备，兜底必达
      中文: 未雨绸缪，兜底必达
      英文:
        完整: Plan ahead. Fallback guaranteed.
        凝练: To plan, to guarantee.
```

---

## 二、开发者信息

| 属性 | 值 |
|------|-----|
| 开发者 / 管理员 / 所属人 | HeYS-Snowe |
| 角色 | 唯一开发者（全栈、架构、维护） |
| 合作伙伴 | 暂无 |
| 所属组织 | Qore（叩心） |

```yaml
开发者:
  username: HeYS-Snowe
  role: sole-developer          # 唯一开发者
  collaborators: []             # 暂无合作伙伴
  owns:
    - D:\Code\.Rules             # AI 编码规则体系
    - D:\Code\.prompt            # 提示词与解决方案库
    # 及其他 Qore 名下项目
```

---

## 三、服务器与域名

| 属性 | 值 |
|------|-----|
| 云服务商 | 腾讯云轻量应用服务器 |
| IPv4 | 62.234.115.217 |
| ICP 备案 | 已备案 |
| 公安联网备案 | 已备案 |
| 个人域名 | heys-snowe.tj.cn |

```yaml
服务器与域名:
  云服务商: 腾讯云轻量应用服务器
  ipv4: 62.234.115.217
  备案:
    icp: 已备案
    公安联网: 已备案
  域名: heys-snowe.tj.cn
```

> 域名 `heys-snowe.tj.cn` 反写为 `cn.tj.snowe.heys`；当与 §四 包名约定冲突时，以**域名反写**为准（遵循 §四 说明）。

---

## 四、包名与命名约定

> 用于 Java / Kotlin / Android / Flutter / Dart 等需要反向域名包名的场景。

```yaml
包名前缀（推荐）: com.qore
备选:
  - org.qore                    # 以组织 / 开源身份发布时
  - io.qore                     # 若持有对应域名
完整包名示例:
  - com.qore.<项目简称>           # 如 com.qore.loop
  - com.qore.<项目简称>.<模块>    # 如 com.qore.loop.feature.auth
```

> 默认取 `com.qore`；若实际持有域名不同，以**域名反写**为准。

### 各场景命名

| 场景 | 约定 | 示例 |
|------|------|------|
| Android applicationId / package | `com.qore.<项目>` | `com.qore.loop` |
| Java / Kotlin 包名 | `com.qore.<项目>.<模块>` | `com.qore.loop.data` |
| Flutter / iOS bundleIdentifier | 反向域名，同上 | `com.qore.loop` |
| npm scope（如适用） | `@qore/<pkg>` | `@qore/ui` |
| GitHub 仓库命名空间 | `qore/<repo>` | `qore/loop` |
| 版权署名 | Copyright (c) `<年份>` Qore | `Copyright (c) 2026 Qore` |

---

## 五、版权与署名

```yaml
版权归属: Qore（叩心）
开发者署名: HeYS-Snowe
版权声明模板: Copyright (c) {年份} Qore. All rights reserved.
```

---

## 六、引用方式

各项目需要组织 / 开发者身份信息时，以本文件为准：

```
身份信息（组织、包名前缀、署名）以 D:\Code\.Rules\OrganizationAndUser.md 为准。
```

- NEVER 在项目内硬编码与本文件不一致的组织名、包名前缀或署名
- 本文件变更时，需同步检查所有依赖项目的包名 / 署名配置

---

## 六、用户数字档案（项目地图与 AI 经历）

> 本部分是用户"所有拥有的"的 SSOT——蒸馏规则（compact-core §〇）的会话前读取对象。
> 维护约定：新项目、工具切换、AI 消费里程碑发生时，同步更新本节。

### 6.1 项目地图（按来源分类）

```yaml
项目来源:
  1_开源改造_自用:            # 开源项目按自身需求二次改造（自己要用）
    - D:\Code\Project\deepcode-cli          # DeepGem 本体
    - D:\Code\Project\VS-Code-Auto-Type-Code
    - D:\Code\Project\AI Resource Hub       # 前身 CC Switch
  2_个人生活需求_兴趣:
    - D:\Code\Project\Qore
    - D:\Code\Project\Loop
    - D:\Code\Project\Lumina
    - D:\Code\Project\Minecraft_Mod
  3_学校比赛项目:
    - D:\Code\Project\intelligent-laboratory-system
    - D:\Code\Project\MindCare_AI
    - D:\Code\Project\Campus_Sentinel
    - D:\Code\Project\Intelligent-Agent
  4_对AI的研究:
    - D:\Code\Project\ABL-Agent-Benchmark-Lab      # 多智能体基准测试
    - D:\Code\Project\Claude_Code-2.1.88           # CC 源码参考库
    - D:\Code\Project\智能体记忆系统研究
  5_使用AI过程中的产物:
    - D:\Code\.AI-TOOLS         # Agents/Skills/Scripts 中心
    - D:\Code\.prompt           # 提示词与解决方案库
    - D:\Code\.Rules            # AI 编码规则体系（本库）
    - D:\Code\AI资源管理        # 资源中转站（libs/批次目录）
  其他:
    - D:\Desktop\Claude临时对话                      # 与 CC 的对话记录
    - D:\Code\Project\Snowe_Stack                    # 部署于腾讯轻量服务器（heys-snowe.tj.cn），个人栈/电子简历
    - D:\Code\AI资源管理\Reaource\260827\AI开源闭源之争新媒体   # 与 DeepSeek 网页/手机 APP 的对话产物
```

### 6.2 个人 AI 经历（时间线）

```yaml
AI经历:
  2025-09-01: 开启天津职业大学学生生活
  2025-11-11 21:42:31: 经 B 站视频首次安装 Coding Agent——Claude Code（CC）
  2025-11-11 21:42:31: 首次使用 CC + glm-4.5；购入 GLM Coding Lite（月订阅，20￥ 新人价）
  2025-11-14 16:54:33: 升级 GLM Coding Pro（月）
  2025-12-12 14:10:55: 购入 GLM Coding Pro（季，266.55￥）
  2026-03-12 20:51:37: 购入 GLM Coding Pro（年，1251.6￥）
  2026-07-28 13:51:53: 因技能大赛-国赛需求与 GLM 不够用，切换至 DeepSeek
  DeepSeek 现状:
    累计充值: 870￥
    累计消费: 865.71￥
    个人用途 key: 242.96￥ / 3,436,447,947 Tokens / 16,880 次 API 请求（与国赛 key 分离）
    账户总消费: 865.71￥ / 7,533,845,950 Tokens
```

### 6.3 工具使用与切换

- 常用 Agent：CC（Claude Code）、codex、opencode、deepcode（DeepGem 二次改造）——**随时可能切换**
- 意义：对话分散在各工具/各项目，但规则与资源库（.Rules / .prompt / .AI-TOOLS）是共享的——这正是蒸馏规则（compact-core §〇）存在的理由
- 消费偏好：性价比优先（GLM → DeepSeek 的切换原因）；个人与赛事 key 分离

---

*本文件由 HeYS-Snowe 维护*
