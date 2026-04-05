# 别再给 AI Agent 买一台 Mac Mini 了

使用 AI 编程一年多了，使用 Claude Code 也超过 9 个月了，最近 2 个月已经开始深度使用 Claude Code + Codex 混合模式。
只能说作为一名工作将近 10 年的码农和开源爱好者，经过一年多的 AI 熏陶已经不会写代码了。当然在使用的过程中踩了无数个坑，爆了无数肝，积累了无数经验。今天分享一些摸爬滚打过来的实战经验。

现如今，各种类型的 AI Agent 运行在个人电脑上几乎都会面临一个进退两难的困境。我相信日常使用 Claude Code 或 Codex 写代码时，一定遇到过这样的体验：一个任务跑到一半，弹了十几次确认框，点 `Yes` 点到手软。点击 `Yes always` 又怕 Agent 一条命令把电脑搞崩。

<!-- 插入图片: Claude_keyboard.png | Claude Code 权限确认框 -->

## 为什么会这样？

Claude Code 和 Codex 提供了类似沙箱的能力允许用户指定权限级别，并给了两个选择：

<!-- 插入图片: contrast-v3.png | 两难困境：锁死权限还是全部放开 -->

## 什么是最适合 AI Agent 的运行环境？

这也是为什么 Mac Mini 突然爆火了。广大养虾群众为了防止龙虾把主人的电脑搞坏，单独给小龙虾配了一台独立的电脑，即使龙虾误删了文件，删的也是龙虾的文件，不会波及主力电脑。思路没问题，但代价是真金白银买一台设备。

更轻量的思路是沙箱：通过软件层面的隔离，让 Agent 在一个受限环境里运行。但 Claude Code 和 Codex 提供的沙箱用起来都有很大的痛点 — 要么权限锁得太死影响效率，要么隔离做得不够彻底。

那如果要给 AI Agent 设计一个沙箱，它应该具备哪些能力才能满足养虾户或 AI coding 的需求呢？

<!-- 插入图片: feature-cards.png | 六大核心能力 -->

基于这些核心需求，于是 [Agents Sandbox](https://github.com/1996fanrui/agents-sandbox) 就诞生了。Agents Sandbox 的目标就是：给 Agent 全部权限，让你的机器绝对安全。

## [Agents Sandbox](https://github.com/1996fanrui/agents-sandbox) 用零成本打破困境

<!-- 插入图片: agents_sandbox_keyboard.png | Agents Sandbox 键盘：放心按 Yes always -->

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

原来一条命令，现在还是一条命令，达到一模一样的效果 — 但 Agent 跑在隔离沙箱里，宿主机完全不受影响。

<!-- 插入图片: terminal-comparison-zh.gif | 沙箱创建 → Agent 执行 → 结果输出 → 沙箱销毁 -->

跟自己手动搭 Docker 容器不同，Agents Sandbox 专门为 AI Agent 场景做了适配：自动挂载项目代码、内置 Agent 运行时环境、用完即毁不留痕迹。

## 安全模型

一句话概括：**沙箱与宿主默认完全隔离，没有例外。**

<!-- 插入图片: security-model.png | 安全模型：宿主与沙箱隔离示意 -->

机器上跑的沙箱，跟一台远程云服务器的隔离程度一样 — 只是零延迟、零成本、数据不出本机。

## 零成本复用已有订阅

用云沙箱跑 Agent，用户需要付两份钱：沙箱按 vCPU 按小时计费，AI 模型按 token 另外收费。跑得越多，账单越长。

Agents Sandbox 完全不同：沙箱运行在你本地的 Docker 上，零基础设施费用。更关键的是，你已有的 Claude Max、Codex Pro 等 CLI 包月订阅可以直接携带进沙箱 — 不需要额外的 API Key，不产生按量计费。用过 API 的都知道，按 token 计费的开销可能是包月订阅的几十倍，而 Agents Sandbox 让你把订阅的价值榨干到最后一滴。

对于重度 Agent 用户来说，这意味着每月能省下一笔可观的开销。

## 实际使用场景

**场景一：让 Agent 全速完成一个编码任务**

给 Claude Code 一个任务：实现一个新功能或修复一个 bug。Agent 需要读代码、改代码、装依赖、跑测试、根据测试结果反复迭代。在宿主机上，每次文件写入、每次命令执行都要用户点"允许"，一个任务弹几十次确认框。在沙箱里，Agent 从头到尾全自主完成，用户只需要在最后 review 结果。跑完删掉沙箱，机器上不留任何痕迹。

<!-- 插入图片: scenario-1.png | 场景一：Agent 全速编码，零确认框中断 -->

**场景二：并行跑多个 Agent 做不同任务**

同时开三个沙箱：一个让 Codex 重构前端组件，一个让 Claude Code 写 API 测试，一个跑数据分析脚本处理一批不可信的用户数据。三个沙箱完全隔离，互不影响，每个 Agent 都有全部权限，不会互相踩到对方的环境。

<!-- 插入图片: scenario-2.png | 场景二：多实例并行隔离 -->

**场景三：CI 流水线中给每次构建一个干净环境**

每次 CI 触发时创建一个沙箱，构建、测试在里面跑完，结果取出来，沙箱销毁。每次运行都是干净的起点，不会被上一次构建的残留影响。

<!-- 插入图片: scenario-3.png | 场景三：用完即毁的生命周期 -->

## 不只是一个容器

Agents Sandbox 不是简单地帮你启动一个 Docker 容器。它是一个专门为 Agent 场景设计的沙箱控制面，开箱即用的同时覆盖了从日常开发到平台集成的完整需求：

<!-- 插入图片: not-just-container.png | 七大功能特性：运行时、挂载拷贝、凭据投射、Companion Container、YAML 配置、SDK、自动清理 -->

## 和其他方案的对比

<!-- 插入图片: compare-table.png | 多方案横向对比 -->

## 项目地址

GitHub：https://github.com/1996fanrui/agents-sandbox
