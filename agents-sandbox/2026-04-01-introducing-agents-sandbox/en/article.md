# Stop Buying a Mac Mini for Your AI Agent — There's a Better Way

> After a year of AI-assisted coding — 9+ months on Claude Code, the last 2 months running a Claude Code + Codex hybrid workflow — I barely write code by hand anymore. But I've gotten much better at driving AI to ship results.
>
> I've been a developer and open-source contributor for nearly 10 years, and this past year completely changed how I work.
>
> Here's what I learned along the way — and where the whole experience led me.

In March 2026, I burned through 17.6 billion tokens across Claude Code and Codex on my Ubuntu workstation alone. Add my MacBook on top, and my daily average was easily over 600 million tokens.

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/zh-CN/images/token_usage.jpg" alt="Token Usage">

I didn't take a single day off in March. Along the way I ran into every pitfall you can think of, pulled more all-nighters than I'd care to admit, and learned some hard lessons. Here's the problem that shaped everything:

Every AI agent running on a personal machine today faces the same lose-lose trade-off. If you use Claude Code or Codex, you know the drill: the agent is midway through a task, and a dozen permission dialogs pop up. You click `Yes` until your fingers go numb. Or you switch to `Yes always` and spend the rest of the day wondering if the agent just `rm -rf`'d something important.

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/zh-CN/images/Claude_keyboard.png" alt="Claude Code Permission Dialog">

## The Permission Dilemma

Claude Code and Codex both offer sandbox-like capabilities that let you configure permission levels, but in practice they boil down to two choices:

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/en/images/contrast-v3.png" alt="The Dilemma: Lock Down Permissions or Open Everything">

This is exactly why Mac Minis took off in the AI agent community. People started buying a dedicated machine just for the agent — that way, even if it trashes something, it's the agent's files, not yours. Makes sense — except now you own a whole extra computer.

## What Should an Agent Runtime Actually Look Like?

Give Claude Code full permissions (`claude --dangerously-skip-permissions`) and you're greeted with Anthropic's own disclaimer: only run this in a sandbox or VM, because dangerous commands will execute without asking. The default option is to decline and exit. You can ignore the warning and run it on bare metal, but if something breaks, that's on you.

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/zh-CN/images/claude_code_reminder.jpg" alt="Claude Code reminder">

So what would a proper sandbox for AI agents need to provide?

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/en/images/feature-cards.png" alt="Six Core Capabilities">

That's the problem **Agents Sandbox** `[1]` `[2]` solves. Here's the deal: give the agent full permissions while keeping your machine completely safe. You stop babysitting the agent. Your entire workflow becomes two things: hit Yes Always and describe what you want.

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/zh-CN/images/2_keyboards.png" alt="Agents Sandbox Keyboard: Just Hit Yes Always">

## Same Workflow, Zero Extra Cost

**Agents Sandbox** `[1]` `[2]` doesn't change your workflow. It was one command before; it's still one command now. The only difference: the agent runs inside an isolated sandbox instead of on your host.

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

Same single command — but now the agent is sandboxed, and your host is untouched. See the **Quick Start** `[3]` for the full walkthrough.

<img style="max-width: 400px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/en/images/terminal-comparison-en.gif" alt="Sandbox Create → Agent Execute → Results → Sandbox Destroy">

This isn't just "spin up a Docker container." Unlike built-in sandboxes, Agents Sandbox auto-mounts your project code and ships a pre-configured runtime — and you'll feel the difference right away. When the task is done, it self-destructs — see **Why Not Built-in Sandboxes** `[4]` for the full comparison.

## Security Model

**The sandbox is fully isolated from the host. No exceptions.**

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/en/images/security-model.png" alt="Security Model: Host and Sandbox Isolation">

Host network access is off by default and will stay off — by design, not oversight.

Need a database or a cache for your agent? Spin up a Companion Container — a built-in container type in Agents Sandbox that shares the sandbox's isolated network but still can't reach your host. Your sandbox and its companions can talk to each other freely while staying completely walled off from everything else. Full details in **Isolation and Security** `[5]`.

A sandbox on your own machine gives you the same isolation as a remote cloud server — without the latency, cost, or data-residency headaches.

## Your Existing Subscriptions Work Out of the Box

Cloud sandboxes charge you twice: compute by the hour, tokens by the million. You're already paying Anthropic for the tokens. Why pay someone else for a server just to use them?

Agents Sandbox flips that model. Sandboxes run locally on Docker — zero infrastructure cost. Your Claude Max or Codex Pro subscription works right inside the sandbox. No extra API key. No per-token billing. My 17.6 billion tokens in March? That would have cost around $8,000 at API rates. With subscriptions, I paid just $400 for the same usage — a 20× difference. Agents Sandbox makes sure every token from your subscription gets used, not wasted.

For heavy users, that's real money back in your pocket every month.

## Real-World Scenarios

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/en/images/scenario-1.png" alt="Scenario 1: Agent Codes at Full Speed, Zero Confirmation Interrupts">

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/en/images/scenario-2.png" alt="Scenario 2: Multiple Isolated Instances in Parallel">

<img style="max-width: 600px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/en/images/scenario-3.png" alt="Scenario 3: Ephemeral Lifecycle — Destroyed When Done">

## More Than Just a Container

Agents Sandbox is not a thin wrapper around `docker run`. It's a sandbox control plane purpose-built for AI agents — works out of the box for everyday development and scales to full platform integration:

<img style="max-width: 400px; width: 100%; display: block; margin: 0 auto;" src="https://raw.githubusercontent.com/DaladaLee/blogs/main/agents-sandbox/2026-04-01-introducing-agents-sandbox/en/images/not-just-container.png" alt="Seven Key Features: Runtime, Mount/Copy, Credential Injection, Companion Containers, YAML Config, SDK, Auto Cleanup">

Explore the full feature set in the **official documentation** `[6]`.

Remember that Mac Mini I mentioned? You don't need one. A single Docker install, one command, and your agent gets full permissions inside a sandbox that can never touch your host. That's what powers my 600-million-token days — no dedicated hardware, no cloud bills, no permission dialogs. Just results.

> [1]https://github.com/1996fanrui/agents-sandbox
> [2]https://agents-sandbox.com
> [3]https://docs.agents-sandbox.com/quick_start
> [4]https://docs.agents-sandbox.com/why_not_builtin_sandboxes
> [5]https://docs.agents-sandbox.com/isolation_and_security
> [6]https://docs.agents-sandbox.com
