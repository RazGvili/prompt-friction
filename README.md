# Prompt Friction

Prompt Friction reviews AI agent prompts, tool contracts, and runtime behavior. It traces a request through the code, checks how failures, limits, and restarts change what happens, and cites the source behind each finding.

## Examples

Shortened excerpts from a synthetic conference agent.

### The prompt and tool disagree on what comes first

`system.md:154`

```text
Call get_audience for the current audience rules before drafting a notice.
```

`tools.json:2469`

```json
{
  "name": "get_audience",
  "description": "For staff notices, preview the proposed body before calling this tool."
}
```

For a fresh staff notice, each step requires the other to happen first.

### The prompt requires a tool that is unavailable

`system.md:83`

```text
Use list_speakers to check who is affected before proposing a schedule change.
```

`tools.json:1498`

```json
{
  "name": "list_speakers",
  "modes": ["ACT", "RECOVERY"]
}
```

Ask for a planning-only schedule proposal. The agent starts in `PLAN`, where it cannot call `list_speakers`.

### The tool returns a value outside its schema

`tools.json:572` · `list_room_slots` output schema

```json
{
  "status": { "enum": ["available", "held", "booked"] }
}
```

`atlas.py:274` · `confirm_room` handler

```python
hold.update(status="confirmed", booking_id=booking_id)
```

Read the slot after confirming the room. `list_room_slots` returns `confirmed`, which is outside the declared enum.

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
