<!--
  Default security policy for the `physicalcontextprotocol` organization.
  GitHub serves this from the `.github` repository to every repository in
  the organization that does not have its own SECURITY.md.

  Repositories that DO have their own SECURITY.md (all of pmcp-python,
  pmcp-typescript, pmcp-rust, pmcp-conformance, pmcp-safety, pmcp-servers,
  pmcp-registry, pmcp-labs, and pmcp-spec) override this file. pmcp-spec's
  copy is the canonical full version.
-->

# Security policy

## Reporting a vulnerability

**Do not open a public GitHub issue for a suspected vulnerability.**

Every repository in this organization has **private vulnerability
reporting** enabled, so you can submit a report through the GitHub UI
without an email address and without making anything public:

1. Open the affected repository.
2. Click **Security** → **Report a vulnerability**.

That is the same form the maintainers read first, and it keeps the
discussion private until a fix exists.

If private reporting is unavailable to you, fall back to an
[organization security advisory](https://github.com/physicalcontextprotocol/security/advisories/new)
rather than a public issue.

Please include a description and impact, reproduction steps (ideally a
minimal proof-of-concept), affected versions or repositories, and your
suggested severity if you would like to propose one.

## Response targets

| Stage | Target |
|---|---|
| Acknowledgement of a valid report | 5 business days |
| Fix or mitigation, high severity | 90 days |
| Fix or mitigation, actively exploited | as fast as we can safely ship |
| Credit | offered in the advisory, unless you prefer otherwise |

## Supported versions

| Repository | Supported | Notes |
|---|---|---|
| `pmcp-spec` | yes | v0.5 protocol line, JSON Schema v0.6.0 |
| `pmcp-python` | yes | v0.5 line |
| `pmcp-typescript` | best-effort | v0.5, skeleton SDK — no test suite yet |
| `pmcp-rust` | best-effort | `pmcp-core` only; `pmcp-ledger` does not compile |
| `pmcp-conformance` | best-effort | v0.5 |
| `pmcp-safety` | best-effort | TEE attestator and safety loop are mock-backed |
| `pmcp-servers` | best-effort | illustrative only, no hardware drivers |
| `pmcp-registry` | **not for untrusted networks** | no auth, CORS `*` on write verbs, no body-size limit |
| `pmcp-labs` | **no** | private quarantine; no support, no guarantees |

Anything marked "best-effort" has provisional security guarantees until
it carries its own supported-version table.

## In scope

- Correctness or safety-pipeline bypasses in the protocol or the
  shipping v0.5 code — most importantly any way to skip or reorder the
  **E-Stop → Lease → Constitution → Shadow** gates.
- Auth, authorization, injection, deserialization, resource-exhaustion
  or SSRF issues in network-facing components, especially
  `pmcp-registry`.
- Supply-chain issues in the SDKs.
- Leaked secrets or credentials in any repository in this organization.
- **Overclaiming.** If a document in this organization asserts something
  is more verified than it is — in particular anything in
  [`LIMITATIONS.md`](https://github.com/physicalcontextprotocol/pmcp-spec/blob/main/LIMITATIONS.md)
  that no longer matches reality — that is a real report and we will
  treat it as one.

## Out of scope

- Clearly labelled placeholder or mock code. `pmcp-safety`'s
  `tee-attestator` ships `MOCK_QUOTE` and its safety loop defaults to
  `--simulator mock`. These are documented limitations, not
  vulnerabilities.
- Examples and demos under `pmcp-servers/examples/`.
- Anything in `pmcp-labs` — unsupported by policy, and not public.
- Docker and compose files under `pmcp-labs/infra/` — they reference
  pre-split paths and do not build.
- Missing hardening that a component's README already names. Reporting
  it is still welcome, but it will be triaged as an enhancement rather
  than a vulnerability.
- Attacks requiring an attacker to already hold the `release` environment
  or a maintainer account.

## Read before relying on this project

[`LIMITATIONS.md`](https://github.com/physicalcontextprotocol/pmcp-spec/blob/main/LIMITATIONS.md)
lists the open research problems that require physical hardware to close.
If Shadow validation, conformal prediction, or UWB localization is
load-bearing for your safety case, validate those specific components on
your own hardware first.
