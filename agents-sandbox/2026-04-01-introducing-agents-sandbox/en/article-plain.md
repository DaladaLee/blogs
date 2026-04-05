# Stop Buying a Mac Mini for Your AI Agent — There's a Better Way

> After a year of AI-assisted coding — 9+ months on Claude Code, the last 2 months running a Claude Code + Codex hybrid workflow — I've forgotten how to write code by hand. But I've gotten much better at driving AI to ship results. Nearly 10 years in the industry as a developer and open-source contributor, and this past year has completely changed how I work.

How much usage are we talking about? In March 2026, Claude Code and Codex burned through 17.6 billion tokens on my Ubuntu workstation alone. Add my MacBook on top, and my daily average was well north of 600 million tokens.

Did March have 21 working days or 31? It had 31 — when you're this deep in, weekends stop existing. Along the way I hit every pitfall imaginable, pulled more all-nighters than I'd care to admit, and learned some hard lessons. Here's the most important one.

Every AI agent running on a personal machine today faces the same catch-22. If you use Claude Code or Codex, you know the drill: the agent is midway through a task, and a dozen permission dialogs pop up. You click `Yes` until your fingers go numb. Switch to `Yes always` and you spend the rest of the day wondering if the agent just `rm -rf`'d something important.

## The Permission Dilemma

Claude Code and Codex both expose permission levels that boil down to two choices:

This is exactly why Mac Minis blew up in the AI agent community. People started buying a dedicated machine just for the agent — that way, even if it trashes something, it's the agent's files, not yours. The logic is solid. The cost is a real computer.

## What Should an Agent Runtime Actually Look Like?

Run Claude Code with all permissions unlocked (`claude --dangerously-skip-permissions`) and it shows you a disclaimer: this mode should only be used inside an isolated sandbox or VM, because dangerous commands will execute without asking. Claude Code even gives you the option to bail out. You can ignore the warning and run it on bare metal, but if something breaks, that's on you.

So what would a proper sandbox for AI agents need to provide?

That's the problem **Agents Sandbox** ([GitHub][1] | [Website][2]) solves. The goal is simple: give the agent full permissions while keeping your machine completely safe. You stop babysitting the agent. Your keyboard shrinks from 4 keys to 2 — Yes Always and the microphone.

## **Agents Sandbox** — Zero-Cost, Zero-Risk

Same workflow you already know. Same single command. The only difference: the agent runs inside an isolated sandbox instead of on your host.

```bash
# Install agents-sandbox (requires Docker and curl)
curl -fsSL https://agents-sandbox.com/install.sh | bash

# Run Claude Code in an isolated sandbox with full permissions
# Equivalent to: claude --dangerously-skip-permissions
agbox agent claude

# Run Codex in an isolated sandbox with full permissions
# Equivalent to: codex --dangerously-bypass-approvals-and-sandbox
agbox agent codex
```

One command before, one command now. Same result — but the agent is sandboxed, and your host is untouched. See the **Quick Start** ([link][3]) for the full walkthrough.

This isn't "just spin up a Docker container." Agents Sandbox is purpose-built for AI agent workflows: it auto-mounts your project code, ships a pre-configured runtime, and self-destructs when done. For a detailed breakdown of how Claude Code and Codex built-in sandboxes compare, see **Why Not Built-in Sandboxes** ([link][4]).

## Security Model

**The sandbox is fully isolated from the host. No exceptions.**

The sandbox can reach the public internet but cannot touch the host network. This is not a toggle — it's the default, and host network access will never be supported. That's a design principle, not a limitation.

Need a database or cache for your agent? Use Companion Containers — built-in container types that share the sandbox's isolated network. They can talk to each other but remain walled off from the host. Full details in **Isolation and Security** ([link][5]).

A sandbox on your own machine gives you the same isolation as a remote cloud server — with zero latency, zero cost, and data that never leaves your machine.

## Your Existing Subscriptions Work Out of the Box

Cloud sandboxes charge you twice: compute billed by vCPU-hour, plus AI model usage billed per token. The more you run, the bigger the bill.

Agents Sandbox flips that model. Sandboxes run on your local Docker — zero infrastructure cost. Your Claude Max or Codex Pro subscription carries straight into the sandbox. No extra API key. No per-token billing. If you've ever compared API pricing to a monthly subscription, you know the difference can be 10-50x. Agents Sandbox lets you wring every last token out of what you're already paying for.

For heavy users, this adds up to real savings every month.

## Real-World Scenarios

**Scenario 1: Agent Codes at Full Speed, Zero Confirmation Interrupts**
The agent modifies code, installs dependencies, runs tests, and iterates — every step requires executing commands. In a sandbox, all of this runs without a single approval dialog.

**Scenario 2: Multiple Isolated Instances in Parallel**
Three sandbox instances running simultaneously, each working on a different task — fully isolated, no environment conflicts.

**Scenario 3: Ephemeral Lifecycle — Destroyed When Done**
Each cycle is a clean slate: create sandbox, build and test, extract results, destroy sandbox. Zero residue. Every run starts fresh.

## More Than Just a Container

Agents Sandbox is not a thin wrapper around `docker run`. It's a sandbox control plane built specifically for agent workflows, covering everything from daily development to platform integration:

- **Ready-to-use Runtime** — Claude, Codex, Git, uv, npm pre-configured. Enter the sandbox and start working.
- **Automatic Project Mounting/Copying** — Current directory auto-mapped; copy mode excludes unnecessary dirs and rejects unsafe symlinks.
- **Secure Credential Injection** — SSH agent forwarding and GitHub CLI auth auto-injected. Private keys never enter the container. `git push` works out of the box.
- **Companion Containers** — Declarative config for PostgreSQL, Redis, etc. Same isolated network, direct connectivity.
- **Declarative YAML Config** — One `sandbox.yaml` per project, version-controlled, with personal override support.
- **Python / Go SDK** — Full programmatic interface for platform integration, batch orchestration, and event subscription.
- **Auto Cleanup** — Destroyed on exit, no trace left. Idle sandboxes reclaimed by idle TTL.

Explore the full feature set in the **official documentation** ([link][6]).

[1]: https://github.com/1996fanrui/agents-sandbox
[2]: https://agents-sandbox.com
[3]: https://docs.agents-sandbox.com/quick_start
[4]: https://docs.agents-sandbox.com/why_not_builtin_sandboxes
[5]: https://docs.agents-sandbox.com/isolation_and_security
[6]: https://docs.agents-sandbox.com
