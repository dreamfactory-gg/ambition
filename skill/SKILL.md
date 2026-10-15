---
name: ambition
description: Turn a request (even a one-line idea) into a structured kickoff prompt, then a deeper brief, a done bar and the right shape (single thread, subagent or workflow) before anything is built. Use it at every kickoff (feature, project, unit, refactor, research), whenever the owner throws in a new idea mid-session, when a pair-programming back-and-forth has lost its end goal, or when the owner says "deeper", "more ambitious", "what would a pro do" or hands over a one-line idea, even if they don't name the skill. Skip bugs that already have a repro (fix those directly) and changes of a few lines you already hold.
---

# Ambition

From Felix Rieseberg, "How to be more ambitious: a tutorial for humans with models" (felixrieseberg.com): ambition comes as Speed, Altitude, Depth and Range. Speed is the default already; this skill runs the rest and ends in a plan the owner says yes to.

This is planning, not building. Stay in plan mode (call `EnterPlanMode` if not in it) and change no files until the owner approves the plan at the ready gate (§ 6).

## 1. Kickoff: turn the ask into a structured prompt

A one-line idea becomes the kickoff a top professional in that field would want to be handed. Run the owner's depth prompt on the ask:

> I want a deeper version of this task, not a bigger one. Before rewriting it: (1) say what a top professional in this field would do here that I'm not asking for; (2) name the rawest material they would start from instead of what has already been digested for me; (3) name the specific methods and tools they would use; (4) say what, other than you or me, would show the result is right. Then rewrite my prompt to ask for exactly that.

The rewrite is the kickoff. Fill every slot from this ask, the code, logs, history and what the owner said; the examples below only show the shape, never content to reuse. Mark any slot only the owner can fill and ask it in § 2:
- **Who's asking, and what they're not**: their expertise and the field they're stepping into (e.g. "a game designer, not a network engineer").
- **The honesty line**: what the result is not, and that it must say so (e.g. "a prototype, not a security review").
- **Outcome**, verbs first in the owner's plain words: what should be true once it's done (e.g. "make repeat requests come back instantly, and never hand back a stale result"). A technical term only where no plain word carries its meaning.
- **Core principle**: the one source of truth; everything else is derived from it.
- **The first step everything depends on.**
- **Constraints**: stack, licences, machines, budget.
- **One vertical slice, deep, not wide**: numbered steps. For each: the raw data (and where to get it), the method, the specific tools, and **what to leave out of v1, and why**.
- **Judges that don't depend on the owner or the model**: e.g. published reference cases, a second independent method, a backtest on held-out data, real-world measurements, an expert reviewer in the field.
- **Where it's likely to overclaim**: what should lower confidence, and where a simpler method is good enough.

Show the owner the kickoff; it stands on its own (they can paste it into another session). If its outcome is one they'd agree with, move on; if not (they're mid back-and-forth, the ask is a fragment, two outcomes compete), interview (§ 2).

## 2. Interview, only when the outcome is unclear or the owner asks

- One question at a time, each with your recommended answer, so the owner mostly confirms or corrects. Plain chat, not a question box, unless the choices are discrete.
- Answer from the code, logs and history yourself first; ask only what only the owner knows.
- One question is always "what real material do you have?": a screenshot, an export, a complaint, a dashboard, what a teammate said, a sample of the input.
- Stop when a builder would no longer guess: about five questions, up to seven for a big build that isn't settled yet. Weigh the count to the task.
- Record the answers on the task's TASKS.md item, not in new docs.

## 3. Depth: ground the kickoff in what's real

- (2) Look for the raw material yourself (logs, the real database, git history, past session transcripts, the live app), and fold anything only the owner holds into § 2.
- (3) The tools a pro reaches for (profiler, SQL, a statistical test, a simulation, a type checker), with a demanding target where one fits (200 ms, not "fast"). Test the task's own assumption against the raw material before building on it.
- Before planning, find what already exists: earlier evals, judges, scripts and their results (the repo's `evals/`, REPORT.md, the code under test). Search for it yourself; an explorer can miss it. A plan that rebuilds an existing eval is the failure here.
- Report the assumption test's number with the measured error of whatever produced it (a regex, a model, a heuristic). An unmeasured judge's verdict is a hypothesis: measure the judge on a blind sample before the number reaches the owner.
- (4) The judge is outside the model and the owner: a command, a real end-to-end run, a replay of history ("would this have caught last month's incident?"), a backtest, a grader. Model self-grading is not a judge.

## 4. The crew: lenses, not costumes

Ask: if this had to be the best version anyone has made, who would be on the team? Pick 3–5 disciplines the verbs in § 1 point to, always including one adversary. Load each as a lens, never a persona: "act as a marketing specialist" adds nothing; what sharpens a frontier model is what the discipline checks, how its work usually fails, which tools it uses, and what "done" means to it. Write each as one line of that plus one concrete thing it does to this task: "forensic accountant: every saved char traces to an id, anything without one is listed as unmatched."

The lenses usually live in this one context. Give a lens its own agent only when its work is separable and heavy (§ 5).

## 5. Shape: single thread, subagent or workflow

Decide; don't ask by default. Sketch the likely shape, using the model-routing rules in your CLAUDE.md if it has them (which model or tool builds, reviews, grades), rough and open to change as decisions land:
- **Single thread**: small, or every step depends on the last and the reading is light.
- **Subagent**: one separable job (a sweep, a build packet, a review) whose dumps would bloat this context.
- **Workflow**: 2+ independent pieces, or stages (explore → build → review → grade) each heavy enough to flood one context. Name each step's agent, e.g. explore: 2 read-only explorer agents; build: one builder held to the done bar; review: an independent reviewer; grade: one strong model.

A workflow runs only on the owner's yes, which the ready gate collects. If the owner asks for a workflow, weigh it honestly; say so when it's really one subagent's job. No cost estimates (subscription quota, not API spend).

## 6. The plan and the ready gate

Write it short:

```
## Ambition: <task>
Outcome: <the § 1 outcome>
Pro move not asked for: <1–3 lines>
Raw material: <sources, with paths/commands>
Methods/tools: <list>
Crew: <discipline: lens + action> ×3–5
Assumption to test first: <one line + how>
Leave out of v1: <what, and why>
Likely to overclaim: <where, and what lowers confidence>
Access needed: <data, connectors, MCPs, machines, secrets, or "none">
Options (not in the plan unless picked): <bigger framings, with a recommendation>
Shape: <single thread | subagent | workflow, with the step/agent sketch>

### Brief
<self-contained, absolute paths, ready for a builder>

### Done bar
<the § 3 (4) judge as a command or real run, with the pass condition>
```

Then the ready gate: present the plan with `ExitPlanMode` (or, outside plan mode, a yes / keep planning / type-something choice). Nothing is built before the owner's yes. On yes: put the done bar on the task (TASKS.md or wherever tasks live), route the build by your routing rules (a runnable done bar is the check any builder can be held to), and turn each owner-only item under Access into one plain ask (the `wizard` skill, if installed, for multi-step setup).

## 7. New ideas and discoveries

The plan doesn't grow silently, and new ideas aren't refused. When the owner throws in an idea or the build turns up something, weigh it against the outcome:
- It pulls off course: it goes to Options.
- It's better than the plan: it may change the outcome itself; restate (§ 1) and pass the ready gate again rather than bolting it on.
- During a build, bring it as a multiple choice, each option with what it serves and what it costs, plus a weighed recommendation. If picked, update the done bar.

Ideas piling up usually means the outcome was never pinned: go back to § 1.
