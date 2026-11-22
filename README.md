# Prompt Friction

Prompt Friction reviews AI agent prompts, tool contracts, and runtime behavior. It traces a request through the code, checks how failures, limits, and restarts change what happens, and cites the source behind each finding.

## Examples

- **Book now or wait?** A room opens up. One rule says “Book the first available room immediately.” Another says “Wait for the organizer’s approval before booking.”
- **The required tool is missing.** The agent must check the speaker roster before proposing a move. Its `list_speakers` tool is unavailable in planning mode.
- **Both steps are waiting.** The speaker notice must link to the published schedule. The schedule can go live only after the notice has been sent.

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

## Use the skill

Start a new session in the project you want to review, with access to the agent's code and configuration. Ask “Use Prompt Friction to audit the refund agent. Can it still check for approval after a restart?”

In Codex, type `$` and select `prompt-friction` in the skill picker. In Claude Code, use `/prompt-friction:prompt-friction`.

Each verified finding gives the triggering state, the observable failure, and source locations.

## Evaluation

In one paired Codex review per project on three snapshots used during development, the released skill text found 18 of 19 predefined defects. Plain review found 12. [Results and method](evals/README.md)
