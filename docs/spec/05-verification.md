# Verification

> Status: stub. Nothing here is decided.

## To specify

- The recovery algorithm, meaning how a verifier searches for and extracts an attestation.
- The verification algorithm, meaning how it checks signature, content anchor, and
  revocation state.
- The result model. "No signature found" must be reported distinctly from both "not AI
  generated" and "suspect". Absence is uninformative in both directions and the result model
  has to make that structurally difficult to misread. See
  [../00-overview.md](../00-overview.md) and
  [../03-threat-model.md](../03-threat-model.md).
- Partial and degraded results. What a verifier reports when it recovers a damaged carrier.
- Failure behavior. Fail closed per R7.

## Reporting guidance

This section should carry normative language on how results are presented to end users, not
only how they are computed. Two symmetrical failures need constraining, and the second is
likelier to cause real harm at scale:

1. Rendering "unsigned" as "human made", which asserts a guarantee the protocol never gave.
2. Rendering "unsigned" as "suspect", which penalizes the entire body of work recorded before
   the protocol existed, along with everyone still on hardware that cannot attest.

Specifying the computation while leaving presentation unconstrained would leave the most
consequential failure mode unaddressed.
