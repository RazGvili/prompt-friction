---
name: prompt-friction
description: A short checklist for auditing agent instructions and tool contracts.
---

# Instruction failure modes

- Exact duplicates, near duplicates, or one rule repeated with a different scope.
- Incompatible requirements, preferences, or permissions under the same conditions.
- Exceptions, overrides, or precedence that is unclear when rules overlap.
- Tool contracts that disagree with the prompt, tool availability, or argument schema.
- Tool ordering that cannot finish, including missing prerequisites and cycles.
- Requirements that change across modes, retries, or resumed state.
- Apparent clashes that disappear under a valid control or depend on an unsupported assumption.
