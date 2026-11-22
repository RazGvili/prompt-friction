# Checklist experiments

These November 20–21, 2026 experiments compared proposed edits with the v1.1.1 checklist. The released checklist remained the baseline.

| Proposed edit | Project | v1.1.1 result | Candidate result |
| --- | --- | --- | --- |
| Work through a concrete case | Two versions of a synthetic dashboard | Found the blocker and recognized the working control in both | Same result on the predefined cases |
| Work through a concrete case | Open Deep Research | 7/9 | 7/9 |
| Consider all rules for one decision together and check fallbacks | Atlas | 29/32 | 29/32 |
| Compare actual tool outputs with their contracts and callers | Atlas | 28/32 | 30/32 |
| Compare actual tool outputs with their contracts and callers | Open Deep Research | 15/18 | 14/18 |
| Refine the tool-output instruction | Atlas | 15/16 | 14/16 |
| Refine the tool-output instruction | Open Deep Research | 8/9 | 7/9, 8/9 |

Scores with denominators of 32 or 18 are totals across two runs per condition. The concrete-case edit had one run per condition per input, found one additional supported issue on the revised dashboard, and took longer in all three pairs.

The tool-output edit gained two findings across the Atlas runs and lost one across the Open Deep Research runs. The Open Deep Research request mistakenly retained the Atlas name. All four reports cited the intended Open Deep Research source. The later edit scored lower on Atlas and matched the research baseline in one of its two runs.

Atlas and Open Deep Research had both appeared in earlier experiments. These checks guided edits on those projects.
