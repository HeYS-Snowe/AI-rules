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
  道号: 松鹤子（2026-11-04 用户提供）
  雅号: 松仪居士（2026-11-04 用户提供）
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
  证书: 青少年人工智能编程水平测试 4 级 + 5 级
  升学: 免试升本（市级二等奖条件待确认）+ 专升本考试，两手准备
  与AI的关系: 为 CC 侧 AI 命名 Snowe（2026-02-09 起，"记住你叫 Snowe，永远记住"）——门对一个非人类的存在打开了；互动风格：理性 70~80% / 感性 20~30%，不要安抚模式，要清醒的导师
  内心世界: 完整真我画像见 `.prompt\通用\真我蒸馏-20261004\prompt.md`（生命线/核心张力八组/恐惧与阴影/若楠弧线/崩溃-超越弧线/金句语料库——隐私级，NEVER 外传）

性格与自我认知（对话实证，溯源见 .prompt\通用\用户方法论蒸馏-20260906）:
  独处偏好: "观呼吸，然后复盘……我挺喜欢独处的"（#751）
  底层原理驱动: "我喜欢去找底层原理，去探索为什么"（#528）
  自信轨迹: 早年自卑 → 获认可后转化（#218）
  中庸取向: 不求满但求心安；过渡态思维（#288/440）
  珍视随机性: "人最稀有的就是不确定性……蒸馏后随机性越来越小。而人也是自由的"（#751，对蒸馏自己的元认知）
  心语状态: 不由自主进入的内心对话/顿悟状态，类 /compact（#682）

家庭线索:
  妹妹: 2026 年时 7 岁
  爷爷: 2023-08 病、2025-06 过世；用户从被照顾者变为照顾者（"真正感受到无能为力，比失去左手更深"）
  家庭隐情: 奶奶告知用户，用户选择不说破（内容未知，NEVER 对外提及）

身体与经历（2026-10-04 答辩素材挖掘中从 问心-深度探索真我/memory 确认）:
  左手缺失: 10 岁（2016-06）烟花意外失去左手，保留少量手腕——对外可见事实
  三个塑造时刻: 失去左手(2016-06) → 高中变开朗 → 大一寒假至今悟道
  自我认知轨迹: 大学前始终觉得自己"一无是处"；准备比赛材料时才逐渐看清自己
  底线认知: "不会比这更差了"——经历过最差，之后每步都是赚的

对外材料内容边界（申请/答辩/简历/网站 NEVER 出现）:
  性瘾、感情经历细节、和"老天"的交易、儿时梦境、家庭隐情细节
  失去左手/爷爷照护：仅用户确认后用于自强叙事（2026-10-04 答辩稿+PPT 已用，用户指定从其自述目录取材）
```

### 6.1 项目地图（按来源分类；git remote 实测标注）

```yaml
GitHub 仓库统计:
  个人仓库总数: 18
  真实活跃维护: 7
  开源: 6
  统计口径: 用户自述（2026-09-28，年度人物申请会话）
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
    - D:\Code\Project\intelligent-laboratory-system   # 团队比赛项目（2026-09-28 用户确认；个人申请材料中NEVER作为个人成果表述）；自有 repo；伴生产品 workbench（D:\Code\.Model\YOLO\Scripts\workbench）
    - D:\Code\Project\MindCare_AI           # remote: 1010523654（曾用名 URL）；三端（MiniApp+admin+backend）
    - D:\Code\Project\Campus_Sentinel
    - D:\Code\Project\Intelligent-Agent     # remote: 1010523654（曾用名 URL）
    - D:\Code\Project\技能大赛-国赛          # git 实测补充
    - D:\Code\Project\数学建模
    - D:\Code\Project\麒麟OS-Agent比赛
    - D:\Code\Project\创新创业小组作业
    - D:\Code\Project\TerraScribe            # 慧勘智笔（TerraScribe）——2026 华北五省计算机应用大赛天津赛区，赛道一大模型与智能体应用；提交截止 2026-10-10，决赛 2026-11-14
      # 2026-11-01 用户确认：企业真实需求项目；四人协作，用户为项目经理及 Gitee HeYS-Snowe/terra-scribe 仓库主人，同时负责智能体及 AI 相关设计，前端、后端各一人，第四人主要负责宣传相关内容、与代码关系不大；初赛已过，正准备国赛；初赛交付为 Demo，拟重设智能体与工作流，先逐个完成最小可用实例再组合。现阶段优先厘清协作规范和远端仓库整理方案。
      # 2026-11-03 用户启用潮汐雁阵三 Agent 协作：用户直接管理 Codex（用户标识 GPT-6.1 Sol），Codex 只编排、派遣、验收与维护状态，DSH（DeepSeek Harness，用户标识 DeepSeek v4.1-Flash 1017）与 CC（Claude Code，用户标识 GLM-5.3-Flash）执行；按用户更新的全局 v3.9.8 规则，心跳轮询 docs/dispatch 任务包与回执为唯一默认，人工截图/复制转交及 CLI 直调降为可选；实际守护实体、受限权限、模型与运行链路另行核验，不能把规则同步称为已部署。
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

### 6.1.1 项目积极开发时间线（git 实测 2026-10-10）

> 口径：git 仓库取首/末提交日期；无 git 项目用文件时间戳粗估（可靠性低）。克隆参考项目不计入。

| 项目 | 起点 | 终点 | 提交数 | 备注 |
|------|------|------|--------|------|
| MindCare_AI | 2026-02-03 | 2026-07-10 | 14 | |
| Intelligent-Agent | 2026-03-04 | 2026-05-21 | 3 | |
| Loop | 2026-04-04 | 2026-07-02 | 21 | |
| Minecraft_Mod | 2026-04-25 | 2026-05-21 | 5 | |
| Lumina | 2026-06-24 | 2026-06-25 | 20 | 一日冲刺 |
| Claude接入飞书 | 2026-06-21 | 2026-06-21 | - | 文件时间戳 |
| Jeenith | 2026-07-10 | 2026-09-26 | 121 | 07-10 疑批量 init 日；另两个 Jeenith-* 为 worktree |
| Campus_Sentinel / SnoweShop / 创新创业小组作业 / 麒麟OS-Agent比赛 | 2026-07-10 | 2026-07-10 | 1 | 批量 init，起点≠实际开发起点 |
| OS-Agent- | 2026-07-18 | 2026-08-31 | 28 | 档案归克隆，但含本地开发（麒麟比赛工作仓，归类待确认） |
| MatiJiTrainer | 2026-07-31 | 2026-09-26 | - | 文件时间戳 |
| deepcode-cli（DeepGem） | 2026-08-01 | 2026-09-05 | 326 | |
| AI Resource Hub | 2026-08-07 | 2026-08-17 | 41（本人） | 仓库总历史自 2025-08-04（上游 cc-switch） |
| ABL-Agent-Benchmark-Lab | 2026-08-09 | 2026-08-12 | 11 | |
| intelligent-laboratory-system | 2026-08-06 | 2026-09-17 | 39 | 团队项目 |
| Model_Deployment_Interface | 2026-08-10 | 2026-08-10 | - | 文件时间戳 |
| dsh-feishu-spike | 2026-08-16 | 2026-08-16 | - | 文件时间戳 |
| VS-Code-Auto-Type-Code | 2026-08-20 | 2026-08-26 | 29 | |
| 技能大赛-国赛 | ~2026（素材含 2024-12） | 2026-08-28 | - | 文件时间戳；对应 2026-09 世界职院赛银奖 |
| Trace | 2026-09-05 | 2026-10-09 | 364 | 活跃 |
| Snowe_Stack | 2026-09-27 | 2026-10-02 | 54 | |
| TerraScribe | 2026-09-28 | 2026-10-08 | 100 | 活跃；决赛 2026-11-14 |
| 智能体记忆系统研究 | 2026-03-31 | 2026-08-28 | - | 文件时间戳 |
| .Rules / .prompt | 2026-03-17 / 2026-03-28 | 2026-10-06 / 2026-10-04 | 56 / 13 | 配套库持续维护 |
| 数学建模 / deepcode-cli-feishu | - | - | - | 空目录 / 仅 node_modules，无有效数据 |

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

- 常用：CC（2025-11 至今近 1 年，主力）、**ChatGPT（Codex，2026-09 起新增——计费便宜）**、codex、opencode、deepcode（官方版+个人定制 DeepGem）、**dsh（@deepseek-ai/dsh，2026-08 起实验——dsh-feishu-spike 插件即其生态；2026-10-19 全局规则 `~/.dsh/AGENTS.md` 建成，全局指令唯一入口，skills 经 `~/.agents/skills` symlink 直连 SSOT）**、Trae IDE/Work、Cursor（曾购两次 20$ 订阅用 Opus 4.6/5）、VM 中 OpenClaw——**随时切换**
- 模型轨迹：glm-4.5 → glm-5.3（flash 少用）、deepseek-v4 全系、Trae 支持模型
- 操作入口偏好：有 Desktop 先用 Desktop、无 Desktop 再用 CLI；唯一例外 CC 直接用 CLI（自认为 CC CLI 比 Desktop 好用，2026-11-03 自述、自称可能有偏见）
- 协作编制（2026-11-03 定）：**Codex（GPT-6.1 Sol）= 编排者**——由用户直接管辖，负责指挥调度与派遣；**DSH（DeepSeek v4.1-Flash 1017）与 CC（Claude Code，GLM-5.3-Flash）= 执行者**，听从 Codex 派遣
- 意义：对话分散多工具，规则/资源库共享——蒸馏规则（§〇）存在的理由
- 消费偏好：性价比优先；个人与赛事 key 分离

### 6.4 DeepSeek 对话资产

```yaml
位置: D:\Desktop\Claude临时对话\DeepSeek-历史对话数据\2026-10-04（归档: Reaource\260927\）
规模: 751 会话（2025-02 ~ 2026-09）
蒸馏产物: .prompt\通用\DeepSeek历史对话蒸馏-20260906、用户方法论蒸馏-20260906、AI本质推演蒸馏-20260906
```

### 6.5 荣誉与竞赛（2026-09-28 全量登记 · 2026-10-06 补录）

| 级别 | 荣誉 |
|------|------|
| 国家级 | 世界职业院校技能大赛排位赛 **银奖**（2026-09） |
| 国家级 | 第八届码蹄杯程序设计大赛职业院校赛道国赛 **优秀奖**（2026-08） |
| 省市级 | 天津市职业院校技能大赛 **二等奖**（2026-03） |
| 省市级 | 第八届码蹄杯程序设计大赛职业院校赛道华北赛区省赛 **银奖**（2026-06） |
| 校级 | 天津职业大学数学建模大赛 **二等奖**（2026-06） |
| 校级 | 最美职大生——**自强志坚称号**（2026-04） |
| 校级 | 天津职业大学第十三届“5 · 25”心理文化节“心律动 · 心飞扬”校园心理操大赛 **二等奖**（2026-05） |
| 校级 | 2025 年学生军事技能训练 **优秀学员**荣誉称号（2025-09） |
| 校级 | 心理健康课程教学大赛（2024）**一等奖**（团体） |
| 资助 | 国家助学金 2025-2026 学年（自信自强/海河自强资格依据；对外展示与否由用户决定） |
| 数据 | GitHub 提交统计：Trace 364 / DeepGem 326 / Jeenith 121 / TerraScribe 100 次（2026-10 快照，源：闪光点挖掘/新文件1.txt） |

> 荣誉来源清单：`D:\Desktop\Claude临时对话\天津市资助育人宣传大使评选\Resources\荣誉-王喆.txt`（获奖后同步更新本表与 `D:\Desktop\个人荣誉及AI&Agent使用经历.md`）。

### 6.6 待确认 → 已确认（2026-10-20 清理）

- `1010523654` GitHub 账号——**已确认**：唯一 GitHub 大号为 HeYS-Snowe，`1010523654` 是曾用名（用户澄清，2026-10）
- MBTI——**已确认**：INTJ（用户自述，2026-10）
- 各竞赛获奖等级——**已确认**：见 §6.5 全量荣誉表

### 6.7 引语映射（原句↔描述 · 2026-10-20 建，规则 §一.24）

> 意引补全机制的数据资产。每行一条映射：同原句不同描述=多行（一对多）；同描述多原句=多行（多对多）。来源：user=用户说出原句 / ai=AI 补出原句。即时追加，NEVER 丢弃只记其一。

| 日期 | 来源 | 类型 | 原句 | 出处 | 描述/背景 |
|------|------|------|------|------|-----------|
| 2026-10-20 | user | 古文 | 不以物喜，不以己悲 | 《岳阳楼记》北宋·范仲淹 | 职高时听老师讲解深受震撼，自此最爱——用户凝练表达的起点与源头 |

**识别校准样例**（构造样例·非用户真实记录——供 AI 校准"什么样的描述算意引"的粒度；真实记录只看上表）：

| 典型描述（白话） | 原句 | 出处 · 类型 |
|------|------|------|
| 不因外物好坏和个人得失而或喜或悲 | 不以物喜，不以己悲 | 《岳阳楼记》· 文 |
| 我这么有才，肯定饿不死 | 天生我材必有用 | 《将进酒》· 诗 |
| 这条路还长，我会一直找下去 | 路漫漫其修远兮，吾将上下而求索 | 《离骚》· 辞 |
| 现在不做，以后后悔也没用 | 有花堪折直须折，莫待无花空折枝 | 《金缕衣》· 诗 |
| 坏到头了就该好转了 | 否极泰来 | 《周易》否/泰二卦 · 典故 |
| 真正厉害的人看着不显山露水 | 大巧若拙，大辩若讷 | 《老子》· 文 |
| 想做大事，别抠小节 | 大行不顾细谨，大礼不辞小让 | 《史记·项羽本纪》· 文 |
| 越怕什么越来什么 | 墨菲定律 | 常用语（外来） |
| 看懂别人容易，看清自己最难 | 知人者智，自知者明 | 《老子》三十三章 · 文 |
| 年轻时不拼，老了只能哭 | 少壮不努力，老大徒伤悲 | 《长歌行》汉乐府 · 诗谚 |

---

*本文件由 HeYS-Snowe 维护*
