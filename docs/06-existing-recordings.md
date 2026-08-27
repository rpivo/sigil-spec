# Existing Recordings

> Status: draft. Non-normative.

Everything recorded before this protocol exists is unsigned, and will stay unsigned unless
something is done about it. That population is large, culturally significant, and permanent.
This document covers what can honestly be claimed about it.

## Three classes of attestation

The central point: **claims about content the signer did not capture are a different kind of
claim, and must not share a representation with claims about content it did.**

| Class | Assertion | Strength |
|---|---|---|
| **Capture attestation** | A device produced these samples at this time. | Strong. Bound to hardware, made at the moment of capture. |
| **Timestamp attestation** | This content existed no later than this date. | Strong where the timestamp is independently verifiable and was actually made at the time. |
| **Provenance assertion** | A party believes something about content it did not witness. | Weak. Reduces entirely to trust in that party and to whatever corroboration is offered. |

A verifier that renders all three identically lets the weakest inherit the credibility of the
strongest. Any result model has to keep them distinct. See
[spec/05-verification.md](spec/05-verification.md).

## Retroactive signing and the forgery oracle

It is tempting to close the back catalogue gap by signing existing recordings after the fact:
older work is known not to be generated, so mark it accordingly.

The difficulty is that **a capability to retroactively sign unattested content is a
capability to sign anything.** Nothing cryptographically distinguishes a retroactive
signature on a genuine 1973 master from one on material generated last week to resemble it.
Both verify. The only separation is the judgment of the key holder, which means retroactive
attestation is a statement about the signer rather than about the recording.

This does not rule the practice out. It establishes that a retroactive claim must be
represented as a provenance assertion, must carry the basis on which it is made, and must
never be presented to an end user as equivalent to a capture attestation.

### Where corroboration exists

Commercially released material has a genuine, auditable evidence trail that no party to the
protocol controls: ISRC registration, copyright deposit, physical pressings, chart and
distribution records. A provenance assertion that cites such evidence can be checked
independently, which makes it considerably stronger than bare assertion.

### Where it does not

Unreleased material, demos, session tapes, field recordings, and personal archives have no
comparable public record. This is where a retroactive signature would carry the most weight
and warrant the least, and any policy on retroactive attestation should treat corroborated
and uncorroborated claims separately.

### A practical objection

Embedding a watermark alters the audio, and archival practice is strongly against modifying
preserved masters. Retroactive marking realistically applies to distribution copies rather
than to masters, which raises the question of which version is authoritative.

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
- **No forgery oracle.** An append-only log cannot be backdated.
- **No hardware and no industry cooperation.** A single catalogue holder can begin alone.
- **Covers the hard case.** It works precisely for material with no public paper trail.

Established mechanisms exist, including RFC 3161 timestamping and transparency logs in the
style of Sigstore's Rekor.

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
device. Generated audio played through an attested capture chain produces a valid signature.
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
- What corroborating evidence a provenance assertion should be required to carry.
- Whether timestamping should be specified here or simply recommended, given it is useful
  independently of the carrier.
- How a verifier presents an attestation chain mixing all three classes, which will be the
  normal case for catalogue material.
