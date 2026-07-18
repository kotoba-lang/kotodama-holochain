# Holochain Agent Actor Runtime Experiment (retired)

This directory now keeps only the historical payload examples and verification
notes for ADR-2605092600. The former Rust Cargo workspace, Holochain zomes,
Docker artifact build, and call-zome smoke have been removed from this repo as
part of the Kotoba/CLJC migration.

Runtime authority moved to the Kotoba-backed actor model and host-owned
adapters. If Holochain support is reintroduced, it should be added as a fresh
adapter repository instead of reviving this embedded Rust workspace.

Example payloads:

- `examples/langgraph-echo-agent.json`
- `examples/langchain-tool-agent.json`
- `examples/commit-actor-event.json`

Historical pushed image:

```text
ghcr.io/etzhayyim/holochain-agent-actor-runtime:experimental-amd64
digest: sha256:29a1f2f037a31a8ae0518272706368714610b3eba845667e9690834b31a031b0
platform: linux/amd64
```

The scaffold was intentionally not the production runtime. The CLI contract
smoke previously lived in `70-tools/etzhayyim/etzhayyim/`; that tree was removed
2026-05-20 along with the etzhayyim CLI. The embedded Holochain smoke is also
retired here.

VKE runtime smoke, 2026-05-09:

- `mitama-udf/langgraph-server` returned `/healthz` OK and listed `echo`.
- `POST /runs` with actor DID `did:web:holochain-agent-runtime.etzhayyim.com`
  completed as run `be761cd8-0a01-46e4-836f-4709ceabd925`.
- The run output was `echo: holochain langgraph actor smoke 2026-05-09`.
- The Holochain artifact image started in namespace `agent-runtime-holochain`
  after copying `ghcr-creds` there, and listed the zome WASM files plus the
  LangGraph/LangChain example payloads.
- A LangChain actor smoke also ran in `agent-runtime-holochain` using
  `langchain-core.RunnableLambda`, producing a successful tool-call receipt for
  `langchain-tool-agent`.

Proof: `90-docs/proof/holochain-langgraph-agent-runtime-smoke-20260509.json`.
