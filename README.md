# Comet 使用说明

> 基于上游文档 `README-zh.md` 整理，面向“想尽快上手 Comet 工作流”的使用者。

## 1. Comet 是什么

Comet 是一个把 **OpenSpec** 和 **Superpowers** 组合起来的 AI 开发工作流工具。

- OpenSpec 负责需求提案、Spec 生命周期、归档等偏 **WHAT**
- Superpowers 负责头脑风暴、设计、计划、构建、评审等偏 **HOW**
- Comet 用统一入口 `/comet` 把两者串起来，形成一个可恢复、可追踪、可归档的多阶段流程

它的核心价值在于：

- 自动识别当前 Spec 所处阶段，支持中断后继续执行
- 用状态文件和守护脚本约束流程，而不是只靠 AI “记住步骤”
- 自动维护交接、状态更新、验证和归档，减少人工提醒

## 2. 适用场景

Comet 比较适合这些情况：

- 需求从分析、设计、实现到验证有完整流程
- 任务周期较长，AI 会话可能中断，需要下次继续
- 希望把提案、设计、计划、验证、归档统一管理
- 希望 AI 按阶段推进，而不是直接跳到编码

如果只是非常小的文案修改或简单配置调整，也可以走 Comet 的快捷路径。

## 3. 安装前准备

需要先具备以下环境：

- Node.js 20+
- `npm` / `npx`
- `git`
- 可运行 `bash` 的 shell 环境

Windows 用户建议使用 **Git Bash** 或等价环境。

## 4. 安装 Comet

全局安装：

```bash
npm install -g @rpamis/comet
```

更新版本：

```bash
comet update
```

或者：

```bash
npm install -g @rpamis/comet@latest
```

如果你所在的 AI 平台可以直接使用通用 `skills` CLI，也可以这样安装 skill 包：

```bash
npx skills add rpamis/comet
```

## 5. 快速开始

进入你的项目目录后执行：

```bash
cd your-project
comet init
```

`comet init` 会完成这些事情：

1. 检测并让你选择 AI 平台
2. 选择安装范围：项目级或全局
3. 选择 Comet 技能语言：中文或英文
4. 安装 OpenSpec 技能
5. 安装 Superpowers 技能
6. 安装 Comet 技能
7. 在项目级安装时创建工作目录

项目级安装通常会生成这些目录：

```text
docs/superpowers/specs/
docs/superpowers/plans/
```

## 6. 核心入口与常用命令

### 6.1 Skill 入口

安装完成后，最常用的是这些 skill：

- `/comet`：主入口，自动识别当前阶段并分派
- `/comet-open`：阶段 1，打开变更
- `/comet-design`：阶段 2，深度设计
- `/comet-build`：阶段 3，规划与构建
- `/comet-verify`：阶段 4，验证与完成
- `/comet-archive`：阶段 5，归档
- `/comet-hotfix`：Bug 修复快捷路径
- `/comet-tweak`：小改动快捷路径

### 6.2 CLI 命令

常用命令如下：

```bash
comet init [path]
comet status [path]
comet doctor [path]
comet update [path]
comet --help
comet --version
```

常见用途：

- `comet init`：初始化工作流和技能
- `comet status`：查看当前活跃变更、任务进度和推荐下一步命令
- `comet doctor`：检查安装是否健康
- `comet update`：更新 Comet 包和已安装技能

## 7. Comet 的五阶段工作流

标准流程如下：

```text
/comet
  -> /comet-open
  -> /comet-design
  -> /comet-build
  -> /comet-verify
  -> /comet-archive
```

### 阶段 1：Open

命令：`/comet-open`

目标：

- 打开一个新变更
- 形成提案、设计草稿、任务拆分

典型产物：

- `proposal.md`
- `design.md`
- `tasks.md`

### 阶段 2：Deep Design

命令：`/comet-design`

目标：

- 进行深入设计和头脑风暴
- 明确实现边界、方案、约束和风险

典型产物：

- Design Doc
- delta spec

### 阶段 3：Plan & Build

命令：`/comet-build`

目标：

- 生成实现计划
- 按计划编码和提交

典型产物：

- 实现计划
- 代码改动
- 提交记录

### 阶段 4：Verify & Finish

命令：`/comet-verify`

目标：

- 完成测试和验证
- 生成验证报告
- 处理分支状态

典型产物：

- 验证报告
- 已处理的分支状态

### 阶段 5：Archive

命令：`/comet-archive`

目标：

- 将 delta spec 同步回主 spec
- 归档变更文档和状态

典型产物：

- 已同步的 spec
- 已归档的 change

## 8. 两条快捷路径

### `/comet-hotfix`

适用于：

- 明确的小型 Bug 修复
- 不需要完整头脑风暴的修复任务

流程：

```text
open -> build -> verify -> archive
```

### `/comet-tweak`

适用于：

- 文案调整
- 配置微调
- 文档优化
- Prompt 优化

流程更轻量，会跳过完整设计和重型规划。

## 9. 状态管理机制

Comet 的一个关键设计点，是把状态从 AI 提示词里“拿出来”，放进结构化文件和脚本里管理。

主要文件：

- `.openspec.yaml`：管理 OpenSpec 生命周期和变更元数据
- `.comet.yaml`：管理 Comet 当前阶段、执行模式、验证结果和归档状态

`.comet.yaml` 常见字段包括：

- `workflow`
- `phase`
- `build_mode`
- `build_pause`
- `isolation`
- `verify_mode`
- `design_doc`
- `plan`
- `verify_result`
- `verification_report`
- `branch_status`
- `archived`

这意味着：

- AI 中断后，可以通过状态文件恢复上下文
- 阶段切换不是“口头说完成”，而是由守护脚本校验后推进
- 验证和归档必须有证据，避免流程空转

## 10. 守护脚本有什么作用

Comet 内置了一组脚本来保障流程可靠：

- `comet-guard.sh`：检查阶段是否允许切换
- `comet-handoff.sh`：生成设计交接上下文
- `comet-archive.sh`：执行归档
- `comet-yaml-validate.sh`：校验 `.comet.yaml`
- `comet-state.sh`：统一读写状态
- `comet-env.sh`：导出内部脚本路径

这些脚本的价值是：

- 防止 AI 跳步骤
- 防止 YAML 状态写错
- 防止没有验证报告就进入归档
- 支持中断恢复和自动推进

## 11. 推荐使用方式

如果你是第一次使用，建议按下面方式实践：

1. 在一个真实但不大的项目里执行 `comet init`
2. 用一个具体需求完整走一遍五阶段
3. 每完成一项任务就同步更新 `tasks.md`
4. 不要跳过设计阶段，优先让 AI 先设计再写代码
5. 在验证通过之前，不要执行归档
6. 如果中途退出，下次优先从 `/comet` 重新进入

## 12. 一个具体示例

下面用“给项目增加暗色模式切换功能”举一个完整的使用示例。

### 12.1 示例背景

需求：

- 在现有 Web 项目中增加亮色/暗色模式切换
- 主题选择需要持久化到本地
- 页面刷新后仍保留用户选择

### 12.2 初始化

先在项目根目录执行：

```bash
comet init
```

初始化完成后，在你使用的 AI 编码平台里调用：

```text
/comet
```

### 12.3 我与 AI 的对话示例

下面的示例演示的是“我”和 AI 如何围绕 Comet 工作流协作。

```text
我：/comet

AI：已检测到当前项目尚未开始活跃变更，建议进入 /comet-open。请描述本次需求目标。

我：给当前 Web 项目增加亮色/暗色模式切换。要求用户切换后写入 localStorage，刷新页面后保留上次选择；同时需要保证默认主题跟随系统主题。

AI：收到，我将进入 /comet-open，先生成提案、初步设计和任务拆分。

AI：已完成 open 阶段草稿，当前产出包括 proposal.md、design.md、tasks.md。下一步建议进入 /comet-design 做深入设计。是否继续？

我：继续。

AI：/comet-design 已开始。我会先做主题状态模型、CSS 变量方案、初始化时机和回退策略设计。

AI：设计结论如下：
1. 使用 `data-theme` 驱动全局主题切换。
2. 主题值优先级为：用户选择 > 系统主题 > 默认亮色。
3. 在应用启动早期读取 localStorage，避免主题闪烁。
4. 通过 CSS 变量定义亮暗两套 token。

AI：设计文档和 delta spec 已更新，建议进入 /comet-build。是否继续？

我：继续，并采用分支隔离。

AI：收到，进入 /comet-build。我将先生成实现计划，再按任务分步提交。

AI：计划如下：
1. 增加 theme manager，负责读取、写入和解析主题状态。
2. 在应用入口注入初始化逻辑，首屏前设置主题。
3. 新增 ThemeToggle 组件。
4. 替换现有样式中的硬编码颜色为 CSS 变量。
5. 增加测试并更新文档。

AI：实现已完成，任务已同步到 tasks.md。下一步建议进入 /comet-verify。

我：开始验证。

AI：/comet-verify 已开始。我将执行主题切换测试、刷新保留测试，以及系统主题跟随验证，并生成验证报告。

AI：验证完成，结果如下：
1. 用户切换主题后，localStorage 中已写入主题值。
2. 刷新页面后，主题状态能够正确恢复。
3. 未设置用户偏好时，可根据系统主题自动初始化。
4. 验证报告已生成，分支状态已处理。

AI：当前满足归档条件，建议执行 /comet-archive。是否继续？

我：继续归档。

AI：/comet-archive 已完成。delta spec 已同步，相关设计和计划文档已标记归档，当前变更流程结束。
```

### 12.4 这个示例体现了什么

这个例子体现了 Comet 的几个关键使用方式：

- 从 `/comet` 统一进入，让系统自动判断当前应该执行哪个阶段
- 每个阶段都有明确产物，而不是直接开始写代码
- 设计、实现、验证、归档之间有清晰边界
- 中间如果 AI 会话中断，下次仍可继续读取活跃状态推进

## 13. 常见建议

### 建议 1：把 `/comet` 当作统一入口

除非你明确知道当前要进入哪个阶段，否则优先使用 `/comet`，让 Comet 自动检测当前状态。

### 建议 2：不要绕开 `tasks.md`

`tasks.md` 不只是待办列表，它还是流程推进的重要依据。完成一项就更新一项，能明显降低 AI 丢上下文的概率。

### 建议 3：不要把“验证通过”当成口头结论

应确保验证报告真实存在，并且分支状态已处理，再进入归档。

### 建议 4：小问题用快捷路径

如果只是小 Bug 或小改动，可以优先考虑：

- `/comet-hotfix`
- `/comet-tweak`

这样比完整五阶段更高效。

## 14. 一句话总结

如果把普通 AI 编码理解为“想到就写”，那么 Comet 更像是给 AI 开发接入了一套 **可恢复的项目流程系统**：先开变更，再做设计，再计划和编码，再验证，最后归档，并且所有步骤都尽量有状态、有证据、可回溯。
