<!--
  Default pull request template for the physicalcontextprotocol
  organization. Small enough to fill in, specific enough to review.
-->

## What and why

<!-- Which repository(s), and which claim / bug / documented gap does this address? -->
<!-- Citing a line in a README or LIMITATIONS.md is a good pattern. -->

## How you verified it

<!--
  Be specific, and give real numbers. "213 tests pass" is worth nothing
  if it does not reproduce; 178 passing, 10 skipped, and why, is worth a
  lot.
-->

```
# the commands you actually ran, and their output
```

## What you did NOT verify

<!--
  Required. This is the part reviewers most need and contributors most
  often leave out. "I only ran the Python suite" is a perfectly good
  answer.
-->

## Anything left for a follow-up

<!-- Including anything you deliberately did not do. -->

## Checklist

- [ ] I updated the CHANGELOG for the repository I touched.
- [ ] I did not add `|| true`, `continue-on-error`, or any other construct
      whose only effect is to make a failing check look like it passed.
      If a check is genuinely non-blocking, it is declared non-blocking
      with a comment saying why.
- [ ] No gate is weakened. The order E-Stop → Lease → Constitution →
      Shadow is normative, and it is unchanged.
- [ ] New error paths fail **closed**. Nothing converts a refusal into a
      pass, and nothing swallows an exception into a "looks safe" verdict.
- [ ] Any README or CHANGELOG statement I changed matches what I
      actually observed.
- [ ] This is not a security vulnerability. (If it is, close this and
      use **Security → Report a vulnerability** instead.)
