# Existing Recordings

> Status: draft. Non-normative.

Everything recorded before this protocol exists is unsigned, and will stay unsigned unless
something is done about it. That population is large, culturally significant, and permanent.
This document covers what can honestly be claimed about it.

## Three classes of attestation

The central point: **claims about content the [signer](glossary.md#terms-sigil-defines) did not capture are a different kind of
claim, and must not share a representation with claims about content it did.**

| Class | Assertion | Strength |
|---|---|---|
| **[Capture attestation](glossary.md#provenance-and-content-authenticity)** | A device produced these samples at this time. | Strong. Bound to hardware, made at the moment of capture. |
| **[Timestamp attestation](glossary.md#cryptography-and-specification)** | This content existed no later than this date. | Strong where the timestamp is independently verifiable and was actually made at the time. |
| **[Provenance assertion](glossary.md#cryptography-and-specification)** | A party believes something about content it did not witness. | Weak. Reduces entirely to trust in that party and to whatever corroboration is offered. |

A [verifier](glossary.md#terms-sigil-defines) that renders all three identically lets the weakest inherit the credibility of the
strongest. Any result model has to keep them distinct. See
[spec/05-verification.md](spec/05-verification.md).

## Retroactive attestation

It is tempting to close the back catalogue gap by attesting to existing recordings after the
fact: older work is known not to be generated, so mark it accordingly.

The mechanism is not cryptographic. Nothing distinguishes a retroactive signature on a genuine
1973 master from one on material generated last week to resemble it. Both verify. The
separation has to come from process.

**That is a normal and workable arrangement.** Certificate authorities, notaries, land
registries, and museum provenance departments all vouch for things they did not witness, on
the basis of documented evidence standards, accreditation, auditability, and liability.
Assay offices have marked precious metal on this model since the 1300s, forging the mark is a
criminal offence, and compulsory hallmarking remains law in the UK. Music already operates
registries of this kind through [ISRC](glossary.md#identifiers-and-file-formats) allocation.

So a governing body administering retroactive [attestation](glossary.md#terms-sigil-defines) is a reasonable design. Two
consequences follow, and neither is a reason to abandon it.

### The governance becomes the product

If a body performs this work, the protocol's value in this area rests on that body's evidence
standards rather than on its cryptography, which merely records the decisions. Establishing
such a body is a substantially larger undertaking than publishing a specification, and is
better sequenced after something is running. See [spec/04-keys-and-trust.md](spec/04-keys-and-trust.md).

### Process evaluates evidence, it does not create it

Where corroboration exists, a body has real material to assess: ISRC registration, copyright
deposit, physical pressings, distribution and chart records. Rigorous process, defensible
outcome.

Where it does not, consider what the body actually does. Presented with an undocumented demo
said to date from 1997, which is in fact recent material printed to tape, no available step
reliably catches it. Forensic examination of media stock is costly and defeatable, and
documentary evidence can be manufactured. The body either declines the category, leaving the
gap where it was, or accepts weaker evidence, which dilutes the meaning of its mark on the
material where it was rigorous.

The corroborated and uncorroborated cases therefore need separate treatment rather than a
single policy. The residual exploit in the uncorroborated case has no available fix and is
accepted explicitly rather than papered over. See
[03-threat-model.md](03-threat-model.md).

### A practical objection

Embedding a [watermark](glossary.md#provenance-and-content-authenticity) alters the audio, and archival practice is strongly against modifying
preserved masters. Retroactive marking realistically applies to distribution copies rather
than to masters, which raises the question of which version is authoritative.

## Learning from the certificate authority failures

The design here shares a structural property with the Web PKI: **any accredited signer could
vouch for any recording.** That property, not weak cryptography, produced that ecosystem's
worst failures. DigiNotar was compromised in 2011 and issued fraudulent certificates for
Google domains, used against users in Iran, and the company collapsed. Symantec was
progressively distrusted by major browsers over sustained misissuance. Trust degraded to the
least careful accredited party and every relying party inherited the result.

Scale applies to bulk backfill of the existing catalogue, which is a one-time problem of
enormous size and unavoidably requires delegation to labels and archives, with rigour varying
across delegates.

It does not apply to the flow of new claims of historical provenance, which is what the fraud
actually consists of. Genuine archival discoveries number in the hundreds per year against
something on the order of a hundred thousand new tracks per day. Four or five orders of
magnitude separate them, which means the category can be examined case by case, by hand, and
can afford to be. See [#asymmetric-verification](#asymmetric-verification).

There is also an incentive constraint. Where a rightsholder vouches for its own catalogue,
signer and beneficiary are the same party, and disclosure may be commercially unwelcome.
Governance design should separate those roles wherever the structure permits.

### The fix worth borrowing

[Certificate Transparency](glossary.md#cryptography-and-specification) did not attempt to prevent misissuance, which proved impossible.
It made every issuance public and append-only, so misissuance became detectable and
attributable after the fact. Browsers now require it.

Applied here: **every retroactive attestation is logged publicly, permanently, with its
evidence basis and its accredited signer.** A bad vouching cannot be prevented. It can be made
discoverable, patterns of them visible, and each signer's record public. That converts an
unfalsifiable claim into an auditable one.

This uses the same append-only infrastructure as the timestamping proposal below, so the two
are complementary rather than competing.

## Asymmetric verification

The volume disparity between the two categories is not only a risk assessment. It determines
how verification effort should be allocated, and the two modes have entirely different
economics.

| Category | Volume | Verification |
|---|---|---|
| New releases | Enormous | Cheap, automated, cryptographic. Must scale to millions. |
| Claims of historical provenance | Tiny | Expensive, human, evidence-based. Can afford per-case scrutiny. |

A single uniform process for both would either make routine verification unaffordable or make
archival claims trivial to fabricate. The disparity is what permits the asymmetry, and the
asymmetry is what makes both tractable.

Note also that base rates measure frequency rather than harm. One fabricated lost recording by
a significant artist does more damage than a large number of undisclosed routine releases. But
rare and high-stakes is exactly the profile that warrants expensive per-case review, and the
small volume is what makes such review affordable, so the impact weighting supports the same
allocation.

## Timestamping: available now, and stronger

Age cannot be established retroactively. It can be established prospectively, starting
immediately, at low cost.

Publishing content hashes to an append-only public log creates verifiable evidence that
specific recordings existed at a specific date. Twenty years of accumulated entries is not an
assertion by an interested party but a timestamp actually made at the time, auditable by
anyone.

The properties compare favourably with retroactive signing:

- **No modification of the audio.** Hashing does not alter the recording, so archival
  objections do not apply.
- **No forgery oracle.** An [append-only log](glossary.md#cryptography-and-specification) cannot be backdated.
- **No hardware and no industry cooperation.** A single catalogue holder can begin alone.
- **Covers the hard case.** It works precisely for material with no public paper trail.
- **It freezes the exposed window.** Once the log runs, material published afterwards is
  expected to have an entry, so a claim of age with no entry is anomalous rather than merely
  unverified. The fakeable window is therefore fixed at everything predating the log and does
  not grow. Each year of operation makes that fixed window a smaller fraction of all recorded
  music, with nobody exercising judgment. Without a log the pool of plausibly claimable dates
  expands indefinitely.

Established mechanisms exist, including RFC 3161 timestamping and transparency logs in the
style of Sigstore's Rekor. See [07-timestamp-log.md](07-timestamp-log.md) for how the log
works, which material it helps, and why coverage can be measured per catalogue.

**This is actionable independently of the rest of the protocol** and gets more valuable the
earlier it starts, which argues for treating it as a first deliverable rather than a later
one.

## What adoption does and does not resolve

Two limits persist regardless of how widely the protocol is adopted or how much time passes.

**The unsigned population never becomes homogeneous.** Generators that decline to watermark,
including open-weights models run locally, keep producing unmarked audio indefinitely. The
unsigned set therefore always mixes the historical catalogue with the deliberately evasive,
and absence never becomes informative. There is no adoption threshold that changes this. See
[03-threat-model.md](03-threat-model.md).

**A signature never means human.** A capture attestation states that samples passed through a
device. Generated audio played through an [attested capture](glossary.md#provenance-and-content-authenticity) chain produces a valid signature.
Universal adoption makes this claim ubiquitous rather than making it stronger.

The supportable end state is narrower than "AI-generated content becomes identifiable" and is
still a substantial improvement:

> **A widely adopted, auditable record of which parties vouched for what, and on what
> basis.**

That serves accountability and disclosure well. It is not a test for human origin, and time
does not turn it into one.

## Open questions

- Whether retroactive provenance assertions belong in this protocol at all, or whether they
  are better left to existing rights and registration systems.
- What corroborating evidence a provenance assertion should be required to carry, and whether
  the standard differs between corroborated and uncorroborated material.
- Who accredits retroactive signers, and whether an existing body such as an ISRC agency
  could take the role rather than a new institution being founded.
- Whether signer and beneficiary can be separated in practice, given rightsholders hold both
  the catalogue and the strongest interest in its standing.
- What happens to attestations already made when an accredited signer is compromised or
  withdrawn. Mass [revocation](glossary.md#cryptography-and-specification) across a back catalogue is a scenario worth designing for
  rather than discovering.
- Whether timestamping should be specified here or simply recommended, given it is useful
  independently of the [carrier](glossary.md#terms-sigil-defines).
- How a verifier presents an attestation chain mixing all three classes, which will be the
  normal case for catalogue material.
