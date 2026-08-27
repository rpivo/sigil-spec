# Verification

> Status: stub. Nothing here is decided.

## To specify

- The recovery algorithm, meaning how a verifier searches for and extracts an attestation.
- The verification algorithm, meaning how it checks signature, content anchor, and
  revocation state.
- The result model, and specifically the requirement that "no signature found" is reported
  distinctly from "not AI generated". See [../00-overview.md](../00-overview.md).
- Partial and degraded results. What a verifier reports when it recovers a damaged carrier.
- Failure behavior. Fail closed per R7.

## Reporting guidance

This section should carry normative language on how results are presented to end users, not
only how they are computed. The likeliest real-world harm from this protocol is an
implementation that renders "unsigned" as "human made". Specifying the computation while
leaving presentation unconstrained would leave the most consequential failure mode
unaddressed.
