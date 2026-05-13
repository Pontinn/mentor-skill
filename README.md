# Professor — Claude Code Skill

An active professor mode for Claude Code that accompanies your project in real time. It never writes code for you — it teaches, challenges, and guides you to find the answers yourself.

---

## Installation

1. Copy `SKILL.md` into `~/.claude/skills/professor/SKILL.md`
2. Restart Claude Code
3. Run `/professor` to start

---

## How It Works

### First Run (Global Profile)

On the very first `/professor` call across all projects, a one-time profile setup runs:

1. **Language selection** — first question, no preamble. Supports English, Portuguese, Spanish, French, German, Italian, Japanese, Chinese, Korean, and more.
2. **Profile questions** — required fields (name, focus language, career goal) followed by optional ones (birth date, background, learning style, area of interest, biggest pain).
3. **Story question** — optional but high-impact. You can describe your tech journey in free text and/or share a portfolio/GitHub/LinkedIn URL. The professor reads it, extracts context, and auto-fills any remaining optional fields from what you shared.
4. **Pain question** — explores blockers like imposter syndrome, inconsistency, self-doubt, or reliance on AI tools. Used to calibrate tone and encouragement throughout all sessions.

The profile is saved to `~/.claude/skills/professor/user_profile.md` and reused in every future session. It is never asked again unless you run `/professor reset-profile`.

> **Birthday detection:** if you provide your birth date, the professor will wish you a happy birthday at the start of the session on your birthday or within 7 days after.

---

### Per-Project Config

After the global profile, each project gets its own one-time configuration:

- Teaching mode, language, terminal behavior, humor style
- Project type (learning vs real/production)
- Saved to `.professor-config` in the project root (automatically added to `.gitignore`)

On subsequent sessions, the config is loaded silently and the professor jumps straight to asking your objective for the day.

Reset with `/professor reset-project-config`.

---

## Invocation

```
/professor                          → show config menu, then start
/professor auto                     → set terminal=auto, skip menu, start
/professor misto ptbr auto ironico  → set all flags, skip menu, start
```

**All arguments only pre-set values — they never skip initialization questions.**

Mid-session flag switch: `/professor auto` (or any flag) changes the setting for the current session without re-running initialization.

---

## Teaching Modes

| Mode | Behavior |
|---|---|
| `questionar` | Never explains directly. Answers every question with a question. |
| `tutor` | Explains concepts and theory. No code. |
| `misto` _(default)_ | Explains theory + guides with questions based on context. |

---

## Humor Styles

`serio` `descolado` `ironico` `descolado+ironico` `pirata` `jedi` `coach` `filosofo` `drill` `hacker` `detetive` `rpg` `cientista` `comentarista` `poeta` `robo` `vilao` `vendedor` `shakespeariano`

Each humor style fully permeates every response — praise, corrections, hints, and call-outs all speak in character.

---

## Strict Mode

Enabled by default. When the professor detects a real problem (during patrol or code review), it:

1. Suspends the active humor: `Humor [name] desativado.`
2. Delivers a direct, serious call-out — harsher if it's a **repeat offense** (cross-referenced against the session problem log)
3. Prefixes with empathy if frustration is detected: _"Entendo sua frustração, mas..."_
4. Waits for your response before resuming humor: `Humor [name] ativado.`

Toggle with `/professor strict off` / `/professor strict on`.

---

## Commands

| Command | Description |
|---|---|
| `/professor hint` | Progressive hint (3 levels: light → medium → strong) |
| `/professor reveal` | Full solution with detailed explanation |
| `/professor debate [topic] [model]` | Spawns a second professor to debate a topic |
| `/professor review` | Session summary: learned, weak points, stats |
| `/professor quiz` | Quick theory questions on session concepts |
| `/professor concept [term]` | Deep explanation of a specific concept |
| `/professor compare [A] vs [B]` | Pedagogical side-by-side comparison |
| `/professor pause` | Save session state to memory |
| `/professor resume` | Load paused session |
| `/professor progress` | List commits made this session |
| `/professor glossary` | New concepts introduced this session |
| `/professor focus` | Disable proactive analysis temporarily |
| `/professor focus off` | Re-enable proactive analysis |
| `/professor goal [obj] [deadline]` | Set a learning goal with a deadline |
| `/professor resource [topic]` | Study resource suggestions (no URLs) |
| `/professor quick-question [q]` | Fast answer without losing context |
| `/professor re-explain` | Re-explain last concept from a different angle |
| `/professor antipattern` | Antipatterns relevant to current project context |
| `/professor achievements` | List accumulated achievements across sessions |
| `/professor history` | Summary of previous sessions from memory |
| `/professor patrol [5\|10\|15\|off]` | Periodic code monitoring (default: off) |
| `/professor challenge [level]` | Integrated coding challenge (básico/intermediário/avançado/expert) |
| `/professor strict [on\|off]` | Toggle harsh call-outs (default: on) |
| `/professor reset-project-config` | Delete project config and re-run initialization |
| `/professor reset-profile` | Delete global profile and re-run onboarding |

All commands also accept natural language: _"me dá uma dica"_, _"quero ver a resposta"_, _"ativa o ronda"_, etc.

---

## Patrol Mode

`/professor patrol [5|10|15]` activates periodic monitoring via scheduled wake-ups.

On each trigger:
- Runs `git diff HEAD`
- Silent if no changes
- Posts a brief pedagogical observation if changes are found
- Triggers strict mode call-out if a real problem is detected

---

## Coding Challenges

`/professor challenge [level]` launches an integrated challenge inside the professor session:

- Language reused from initialization — never asked again
- Level remembered after first challenge in a session
- Professor never writes solution code or gives algorithmic hints
- `/ff` reveals the full solution with explanation
- Solved challenges are committed and tracked in session stats

---

## Adaptive Level

The professor monitors performance across hints used, reveals triggered, and problem-solving speed. It silently adjusts complexity over time and updates the level in memory across sessions.

---

## Memory & Persistence

| File | Purpose |
|---|---|
| `~/.claude/skills/professor/user_profile.md` | Global user profile (once per user) |
| `[project]/.professor-config` | Per-project session config |
| `[project]/.claude/memory/professor_sessions.md` | Session history and streak tracking |
| `[project]/.claude/memory/professor_problem_log.md` | Problem log for strict mode repeat detection |

---

## Platform Support

Detects platform at runtime:
- **Windows** → uses PowerShell tool for all shell operations
- **macOS / Linux** → uses Bash tool for all shell operations

When `terminal: auto` is active, `.claude/settings.json` is written to the project root **immediately** — before any question is asked — so no permission prompts appear during the session.

---

## What the Professor Never Does

- Write functional code for you
- Give the answer before `/professor reveal` or `/ff`
- Skip initialization steps based on arguments passed
- Expose internal routing labels (PATH-A, PATH-B, STEP N) in any message
- Generate URLs for resources
