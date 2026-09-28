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
| `pmcp-spec` | public | schema meta-validation + examples pass; TLA+ checked, mutants rejected |
| `pmcp-python` | public | 178 passed, 10 skipped, 187 collected |
| `pmcp-typescript` | public | **does not compile** — disclosed, not fixed |
| `pmcp-rust` | public | `pmcp-core` 43 passed; `pmcp-ledger` **does not compile** — disclosed |
| `pmcp-conformance` | public | 42 passed, 41 skipped |
| `pmcp-safety` | public | policy evaluation, static analysis, audit tooling |
| `pmcp-servers` | public | reference servers |
| `pmcp-registry` | public after seeding | in-memory TTL service discovery — **not a curated registry**, see below |
| `pmcp-labs` | **private, permanently** | quarantined, unsupported, no public channel |
| `.github` | public | org profile, issue templates, CI docs, security policy |

### Verification claims corrected

- The **"213/213 Python tests"** figure did not reproduce in any run. The
  real result is **178 passed, 10 skipped (187 collected)**. The wrong
  number had propagated into `LIMITATIONS.md`, the org profile README, the
  Python `CHANGELOG.md`, and the launch copy. All corrected.
- **TypeScript and `pmcp-ledger` do not compile** (19 errors each). This
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

### Security

- `gitleaks` clean across all ten repositories, with one narrow
  allowlist for a `shadowToken` documentation placeholder in
  `pmcp-spec`.
- The monorepo-era `workflows/ci.yml` is deliberately **not** published
  as the org's default workflow: it would attempt to run in every
  repository in the org, including ones it has nothing to do with.
- No component repository ships a default-workflow override; the org
  default is intentionally empty.
