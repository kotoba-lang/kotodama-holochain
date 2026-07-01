# Holochain Agent Actor Runtime Payloads

This directory now keeps only the payload examples and historical notes from the
ADR-2605092600 Holochain agent actor runtime experiment. The in-repository Rust
zome scaffold and Docker build path were removed.

The former experiment modeled a etzhayyim actor cell as:

- DNA: `agent_actor_runtime`
- Role: `agent_actor_runtime`
- Coordinator zome: `actor_runtime`
- LangGraph registration: `register_langgraph_actor`
- LangChain registration: `register_langchain_actor`
- Run start: `start_graph_run`
- Command/event commit: `commit_actor_event`
- Query/signal function: `latest_actor_head`, `list_actor_events`

The hApp does not execute Python LangChain/LangGraph code in the zome. It commits
the actor definition and run/event receipts in a Holochain cell, while execution
remains in the existing Python runtime and query projection remains in
RisingWave.

Example payloads:

- `examples/langgraph-echo-agent.json`
- `examples/langchain-tool-agent.json`
- `examples/commit-actor-event.json`

The scaffold was intentionally not the production runtime. The CLI contract
smoke previously lived in `70-tools/etzhayyim/etzhayyim/`; that tree was removed
2026-05-20 along with the etzhayyim CLI. Until the Holochain plan is re-ported
(target: `e7m agent holochain-plan` or similar), the smoke is unavailable:

```sh
# (former) cd 70-tools/etzhayyim/etzhayyim
# (former) go test . -run Holochain
# (former) go run . agent-runtime holochain-plan \
  --agent-did did:web:kami-agent.etzhayyim.com \
  --happ-uri ipfs://bafy-happ \
  --dna-hash uhC0kagentactorruntime
```

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
