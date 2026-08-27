# Soft Binding and Signal-Carried Attestation

> Status: draft. Non-normative.

The two approaches are much closer than early drafts of this repository implied. This document
sets out what they actually share, what genuinely separates them, and why the distinction may
not survive contact with the capacity measurement.

## The mechanical difference, in one sentence

Both embed something imperceptible in the signal. A **[soft binding](glossary.md#provenance-and-content-authenticity)** embeds an identifier that
points to a [manifest](glossary.md#provenance-and-content-authenticity) held elsewhere. A **signal-carried [attestation](glossary.md#terms-sigil-defines)** embeds the attestation
itself.

That is the entire difference. Everything below follows from it.

## What they share

Nearly all of the hard part.

- The same [carrier](glossary.md#terms-sigil-defines) technology, meaning imperceptible embedding in audio or video.
- The same imperceptibility engineering and the same [perceptual masking](glossary.md#audio-and-video-production) constraints.
- The same robustness requirements against transcoding, loudness normalisation, and format
  conversion.
- The same behaviour under adversarial removal. Neither survives an attacker willing to
  degrade the file.
- The same dependence on trusting whoever signed.

If the carrier is the hard problem, the two approaches are the same project.

## The claim this repository overstated

Earlier text presented **offline verification** as the clean advantage of signal-carried
attestation. That is weaker than it appeared.

A signal-carried attestation still requires the [signer](glossary.md#terms-sigil-defines)'s [public key](glossary.md#cryptography-and-specification) and current [revocation](glossary.md#cryptography-and-specification)
state. If either must be fetched at verification time, a network dependency has been
reintroduced. R10 in [04-requirements.md](04-requirements.md) flags exactly this and does not
resolve it.

So "verifies offline" is conditional on key distribution being solved, rather than being a
property that falls out of the design.

## What actually differs

Set the overstatement aside and the real distinction is not about the network. It is about
**what has to exist, and for how long**.

The crisp version:

> Soft binding requires infrastructure proportional to the number of **assets**.
> Signal-carried requires infrastructure proportional to the number of **signers**.

Key material is few, long-lived, cacheable, and can ship with the verifying software. A
manifest repository is per-asset, unbounded, and must be maintained indefinitely. Those are
very different operational commitments, and four consequences follow.

1. **No per-asset repository has to exist.** Nothing must be provisioned, funded, or operated
   in proportion to catalogue size.
2. **No single point of failure.** If a repository is decommissioned in year twelve, every
   soft-binding claim on every file becomes unverifiable. The files survive. The proof does
   not. For archives, whose entire concern is longevity, this is a serious objection.
3. **The claim cannot be altered after signing.** Whoever operates the repository controls
   what the manifest says and can change or withdraw it. A signal-carried attestation is
   fixed when signed.
4. **Verification leaks nothing.** Every soft binding resolution tells the repository operator
   that someone is checking a specific file, which is a surveillance surface that local
   verification does not create.

## Where soft binding is genuinely better

This is not a one-sided comparison, and the advantages below are real.

- **Expressiveness is unbounded.** A manifest can carry a full edit history, multiple
  assertions, thumbnails, and arbitrary detail. A signal-carried attestation is capped at
  whatever the carrier holds, which is likely to be on the order of a hundred bytes.
- **Claims can be corrected or extended after the fact.** Mutability is a liability against
  tampering and an asset when an error needs fixing or a later assertion needs adding.
- **It already exists.** [C2PA](glossary.md#organizations-and-standards-bodies) defines soft bindings, maintains an approved algorithm list, and
  publishes a [Soft Binding Resolution API](glossary.md#provenance-and-content-authenticity). Adopting it means joining something rather than
  building something.

## Side by side

|  | Soft binding | Signal-carried |
|---|---|---|
| Carrier payload | Identifier or pointer | Full attestation |
| Approximate payload size | Tens of bits | ~128 bytes |
| Per-asset repository required | Yes, indefinitely | No |
| Key distribution required | Yes | Yes |
| Infrastructure scales with | Number of assets | Number of signers |
| Claim alterable after signing | Yes, by the repository operator | No |
| Verification observable by a third party | Yes | No |
| Expressiveness of the claim | Unbounded | Capped by carrier capacity |
| Standardised today | Yes, within C2PA | No |

## Why the capacity measurement decides whether this distinction exists

A full attestation is estimated at roughly 128 bytes. Established audio [watermarks](glossary.md#provenance-and-content-authenticity) carry on
the order of tens of bits per second, which implies something like 20 to 100 seconds of audio
per payload. See [notes/adoption-strategy.md](notes/adoption-strategy.md).

**If that proves infeasible, the distinction collapses.** An attestation that will not fit
must be replaced by a pointer, and a pointer is a soft binding.

That makes open question 1 in [notes/open-questions.md](notes/open-questions.md) more
consequential than it first appears. It does not merely refine the design. It determines
whether this project has a distinct technical identity or is a specialisation of something
that already exists.

Either outcome is worth pursuing. A soft binding algorithm with a music-specific,
DDEX-aligned payload is a genuine contribution. But it is a contribution **within** C2PA
rather than an alternative to it, and the project should be honest about which one it is.

## The likely design

A hybrid, worth aiming at deliberately rather than arriving at by defeat.

Carry enough in the signal to verify the essential claim locally: a compact signature over a
truncated [content anchor](glossary.md#terms-sigil-defines) plus a small number of DDEX-aligned claim bits. Optionally also carry
a pointer, so a richer manifest can be resolved when a network is available.

That yields offline verification of the claim that matters most, richer detail when
reachable, and degradation to something still useful when the repository is gone. It also
remains registrable as a C2PA soft binding, since the pointer is present.

## Open questions

- Whether the minimum useful attestation is smaller than 128 bytes, and what can be cut.
- Whether key distribution can be solved well enough that offline verification is real rather
  than nominal.
- Whether the hybrid should be one carrier payload or two independently recoverable ones.
- Whether a signal-carried attestation should be permitted to reference a manifest it does not
  contain, and what a [verifier](glossary.md#terms-sigil-defines) reports when that manifest is unreachable.
