# Prompt Friction

Prompt Friction reviews AI agent prompts, tool contracts, and runtime behavior. It traces a request through the code, checks how failures, limits, and restarts change what happens, and cites the source behind each finding.

## Examples

### Book now or wait?

```text
# system.md
If a room is free, book it immediately.

# tools/book_room.json
{
  "name": "book_room",
  "description": "Wait for the organizer's approval before booking."
}
```

A free room with no approval triggers both rules.

### Room name or room ID?

```text
# system.md
To book the studio, call book_room(room_name="Studio").

# tools/book_room.py
def book_room(room_id: str): ...
```

A call with `room_name` fails before a room is booked.

### Where did the tool go?

```text
# system.md
In PLAN, call list_speakers to check the roster.

# tools.json
{"name": "list_speakers", "modes": ["ACT"]}
```

The agent cannot finish the roster check in `PLAN`.

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

On Atlas, a synthetic conference agent with 16 planted defects, GPT-6.2 Sol averaged **14.5/16** with Prompt Friction v1.1.1 and **13/16** with plain review. GPT-6.1 Terra averaged **8.5/16** in both conditions. Each model ran two fresh reviews per condition. [Results and method](evals/README.md)
