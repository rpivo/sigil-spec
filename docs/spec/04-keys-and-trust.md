# Keys and Trust

> Status: stub. Nothing here is decided.

## To specify

- Signature scheme, with [algorithm agility](../glossary.md#cryptography-and-specification) per R11.
- How a [verifier](../glossary.md#terms-sigil-defines) obtains a [signer](../glossary.md#terms-sigil-defines)'s [public key](../glossary.md#cryptography-and-specification), and how that stays consistent with the
  offline verification requirement (R8).
- Trust model. A [certificate hierarchy](../glossary.md#cryptography-and-specification) resembling [C2PA](../glossary.md#organizations-and-standards-bodies)'s, a registry of known [signers](../glossary.md#terms-sigil-defines), or a
  decentralized model. This is a governance decision as much as a technical one.
- [Revocation](../glossary.md#cryptography-and-specification), and specifically how [revocation](../glossary.md#cryptography-and-specification) checking avoids reintroducing a hard network
  dependency (R10).
- Who is entitled to sign, and how that is administered.

## Note

The governance question is likely to be harder than the cryptography. [C2PA](../glossary.md#organizations-and-standards-bodies)'s [conformance
program](../glossary.md#cryptography-and-specification) exists because a signature is only as meaningful as the process that decides who
holds keys. Any serious version of Sigil needs an answer here, and inheriting an existing
trust framework rather than building one is worth considering seriously.
