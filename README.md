# Mentor — Claude Code Skill

An active mentor mode for Claude Code that accompanies your project in real time. It never writes code for you — it teaches, challenges, and guides you to find the answers yourself.

---

## Installation

1. Copy `SKILL.md` into `~/.claude/skills/mentor/SKILL.md`
2. Restart Claude Code
3. Run `/mentor` to start

---

## How It Works

### First Run (Global Profile)

On the very first `/mentor` call across all projects, a one-time profile setup runs:

1. **Language selection** — first question, no preamble. Supports English, Portuguese, Spanish, French, German, Italian, Japanese, Chinese, Korean, and more.
2. **Profile questions** — required fields (name, focus language, career goal) followed by optional ones (birth date, background, learning style, area of interest, biggest pain).
3. **Story question** — optional but high-impact. You can describe your tech journey in free text and/or share a portfolio/GitHub/LinkedIn URL. The mentor reads it, extracts context, and auto-fills any remaining optional fields from what you shared.
4. **Pain question** — explores blockers like imposter syndrome, inconsistency, self-doubt, or reliance on AI tools. Used to calibrate tone and encouragement throughout all sessions.

The profile is saved to `~/.claude/skills/mentor/user_profile.md` and reused in every future session. It is never asked again unless you run `/mentor reset-profile`.

> **Birthday detection:** if you provide your birth date, the mentor will wish you a happy birthday at the start of the session on your birthday or within 7 days after.

---

### Per-Project Config

After the global profile, each project gets its own one-time configuration:

- Teaching mode, language, terminal behavior, humor style
- Project type (learning vs real/production)
- Saved to `.mentor-config` in the project root (automatically added to `.gitignore`)

On subsequent sessions, the config is loaded silently and the mentor jumps straight to asking your objective for the day.

Reset with `/mentor reset-project-config`.

---

## Invocation

```
/mentor                              → show config menu, then start
/mentor auto                         → set terminal=auto, skip menu, start
/mentor mixed auto ironic            → set all flags, skip menu, start
```

**All arguments only pre-set values — they never skip initialization questions.**

Mid-session flag switch: `/mentor auto` (or any flag) changes the setting for the current session without re-running initialization.

---

## Teaching Modes

| Mode | Behavior |
|---|---|
| `socratic` | Never explains directly. Answers every question with a question. |
| `tutor` | Explains concepts and theory. No code. |
| `mixed` _(default)_ | Explains theory + guides with questions based on context. |

---

## Teaching Philosophy

Nine rules govern HOW the mentor teaches, across all modes:

1. **Engagement lock** — when you signal you want to understand something, the topic is locked. The mentor will never suggest skipping or postponing.
2. **Demo-first** — for "why does X behave this way?" questions with observable answers, you'll be instructed to run a test BEFORE any theory.
3. **Variable manipulation > read-only execution** — when a concept hinges on a value (default, parameter, constant), the mentor instructs you to MODIFY it and observe — not just run as-is.
4. **Self-discovery > told answer** — hypothesis → experiment → you verbalize the discovery → mentor confirms. The explanation comes AFTER you saw it happen.
5. **Loop detector** — if 3+ clarification rounds on the same point fail, the mentor switches form (text → analogy → demo → smaller unit). Never piles more text.
6. **Comprehension-check budget** — at most one "makes sense?" per concept. Active confirmation (apply, test, rephrase) preferred.
7. **Minimum viable explanation** — 1–3 sentence answers by default. Extended only when you ask for more.
8. **Socratic with a pragmatic floor** — philosophical "why" questions only AFTER you know the basic mechanic. Before that, the rule comes direct.
9. **No abandoning under engagement** — phrases like "let's skip this" or "not worth getting stuck here" are forbidden while you're explicitly engaged.

---

## Humor Styles

`serious` `casual` `ironic` `casual+ironic` `pirate` `jedi` `coach` `philosopher` `drill` `hacker` `detective` `rpg` `scientist` `commentator` `poet` `robot` `villain` `salesman` `shakespearean`

Each humor style fully permeates every response — praise, corrections, hints, and call-outs all speak in character.

---

## Strict Mode

Enabled by default. When the mentor detects a real problem (during patrol or code review), it:

1. Suspends the active humor: `Humor [name] disabled.`
2. Delivers a direct, serious call-out — harsher if it's a **repeat offense** (cross-referenced against the session problem log)
3. Prefixes with empathy if frustration is detected: _"I understand your frustration, but..."_
4. Waits for your response before resuming humor: `Humor [name] enabled.`

Toggle with `/mentor strict off` / `/mentor strict on`.

---

## Commands

| Command | Description |
|---|---|
| `/mentor hint` | Progressive hint (3 levels: soft → medium → strong) |
| `/mentor reveal` | Full solution with detailed explanation |
| `/mentor debate [topic] [model]` | Spawns a second mentor to debate a topic |
| `/mentor review` | Session summary: learned, weak points, stats |
| `/mentor quiz` | Quick theory questions on session concepts |
| `/mentor concept [term]` | Deep explanation of a specific concept |
| `/mentor compare [A] vs [B]` | Pedagogical side-by-side comparison |
| `/mentor pause` | Save session state to memory |
| `/mentor resume` | Load paused session |
| `/mentor progress` | List commits made this session |
| `/mentor glossary` | New concepts introduced this session |
| `/mentor focus` | Disable proactive analysis temporarily |
| `/mentor focus off` | Re-enable proactive analysis |
| `/mentor goal [obj] [deadline]` | Set a learning goal with a deadline |
| `/mentor resource [topic]` | Study resource suggestions (no URLs) |
| `/mentor docs [url]` | Read external API/library docs and point you to relevant sections (never the answer) |
| `/mentor quick-question [q]` | Fast answer without losing context |
| `/mentor re-explain` | Re-explain last concept from a different angle |
| `/mentor antipattern` | Antipatterns relevant to current project context |
| `/mentor achievements` | List accumulated achievements across sessions |
| `/mentor history` | Summary of previous sessions from memory |
| `/mentor patrol [5\|10\|15\|off]` | Periodic code monitoring (default: off) |
| `/mentor challenge [level]` | Integrated coding challenge (basic/intermediate/advanced/expert) |
| `/mentor strict [on\|off]` | Toggle harsh call-outs (default: on) |
| `/mentor reset-project-config` | Delete project config and re-run initialization |
| `/mentor reset-profile` | Delete global profile and re-run onboarding |

All commands also accept natural language: _"give me a hint"_, _"show me the answer"_, _"patrol on"_, etc. Natural-language recognition adapts to the user's chosen language.

---

## Patrol Mode

`/mentor patrol [5|10|15]` activates periodic monitoring via scheduled wake-ups.

On each trigger:
- Runs `git diff HEAD`
- Silent if no changes
- Posts a brief pedagogical observation if changes are found
- Triggers strict mode call-out if a real problem is detected

---

## Documentation Guidance

When your objective involves an external API, library, or service (Stripe, OpenAI, AWS, etc.), the mentor offers to read the official docs and point you to the relevant sections — never to give you the answer.

- Proactive offer when API/integration is detected
- Use `/mentor docs [url]` to send a specific page or the docs home
- **Multi-page navigation:** send the docs home/index → mentor extracts the sidebar topic tree, smart-filters topics matching your objective, fetches up to 10 relevant pages, caches the whole tree
- **On topic change:** if you shift to a new topic (e.g. auth → webhooks), mentor asks before fetching new pages
- **Refresh:** weekly automatic refresh of the index, or on demand (_"refresh docs"_)
- Output is always directional: _"Read Section X → Subsection Y for the authentication flow you need. Note the `idempotency_key` field."_
- Never pastes code from the docs. Never summarizes "here's how to do it"
- Multi-doc supported (API + SDK + tutorial)
- Private/internal docs: paste the content directly, same treatment
- Follow-up _"I read X but didn't understand"_: mentor doesn't re-summarize — applies pedagogy rules (demo, experiment) on what the section describes

---

## Coding Challenges

`/mentor challenge [level]` launches an integrated challenge inside the mentor session:

- Language reused from initialization — never asked again
- Level remembered after first challenge in a session
- Mentor never writes solution code or gives algorithmic hints
- `/ff` reveals the full solution with explanation
- Solved challenges are committed and tracked in session stats

---

## Adaptive Level

The mentor monitors performance across hints used, reveals triggered, and problem-solving speed. It silently adjusts complexity over time and updates the level in memory across sessions.

---

## Memory & Persistence

| File | Purpose |
|---|---|
| `~/.claude/skills/mentor/user_profile.md` | Global user profile (once per user) |
| `[project]/.mentor-config` | Per-project session config |
| `~/.claude/projects/[project]/memory/mentor_sessions.md` | Session history and streak tracking |
| `~/.claude/projects/[project]/memory/mentor_problem_log.md` | Problem log for strict mode repeat detection |
| `~/.claude/projects/[project]/memory/mentor_challenge_history.md` | Completed challenges |
| `~/.claude/projects/[project]/memory/mentor_docs_cache.md` | Cached external documentation analysis |

---

## Platform Support

Detects platform at runtime:
- **Windows** → uses PowerShell tool for all shell operations
- **macOS / Linux** → uses Bash tool for all shell operations

When `terminal: auto` is active, `.claude/settings.json` is written to the project root **immediately** — before any question is asked — so no permission prompts appear during the session.

---

## What the Mentor Never Does

- Write functional code for you
- Give the answer before `/mentor reveal` or `/ff`
- Skip initialization steps based on arguments passed
- Expose internal routing labels in any message
- Generate URLs for resources
