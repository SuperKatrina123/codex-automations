# Codex Automations

这个仓库用来保存和复用 Codex 自动化工作流。

它不只是 prompt 仓库，也包含可安装的 Codex Skill。目标是把常用自动流沉淀成可版本管理、可审阅、可迁移的模板：既能直接复制 prompt，也能通过 skill 引导 Codex 自动完成安装、依赖检查和自动流创建。

## 仓库结构

| 路径 | 作用 |
|---|---|
| `prompts/` | 可直接复制到 Codex automation 的 prompt 模板 |
| `skills/` | 可安装的 Codex Skill，用于引导 Codex 创建或更新自动流 |
| `examples/` | 已发送或可参考的自动化输出示例 |

## 当前工作流

| 工作流 | 文件 | 用途 |
|---|---|---|
| AI Daily Radar | `prompts/ai-daily-radar.md` | 工作日 AI 应用层雷达邮件：默认抓取过去 24 小时 AI HOT 精选源，也可替换成其他信息抓取 skill；发送后把 Markdown 报告归档到 `examples/`。 |
| AI Daily Radar Installer | `skills/ai-daily-radar-automation/SKILL.md` | Codex Skill：检查信息源 skill、Gmail connector、收件人和标签变量，并创建或更新 Codex automation。 |

## 推荐用法

### 1. 安装 Skill

如果当前 Codex 环境还没有这个 skill，可以对 Codex 说：

```text
请从 https://github.com/SuperKatrina123/codex-automations 安装 skills/ai-daily-radar-automation 这个 skill。
```

安装后，对 Codex 说：

```text
用 AI Daily Radar automation skill 帮我创建自动流。
```

Codex 会按 skill 流程完成：

1. 检查信息抓取 skill，默认是 `aihot` / AI HOT。
2. 检查 Gmail connector 是否已连接。
3. 确认收件人和 Gmail 标签变量。
4. 读取 prompt 模板。
5. 创建或更新工作日 10:30 的 Codex automation。

### 2. 直接复制 Prompt

如果不想安装 skill，也可以直接复制：

```text
prompts/ai-daily-radar.md
```

复制后请先替换这些变量：

| 变量 | 含义 |
|---|---|
| `SOURCE_SKILL` | 信息抓取 skill，默认是 `aihot` |
| `SOURCE_NAME` | 数据源显示名，默认是 `AI HOT` |
| `REPORT_RECIPIENT_EMAIL` | 日报收件邮箱 |
| `GMAIL_LABEL_NAME` | Gmail 标签名，默认可用 `AI Daily Radar` |

## 前置依赖

| 工作流 | 必需依赖 |
|---|---|
| AI Daily Radar | 一个可用的信息抓取 skill，默认是 `aihot` / AI HOT；Gmail connector；可推送到归档仓库的 Git 权限 |

如果缺少信息抓取 skill，自动化应该停止执行并提示安装、启用或替换对应 skill，不应该凭过时知识编造内容。

Gmail 推送依赖 Codex 环境里的 Gmail connector/app 授权，不需要 Chrome 插件。

GitHub 归档依赖本地 Git 凭证能推送到 `SuperKatrina123/codex-automations`。如果归档失败，自动化仍应完成 Gmail 推送，并在当前线程报告 Git 错误。

## 隐私规则

- 不在仓库中保存私人邮箱、token、API key、OAuth 凭证或环境变量。
- 公开模板里使用变量占位，例如 `REPORT_RECIPIENT_EMAIL`。
- 只有在创建具体 Codex automation 时，才把变量替换成个人配置。
- 如果要公开分享新 workflow，先检查是否包含邮箱、内部 URL、私有 repo、密钥或个人身份信息。

## 维护约定

- `prompts/` 下的文件使用小写短横线命名，例如 `ai-daily-radar.md`。
- `skills/` 下每个目录对应一个可安装 Codex Skill。
- 一个 prompt 或 skill 只描述一个自动化工作流。
- 调整自动流后，同步更新 prompt 和 skill reference，避免 Codex app 里的版本与仓库版本漂移。
