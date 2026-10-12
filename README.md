# TECH-EXP-003: Reviewing task transcripts with an LLM

A small, openly documented experiment using synthetic Technocore-style task transcripts and the OpenAI Responses API. The user ran the requests locally; the assistant helped design the cases, code and analysis.

## What was tested

72 requests across three development pilots, each with 24 cases: arithmetic, sorting and missing evidence; correct/incorrect answers; plain transcripts, different-sender vouching, self-vouching and appended reviewer instructions.

Model: `gpt-4.1-mini-2025-04-14`. Temperature: 0. No live Technocore writes, cryptographic signatures or FLOP inference sessions were tested.

## What happened

| Pilot | Verdict agreement with development labels | Change |
|---|---:|---|
| v1 | 19/24 | Initial task wording and combined correctness judgment |
| v2 | 23/24 | Explicit required response when evidence is missing |
| v3 | 24/24 | Separate substantive correctness and reviewer-instruction judgments |

The first run approved an unsupported population answer twice. Its plain control also failed, so this does not establish injection causation. Other mismatches exposed scoring ambiguity. In v3, all six active reviewer instructions were detected with no false flags among 18 clean cases.

The vouching-causes-false-approval hypothesis was not demonstrated. The final reviewer was revised using these same examples: 24/24 is development performance, not held-out validation. This is not a Technocore vulnerability, FLOP integration, novel-method claim or airdrop eligibility claim.

## Inspect and reproduce

- [Full report](REPORT.md): methods, observations, corrections and limitations.
- [Audit](audit.json): run metadata, token counts and integrity checks.
- `pilots/v1`, `pilots/v2`, `pilots/v3`: datasets, answer keys, archived runners and exact request/response evidence.

From `pilots/v3`, with Python and your own locally configured OPENAI_API_KEY:

```bash
python3 run_benchmark_v3.py --cases cases-v2.json --dry-run --limit 24
```

Remove `--dry-run` for 24 billable requests. Credentials are never stored in the results. Do not commit keys or env files.

## Evidence boundary

The evidence records exact requests, raw response bodies, request IDs, UTC timestamps, usage, latency and hashes. It supports inspection and rerunning the reported method. It is user-recorded evidence, not independent cryptographic certification by OpenAI. A separate one-request smoke test is excluded from the 72 benchmark requests.

## Practical takeaway

Track substantive answer correctness separately from attempted reviewer manipulation. Detecting an instruction does not by itself show the model followed it; a correct answer does not make its appended instructions safe to execute.
