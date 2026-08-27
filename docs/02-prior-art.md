# Prior Art

> Status: draft. Non-normative.
> Last surveyed: 2026-08-27.

This document exists to keep the project honest. Sigil is only worth building if it does
something the systems below do not. Each entry states what the system actually does and
where it stops.

**If any entry here is wrong or out of date, that is a bug. Please open an issue.**

---

## C2PA / Content Credentials

**What it is.** An open, royalty-free standard from the Coalition for Content Provenance and
Authenticity, formed out of Adobe's Content Authenticity Initiative and Project Origin.
Members include Adobe, Microsoft, Google, OpenAI, Sony, Nikon, Leica, Intel, BBC, Truepic,
and [IPTC](glossary.md#organizations-and-standards-bodies). It has a formal [conformance program](glossary.md#cryptography-and-specification) and thousands of [CAI](glossary.md#organizations-and-standards-bodies) member organizations.

**How it binds.** Cryptographically signed metadata structures called [C2PA](glossary.md#organizations-and-standards-bodies) [manifests](glossary.md#provenance-and-content-authenticity),
embedded in the file container as [JUMBF](glossary.md#identifiers-and-file-formats). Each edit appends to the provenance chain.

**Watermarking role.** [Watermarks](glossary.md#provenance-and-content-authenticity) and content fingerprints appear as *[soft bindings](glossary.md#provenance-and-content-authenticity)*, used to
re-discover a manifest that has been stripped from a file. The [Soft Binding Resolution API](glossary.md#provenance-and-content-authenticity)
gives clients a standard way to retrieve a manifest from a repository. Watermark vendors
implement against an approved algorithm list. A manifest with one or more soft bindings is
termed a [Durable Content Credential](glossary.md#provenance-and-content-authenticity).

**Where it stops.**
- The primary binding is destroyed by any non-C2PA-aware re-encode.
- The soft binding carries a pointer, not the proof, so verification needs a network lookup
  against a live repository.
- Maturity is strongest in imaging. Audio and video are in the spec and progressing, with
  v2.3 adding live video streaming support and Sony shipping [Content Credentials](glossary.md#provenance-and-content-authenticity) in the
  PXW-Z300, but professional music workflows remain thin.

**Relationship to Sigil.** Complementary. Registering as a soft binding algorithm is a more
plausible adoption path than competing. See [spec/06-interop.md](spec/06-interop.md).

---

## SynthID (Google DeepMind)

**What it is.** Google's imperceptible watermarking system for its own generative output,
spanning Imagen, Veo, Lyria, and Gemini text.

**Where it stops.** It is not an open multi-vendor standard. The text variant was open
sourced. Image, audio, and video remain proprietary, with detection running principally
through Google. Adoption spreads through bilateral partnership, including OpenAI, ElevenLabs,
NVIDIA, and Kakao, rather than open governance. Meta and Microsoft operate separate systems,
so cross-vendor detection is fragmented.

**Relationship to Sigil.** Fails the "any adopting vendor can verify" requirement by
construction. It is the thing Sigil is trying not to be.

---

## DDEX AI disclosure

**What it is.** An industry-standard metadata vocabulary for AI involvement in a recording,
developed through [DDEX](glossary.md#organizations-and-standards-bodies) with Spotify's participation and announced September 2025. Five
categories: AI-generated vocals, AI instrumentation, AI composition, AI post-production, and
AI lyrical content. Delivered through distributor upload workflows and surfaced in credits.

**Where it stops.** Entirely self-reported. No cryptography, no binding to the audio, no
verification path. It records what a rightsholder asserted.

**Relationship to Sigil.** The most important adjacency. DDEX defines *what* to say about AI
involvement and moves it through the pipeline the music industry already uses. Sigil could
supply the missing half by making that assertion verifiable. Aligning the Sigil payload
vocabulary with the DDEX categories is a live design option.

---

## SMPTE ST 2112, Open Binding of IDentifiers (OBID)

**What it is.** A published [SMPTE](glossary.md#organizations-and-standards-bodies) standard family, ST 2112-10, RP 2112-11, ST 2112-20, and
RP 2112-21, that binds [Ad-ID](glossary.md#identifiers-and-file-formats) identifiers to commercials and [EIDR](glossary.md#identifiers-and-file-formats) codes to programming using
inaudible audio watermarking. The watermarking technology is Kantar Media's, available for
licensing. Originated in [CIMM](glossary.md#organizations-and-standards-bodies)'s [TAXI](glossary.md#identifiers-and-file-formats) Complete initiative.

**Where it stops.** It carries *identifiers*, not [attestations](glossary.md#terms-sigil-defines). Purpose-built for real-time
content identification and audience measurement, not authenticity. A recovered [OBID](glossary.md#identifiers-and-file-formats) payload
tells you which asset this is, not who vouched for it or how it was made.

**Relationship to Sigil.** This is the closest existing proof that the [carrier](glossary.md#terms-sigil-defines) mechanism
works at professional scale. It substantially de-risks the embedding layer and it means the
carrier is not where Sigil's contribution lies. Study before specifying anything.

---

## ATSC A/334 and A/335

**What it is.** Audio and video watermark standards deployed in [ATSC](glossary.md#organizations-and-standards-bodies) 3.0 NextGen TV,
carrying signaling and identification data through the broadcast chain.

**Where it stops.** Same shape as OBID. Signaling and identification, not provenance
attestation.

---

## Simmons and Winograd, arXiv 2405.12336

**What it is.** *Interoperable Provenance Authentication of Broadcast Media using Open
Standards-based Metadata, Watermarking and Cryptography.* Proposes combining C2PA
cryptographic metadata with ATSC audio and video watermarking to validate the provenance of
broadcast news redistributed on social platforms.

**Where it stops.** Scoped to broadcast news and built on the ATSC watermark stack.

**Relationship to Sigil.** The nearest published prior art to the core idea, combining the
same three ingredients. Read in full before drafting spec text. Any claim of novelty has to
be stated relative to this paper.

---

## SMPTE Content Provenance and Authenticity in Media Study Group

**What it is.** A SMPTE study group formed 16 July 2025, headed by Thomas Bause Mason,
SMPTE's director of standards, with participants from Ross Video, Sony, Adobe, the [EBU](glossary.md#organizations-and-standards-bodies), and
Metaglue. Charter is to identify relevant provenance technologies, review work by other
professional media organizations, gather use cases and requirements, and recommend where
SMPTE should update or create standards. A flagged priority is carriage of provenance
information in [MXF](glossary.md#identifiers-and-file-formats), described as an urgent industry need.

**Status.** Pre-standard. SMPTE has indicated it does not intend to invent a competing
scheme, but rather to ensure its standards can carry whatever the market adopts, plausibly
C2PA.

**Relationship to Sigil.** Not prior art so much as a venue. Study groups explicitly solicit
use cases and requirements, which makes this one of the few realistic routes for an outside
proposal to reach the film and television standards process.

---

## Summary of the gap

Every ingredient exists. Inaudible carriers are standardized and deployed. Cryptographic
provenance manifests are standardized and adopted. A music industry disclosure vocabulary
exists. Combining metadata, watermarking, and cryptography has been published for broadcast.

What does not appear to exist is a **self-contained cryptographic attestation carried in the
signal, designed for the music and film post-production chain, verifiable offline by any
adopting vendor**. That is the claim Sigil has to defend, and it should be re-tested against
this document regularly.
