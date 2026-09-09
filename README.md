# Bambuser Agent Skills

[Agent Skills](https://agentskills.io) that teach your AI agent [Bambuser](https://bambuser.com) video commerce — **Live Shopping, Shoppable Video, Video Consultation, Chat, and the App Framework** *(Beta)*.

| Skill | For | Covers |
|-------|-----|--------|
| **`bambuser-integration`** | Developers integrating Bambuser into a site or app | Embeds, cart/product data, tracking, regions, SSO, mobile SDKs, REST APIs, App Framework |
| **`bambuser-knowledge`** | Anyone *using* Bambuser — hosts, agents, marketers, PMs | Dashboard how-tos, broadcasting/RTMP, bookings, moderation, stats, capabilities & limits |

Both are deliberately **thin**: the durable concepts and gotchas live in the skill, and the latest specifics are **fetched at runtime** from [bambuser.com/docs](https://bambuser.com/docs) and [knowledge.bambuser.com](https://knowledge.bambuser.com), so answers stay current as the docs evolve. They work with Claude Code, Cursor, Codex, Antigravity CLI, GitHub Copilot, Amp, and other tools that support the Agent Skills standard.

## Install

```bash
npx skills add bambuser/bambuser-agent-skills      # into the current project
npx skills add -g bambuser/bambuser-agent-skills   # or globally, for all your projects
```

> **claude.ai users (one-time):** allow `bambuser.com` and `knowledge.bambuser.com` at [claude.ai/settings/capabilities](https://claude.ai/settings/capabilities), or the skills can't fetch the docs they rely on.

**Verify:** `npx skills list` shows both skills — then start a new session.

<details>
<summary><b>Alternative — manual copy</b></summary>

First, clone the repository:

```bash
git clone https://github.com/bambuser/bambuser-agent-skills
```

#### Global install

For agents that use the shared skills directory, create it:

```bash
mkdir -p ~/.agents/skills
```

Then copy the skills:

```bash
cp -R bambuser-agent-skills/skills/. ~/.agents/skills/
```

For Claude Code, use `~/.claude/skills` instead:

```bash
mkdir -p ~/.claude/skills
```

```bash
cp -R bambuser-agent-skills/skills/. ~/.claude/skills/
```

#### Per-project install

From your project root, create the shared skills directory:

```bash
mkdir -p .agents/skills
```

Then copy the skills from the cloned repository:

```bash
cp -R /path/to/bambuser-agent-skills/skills/. .agents/skills/
```

For Claude Code, use `.claude/skills` instead:

```bash
mkdir -p .claude/skills
```

```bash
cp -R /path/to/bambuser-agent-skills/skills/. .claude/skills/
```

Replace `/path/to/bambuser-agent-skills` with the location of your clone. Other
agents may document a different skills directory.
</details>

## Use

Optionally, add your workspace facts and the routing rules to your project's instructions file
(`CLAUDE.md`, `AGENTS.md`, Cursor rules, etc.). Fill in the top two lines — they save
the agent from asking every session, and the region decides which hosts it writes:

```text
Bambuser workspace: 
- region: <Global | EU>, 
- orgId: <your-org-id>

Two Bambuser skills are installed — route by intent, not keyword:
- Writing or changing code (embeds, cart/product data, tracking, REST APIs, SSO, mobile SDKs, App Framework) → the bambuser-integration skill.
- Using the product, no code (dashboard how-tos, shows, broadcasting/RTMP, bookings, stats meanings, "can Bambuser do X?") → the bambuser-knowledge skill.
- Mixed requests: Usability diagnostic part with bambuser-knowledge, fix with bambuser-integration.
- Don't use Bambuser API/config specifics from the model's own knowledge — fetch the live docs page first, and treat it as authoritative if it contradicts the skill.
- Workspace-specific values (region, orgId, show/video/playlist IDs, existing cart APIs) come from the lines above, the user, or their code — never from a guess unless for testing.
```

Both are in your BamHub URL: the domain gives the region (`lcx.bambuser.com` = Global,
`lcx-eu.bambuser.com` = EU) and the path segment after it is the `orgId`
(`lcx.bambuser.com/<orgId>`). Leave a placeholder as-is if you don't know it yet — the
skills will ask rather than invent one.

Now you can just ask in natural language — the skills trigger on Bambuser topics. Examples:

| Example prompt | Routes to |
|---|---|
| "Create a Bambuser live shopping landing page and enable mini player." | integration |
| "Set up `<bam-playlist>` shoppable video in carousel mode with a soft limit of 5." | integration |
| "Implement product and cart integration for the Bambuser player." | integration |
| "Pull show stats from the Bambuser REST API into our BI tool." | integration |
| "In Bambuser, how do I stream with an external camera over RTMP?" | knowledge |
| "On Bambuser dashboard, how are unique viewers counted in the stats page?" | knowledge |
| "Can I download the video after a Bambuser live show?" | knowledge |

## What's inside

```
skills/
├── bambuser-integration/   # SKILL.md + references: products, regions, cart & product
│                           # data, tracking, REST APIs, SSO, mobile SDKs, App Framework,
│                           # troubleshooting, doc index
└── bambuser-knowledge/     # SKILL.md + scripts/search.py (knowledge-base search)
                            # + references: knowledge-base map, curated FAQs per product
```

## License

[Apache-2.0](LICENSE) © Bambuser
