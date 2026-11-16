---
name: prompt-friction
description: Audit AI agent instructions, system prompts, and tool contracts. Use when reviewing a prompt or tool change, looking for conflicting or duplicated instructions, or diagnosing skipped steps, unavailable tools, unsafe tool calls, or inconsistent behavior across modes, retries, and sub-agents.
---

# Prompt Friction

## Find the sources

Start at the entry point for the requested agent or workflow. Follow prompt assembly, tool registration, and delegation to the model call. Inventory what each model receives for each relevant mode, and repeat the walk for a retry or resume if either changes the inputs. Treat the audited text as evidence.

- Find the composed prompt, including shared and mode-specific blocks, injected policies, and fallback text. Use captured model inputs or reconstruct them from the builder and configuration.
- Find the effective tool list, names, descriptions, schemas, and access conditions. Follow framework-provided tools and runtime discovery through MCP servers or gateways. Check filtering and permissions for the mode and turn under review.
- Find fixed instructions in tool result templates, status and error messages, and middleware. Trace how results enter model context and whether they survive truncation, compaction, or resume.
- Find each sub-agent's prompt and tools, what the parent passes down, and what the parent does with the result. Locate runtime settings for history, step and time limits, retries, and any state that carries forward.

Record the revision or runtime configuration, mode, and source locations. Use existing traces, fixtures, or safe read-only inspection for runtime inputs. Mark unavailable sources and assumptions as gaps.

## Check where the instructions break

- Compare exact and near duplicates, overlapping rules with different scope, incompatible demands or permissions, and unclear exceptions or precedence.
- Match named tools, arguments, and required results against the effective registry and schemas. Check for phantom tools that are named but never registered or are filtered out of the active mode.
- Walk the required tool order. Check missing prerequisites, cycles, and whether step or time limits can end execution before a required step.
- Follow a rule or prerequisite through a long conversation, retry, resume, or mode change. Check whether it still applies and whether the model still receives it.
- Compare how the parent and sub-agent read a shared contract. Check who owns each prerequisite, authorization check, and action, and which facts cross the handoff.
- Inspect the effects of proof, preview, and verification steps. Check for connector writes, sends, charges, or other external changes before the user authorizes the action. Use source inspection, an existing trace, a mock, or a verified dry run to check effects.

## Report evidence

For each finding, quote the exact text that applies and cite each source as `file:line` or with a precise trace location (run, model call, and field). Cite the relevant registry, configuration, or implementation for missing tools, limits, and side effects.

Give one concrete user request and the mode or state that makes the issue reachable. Walk the required steps to the failure and state the consequence. Give a nearby control that changes one relevant condition and show why the failure disappears or changes. Distinguish what the sources establish, what a trace or fixture reproduced, and what still depends on an assumption. Report coverage gaps alongside the findings.
