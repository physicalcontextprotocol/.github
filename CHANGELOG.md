# Changelog

Organisational profile changes — the repositories in this org, and the
rules that govern them. Per-component history lives in each repository's
own `CHANGELOG.md`.

## [Unreleased]

Nothing yet. The first entry lands when the v1.0.0 repositories are
published.

## 2026-09-28 — Pre-launch preparation

Ten independent repositories prepared as release candidates. No GitHub
repositories existed yet at this point; this entry describes local
preparation only.

### Repositories prepared

| Repository | Visibility at launch | State |
|---|---|---|
| `pcp-spec` | public | schema meta-validation + examples pass; TLA+ checked, mutants rejected |
| `pcp-python` | public | 214 collected, 213 passed, 1 skipped |
| `pcp-typescript` | public | **does not compile** — disclosed, not fixed |
| `pcp-rust` | public | `pcp-core` 43 passed; `pcp-ledger` **does not compile** — disclosed |
| `pcp-conformance` | public | 42 passed, 41 skipped |
| `pcp-safety` | public | policy evaluation, static analysis, audit tooling |
| `pcp-servers` | public | reference servers |
| `pcp-registry` | **held private** | in-memory TTL service discovery — no persistence, no seeds, no auth. Not published. |
| `pcp-labs` | **private, permanently** | quarantined, unsupported, no public channel |
| `.github` | public | org profile, issue templates, CI docs, security policy |

### Python test count

`pcp-python` is **214 collected, 213 passed, 1 skipped** on a default
`pip install -e ".[dev,numerics]"`, reproducible across runs. The one skip
is `test_hnn_gate.py:208`, which only runs when torch/numpy are absent.

The count moved between drafts — 187, then 214 — so it is now stated as
collected *and* passed *and* skipped, with the command to reproduce it,
everywhere it appears. An intermediate draft also recorded
"187 collected, 178 passed, 10 skipped", which was arithmetically
impossible (178 + 10 = 188 outcomes from a 187-test collection) and has
been removed. `LIMITATIONS.md` records the episode in full.
- **TypeScript and `pcp-ledger` do not compile** — 32 and 19 errors respectively. This
  was previously hidden behind a non-blocking CI step. It is now a
  declared, disclosed failure rather than a silent pass.

### Formal methods

The TLA+ CI job had been permanently disabled behind `if: ${{ false }}`,
on the stated grounds that TLC "needs a JDK" and the sweep was "slow
enough to need a real runner". Both were wrong — TLC is a single jar and
the four-model sweep finishes in about a second. The job now runs on a
GitHub-hosted runner with a pinned, checksum-verified toolchain, and
fails unless TLC *rejects* the two deliberately mutated specs.

The models explore 10 and 8 distinct states. "Verified" here means the
specs are internally consistent and the checker is not passing
vacuously — **not** that real-robot behaviour is proved.

### pcp-registry is held private

The launch plan had assumed pcp-registry was a public registry seeded
with reference servers. It is an in-memory TTL discovery service: no
persistence, no seed data, no auth, `CORS *` on writes. Everything
registered vanishes on restart and nothing is curated. It is **held
private** for now rather than published under a description it does not
match. It becomes public when it has persistence, then seeds, then auth.

### Version tags are per-repository

The publishable packages were bumped from 0.5.0 to 1.0.0, so `v1.0.0` is
accurate for them. `pcp-spec` is a different artifact and stays at
**v0.6.0**, matching the version its JSON Schema carries. A tag that
disagrees with the artifact is a small lie that somebody will notice.

### TypeScript error count

`pcp-typescript` has **32** compile errors, not 19 — the 19 belonged to
`pcp-ledger`. The cause of the stale figure is worth knowing: with no
local `node_modules`, `npx tsc --noEmit` prints a decoy banner and exits
0, so grepping its output for `error TS` returns zero and a compile
failure reads as a clean build.

### Security

- `gitleaks` clean across all ten repositories, with one narrow
  allowlist for a `shadowToken` documentation placeholder in
  `pcp-spec`.
- The monorepo-era `workflows/ci.yml` is deliberately **not** published
  as the org's default workflow: it would attempt to run in every
  repository in the org, including ones it has nothing to do with.
- No component repository ships a default-workflow override; the org
  default is intentionally empty.
