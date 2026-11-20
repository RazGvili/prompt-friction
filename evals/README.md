# Prompt Friction evaluation

The Prompt Friction skill text shipped in v1.1.0 and v1.1.1 identified **18 of 19 predefined defects** across three project snapshots. Plain review identified **12 of 19** in the paired comparison.

| Project | Plain review | Prompt Friction v1.1.0 |
| --- | ---: | ---: |
| Open Deep Research | 5/9 | 8/9 |
| Google ADK small-business loan agent | 5/6 | 6/6 |
| Magentic-UI OmniAgent | 2/4 | 4/4 |
| **Total** | **12/19** | **18/19** |

## Method and scope

Each project had one plain and one skill-assisted review in fresh Codex sessions. Both reviewers started at the same project root, saw the same file set and request, received no defect hints, and had a 900-word reporting cap. Only the skill-assisted reviewer received the Prompt Friction instructions. The LangChain and Google ADK request was “Can you look through this project and tell me what could go wrong with the agent?” The Magentic-UI request was “Can you look through this project and tell me what could go wrong when the agent stops and continues?”

The checklist was fixed before the scored reviews. A hit required the final report to identify the failure and a reachable mechanism supported by source or a local mock. Each distinct mechanism counted once. The score measures coverage of the listed defects. It leaves precision and additional findings unscored.

The same three projects informed skill development, so the scores describe this suite. Reviewers used source inspection and local mocks. Live agent behavior and production deployments fall outside the test. The published record contains the inputs and scores. Exact model settings and raw reports are unavailable in the repository. [Full evaluation record and spot checks](https://github.com/RazGvili/prompt-friction/blob/5d5e677e05807bf717425ddbfb308259530fd18b/evals/2026-11-18-project-reviews/README.md)

## Results by defect

| Predefined defect | Plain review | Prompt Friction v1.1.0 |
| --- | --- | --- |
| Research. Supervisor cap prevents required planning, delegation, or reflection | Missed | Found |
| Research. Failed worker drops a successful sibling | Found | Found |
| Research. Control tools bypass the guard when no source is available | Found | Found |
| Research. Writer receives reflection and overflow errors as evidence | Missed | Found |
| Research. Supervisor counts rounds against an individual-call limit | Missed | Found |
| Research. Completion skips sibling research in the same batch | Found | Found |
| Research. Compression uses wrong provider for error handling | Found | Found |
| Research. Compression truncation loses the latest evidence | Found | Found |
| Research. Three-source stop can violate requested coverage | Missed | Missed |
| Loan. Active priced loan can resume at decision before approval | Found | Found |
| Loan. Existing session ignores a second loan ID | Found | Found |
| Loan. First status inquiry can create a new record | Missed | Found |
| Loan. Missing or failed output can persist as completed | Found | Found |
| Loan. Rejection leaves an active application resumable | Found | Found |
| Loan. Final-answer judge runs after checkpoint side effects | Found | Found |
| OmniAgent. Round-limit Stop drops pending tool outcomes from history | Found | Found |
| OmniAgent. Resume mislabels completed, rejected, and unstarted calls | Found | Found |
| OmniAgent. Oversized write leaves a stale checkpoint for restart | Missed | Found |
| OmniAgent. Bare text fallback can report completion without evidence | Missed | Found |

## Inputs

Copy only the files listed in [the checksum manifest](inputs.sha256). Use the following paths under a common directory.

| Prefix | Pinned source |
| --- | --- |
| `research/` | Paths relative to the [Open Deep Research repository root](https://github.com/langchain-ai/open_deep_research/tree/1b7d2e80db9faa586165c60e09096dbbfd483a64) |
| `loan/` | Paths relative to the [Google ADK recipe directory](https://github.com/google/adk-recipes/tree/e05958ea5c6577c84e04c0f5063fe1e8544f5b00/contrib/python/small-business-loan-agent) |
| `omni/` | Paths relative to the [Magentic-UI repository root](https://github.com/microsoft/magentic-ui/tree/d3c9d13c39288257286a66daabf7c5b5fb72ee69) |
| `prompt-friction/SKILL.md` | [Evaluated skill text](https://github.com/RazGvili/prompt-friction/blob/5d5e677e05807bf717425ddbfb308259530fd18b/plugins/prompt-friction/skills/prompt-friction/SKILL.md) |

From the common parent directory, run `shasum -a 256 -c /path/to/evals/inputs.sha256` to verify the input files.
