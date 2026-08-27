# Payload

> Status: stub. Nothing here is decided.

What an attestation contains.

## To specify

- **Encoding.** Candidate considerations: CBOR for compactness given the capacity pressure
  described in [../04-requirements.md](../04-requirements.md) R12, versus COSE for
  alignment with existing signed-object tooling.
- **Claim vocabulary.** Strong argument for aligning with the DDEX AI disclosure categories
  (AI vocals, AI instrumentation, AI composition, AI post-production, AI lyrical content) so
  a Sigil attestation can make an existing DDEX declaration verifiable rather than
  introducing a competing taxonomy. See [06-interop.md](06-interop.md).
- **Signer identity.** How a signer is named, and how that maps to key lookup.
- **Timestamp.** Whether to include one, and whether it needs to be independently attested.
- **Versioning.** Required by R11 for algorithm agility.
- **Content anchor.** The hardest part. See [../03-threat-model.md](../03-threat-model.md).

## Constraint

The total payload budget is set by what the carrier can hold at the required
imperceptibility and robustness. That budget is unknown and should be measured before this
section is drafted, since it may rule out design options outright.
