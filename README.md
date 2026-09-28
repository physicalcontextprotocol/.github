# physicalcontextprotocol/.github

Default community health files and the organization profile for
[`physicalcontextprotocol`](https://github.com/physicalcontextprotocol).

GitHub serves the files at this repository's root as defaults across the
whole organization, and renders `profile/README.md` as the org's profile
page.

## What is here

| File | Effect |
|---|---|
| `profile/README.md` | The organization profile page — the pitch, the repo table, the maturity table, and a link to `LIMITATIONS.md` |
| `SECURITY.md` | Default security policy for any repository without its own |
| `CONTRIBUTING.md` | Default contributor guide — expectations that hold org-wide |
| `CODE_OF_CONDUCT.md` | Code of conduct |
| `PULL_REQUEST_TEMPLATE.md` | Default pull request template |
| `ISSUE_TEMPLATE/` | `bug_report.md` and `claim_mismatch.md` |

Every repository in the organization **overrides** `SECURITY.md` and
`CONTRIBUTING.md` with its own. The canonical full security policy lives
in
[`pmcp-spec/SECURITY.md`](https://github.com/physicalcontextprotocol/pmcp-spec/blob/main/SECURITY.md);
the copy here is the org-level fallback.

## What is deliberately NOT here

**No `workflows/` directory.**

There used to be a `workflows/ci.yml` in this location. It was a
monorepo-era pipeline whose every job referenced paths that no longer
exist after the split — `working-directory: pmcp-python`,
`working-directory: pmcp-rust/pmcp-core`, and so on.

That file was removed rather than left in place, because a workflow at
the root of a `.github` repository is not inert: it is a **default
workflow applied to every repository in the organization**. Shipping it
would have rolled a broken pipeline onto all ten repositories the moment
this repository was created. Its monorepo paths could not succeed
anywhere.

Each repository owns its own CI instead, with paths verified against its
actual contents:

| Repository | Blocking checks |
|---|---|
| `pmcp-python` | pytest (3.9–3.12), mypy, ruff, black, bandit, pip-audit, integration, benchmark |
| `pmcp-conformance` | pytest (3.9–3.12), plus a test-count guard |
| `pmcp-rust` | `pmcp-core` build + 43 tests. `pmcp-ledger` is declared non-blocking with its 19 known errors enumerated |
| `pmcp-typescript` | build + type-check + lint. **No test job — there is no test suite** |
| `pmcp-spec` | JSON Schema meta-validation + example round-trip. TLA+ model checking is defined but disabled pending a self-hosted runner |
| `pmcp-safety` | module import check + mock-backend guard |
| `pmcp-servers` | documented-setup smoke test |
| `pmcp-registry` | both aiohttp apps construct, frontend builds |
| `pmcp-labs` | a Python syntax check that is explicitly allowed to fail |

If you add a workflow here, remember it applies to **every** repository
in the organization. That is almost never what you want.

## Adding a default

Before adding an org-wide default, check whether it is genuinely
org-wide. `CODE_OF_CONDUCT` and a PR template are reasonable defaults.
Anything that runs code in CI is not, and a `SECURITY.md` default that
is vaguer than a repository's own is actively worse than no default —
a reader could mistake a summary for the real policy.
