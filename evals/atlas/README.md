# Atlas synthetic agent

Evaluated November 21, 2026 with Prompt Friction v1.1.1.

Atlas is a conference operations agent with 200 instructions, 30 tool contracts, three modes, 16 planted defects, and nine working controls. The comparison used the [v1.1.1 skill text](https://github.com/RazGvili/prompt-friction/blob/b8b40c2af91e1bef1d12e7e0564769b40ae89b7f/plugins/prompt-friction/skills/prompt-friction/SKILL.md).

## Results

| Codex model | Plain review | With Prompt Friction | Mean change |
| --- | ---: | ---: | ---: |
| GPT-6.2 Sol | 12/16, 14/16 | 15/16, 14/16 | +9.4 percentage points |
| GPT-6.1 Terra | 9/16, 8/16 | 9/16, 8/16 | 0.0 percentage points |

## Method

Eight fresh Codex sessions got the same project files and review request. The skill sessions also received Prompt Friction. Two graders independently scored the reports against a frozen rubric with model and condition labels removed. A third resolved four disputed labels. No report flagged any of the nine working controls as broken.

With the skill, Terra found one more missing-tool defect and one fewer handoff conflict across its two runs. Each condition had two runs on the same fixture. The results describe performance on Atlas. The Atlas fixture and full review reports are unpublished.

## Individual runs

| Codex model | Condition | Defects found | Elapsed time |
| --- | --- | ---: | ---: |
| GPT-6.1 Terra | Prompt Friction | 9/16 | 47 s |
| GPT-6.2 Sol | Plain review | 12/16 | 113 s |
| GPT-6.2 Sol | Prompt Friction | 15/16 | 124 s |
| GPT-6.1 Terra | Plain review | 9/16 | 40 s |
| GPT-6.1 Terra | Plain review | 8/16 | 40 s |
| GPT-6.2 Sol | Prompt Friction | 14/16 | 132 s |
| GPT-6.1 Terra | Prompt Friction | 8/16 | 40 s |
| GPT-6.2 Sol | Plain review | 14/16 | 119 s |

Elapsed time includes tool and session scheduling.
