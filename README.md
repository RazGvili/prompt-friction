# Prompt Friction

Prompt Friction finds defects across agent prompts, tools, and runtime control flow. It traces a normal request, checks what happens when one condition changes, and cites the code behind each finding. See the [project-level eval](evals/2026-11-18-project-reviews/README.md) for the tested projects and results.

## Install in Codex

```sh
codex plugin marketplace add RazGvili/prompt-friction
codex plugin add prompt-friction@prompt-friction
```

## Install in Claude Code

```sh
claude plugin marketplace add RazGvili/prompt-friction
claude plugin install prompt-friction@prompt-friction
```

Start a session in the project you want to audit. Ask “Use Prompt Friction to audit the refund agent. Can it still check for approval after resuming a long conversation?” In Claude Code, you can invoke the skill with `/prompt-friction:prompt-friction`.

For another Agent Skills host, copy the [`prompt-friction` skill directory](plugins/prompt-friction/skills/prompt-friction) into that host's skill directory. [Read the skill](plugins/prompt-friction/skills/prompt-friction/SKILL.md).
