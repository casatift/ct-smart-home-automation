# CT智能家居自动化

让 AI 根据你家的设备与生活习惯，规划、建设和维护 Home Assistant 自动化。

**当前版本：v1.0.2。** 这是可供 AI 读取或安装的技能与参考文档，不是 HA 集成、自动运行程序或预设全屋配置。除非用户指定其他方案，创建自动化默认优先使用 HA 原生自动化与脚本，界面或 YAML 均可；需要保存、共享和人工配置的变量使用 Helper 实体。用户指定 AppDaemon、Node-RED、Pyscript 等时尊重选择，不强制迁移已有规则，也不要求购买一套固定设备。

## 快速开始：把链接发给 AI

复制下面这段话给你使用的 AI：

```text
请读取 https://github.com/casatift/ct-smart-home-automation 的 README，
再读取 skills/home-automation-design/SKILL.md，并按当前任务读取相关 references。
使用“CT智能家居自动化”帮助我规划 Home Assistant。
先确认你能否读取完整仓库、是否支持技能安装、有没有 HA 连接工具。
先做只读盘点与方案讨论，不修改生产配置，不发送设备动作。
优先讨论最影响行为的少量选项，不一次给我大量问卷。
如果不能读取链接，请明确告诉我需要提供哪些文件。
```

**链接不是万能安装命令。** 使用方式取决于 AI 客户端的能力：

| 客户端能力 | 使用方式 | 能承诺什么 |
|---|---|---|
| 支持技能安装及本地/仓库读取 | 使用客户端支持的安装功能，安装下面的技能目录并验收 | 按该客户端机制发现和调用技能；不是所有客户端自动通用 |
| 能读网页、仓库或附件，但不支持安装 | 读取入口及所需参考，作为当前任务指导 | 本次读取使用；不承诺永久保存或跨会话自动生效 |
| 无法访问仓库链接 | 下载源码，把技能入口和所需参考作为附件提供 | 按可读取的内容工作；附件和上下文限制由客户端决定 |

仅看 README 不等于已读完技能。安装技能也不自动连接 HA、创建凭据或获得生产操作授权。

## 支持技能安装的客户端

技能目录：[`skills/home-automation-design`](skills/home-automation-design/)。安装时保留目录中的 `SKILL.md`、`references/`、`assets/` 和 `agents/`，不要只复制入口文件。

以下按官方文档整理，菜单、目录与可用功能仍以所装版本为准，不表示所有客户端均已实机验证。`~` 表示当前用户主目录；项目级路径相对于实际工作项目，不是本技能仓库。已有同名技能时先比较和备份自定义内容，避免多份旧版本互相覆盖。

| 客户端 | 推荐方式 | 安装后的检查 |
|---|---|---|
| WorkBuddy | 下载技能 ZIP，在技能页面上传导入 | “已安装”中可见且启用，新对话可调用 |
| DeepSeek Harness（DSH） | 完整目录放入项目 `.dsh/skills/` 或用户技能根目录 | 当前会话的技能目录能发现并加载正文及参考 |
| Codex | 使用客户端提供的 `$skill-installer` | 能发现并调用 `$home-automation-design` |
| Claude Code | 完整目录放入 `.claude/skills/` 或 `~/.claude/skills/` | `/home-automation-design` 可用 |
| Cursor | 完整目录放入 `.cursor/skills/` 或 `~/.cursor/skills/` | Agent 的 `/` 技能列表可找到 |
| GitHub Copilot | 完整目录放入项目 `.github/skills/` | 支持 Agent Skills 的对应端可发现并使用 |
| Gemini CLI | `gemini skills install` 指定仓库子目录 | `/skills list` 中可见 |
| OpenClaw | 将完整目录放入配置工作区的 `skills/` | 当前 Agent 的技能列表中可用 |

### 下载技能包或目录

从 [v1.0.2 Release](https://github.com/casatift/ct-smart-home-automation/releases/tag/v1.0.2) 下载附件 `ct-smart-home-automation-v1.0.2.zip`。**该技能 ZIP 的包根目录直接包含 `SKILL.md`、`references/`、`assets/`、`agents/` 和许可证**，适合上传导入。它与 GitHub 自动生成的整个仓库 Source code ZIP 不同。

目录式安装时，将技能 ZIP 解压到名为 `home-automation-design` 的目标目录，最终检查 `<技能根目录>/home-automation-design/SKILL.md` 存在。也可从源码直接复制 `skills/home-automation-design` 整个文件夹。不要把整个仓库套在技能目录内。

### WorkBuddy

1. 下载上面的技能 ZIP。
2. 打开左侧“专家·技能·连接器”→“技能”→“添加技能”→“上传技能”，选择 ZIP 导入；菜单名称以客户端版本为准。
3. 到“已安装”确认技能存在且处于启用状态，新建对话后输入：“使用 CT智能家居自动化（home-automation-design），先只读规划我的 Home Assistant，不修改配置、不操作设备。”

官方文档说明：对话中搜索并安装技能主要面向技能市场，自行提供的技能包应通过“上传技能”导入。本仓库未声称已上架 WorkBuddy 技能市场或 SkillHub；直接粘贴 GitHub 链接不等于完成安装。参见 [WorkBuddy 技能说明](https://www.workbuddy.cn/docs/workbuddy/From-Beginner-to-Expert-Guide/Function-Description/Skills-Market)。

### DeepSeek Harness（DSH）

DSH 指 **DeepSeek Harness**，不是普通 DeepSeek 网页聊天。安装完整技能目录，选择一种范围：

- 当前项目：`.dsh/skills/home-automation-design/SKILL.md`；官方也支持项目 `.agents/skills/`。
- 用户范围：默认 `~/.dsh/skills/home-automation-design/SKILL.md`；若配置了 `DSH_HOME` 或提供方的 `dshHome`，使用实际根目录下的 `skills/`。

在对应项目中打开 DSH，让它列出可用技能并加载 `home-automation-design`，再开始只读规划；若未发现，检查当前工作目录、所用版本和 filesystem skill provider 是否启用，再刷新或新建会话。应能继续读取引用的 `references/`，不能只确认名称出现在列表。

本仓库提供目录式技能，不是 DSH 插件包，不使用插件安装命令安装整个仓库。不要多套一层 `skills/home-automation-design`：DSH 的本地提供方不递归发现任意深度的 `SKILL.md`。参见 [DSH 官方 Skills 文档](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/subsystems/skills.zh.md)和[本地提供方配置](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/skill-filesystem/src/index.ts)。

### Codex

如果客户端提供 `$skill-installer`，可以输入：

```text
请使用 $skill-installer，从
https://github.com/casatift/ct-smart-home-automation/tree/main/skills/home-automation-design
安装技能。若已有 home-automation-design，请先比较版本并保留自定义内容，
不要直接覆盖已有目录。安装后确认技能可被发现。
```

安装后可使用：

```text
使用 $home-automation-design，先只读审阅我的家庭自动化，
优先讨论卧室、照明、书房和清扫策略，不修改生产系统。
```

展示名称是“CT智能家居自动化”，技能标识保留为 `home-automation-design`。Codex 的扫描目录和发现方式可能随版本变化，以[官方技能文档](https://developers.openai.com/codex/skills/)及客户端实际安装工具为准；若安装后未显示，按客户端提示刷新或重启。

### Claude Code

将完整目录放入项目 `.claude/skills/home-automation-design/`，或用户目录 `~/.claude/skills/home-automation-design/`。在 Claude Code 中输入：

```text
/home-automation-design 先只读审阅我的 Home Assistant，讨论方案，不修改生产系统。
```

本地个人技能不会因此自动进入 Claude 网页聊天、Cowork 或云端会话；这些产品使用各自的技能管理与同步机制。参见 [Claude Code Skills 官方文档](https://code.claude.com/docs/en/skills)。

### Cursor

将完整目录放入项目 `.cursor/skills/home-automation-design/`，或用户目录 `~/.cursor/skills/home-automation-design/`。在 Agent 对话中输入 `/`，搜索并选择 `home-automation-design`，再说明只读规划目标；也可要求 Agent 使用该技能。

本地目录可用不代表 Cloud Agents 已同步。Cursor 的“从 GitHub 导入插件”还需要插件/marketplace 清单，本仓库目前没有该包装，应按目录式安装，不把技能仓库当可直接导入的 Cursor 插件。参见 [Cursor Agent Skills 官方文档](https://cursor.com/docs/context/skills)。

### GitHub Copilot

将完整目录放入工作仓库 `.github/skills/home-automation-design/`，在该仓库打开支持 Agent Skills 的 Copilot Agent，让它使用此技能。Copilot CLI 的个人技能还可使用 `~/.copilot/skills/home-automation-design/`；其他端支持范围按其版本确认。

只在聊天里粘贴链接，不会自动将技能装入所有 Copilot 端。云端 Agent 还需在其执行环境能读取该目录，并单独解决 HA 可达性。参见 [GitHub Copilot Agent Skills 官方文档](https://docs.github.com/en/copilot/concepts/customizations/agent-skills)。

### Gemini CLI

支持技能安装的版本可在终端执行：

```sh
gemini skills install https://github.com/casatift/ct-smart-home-automation.git --path skills/home-automation-design --scope user
```

仅当前项目使用时改为 `--scope workspace`。在交互会话中运行 `/skills reload`，再用 `/skills list` 检查并要求使用 `home-automation-design`；依客户端提示确认技能激活。此处是 Gemini CLI，不等于 Gemini 网页聊天。参见 [Gemini CLI Agent Skills 官方文档](https://geminicli.com/docs/cli/skills/)。

### OpenClaw

将完整技能目录放入当前 Agent 已配置工作区的 `skills/home-automation-design/`，确认 `SKILL.md` 位于该目录根部，再在相应 Agent 的新会话中检查并调用。工作区位置以实际配置为准；其他工作区或节点不自动共享。

支持相应命令的版本也可在下载完整仓库后运行：

```sh
openclaw skills install ./ct-smart-home-automation/skills/home-automation-design
```

这是从本地技能目录安装。当前仓库根目录没有 `SKILL.md`，不直接使用要求源根目录有入口的 Git 仓库安装命令；本技能也未声称已上架 ClawHub。参见 [OpenClaw Skills 官方文档](https://docs.openclaw.ai/tools/skills)。

### 其他 AI Agent

先确认是否支持 `SKILL.md` 格式及具体技能目录，再使用其官方安装方法。无法确认时，按“读取文档”使用，不宣称已经永久安装。不要求任何客户端额外安装 Codex 专用工具才能理解通用正文。

普通 DeepSeek、ChatGPT、Gemini 网页聊天以及其他只读文档的客户端，可使用开头的通用提示或上传完整文件；除非其当前版本明确提供自定义技能安装，否则只能说明“本次已读取”，不能宣称永久安装。

## 能帮助你做什么

- **从空白 HA 起步**：辨认安装方式与版本，引导初始化、AI 维护环境、官方 MCP Server、必要认证及少量设备接入验收。
- **审阅已有家庭**：盘点设备能力和全部可见控制入口，识别重复控制、模式冲突、未知状态与遗留规则。
- **复用卧室逻辑**：主卧、次卧、儿童房使用共同契约，按房间配置设备、作息和参数，覆盖午睡、起夜、同床多人、访客与轮班。
- **设计照明与书房模式**：覆盖“先暗后有人”和“先有人后变暗”，处理人工关灯、防振荡，以及学习/工作/日常与短暂离席。
- **选择全屋联动**：客厅、餐厅、厨房默认独立；是否保护邻区灯光、主动开灯或共同退出，分别由用户选择。
- **按占用安排清扫**：定义房间有人时长累计，达到阈值形成需求；检查无人、睡眠、通路与机器人状态后调度，按确认完成的范围结算。
- **处理缺设备和故障**：未配置、正常否定值、未知、故障分别处理，不将 PIR 静默当可靠无人。
- **接入苹果家庭**：比较原生接入、HomeKit Bridge 与 Matter，核验高级能力、分区清扫、容量及配对限制，不强制同一路线。
- **支持后续维护**：统一仲裁、人工保持、过期任务失效、重启对账、部分失败、解释原因与回滚。

## 原生自动化、变量与模式控制

- **默认优先使用 HA 自带自动化和脚本。** 用户指定其他引擎时遵循；若原生能力确有缺口，说明原因并由用户选择替代方案，不默认新增外部运行环境。
- **业务变量使用 Helper 实体。** 模式用 `input_select`，开关偏好/保持用 `input_boolean`，阈值和累计量用 `input_number`/`counter`，截止时间用 `input_datetime` 等。单次动作的临时计算仍可用原生局部变量；持久和共享状态不只藏在脚本内存里。
- **模式都提供可手动选择的入口。** 每个用户可选业务模式轴提供选择器，在 HA 界面可操作，并提供恢复自动/解除人工保持；用户手动指定后遵守约定保持，自动更新不能立即将其覆盖。故障和诊断状态保持真实，不作为可强制选择的业务模式。
- **添加 HomeKit Bridge 时同时导出模式控制。** 默认纳入用户可选模式及恢复自动入口，用户明确排除的除外。当前官方文档将 `input_select`/`select` 映射为每选项一个开关；按目标 HA 版本核对支持情况，必要时提供兼容映射。选择、真实状态与人工保持须双向验证，不承诺在苹果家庭显示原样下拉框。

模式选择须经过同一仲裁；HA 与苹果家庭反馈不能形成循环，待处理或被阻止的请求不能显示成已生效。细节见[协调与执行](skills/home-automation-design/references/coordination.md)和[模式导出说明](skills/home-automation-design/references/apple-and-matter.md)。

## 第一次使用，如何推进

1. **先说目标和范围。** 例如“空白 HA 起步”“只读审计”“设计照明”“讨论清扫”，并说明是否已有可用连接。
2. **先盘点少量关键资料。** HA 版本、安装方式、设备及传感器能力；已有家庭说明当前规则和偏好。无需一开始填完整模板。
3. **逐项确认行为。** 先讨论最影响体验的选项；标记已确认、建议待确认、不启用和未讨论。建议值不自动进入生产。
4. **形成适合你家的方案。** 选择实现工具、设备映射、模式与规则契约、故障降级及验证方式。
5. **按任务授权实施。** 需要上线时再做备份、局部验证、交付和回滚；研究或技能安装本身不授权家庭操作。

空白实例可让 AI 读取[起步与设备接入](skills/home-automation-design/references/onboarding.md)。优先核对目标 HA 版本支持的官方 MCP Server；它与 HA 调用外部服务的 MCP client 是不同方向。工具能力和只读保证需要实际验收，不承诺每个版本均能编辑任意自动化或文件。

支持时可使用 OAuth；确需长效令牌时，由用户通过安全方式创建并保存到客户端凭据存储。不要把密码、令牌、配对码放入聊天、README 或公开仓库；长效令牌也不等于细粒度只读权限。云端 AI 与内网 HA 的网络可达性需另行解决，不默认开放公网端口或 SSH。

## 几个关键默认方向

| 项目 | 建议方向 |
|---|---|
| 实现方式 | 用户未指定时优先 HA 原生自动化与脚本，持久/共享变量用 Helper |
| 模式控制 | HA 选择器可手动指定；添加 HomeKit Bridge 时同步纳入模式入口 |
| 卧室 | 一套共用逻辑，各房参数化；自动睡眠推断待确认后启用 |
| 灯光 | 用户选择灯组、亮度和色温；不同事件顺序进入同一判断 |
| 人工关灯 | 按约定范围和期限抑制自动重亮，不被巡检立即撤销 |
| 书房 | 学习/工作仅场景不同则复用活动逻辑；离席保持可讨论 10 分钟试用值 |
| 跨房间联动 | 逐项选择，保护现有照明与主动开灯分开 |
| 占用累计 | 可选；建议按房间有人时长，多人同时在场只计一份 |
| 清扫 | 阈值由家庭选择；达到阈值只形成需求，启动不算完成 |

这些是讨论起点，不是已经启用的规则。睡眠持续时间、门帘门槛、正常照明与夜灯识别、照度阈值、清扫占用阈值等不照搬其他家庭。

## 文档与规划模板

| 文件 | 用途 |
|---|---|
| [技能入口](skills/home-automation-design/SKILL.md) | Agent 的共同行为与按需阅读入口 |
| [起步与接入](skills/home-automation-design/references/onboarding.md) | 空白 HA、维护连接、MCP、认证及设备接入 |
| [协调与执行](skills/home-automation-design/references/coordination.md) | 模式仲裁、计时、并发、重启及部分失败 |
| [房间与照明](skills/home-automation-design/references/rooms-and-lighting.md) | 卧室、书桌、开关灯条件与可选联动 |
| [占用与清扫](skills/home-automation-design/references/occupancy-and-cleaning.md) | 占用计量、清扫需求、调度与完成结算 |
| [能力与降级](skills/home-automation-design/references/capabilities.md) | 多集成能力来源、缺设备和故障处理 |
| [苹果家庭与 Matter](skills/home-automation-design/references/apple-and-matter.md) | 导出选路、分区清扫及兼容性验收 |
| [验证与交付](skills/home-automation-design/references/validation.md) | 证据边界、场景验证、上线与分发 |
| [空白家庭规划表](skills/home-automation-design/assets/household-profile.yaml) | 可选规划模板，不能直接加载为 HA 配置 |

填写后的规划表应保存在私有位置。发布包只包含通用说明和空白模板，不含任何家庭报告、真实设备映射、源码快照、地图、网络配置或凭据。

## 版本与验证边界

v1.0.1 补充 HA 原生自动化优先、Helper 变量、模式选择器与 HomeKit 模式导出的约定；v1.0.2 补充多个 AI 客户端的安装与调用方法，并提供包根目录直接含 `SKILL.md` 的上传用 ZIP。本技能提供设计与维护指导，不附带通用自动化运行引擎、计数器或机器人调度程序。具体实现由 Agent 根据目标家庭生成并验证，不能仅安装技能就宣称相关自动化已部署。

结构、YAML、相对链接和公开内容检查不等于所有设备、HA 版本和 AI 客户端均已实机通过。实际建设须区分源码、运行状态、历史与物理反馈，验证正常执行及应该被阻止的情况。

反馈问题时请描述设备型号、版本、期望和实际行为，并删除令牌、地址、地图、个人作息及其他家庭隐私信息。

## 许可

[MIT License](LICENSE)。
