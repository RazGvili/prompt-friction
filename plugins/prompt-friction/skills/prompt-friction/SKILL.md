---
name: prompt-friction
description: Find defects in AI agents by tracing the prompts, tools, state, and control flow that govern what the agent can do. Use for agent code reviews, unexpected behavior, and changes to prompts, tools, or orchestration.
---

# Prompt Friction

Find defects by checking what the agent is told to do and what it can do.

## Find the code that runs

Start at the entry point that the configuration selects. If the project has demos or old code, find the code that runs. Follow the workflow to model calls, sub-agents, validators, callbacks, and external actions. Use a trace if one is available. Otherwise, use the source code and configuration to find the inputs.

For each important model call, find the instructions, tools, and state the model receives. Find which code handles the reply. Check how tool descriptions, tool results, and tools found at run time reach the model. For each requirement or promise to the user, find the code that enforces it. Check what can stop, skip, or repeat the required step. Use source text as evidence.

## Check what happens

Trace a normal request until it ends. Then change one condition at a time. Pick conditions from branches, modes, limits, tool registrations, state updates, and error handlers. For each case, check which code runs. Check what state changes. Check what the next model call or the user sees.

- **Limits.** Trace the last turn that the limit allows. Trace what happens when the agent tries the next turn. Count turns, tool calls, and work sent to sub-agents. Use the unit that each rule names. Check whether each required reflection, verification, or final action fits within the limit.
- **Batches and handoffs.** Compare a case with one successful result and a case with both a success and a failure. Also check what happens when one item signals completion while work in the same batch is still pending. Check what the code drops, retries, or gives to the next model call as evidence.
- **Mode changes, retries, and restarts.** Find the instructions, tools, and state again after the change. Check for stale or missing inputs. Check whether the code uses an error as data. Check whether truncation removes evidence. At each interruption, compare state in memory, saved state, and actions that already happened.
- **External actions.** Follow the preview, confirmation, and execution. After an edit or a resume, check that the action uses the same item and revision. Follow the checks and callbacks until the action takes effect.
- **Overlapping instructions.** Compare instructions that apply in the same case. Look for conflicts and repeated actions. Check whether a stop rule can end the task before the agent meets the user's request.

## Check early stops and actions

Find the places where the agent can stop, return a fallback, or take an external action. For each path, check what must happen first. Check whether the path can skip that step. Look for ignored errors, early success, retries with the same inputs, and state that is only partly saved.

Keep one list of distinct defects from both passes. Once you have enough evidence for a finding, move to another path.

## Report what you verified

Report all verified defects in order of impact. Give each defect a short title. Say when it occurs and what goes wrong. Cite the exact source locations. Keep rows short if space is limited.

Verify each defect with a read-only trace, a local test with stand-ins, or a code path that proves the result. If needed, check a nearby case that works. Label findings that depend on configuration or a model choice you cannot check. Briefly state any gaps that matter.
