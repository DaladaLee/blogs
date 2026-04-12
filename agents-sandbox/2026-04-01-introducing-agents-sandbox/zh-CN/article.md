# 零成本提供最适合 AI Agent 的运行环境，别再给 AI Agent 买一台 Mac Mini 了

> 使用 AI 编程一年多了，使用 Claude Code 也超过 9 个月了，最近 2 个月已经开始深度使用 Claude Code + Codex 混合模式。只能说作为一名工作将近 10 年的码农和开源爱好者，经过一年多的 AI 熏陶已经不会写代码了，但可能更会驾驭 AI 来交付结果了。

例如：2026年3月，我的 Ubuntu 主力开发机上 Claude Code 和 Codex 共计使用了 176 亿 token。加上 Macbook 的使用量，日均 token 使用量远超 6 亿。

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/token_usage.jpg" alt="Token Usage">

如果你问，3 月份工作日到底是 21 天还是 31 天？那肯定是 31 天，这么有趣的事情，怎么可能有休息日。当然在使用的过程中踩了无数个坑，爆了无数肝，积累了无数经验。今天分享一些摸爬滚打过来的实战经验。

现如今，各种类型的 AI Agent 运行在个人电脑上几乎都会面临一个进退两难的困境。我相信日常使用 Claude Code 或 Codex 写代码时，一定遇到过这样的体验：一个任务跑到一半，弹了十几次确认框，点 `Yes` 点到手软。点击 `Yes always` 又怕 Agent 一条命令把电脑搞崩。

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/Claude_keyboard.png" alt="Claude Code 权限确认框">

## 为什么会这样？

Claude Code 和 Codex 提供了类似沙箱的能力允许用户指定权限级别，并给了两个选择：

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/contrast-v3.png" alt="两难困境：锁死权限还是全部放开">

这也是为什么 Mac Mini 突然爆火了。广大养虾群众为了防止龙虾把主人的电脑搞坏，单独给小龙虾配了一台独立的电脑，即使龙虾误删了文件，删的也是龙虾的文件，不会波及主力电脑。思路没问题，但代价是真金白银买一台设备。

## 什么是最适合 AI Agent 的运行环境？

如果使用 Claude 开启所有权限(claude --dangerously-skip-permissions)，Claude Code 就会进行免责声明（甩锅）。
哦不是，Claude Code 就会建议当前模式仅在一个独立的沙箱容器或虚拟机内使用，因为 Claude Code 执行一些危险命令将不会跟用户确认。默认 Claude Code 给用户选择不接受并退出。用户可以在自己的电脑上开所有权限，但是电脑坏了，跟 Claude Code 没关系，用户要承担所有后果。

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/claude_code_reminder.jpg" alt="Claude Code reminder">

那如果要给 AI Agent 设计一个沙箱，它应该具备哪些能力才能满足养虾户或 AI coding 的需求呢？

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/feature-cards.png" alt="六大核心能力">

基于这些核心需求，于是 **Agents Sandbox** `[1]` `[2]` 就诞生了。Agents Sandbox 的目标就是：给 Agent 全部权限，让用户的机器绝对安全。用户不再是 Agent 的保姆，从此与 Agent 交互的键盘从 4 键变成了 2 键（Yes Always 和麦克风）。

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/2_keyboards.png" alt="Agents Sandbox 键盘：放心按 Yes always">

## **Agents Sandbox** `[1]` `[2]` 用零成本打破困境

原来怎么跑 Agent，现在还是怎么跑 — 只是从宿主机换到了隔离沙箱里：

```bash
# 安装 agents-sandbox（需要 Docker 和 curl）
curl -fsSL https://agents-sandbox.com/install.sh | bash

# 在隔离沙箱里跑 Claude Code，沙箱内全权限
# 等价于 claude --dangerously-skip-permissions
agbox agent claude

# 在隔离沙箱里跑 Codex，沙箱内全权限
# 等价于 codex --dangerously-bypass-approvals-and-sandbox
agbox agent codex
```

原来一条命令，现在还是一条命令，达到一模一样的效果 — 但 Agent 跑在隔离沙箱里，宿主机完全不受影响。更多使用细节请参考 **Quick Start** `[3]`。

<img style="max-width: 400px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/terminal-comparison-zh.gif" alt="沙箱创建 → Agent 执行 → 结果输出 → 沙箱销毁">

跟自己手动搭 Docker 容器不同，Agents Sandbox 专门为 AI Agent 场景做了适配：自动挂载项目代码、内置 Agent 运行时环境、用完即毁不留痕迹。关于 Claude Code 和 Codex 内置沙箱的痛点与 Agents Sandbox 的对比分析，详见 **Why Not Built-in Sandboxes** `[4]`。

## 安全模型

**沙箱与宿主完全隔离，没有例外。**

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/security-model.png" alt="安全模型：宿主与沙箱隔离示意">

沙箱可以访问公共网络，但无法访问宿主网络。这不是可选配置，而是默认行为。并且宿主网络隔离将永远不会开放（will never be supported）— 这是项目的设计底线。

如果 Agent 需要数据库、缓存等外部依赖，请使用 Companion Containers（伙伴容器）。它们是 Agents Sandbox 内置的容器类型，和沙箱运行在同一隔离网络内，互相可达但依然与宿主隔离。完整的隔离与安全机制详见 **Isolation and Security** `[5]`。

机器上跑的沙箱，跟一台远程云服务器的隔离程度一样 — 只是零延迟、零成本、数据不出本机。

## 零成本复用已有订阅

用云沙箱跑 Agent，用户需要付两份钱：沙箱按 vCPU 按小时计费，AI 模型按 token 另外收费。跑得越多，账单越长。

Agents Sandbox 完全不同：沙箱运行在你本地的 Docker 上，零基础设施费用。更关键的是，你已有的 Claude Max、Codex Pro 等 CLI 包月订阅可以直接携带进沙箱 — 不需要额外的 API Key，不产生按量计费。用过 API 的都知道，按 token 计费的开销可能是包月订阅的几十倍，而 Agents Sandbox 让你把订阅的价值榨干到最后一滴。

对于重度 Agent 用户来说，这意味着每月能省下一笔可观的开销。

## 实际使用场景

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/scenario-1.png" alt="场景一：Agent 全速编码，零确认框中断">

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/scenario-2.png" alt="场景二：多实例并行隔离">

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/scenario-3.png" alt="场景三：用完即毁的生命周期">

## 不只是一个容器

Agents Sandbox 不是简单地帮你启动一个 Docker 容器。它是一个专门为 Agent 场景设计的沙箱控制面，开箱即用的同时覆盖了从日常开发到平台集成的完整需求：

<img style="max-width: 400px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/1996fanrui/agents-sandbox/issue-122-promotional-blog/docs/blogs/2026-04-01-introducing-agents-sandbox/zh-CN/images/not-just-container.png" alt="七大功能特性：运行时、挂载拷贝、凭据投射、Companion Container、YAML 配置、SDK、自动清理">

更多细节请参考 **官方文档** `[6]`。

> [1]https://github.com/1996fanrui/agents-sandbox
> [2]https://agents-sandbox.com
> [3]https://docs.agents-sandbox.com/quick_start
> [4]https://docs.agents-sandbox.com/why_not_builtin_sandboxes
> [5]https://docs.agents-sandbox.com/isolation_and_security
> [6]https://docs.agents-sandbox.com
