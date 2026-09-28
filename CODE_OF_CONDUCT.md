# Code of conduct

## Our stance

This project makes claims about safety enforcement, and those claims are
only worth something if the people writing the code and reviewing it are
straight with each other. So the expectation here is narrow and specific:
**be kind, be specific, and be honest about what you did and did not
verify.**

That last one is not an aspiration. In a safety protocol, an
overclaimed verification result is the actual harm we are trying to
avoid. It is not a style issue.

## Expected behaviour

- **Report honestly.** If a test does not pass, say it does not pass. If
  a number is not reproducible, say so and correct the document rather
  than the expectation.
- **Disclose what you did not check.** "I ran the Python suite; I did not
  run the Rust one" is a useful sentence.
- **Criticise the work, not the person.** "This gate can be skipped when
  the batch size is zero" is helpful. "This is careless" is not.
- **Assume good faith, and correct course rather than escalating.**
- **Respect people's time.** Read the linked issue before proposing a
  change; the answer is often already there.

## Unacceptable behaviour

- Harassment, personal attacks, or sustained disruption.
- Publishing someone's private information without explicit permission.
- Presenting an unverified result as verified — in a pull request, an
  issue, a comment, or a blog post about this project.
- Removing or bypassing a CI check in order to make a build pass,
  without saying so in the change itself.

## Reporting

Report concerns to the maintainers privately through the security
advisory channel or direct contact. Reports are handled confidentially.
We will not name a reporter to the person reported without consent.

## Scope

This applies in all project spaces — repositories, issues, pull
requests, discussions — and when representing the project publicly.

## License

This Code of Conduct is adapted from the
[Contributor Covenant](https://www.contributor-covenant.org/), version
2.1.
