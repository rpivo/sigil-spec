# Glossary

> Status: informational.
> Last reviewed: 2026-08-27.

This project sits between two fields that do not share a vocabulary. Audio and film
professionals and cryptography and standards people are both needed here, and each group's
everyday terms are opaque to the other. This glossary exists so that neither has to bounce
off the document.

## How to read this

Terms are marked with the body that owns them:

- **[Sigil]** defined by this project. Normative definitions live in
  [spec/00-conventions.md](spec/00-conventions.md), not here.
- **[C2PA]**, **[SMPTE]**, **[DDEX]**, **[ATSC]**, **[IETF]** borrowed from that body.
- **[general]** ordinary industry usage, no single owner.

**For any borrowed term, the owning body's specification governs.** The definitions below
are short informal restatements to keep this repository readable. Where a definition here
conflicts with the source standard, the source standard is correct and this file has a bug.

---

## Terms Sigil defines

These are restated for convenience only. See
[spec/00-conventions.md](spec/00-conventions.md) for the authoritative versions.

| Term | Short meaning |
|---|---|
| **Attestation** *[Sigil]* | A claim plus its signature and content anchor. The thing that travels in the signal. |
| **Carrier** *[Sigil]* | The imperceptible channel in the signal that conveys an attestation. |
| **Claim** *[Sigil]* | The structured assertion a signer makes about a recording. |
| **Content anchor** *[Sigil]* | The content-derived value tying an attestation to one specific recording. Prevents transplanting a valid signature onto different content. |
| **Recovery** *[Sigil]* | Extracting an attestation from a carrier. Distinct from verifying it: you can recover an attestation and then find it invalid. |
| **Signer** *[Sigil]* | The party whose key signs a claim. A tool, model operator, studio, or individual. |
| **Verifier** *[Sigil]* | An implementation that recovers and checks an attestation. |

Note the deliberate avoidance of the bare word *binding*, which is already a term of art for
both C2PA and SMPTE. See [spec/00-conventions.md](spec/00-conventions.md).

---

## Organizations and standards bodies

| Term | Meaning |
|---|---|
| **ATSC** | Advanced Television Systems Committee. Sets digital television broadcast standards in North America, including the ATSC 3.0 / NextGen TV suite. |
| **AES** | Audio Engineering Society. The professional audio standards body. Individual rather than organisational membership; its standards committee working groups are open to affected individuals, and anyone may propose new work. Not to be confused with the AES encryption standard. |
| **C2PA** | Coalition for Content Provenance and Authenticity. Publishes the open, royalty-free Content Credentials standard. Formed from Adobe's CAI and Project Origin. |
| **CAI** | Content Authenticity Initiative. Adobe-led membership community promoting Content Credentials. Distinct from C2PA, which develops the specification itself. |
| **CIMM** | Coalition for Innovative Media Measurement. US media measurement body. Originated the TAXI initiative that led to SMPTE ST 2112. |
| **DDEX** | Digital Data Exchange. Not-for-profit standards body for digital music value chain metadata. Owns the AI disclosure standard. |
| **EBU** | European Broadcasting Union. European public service broadcaster alliance, active in media technology standards. |
| **IETF** | Internet Engineering Task Force. Publishes RFCs, including the CBOR and COSE specifications relevant to payload design. |
| **IPTC** | International Press Telecommunications Council. News industry standards body, notably for photo and news metadata. |
| **SMPTE** | Society of Motion Picture and Television Engineers. Standards body for professional film and television engineering. |

---

## Provenance and content authenticity

| Term | Meaning |
|---|---|
| **Content Credentials** *[C2PA]* | The consumer-facing name for C2PA provenance data attached to a file. |
| **Content fingerprinting** *[general]* | Deriving an identifying value from the content itself, then matching it against a database. Unlike a watermark, it adds nothing to the file. C2PA permits it as a soft binding. |
| **Analog hole** *[general]* | The unavoidable gap where content leaves the digital domain and can be re-captured. Any source can be played into an analog input, so capture attestation cannot establish that a performance was live. |
| **Absence inference** *[Sigil]* | The unsupported reading of a missing signature as evidence about origin, in either direction. Named so it can be argued against. See [03-threat-model.md](03-threat-model.md). |
| **Assurance level** *[C2PA]* | A measure of how strongly an attestation is protected, for example whether signing occurs in dedicated hardware. Google integrated Assurance Level 2 into Pixel camera hardware. |
| **Attested capture** *[general]* | Signing at the point content enters the digital domain, so the capture device vouches for the samples it produced. |
| **Capture attestation** *[Sigil]* | A claim by the device that produced the samples. The strongest class, bound to hardware and made at capture time. |
| **Chain of custody** *[general]* | A record of each party that handled content and what each attested to. See [05-chain-of-custody.md](05-chain-of-custody.md). |
| **DRM** *[general]* | Digital Rights Management. Technology restricting how content may be used or copied. An explicit non-goal for Sigil, which attests to origin and does not restrict anything. The distinction matters because a protocol that embeds data in media is routinely mistaken for DRM. |
| **Durable Content Credential** *[C2PA]* | A Content Credential with one or more soft bindings, so the manifest can be rediscovered after the embedded metadata is stripped. |
| **Hard binding** *[C2PA]* | Provenance data cryptographically bound into the asset itself. Contrast soft binding. |
| **Manifest** *[C2PA]* | The signed metadata structure carrying provenance and edit history. Embedded in the container as JUMBF. |
| **Provenance** *[general]* | The record of where content came from and how it was changed. Distinct from authenticity, which is a judgment about whether that record is true. |
| **Soft binding** *[C2PA]* | A watermark or fingerprint that lets a verifier rediscover a manifest which has been removed from a file. Carries a pointer to a remotely stored manifest rather than the provenance data itself. |
| **Soft Binding Resolution API** *[C2PA]* | The standardized interface a client uses to retrieve a manifest from a repository given a recovered soft binding. |
| **Watermark** *[general]* | Data embedded in the signal itself, typically imperceptibly, surviving processing that would discard container metadata. Sigil's carrier is a watermark; the contribution is what it carries. |

---

## Identifiers and file formats

| Term | Meaning |
|---|---|
| **Ad-ID** | Industry identifier for advertising assets in the United States. Bound into content by SMPTE ST 2112. |
| **EIDR** | Entertainment Identifier Registry. Global identifier for movies and television programming. Also bound by ST 2112. |
| **ISRC** | International Standard Recording Code. The music industry's per-recording identifier. Relevant context for how a Sigil claim might reference a recording. |
| **JUMBF** | JPEG Universal Metadata Box Format, ISO/IEC 19566-5. The container structure C2PA uses to embed manifests. |
| **MXF** | Material Exchange Format, SMPTE ST 377. Professional media container widely used in film and television. Carriage of provenance data in MXF is the SMPTE study group's flagged priority. |
| **OBID** | Open Binding of IDentifiers. The SMPTE ST 2112 family, which binds Ad-ID and EIDR identifiers into content using inaudible audio watermarking. Carries identifiers, not attestations. |
| **TAXI** | Trackable Asset Cross-Platform Identification. The CIMM initiative that produced the work standardized as ST 2112. |

---

## Audio and video production

For readers arriving from cryptography or standards work.

| Term | Meaning |
|---|---|
| **A/D converter** | Analog-to-digital converter. Turns an analog signal into samples. The earliest point in a conventional signal path where attestation is possible. |
| **Adaptive streaming** | Delivering multiple encodings at different bitrates so a player can switch based on bandwidth. Means one release exists as many re-encoded files. |
| **Bit depth** | Bits per audio sample, commonly 16 or 24. Conversion between depths is routine and a Sigil carrier must survive it. |
| **DAW** | Digital Audio Workstation. The recording and production environment, such as Ableton Live or Pro Tools. The only party observing every source in a session. |
| **Bounce** | Rendering a mix or session to a single audio file. A common point where metadata is lost. |
| **Colour grading** | Adjusting colour and tone in post. Aggressive enough that it is a serious robustness challenge for a video carrier. |
| **Finishing** | The final stage of picture post before delivery. |
| **Letterbox** | Adding bars to fit content to a different aspect ratio. Alters the frame in ways with no clean audio analogue. |
| **Loudness normalization** | Platforms adjusting playback level to a target. Applied to essentially all streamed music, so a carrier must survive it. |
| **Mastering** | The final audio stage before distribution. Mastering engineers are the gatekeepers for anything claiming inaudibility, since they monitor on reference systems where artifacts are audible. |
| **Perceptual masking** | The effect where one sound or image detail hides another. The mechanism that makes imperceptible embedding possible, and the reason sparse material such as solo piano or near-silence is the hard case. |
| **Sample rate** | Audio samples per second, commonly 44.1 or 48 kHz. Conversion is routine. |
| **Secure element** | Tamper-resistant hardware holding a device key so it cannot be extracted. What a capture device would need in order to attest. |
| **Stem** | A submix, for example all drums or all vocals, delivered separately for later mixing. Complicates chain of custody, since a release may be assembled from separately exported files. |
| **Transcode** | Re-encoding from one format or bitrate to another. The single most common way provenance metadata is destroyed. |

---

## Cryptography and specification

For readers arriving from music or film.

| Term | Meaning |
|---|---|
| **Algorithm agility** *[general]* | Designing a format so the cryptographic algorithms can be replaced later without breaking existing implementations. Necessary because signature schemes eventually get broken. |
| **BCP 14** *[IETF]* | The convention giving MUST, SHOULD, MAY and similar words precise meanings in specifications. RFC 2119 and RFC 8174. See [spec/00-conventions.md](spec/00-conventions.md). |
| **CBOR** *[IETF]* | Concise Binary Object Representation, RFC 8949. A compact binary data format. A candidate payload encoding because carrier capacity is tight. |
| **COSE** *[IETF]* | CBOR Object Signing and Encryption, RFC 9052. Standard structures for signing CBOR data. |
| **Conformance program** *[general]* | The process deciding who may implement a standard and hold signing keys. C2PA runs one. A signature means little without a process governing who holds keys. |
| **Certificate authority** *[general]* | A body that vouches for identities it did not itself establish, on the basis of a documented process. The closest existing analogue to an accredited retroactive signer, including in its failure modes. |
| **Certificate Transparency** *[general]* | RFC 6962. Public append-only logging of issued certificates, making bad issuance detectable and attributable rather than attempting to prevent it. The model proposed for logging retroactive attestations. |
| **Certificate hierarchy** *[general]* | A trust model where authorities vouch for signers in a chain up to a trusted root. One candidate trust model for Sigil. |
| **Fail closed** *[general]* | On error or ambiguity, report failure rather than success. Requirement R7: a damaged carrier must never yield a false valid result. |
| **Provenance assertion** *[Sigil]* | A claim by a party about content it did not capture. Reduces to trust in that party plus whatever corroboration is offered. Must never be presented as equivalent to a capture attestation. See [06-existing-recordings.md](06-existing-recordings.md). |
| **Provenance laundering** *[general]* | Passing unattested content through a trusted signer so it emerges with a valid attestation. Requires no forgery: the chain is used as designed. See [03-threat-model.md](03-threat-model.md). |
| **Normative** *[general]* | Text stating a requirement that must be satisfied to conform to a specification, marked with BCP 14 keywords. Contrasts with **informative** text, which explains and gives context. A property of the text itself, not of the document's standing: a specification can be fully normative and adopted by nobody. Published standards contain both kinds. |
| **Normative reference** *[general]* | A referenced document that must also be complied with in order to conform. An informative reference is context only. Referencing another standard normatively is a substantive commitment. |
| **Public key** *[general]* | The half of a keypair used to verify a signature. Distributable freely, which is what makes offline verification possible. |
| **Append-only log** *[general]* | A record that can be added to but not rewritten, so an entry proves something existed when it was made. See [07-timestamp-log.md](07-timestamp-log.md). |
| **Merkle tree** *[general]* | A structure hashing entries into parents up to a single root. Publishing the root widely makes any later alteration of earlier entries detectable, which is what makes a log append-only in practice. |
| **Perceptual fingerprint** *[general]* | An identifier derived from how content sounds or looks rather than from its exact bytes, so it survives re-encoding. Chromaprint is a common audio example. Matching is approximate. |
| **Revocation** *[general]* | Declaring a key no longer trusted, typically after compromise. A mitigation, not a prevention: signatures made before revocation still need a policy. |
| **Timestamp attestation** *[Sigil]* | Evidence that content existed no later than a given date, created at that time rather than claimed afterwards. Cannot be produced retroactively, which is what makes it trustworthy. |
| **Trusted timestamping** *[general]* | Obtaining a date from a party the signer does not control, because a date a signer places in its own payload proves only that the signer asserted it. RFC 3161 defines one mechanism. |
| **Transplant attack** *[general]* | Lifting a valid signature from one recording and attaching it to another. Prevented by the content anchor. See [03-threat-model.md](03-threat-model.md). |

---

## Regulatory

| Term | Meaning |
|---|---|
| **EU AI Act Article 50** | Transparency obligations for generative AI systems, applied 2 August 2026. Includes machine-readable marking of synthetic content. |
| **California AI Transparency Act** | Effective 2 August 2026. Requires embedded disclosures and a free public tool for surfacing provenance data. |

See [notes/regulatory.md](notes/regulatory.md), including the 1 January 2027 platform
obligation not to strip provenance data.

---

## Missing something?

If a term in this repository is not here, or a definition above misstates what the owning
body means by it, please open an issue. See [../CONTRIBUTING.md](../CONTRIBUTING.md).
