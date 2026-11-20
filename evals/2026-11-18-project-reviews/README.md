# Project-level defect detection

The first round used one fresh Codex review per condition on each of three projects. The second round repeated the comparison with fresh reviewers, the plain prompt, and the second revision. All conditions used the same user request within each project. The only substantive addition was the instruction to read the supplied skill. Reviewers started from the project root without scenario hints or expected answers, inspected the same files for each project, and had a 900-word reporting cap. Local inspection and in-memory mocks were allowed. No live agents or external services were invoked by the reviewers.

## Inputs

- Plain request for LangChain and Google ADK: “Can you look through this project and tell me what could go wrong with the agent?”
- Plain request for Magentic-UI: “Can you look through this project and tell me what could go wrong when the agent stops and continues?”
- Current skill: [PR #1 at c5273cd](https://github.com/RazGvili/prompt-friction/blob/c5273cdb5fbf2674c71cd81f7adf753afa1edf05/plugins/prompt-friction/skills/prompt-friction/SKILL.md).
- Revised skill: audited the default request, varied one supported condition at a time, then traced runtime exits and effects back to their prerequisites. Apply [first-revision.patch](first-revision.patch) to the PR version to recover the first revision. The SHA-256 of each evaluated version appears in `inputs.sha256`.
- [Open Deep Research at 1b7d2e8](https://github.com/langchain-ai/open_deep_research/tree/1b7d2e80db9faa586165c60e09096dbbfd483a64) was the development project. The snapshot includes the active graph, runtime, configuration, deployment auth module, Python evals, and top-level metadata.
- [Google ADK small-business loan agent at e05958e](https://github.com/google/adk-recipes/tree/e05958ea5c6577c84e04c0f5063fe1e8544f5b00/contrib/python/small-business-loan-agent) and [Magentic-UI OmniAgent at d3c9d13](https://github.com/microsoft/magentic-ui/tree/d3c9d13c39288257286a66daabf7c5b5fb72ee69/src/magentic_ui/teams/omniagent) were held out from drafting. The snapshots contain the ADK recipe's entire application and Magentic-UI's entire OmniAgent package plus configuration docs, approval adapter, and project metadata.
- `inputs.sha256` lists the exact source snapshots. Paths start at the project roots passed to the reviewers. For Google ADK, that root is the recipe directory in the upstream checkout.

## Results

| Project | Plain 1 | PR #1 | Revision 1 | Plain 2 | Revision 2 |
| --- | ---: | ---: | ---: | ---: | ---: |
| Open Deep Research | 6/9 | 5/9 | 7/9 | 5/9 | **8/9** |
| Google ADK loan agent | 5/6 | 5/6 | 6/6 | 5/6 | **6/6** |
| Magentic-UI OmniAgent | 3/4 | 2/4 | 2/4 | 2/4 | **4/4** |
| **Total** | **14/19** | **12/19** | **15/19** | **12/19** | **18/19** |

Revision 2 beat plain review on each project in its paired round. It also exceeded the stronger of the two plain runs on each project. It recovered a required reflection dropped at the supervisor cap, a status inquiry that creates a workflow record, and the two OmniAgent defects missed by plain review in the second round. Every reviewer in these rounds missed the researcher's three-source stopping rule when a user requests more examples.

For spot checks, the original supervisor function discarded a requested reflection at iteration seven and recorded it at iteration six. The recipe's original status function created a record with a Firestore stand-in. OmniAgent's original writer left an older checkpoint on disk after rejecting an oversized replacement, while an in-limit write replaced it. Live provider and full-stack behavior remain outside this test.

## Scoring

Before dispatch, we listed known defects for each scope. A hit required the report to describe the failure and its reachable mechanism. The reviewer could find it independently from the source or reproduce it using a local stand-in. A closely related report that misses the failure receives no credit. Distinct failure mechanisms count once. Evidence formatting has no score. New supported findings are recorded separately because their denominator was unknown before review. No deployment behavior is inferred from a mock alone.

| Project and expected defect | Plain 1 | PR #1 | Revision 1 | Plain 2 | Revision 2 |
| --- | --- | --- | --- | --- | --- |
| Research. Supervisor cap prevents required planning, delegation, or reflection | | ✓ | ✓ | | ✓ |
| Research. Failed worker drops a successful sibling | ✓ | ✓ | ✓ | ✓ | ✓ |
| Research. Control tools bypass the guard when no source is available | ✓ | ✓ | ✓ | ✓ | ✓ |
| Research. Writer receives reflection and overflow errors as evidence | ✓ | | | | ✓ |
| Research. Supervisor counts rounds against an individual-call limit | | | ✓ | | ✓ |
| Research. Completion skips sibling research in the same batch | ✓ | | ✓ | ✓ | ✓ |
| Research. Compression uses wrong provider for error handling | ✓ | ✓ | ✓ | ✓ | ✓ |
| Research. Compression truncation loses the latest evidence | ✓ | ✓ | ✓ | ✓ | ✓ |
| Research. Three-source stop can violate requested coverage | | | | | |
| Loan. Active priced loan can resume at decision before approval | ✓ | ✓ | ✓ | ✓ | ✓ |
| Loan. Existing session ignores a second loan ID | ✓ | ✓ | ✓ | ✓ | ✓ |
| Loan. First status inquiry can create a new record | | ✓ | ✓ | | ✓ |
| Loan. Missing or failed output can persist as completed | ✓ | ✓ | ✓ | ✓ | ✓ |
| Loan. Rejection leaves an active application resumable | ✓ | | ✓ | ✓ | ✓ |
| Loan. Final-answer judge runs after checkpoint side effects | ✓ | ✓ | ✓ | ✓ | ✓ |
| OmniAgent. Round-limit Stop drops pending tool outcomes from history | ✓ | ✓ | ✓ | ✓ | ✓ |
| OmniAgent. Resume mislabels completed, rejected, and unstarted calls | ✓ | ✓ | ✓ | ✓ | ✓ |
| OmniAgent. Oversized write leaves a stale checkpoint for restart | ✓ | | | | ✓ |
| OmniAgent. Bare text fallback can report completion without evidence | | | | | ✓ |

The first revision scored 15/19 versus 14/19 plain and 12/19 current. It missed the oversized checkpoint issue in its final report even though its reviewer identified that issue during the audit. The second revision adds an explicit comparison of in-memory state, persisted state, and actual effects at interruption points. It also instructs the reviewer to include every verified defect in the final report.

The score tracks known defects. The research reviewers sometimes treated omitted packaging files as a build problem. Those files exist upstream and were deliberately absent from the projection. These scope errors received no credit in either condition. The same rule excluded any claims about missing loan test, data, or deployment files. Both approaches independently found genuinely additional source defects. Examples include dynamic tool rediscovery failing dispatch in Open Deep Research, the loan judge lacking authoritative status data, and pause checks happening after a whole batch in OmniAgent. Only the first revision's final OmniAgent report flagged literal answer markup inside a tool argument being parsed as a completed run. Only the current skill's final OmniAgent report flagged a browser-capacity retry ignoring the user's reply. Deployment-dependent security claims were excluded unless the source path and assumptions were established.

Counts are from final reports. A finding in progress notes that does not appear in the final report receives no credit. Repetitions use the same projects, so they measure performance on this suite rather than generalization to another project. The second revision was informed by the first round on all three projects.
