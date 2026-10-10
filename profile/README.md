<!--
  This is the organization profile for `physicalcontextprotocol`.
  GitHub renders profile/README.md at https://github.com/physicalcontextprotocol

  Two deliberate choices in the copy below:

    1. The pitch leads with what PCP does, not with what it is.
    2. LIMITATIONS.md is linked in the first screen, not buried. If a
       reader only reads one thing here, it should be the list of
       things we have NOT proven.
-->

# Physical Context Protocol

**An MCP-compatible protocol for commanding physical robots, where every
actuation passes a safety pipeline that an AI agent cannot bypass.**

PCP is a thin protocol layer. Any MCP-compatible client — Claude, GPT,
Grok, Gemini, a local Ollama — can command robots through the standard
MCP `tools/call` interface, and the safety gates live on the robot side of
the wire, not in the model's good intentions:

```
Any LLM (Claude · GPT · Grok · Gemini · Ollama)
              │  Standard MCP JSON-RPC 2.0
              ▼
    ┌──────────────────────────────────────────────────┐
    │           PCP Server (robot-side)              │
    │                                                  │
    │  tools/list   → Robot actuations                 │
    │  tools/call   → E-Stop → Lease → Constitution →  │
    │                  Shadow → Execute                │
    │  resources/*  → Live sensor streams              │
    │  shadow/*     → Pre-flight simulation            │
    │  lease/*      → Zone ownership                   │
    └──────────────┬───────────────────────────────────┘
                   │  Safety Pipeline
       E-Stop (latch) → Lease → Constitution → Shadow → Execute
                   │
          Physical Robot Hardware
```

The gate order is normative: **E-Stop → Lease → Constitution → Shadow**.
E-Stop is a real actuation-blocking latch, not an advisory flag another
gate has to remember to check.

## Three peer SDKs, not one implementation and two bindings

This is the reason the project is split across ten repositories rather
than one. The Python, TypeScript, and Rust SDKs are independent
implementations of the same wire format, all validated against a single
shared conformance suite and a single JSON Schema. None of them is the
reference implementation, and none of them calls the others.

| | |
|---|---|
| **Specification** | [`pcp-spec`](https://github.com/physicalcontextprotocol/pcp-spec) — protocol, JSON Schema, TLA+ models, and [`LIMITATIONS.md`](https://github.com/physicalcontextprotocol/pcp-spec/blob/main/LIMITATIONS.md) |
| **Python SDK** | [`pcp-python`](https://github.com/physicalcontextprotocol/pcp-python) — 214 tests collected, 213 pass / 1 skip by default |
| **TypeScript SDK** | [`pcp-typescript`](https://github.com/physicalcontextprotocol/pcp-typescript) — skeleton, no test suite yet |
| **Rust SDK** | [`pcp-rust`](https://github.com/physicalcontextprotocol/pcp-rust) — `pcp-core` builds clean, 43 tests pass; `pcp-ledger` does not compile |
| **Conformance** | [`pcp-conformance`](https://github.com/physicalcontextprotocol/pcp-conformance) — 42 tests any implementation must pass |
| **Safety modules** | [`pcp-safety`](https://github.com/physicalcontextprotocol/pcp-safety) — safety loop, TEE attestator, multisig gate, edge hardening |
| **Reference servers** | [`pcp-servers`](https://github.com/physicalcontextprotocol/pcp-servers) — illustrative robot servers |
| **Registry** | `pcp-registry` — **held private**; in-memory TTL discovery with no persistence, no seeds and no auth. Not on the public list until it has all three. |
| **Quarantine** | `pcp-labs` — private, unsupported, reference only |

## What is verified, and what is not

We think the second question matters more than the first, so it gets its
own document and its own link:
**[`LIMITATIONS.md`](https://github.com/physicalcontextprotocol/pcp-spec/blob/main/LIMITATIONS.md).**

It lists what is backed by an actual run — the test counts, the
model-checker results, the five defects that executing code found and
reading it did not — and, separately, the open research problems that
require physical hardware to close:

- Cross-hardware HNN determinism budget for the Shadow gate.
- Conformal-prediction behaviour under adversarial input.
- UWB anchor geometry for robots in motion.

**If Shadow validation, conformal prediction, or UWB localization is
load-bearing for your safety case, validate those components on your own
hardware first.** The lease coordination, gate ordering, and E-Stop
latch are the parts we can stand behind unconditionally today.

## Maturity, stated per repository

Not everything here is finished, and we would rather say so than let you
find out.

| Repository | State | Do not do this |
|---|---|---|
| `pcp-spec` | v0.5, schema v0.6.0, model-checked | — |
| `pcp-python` | 213 of 214 tests pass on a default install; the 1 skip guards a missing-dependency branch | — |
| `pcp-conformance` | 42 tests pass | — |
| `pcp-typescript` | **skeleton** | do not assume parity with Python or Rust |
| `pcp-rust` | `pcp-core` only | do not expect `pcp-ledger` to build |
| `pcp-safety` | TEE and safety loop are **mock-backed** | do not treat either as a security boundary |
| `pcp-servers` | illustrative, no hardware drivers | do not copy one into production without redoing its review |
| `pcp-registry` | **held private** — no auth, CORS `*` on writes, no persistence | not published; see its README |

Each repository's README and CHANGELOG carries the detail. Every
repository has [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
enabled, and the organization-wide policy is in
[`pcp-spec/SECURITY.md`](https://github.com/physicalcontextprotocol/pcp-spec/blob/main/SECURITY.md).

## Contributing

Read [`CONTRIBUTING.md`](CONTRIBUTING.md) in this repository first — it
covers the organization-wide expectations. Then read the target
repository's own `CONTRIBUTING.md`, which is where the local guidance
lives.

The most valuable open contributions, roughly in order:

1. **Making `pcp-typescript` a real implementation** — a test suite
   that runs `pcp-conformance` against it would turn "three peer SDKs"
   from a claim into something you can check.
2. **Getting `pcp-ledger` to compile** — 19 known type errors, all
   enumerated in `pcp-rust/CHANGELOG.md`.
3. **Consolidating `pcp-python`'s duplicate implementations** — there
   are at least four client implementations shipping side by side.
4. **Hardening `pcp-registry`** — auth first, then the SSRF pivot on
   `register`.
5. **A real TEE verification path** in `pcp-safety`, replacing
   `MOCK_QUOTE`.

## License

Apache 2.0 throughout. Every repository carries its own copy of the
license so it stands alone.
