# ambition

A Claude Code skill that turns a request, even a one-line idea, into a plan worth building before anything is built.

It adapts Felix Rieseberg's ["How to be more ambitious: a tutorial for humans with models"](https://felixrieseberg.com/how-to-be-more-ambitious-a-tutorial-for-humans-with-models/), which names four kinds of ambition: Speed, Altitude, Depth and Range. Speed is what models already give you; this skill runs the other three and ends at a ready gate you approve.

## What it does

1. **Kickoff**: runs the article's depth prompt on your ask and writes a structured kickoff: who's asking and what they're not, the honesty line, the outcome, the core principle, constraints, one deep vertical slice (with raw data, method, tools and what to leave out of v1 for each step), judges that don't depend on you or the model, and where it's likely to overclaim. The kickoff stands on its own; paste it into any session.
2. **Interview**: only when the outcome is unclear. One question at a time, each with a recommended answer.
3. **Depth**: finds what already exists, the raw material, and tests the task's own assumption before building on it, with the error of whatever measured it.
4. **Crew**: 3 to 5 disciplines as lenses (what each checks, how its work fails, its tools, its idea of done), never costumes, always including an adversary.
5. **Shape**: single thread, subagent or workflow.
6. **Ready gate**: the plan, a brief a builder can take, and a done bar; nothing is built before your yes.
7. **New ideas**: weighed against the outcome; never bolted on silently, never refused.

## Install

    git clone https://github.com/dreamfactory-gg/ambition ~/src/ambition
    ln -s ~/src/ambition/skill ~/.claude/skills/ambition

Claude Code picks it up in new sessions. It triggers on kickoffs (a feature, project, refactor or research), on a one-line idea, and on "go deeper" or "what would a pro do"; you can also run `/ambition`. To make it the default, add a line to your `CLAUDE.md`, for example: "Any kickoff: run the `ambition` skill; its done bar decides who builds."

## License

MIT
