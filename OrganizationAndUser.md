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
  accounts:
    github: HeYS-Snowe          # GitHub 大号（唯一账号）；曾用名 1010523654
    gitee: HeYS-Snowe           # Gitee 账号
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
| GitHub | [github.com/HeYS-Snowe](https://github.com/HeYS-Snowe) | 唯一 GitHub 大号（曾用名 1010523654） |
| Gitee | [gitee.com/HeYS-Snowe](https://gitee.com/HeYS-Snowe) | Gitee 账号 |

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
| GitHub 仓库命名空间 | `Qore-Origins/<repo>`（GitHub 简称 Qore 已被占用，用全称命名空间） | `Qore-Origins/Trace` |
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

## 六、用户数字档案（画像 / 项目 / 经历）

> 本部分是用户"所有拥有的"的 SSOT——蒸馏规则（compact-core §〇）的会话前读取对象。
> **持续蒸馏义务**：本节是活文档。任何会话中发现新的画像属性、项目、荣誉、偏好变化，**即时更新本节**（NEVER 等到收尾）；更新后画像即为最新权威版本，旧快照作废。

### 6.0 个人画像属性

```yaml
基本属性:
  姓名: 王喆
  出生日期: 阳历 2006-12-03（出生证明未精确到分钟；母亲述 9:15 出手术室、本人 9:00 左右出生）
  出生时辰: 辰时（2026-09-27 用户确认）
  性别: 男（2026-09-28 用户确认——排盘大运方向依据）
  星座: 射手座
  八字四柱: 丙戌(年) 己亥(月) 丙寅(日) 壬辰(时)——2026-09-28 本地公式排盘（双锚点验证 + 节气边界 2006 大雪 12-07 14:25），计算方法见 `.prompt\解决方案\通用\八字四柱本地计算-20260928`
  神煞: 华盖/天乙贵人/学堂（日坐长生）/天喜 成立；劫煞/孤辰/元辰/空亡（年月支戌亥落甲子旬空）同在；十灵日/太极贵人/文昌/桃花/驿马/羊刃/禄神/将星/魁罡均不成立（2026-09-28 全量复算 17 项，判定表见  §六）
  纳音: 屋上土(年) / 平地木(月) / 炉中火(日) / 长流水(时)
  大运: 阳年男顺排（性别已确认），约 1.4 岁起运；当前行辛丑运（2017~2027），2027 底换壬寅运；完整八步大运表见 solution §八
  生命灵数: 5（2026-09-28 计算）
  MBTI: INTJ（2026-09-28 用户确认）

性格与自我认知（对话实证，溯源见 .prompt\通用\用户方法论蒸馏-20260906）:
  独处偏好: "观呼吸，然后复盘……我挺喜欢独处的"（#751）
  底层原理驱动: "我喜欢去找底层原理，去探索为什么"（#528）
  自信轨迹: 早年自卑 → 获认可后转化（#218）
  中庸取向: 不求满但求心安；过渡态思维（#288/440）
  珍视随机性: "人最稀有的就是不确定性……蒸馏后随机性越来越小。而人也是自由的"（#751，对蒸馏自己的元认知）
  心语状态: 不由自主进入的内心对话/顿悟状态，类 /compact（#682）

家庭线索:
  妹妹: 2026 年时 7 岁
```

### 6.1 项目地图（按来源分类；git remote 实测标注）

```yaml
项目来源:
  1_开源改造_自用:
    - D:\Code\Project\deepcode-cli          # DeepGem 本体（自有 repo）
    - D:\Code\Project\VS-Code-Auto-Type-Code
    - D:\Code\Project\AI Resource Hub       # 前身 CC Switch（基于 farion1231/cc-switch MIT 改造）
    - deepcode-cli-feishu / dsh-feishu-spike / Claude接入飞书   # deepcode 生态实验（2026-09 git 实测补充）
  2_个人生活需求_兴趣:
    - D:\Code\Project\Qore                  # 组织主目录（Trace / Jeenith）
    - D:\Code\Project\Loop                  # remote: 1010523654（HeYS-Snowe 账号曾用名，改名前 URL 仍生效）
    - D:\Code\Project\Lumina                # remote: 1010523654（曾用名 URL）
    - D:\Code\Project\Minecraft_Mod         # remote: 1010523654（曾用名 URL）
  3_学校比赛项目:
    - D:\Code\Project\intelligent-laboratory-system   # 自有 repo；伴生产品 workbench（D:\Code\.Model\YOLO\Scripts\workbench）
    - D:\Code\Project\MindCare_AI           # remote: 1010523654（曾用名 URL）；三端（MiniApp+admin+backend）
    - D:\Code\Project\Campus_Sentinel
    - D:\Code\Project\Intelligent-Agent     # remote: 1010523654（曾用名 URL）
    - D:\Code\Project\技能大赛-国赛          # git 实测补充
    - D:\Code\Project\数学建模
    - D:\Code\Project\麒麟OS-Agent比赛
    - D:\Code\Project\创新创业小组作业
  4_对AI的研究:
    - D:\Code\Project\ABL-Agent-Benchmark-Lab      # 多智能体基准测试
    - D:\Code\Project\Claude_Code-2.1.88           # CC 源码参考库
    - D:\Code\Project\智能体记忆系统研究
  5_使用AI过程中的产物:
    - D:\Code\.AI-TOOLS         # Agents/Skills/Scripts/manifest 中心
    - D:\Code\.prompt           # 提示词与解决方案库
    - D:\Code\.Rules            # AI 编码规则体系（本库）
    - D:\Code\AI资源管理        # 资源中转站
  其他_自有:
    - D:\Desktop\Claude临时对话                      # 与 CC 的对话记录
    - D:\Code\Project\Snowe_Stack                  # 腾讯轻量服务器（heys-snowe.tj.cn），个人栈/电子简历
    - D:\Code\Project\SnoweShop
    - D:\Code\Project\MatiJiTrainer / Model_Deployment_Interface
    - D:\Code\AI资源管理\Reaource\260827\AI开源闭源之争新媒体   # DeepSeek 对话产物
  克隆_参考项目（非自研，勿计入作品）:
    - marktext / marktext-develop     # 开源 Markdown 编辑器
    - lottery                         # 抽奖
    - Recruitment-Data-Visualization-Project
    - competition_2026                # gitee 比赛模板
    - OS-Agent-                       # sunhao33
    - VSCode-win32-x64                # VS Code 官方构建产物
```

### 6.2 个人 AI 经历（时间线）

```yaml
AI经历:
  2025-09-01: 开启天津职业大学学生生活
  2025-11-11 21:42:31: 经 B 站视频首次安装 Coding Agent——Claude Code（CC）；首次 CC + glm-4.5；购入 GLM Coding Lite（月，20￥ 新人价）
  2025-11-14 16:54:33: 升级 GLM Coding Pro（月）
  2025-12-12 14:10:55: 购入 GLM Coding Pro（季，266.55￥）
  2026-03-12 20:51:37: 购入 GLM Coding Pro（年，1251.6￥）
  2026-07-28 13:51:53: 因技能大赛-国赛与 GLM 额度不足，切换 DeepSeek
  DeepSeek 现状:
    累计充值: 870￥ / 累计消费: 865.71￥
    个人 key: 242.96￥ / 3,436,447,947 Tokens / 16,880 次请求（与国赛 key 分离）
    账户总计: 7,533,845,950 Tokens
```

### 6.3 工具与模型使用

- 常用：CC（2025-11 至今近 1 年，主力）、codex、opencode、deepcode（官方版+个人定制 DeepGem）、Trae IDE/Work、Cursor（曾购两次 20$ 订阅用 Opus 4.6/5）、VM 中 OpenClaw——**随时切换**
- 模型轨迹：glm-4.5 → glm-5.3（flash 少用）、deepseek-v4 全系、Trae 支持模型
- 意义：对话分散多工具，规则/资源库共享——蒸馏规则（§〇）存在的理由
- 消费偏好：性价比优先；个人与赛事 key 分离

### 6.4 DeepSeek 对话资产

```yaml
位置: D:\Desktop\Claude临时对话\DeepSeek-历史对话数据（归档: Reaource\260927\）
规模: 751 会话（2025-02 ~ 2026-09）
蒸馏产物: .prompt\通用\DeepSeek历史对话蒸馏-20260906、用户方法论蒸馏-20260906、AI本质推演蒸馏-20260906
```

### 6.5 荣誉与竞赛（待补全）

| 事项 | 状态 |
|------|------|
| 技能大赛-国赛（暨世界职业院校技能大赛排位赛） | **银奖**（2026） |
| 数学建模竞赛 | 已参赛（2026，MATLAB 方向，#277） |
| 创新创业 | 小组作业/双创总结会参与 |
| 麒麟OS-Agent比赛 | 有项目目录，详情待补 |

> 荣誉获奖信息出现时**即时登记本表**（持续蒸馏义务）。

### 6.6 待确认

- `1010523654` GitHub 账号是否为用户小号（Loop/Lumina/Minecraft_Mod/Intelligent-Agent/MindCare_AI 的 remote 归属）
- MBTI 测评结果（如已测）
- 各竞赛获奖等级

---

*本文件由 HeYS-Snowe 维护*
