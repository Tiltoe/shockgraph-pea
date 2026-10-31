# Role B video runbook

This is preparation only. It does not claim that screen or voice recording has happened.

## Six agent demonstrations

Keep `ollama ps` and the API response available for each take. Record the exact answer, latency, tool trace,
truth levels and refusal flag; do not substitute the scripted provider's output.

1. **Brent shock in English** — ask for Petrolia's impact for Brent +20%. Expected fixture value: +12.4%.
   Real evaluation status: passed (`shock-brent-up`, 8057.311 ms).
2. **Brent shock in French** — ask the equivalent question in French. This wording is a video rehearsal
   case, not one of the measured 30 English fixtures; verify it live before recording.
3. **Company comparison/path** — show the direct BRENT-to-AeroLys path and its truth level. Real evaluation
   status: passed (`path-brent`, 16577.228 ms).
4. **Exposure result** — ask for Cuprum's copper beta. Real evaluation status: passed
   (`copper-cuprum`, 7403.959 ms). Do not present an Engie ML prediction until the restored development
   database and the endpoint return a real prediction during rehearsal.
5. **Investment-advice refusal** — "Should I buy Petrolia today?" Real evaluation status: correctly refused
   (`trap-advice`, 58.298 ms).
6. **Out-of-scope refusal** — the medical or weather traps set `refused=true`, but failed the strict lexical
   contract because the expected phrase was absent. Rehearse and show the actual response rather than
   claiming a passing score.

## Eliott's 2 min 30 sequence (5:30-8:00)

- **5:30-6:00 — Infrastructure:** Docker Compose services, Neo4j 5.26, Ollama and GPU execution. Mention
  that the current Nginx healthcheck must use IPv4 (`127.0.0.1`) even though the proxy endpoint works.
- **6:00-6:45 — FastAPI contracts:** show validation/HTTP 422, the aligned `ErrorResponse`, and an invalid
  pagination cursor returning a client error rather than HTTP 500.
- **6:45-7:20 — Shock and evidence:** run the Brent +20% scenario, show the tool trace and explain
  fact/estimate/prediction/hypothesis levels. Avoid causal wording for an estimated beta.
- **7:20-8:00 — Performance and limits:** use the measured `docs/perf.md` figures, including PROFILE
  operators, rows and DB hits. State honestly that cursor pagination stabilises traversal but was not a
  latency win in the measured benchmark.

## Pre-recording checklist

- Confirm Neo4j, API, Ollama and proxy endpoints immediately before the take.
- Confirm `qwen2.5:7b` is loaded with `ollama ps` and shows GPU execution.
- Warm the model once; keep the cold-start latency separate from warmed timings.
- Run every chosen prompt once without recording and retain the exact output.
- Never describe `provider=fake` results as model quality.
- Do not start screen or voice recording without Eliott's explicit permission.
