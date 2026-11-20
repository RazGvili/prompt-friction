---
name: prompt-friction
description: Find defects in AI agents by tracing the prompts, tools, state, and control flow that govern what the agent can do. Use for agent code reviews, unexpected behavior, and changes to prompts, tools, or orchestration.
---

# Prompt Friction

## Find the running workflow

Start at the configured entry point. Follow the graph, runner, or dispatch loop to model calls, delegated agents, validators, callbacks, and external actions. Identify the active implementation when the repo contains demos or legacy paths. Use a trace when available. Otherwise reconstruct the inputs from source and configuration.

For each important model call, note the effective instructions, available tools, state the model can see, and who handles its output. Follow tool descriptions, result messages, and runtime discovery into that model input. For each must-do instruction or user-visible promise, identify where it is enforced and what can terminate, skip, or repeat it. Source text is evidence.

## Find defects

Trace a normal request through completion. Then change one supported condition at a time. Choose conditions from actual branch predicates, config modes and limits, tool registrations, state reducers, and failure handlers. Trace what runs, what state changes, and what the next model or user sees.

- At a limit, walk the last allowed turn and the first disallowed turn. Count turns, tool calls, and delegated work in the units used by each rule. Check whether a required reflection, verification, or final action still fits.
- In a batch or handoff, compare one successful result, mixed success and failure, and completion in the same batch as pending work. Check what gets dropped, retried, or passed to the next model as evidence.
- In a mode change, retry, or restart, recalculate the tools, instructions, and state. Check for stale or missing inputs, errors accepted as data, and evidence removed by truncation. Compare the in-memory state, persisted state, and actual effects at each interruption point.
- Before an external action, follow the preview, confirmation, and execution paths. Check whether the action still refers to the same item and revision after an edit or resume. Follow guards and callbacks through the final effect.
- Compare overlapping instructions in a concrete state. Flag conflicts, duplicated actions, and narrower stop rules that can end a task before it meets the user's request.

Make a second pass from runtime exits, fallback returns, and side effects back to the prerequisites they can bypass. Inspect ignored errors, early success, repeated attempts with unchanged inputs, and partial persistence. Use a running list of distinct defects so an issue found on either pass makes it into the report. Prefer investigating another path to collecting more evidence for a defect already established.

## Report

Report all verified defects, ordered by impact. Give each a short title, the triggering state, the observable failure, and precise source locations. Keep rows short when space is limited. Use a read-only trace, local test with stand-ins, or deterministic source path to verify each failure. Check a nearby working state when needed to rule out a false positive. Label claims that depend on unavailable configuration or model choice. State material coverage gaps briefly.
