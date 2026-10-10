# Contributing to the physicalcontextprotocol organization

Thanks for your interest. This is an early-stage project and a lot of
what "contributing" means is still settling, so this document is short
on ceremony and specific about what actually helps.

Each repository also has its own `CONTRIBUTING.md` with the local
details — the SDK, the spec, and the safety modules all have different
constraints. Read the one for wherever you are working.

## Before you start

1. **Read [`pcp-spec/LIMITATIONS.md`](https://github.com/physicalcontextprotocol/pcp-spec/blob/main/LIMITATIONS.md).**
   It lists what is verified and, more importantly, what is not. A
   contribution aimed at an open problem in that list is much more
   likely to be useful than one aimed at a solved one.
2. **Check the target repository's README for its maturity.** Several
   repositories here are explicitly incomplete, and their READMEs say
   which parts.
3. **Open an issue before a large change**, especially in
   `pcp-registry`, where the architecture is genuinely undecided.

## Pick the right repository

| If you are working on… | Go to |
|---|---|
| The wire protocol, schemas, TLA+ models | `pcp-spec` |
| The Python SDK | `pcp-python` |
| The TypeScript SDK | `pcp-typescript` |
| The Rust crates | `pcp-rust` |
| Protocol compliance tests | `pcp-conformance` |
| The safety loop, TEE, multisig, edge hardening | `pcp-safety` |
| Example robot servers | `pcp-servers` |
| The fleet registry | `pcp-registry` |
| Anything unowned, experimental, or broken | **nowhere** — open an issue first |

`pcp-labs` is a private quarantine. Do not contribute there and do not
open issues against it. It exists so that material without an owner
stops looking like it has one.

## Cross-repository changes

The repositories are independent, which means a change that spans two of
them is two pull requests. That is deliberate — a spec change and an SDK
change have different review needs and different risk. When they are
related, say so explicitly in both descriptions and link them, so a
reviewer can check them together.

The order is usually spec first, then conformance, then the SDKs. A spec
change with no test that would catch a violation is a change nobody can
rely on.

## What we care about

**Honesty over polish.** If something does not work, say it does not
work. Every README in this organization has a section on known
limitations, and several of those limitations are severe. Keeping those
sections accurate is a contribution, and letting them go stale is a bug
— a security-relevant one for a project whose whole claim is that its
safety properties are enforced rather than asserted.

Concretely, please:

- Say what you actually ran. "213 tests pass" is worth nothing if it
  does not reproduce; 213 passing, 1 skipped, and why, is worth a lot.
- Do not add `|| true`, `continue-on-error`, or any other construct whose
  effect is to make a red check look green. If a check cannot pass yet,
  declare it non-blocking and explain why in a comment.
- Do not weaken a gate. E-Stop, Lease, Constitution, and Shadow are
  normative, in that order. If your change needs a gate bypassed to
  produce output, the example is probably showing something the protocol
  does not permit.
- Make new error paths fail **closed**. A new error path may refuse an
  operation; it may never turn a refusal into a pass, and it may never
  swallow an exception into a "looks safe" verdict.
- Update the CHANGELOG in the same PR that changes behaviour.

## What we do not want

- Defensive wrappers around code you have not read.
- A second implementation of a design that is still undecided.
  Propose it in an issue first.
- Silent renames of public API. If a name changes, the CHANGELOG says
  so, loudly.
- Passing a CI check by removing the check.

## Pull request description

Please include:

1. Which repository(s) you touched.
2. Why the change is needed — which claim, bug, or documented gap it
   addresses. Citing a line in a README or `LIMITATIONS.md` is a good
   pattern.
3. How you verified it, with real numbers. Which commands, which
   versions, which results.
4. Anything you deliberately left for a follow-up.

## Reporting a security issue

Not a pull request. See
[`pcp-spec/SECURITY.md`](https://github.com/physicalcontextprotocol/pcp-spec/blob/main/SECURITY.md).
Private vulnerability reporting is enabled on every repository.

## Code of conduct

Be kind, be specific, and be honest about what you did and did not
verify. That last one is not just politeness here — an overclaimed
verification result in a safety protocol is the actual harm we are
trying to avoid.

## License

By contributing, you agree that your contribution is licensed under the
Apache License 2.0, matching the rest of the organization.
