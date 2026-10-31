# Agent evaluation

## Scope

`python -m scripts.agent_eval` defines 30 fixture-grounded questions: 20 analytical questions and ten
traps covering investment advice, writes, an invented company, ambiguity, unsupported causation and
out-of-scope requests. Each case checks both an expected phrase and the expected refusal flag.

## Reproducible harness result

Run on 2026-10-28:

```text
provider: fake
validation: scripted-harness-only
cases: 30
traps: 10
accuracy: 100% (30/30)
correct refusals: 10/10
removed claims: 0
tool errors: 0
```

This is a validation of the evaluation code, **not an LLM quality result**. The scripted double returns
the fixture expectation, so its accuracy cannot be compared with a model. Its latency, removed-claim and
tool-error values are likewise not representative of a real agent.

## Real-model status

The real mode is available through `python -m scripts.agent_eval --provider real`. It constructs the
frozen `ShockAgent` interface against the test database and records the same metrics.

## Real-model run (2026-10-31)

This run happened after C4/C5 were merged into `main` (`6ea0b7f`). It used the real Ollama provider, not
the scripted double.

```text
provider: ollama
validation: real-agent
model (planner, writer and advice judge): qwen2.5:7b
model execution: 100% GPU reported by `ollama ps`
GPU: NVIDIA GeForce RTX 4070 Laptop GPU, 8188 MiB
Neo4j: 5.26 test database on bolt://localhost:7688 (45 nodes, 93 relationships)
cases: 30
traps: 10
passed: 7/30 (23.3%)
strict trap score: 2/10
trap refusal flag: 9/10
removed claims: 0
tool errors: 0
mean latency: 4299.158 ms
median latency: 1192.917 ms
p95 latency: 16577.228 ms
range: 56.069-49702.666 ms
```

The seven passing cases were `brent-helios`, `copper-cuprum`, `shock-brent-up`, `shock-brent-air`,
`path-brent`, `trap-advice` and `trap-sell`. The strict trap score is lower than the refusal count because
the scorer requires both `refused == true` and one expected phrase in the answer. Nine traps were refused,
but only the two direct advice refusals contained the exact expected wording. `trap-ambiguous` was not
refused: it continued with a clarification-style response and therefore failed this contract.

No tool call reported an error. The low score instead comes mainly from scope/refusal wording and answer
content that did not contain the fixture's expected token. The first question was a cold start and took
49.7 s; the median is therefore more representative than the mean for the warmed model.

`removed_claims == 0` means that this run did not exercise a writer claim subsequently removed by C5. It
does **not** prove an absence of hallucinations. The current JSON report stores scores, refusal flags,
latencies, removed-claim counts and tool-error counts, but not the full answers or traces. Consequently,
hallucinations and unjustified claims cannot be audited retrospectively from this run and no value is
invented for them. A follow-up evaluator should persist redacted answers, claims, evidence and traces before
claim-level rates are published.

The generated `agent-eval-results-real.json` is the local evidence for this run and is intentionally not
committed.
