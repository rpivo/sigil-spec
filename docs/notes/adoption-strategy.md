# Adoption Strategy

> Status: working plan. Non-normative. Reflects circumstances as of 2026-08-27 and should be
> revisited as they change.

## The governing decision

**Do not attempt to establish a new standard. Become a component of existing ones.**

"A new industry standard for media provenance" is a decade-long institutional undertaking with
poor odds for a proposal without major backing. "The verification layer for DDEX AI
disclosure" and "a registered C2PA soft binding algorithm" are achievable on a two to three
year horizon, accomplish the same objective, and remain the best available route to broader
standing if that becomes possible.

The second governing rule: **standards follow deployment.** Successful standards codify
something already running. A specification with no implementation is a proposal among
thousands.

## Sequence

### Phase 0. Settle the feasibility question

Open question 1 in [open-questions.md](open-questions.md), measured rather than argued.
Nothing else is worth doing until the answer is known.

Approximate payload requirement:

| Field | Bytes |
|---|---|
| Signature (Ed25519) | 64 |
| Key identifier | 4 to 8 |
| Content anchor (truncated) | 16 |
| Claim flags, DDEX-aligned | 2 to 4 |
| Version | 1 |
| **Subtotal** | **~90** |
| With framing and error correction | ~128 |

Established audio watermarks carry on the order of tens of bits per second, which puts 128
bytes somewhere around 20 to 100 seconds of audio per payload. That is tolerable for a
complete track and is in direct tension with R6, recovery from an excerpt, in
[../04-requirements.md](../04-requirements.md).

The likely finding is that a hybrid is required: a short index carried in the signal with the
full attestation resolved from the log. That is closer to C2PA's soft binding than the
original premise and is much better discovered early than late.

### Phase 1. Operate the log

See [../07-timestamp-log.md](../07-timestamp-log.md). Requires nobody's cooperation, produces
running infrastructure, compounds in value, and provides standing in any subsequent
conversation.

### Phase 2. Reference implementation and test vectors

The artifact standards bodies actually evaluate. R14 in
[../04-requirements.md](../04-requirements.md).

### Phase 3. Pilot on real releases

A platform under the proposer's control is an unusual asset for a protocol proposal. Evidence
from production releases outweighs specification text.

### Phase 4. A second independent implementation

Two interoperating implementations is the conventional threshold distinguishing a standard
from a product, and is required for IETF standards-track advancement.

## The bodies, and how each takes input

Access is cheaper and more open than generally assumed.

### SMPTE, time-sensitive

The Content Provenance and Authenticity in Media study group is active and gathering use cases
and requirements, with its report partially drafted. Contributions during drafting influence
the output; contributions afterwards do not.

- Non-members may purchase a Standards Participation Subscription, roughly $775 per year.
- Separate guest participation exists at the discretion of the subgroup chair, intended for
  contributors not expected to join the standards community.
- Headed by Thomas Bause Mason, SMPTE's director of standards.

An open door with a closing window and a negligible price. Highest value per unit of effort
currently available.

### DDEX, best substantive fit

Membership is open to organizations with a business interest in digital media content, across
Charter, Full, Full Individual and Associate categories. The qualifying condition is having
implemented at least one DDEX standard in production **or** having proposed development of a
new one.

Working group participation requires Charter or Full membership. New requirements are
documented and assigned by the Board, advised by the Plenary, to a working group.

DDEX is the strongest substantive fit because the gap is specific and acknowledged: the
disclosure vocabulary exists and has no verification mechanism. See
[../spec/06-interop.md](../spec/06-interop.md).

### C2PA

No license fees; the specification is open and royalty-free. The relevant objective is
registration as an approved soft binding algorithm rather than membership as such.
Conformance costs, where they arise, are set by certificate authorities rather than by C2PA.

## Allies

**Watermarking vendors are partners rather than competitors.** The carrier does not need to be
invented; what is missing is a standardized cryptographic payload to put inside one. Verance,
Digimarc and Kantar each operate proven carriers and each has commercial reason to want such a
payload standardized on their technology.

Joseph Winograd of Verance co-authored the closest prior art, arXiv 2405.12336. Engaging
Verance early could resolve most of Phase 0, since the capacity figures being sought are
figures they already hold.

Other parties worth tracking: the EBU, Ross Video, Sony, Adobe and Metaglue, all participating
in the SMPTE study group.

## Immediate actions

1. Read Simmons and Winograd in full. Any informed reviewer will raise it.
2. Contact SMPTE regarding study group participation. Time-sensitive.
3. Determine whether catalog.fm already implements a DDEX standard, which decides the
   membership route.
4. Begin operating the log.
5. Measure carrier capacity, or obtain the figures from a watermarking vendor.
6. Conduct a patent search before further investment. See open question 14.

## Realistic expectation

The proposal originates with an individual, a draft, and a small platform, and its technical
core is unproven. Most attempts of this kind fail, and those that succeed generally have
substantial institutional backing early.

The plausible good outcome is not necessarily that this becomes an industry standard. It is
that **its ideas are absorbed into DDEX and C2PA work with the originator holding standing and
credit in that process.** That satisfies the stated objective in
[../01-motivation.md](../01-motivation.md), which is friction and accountability rather than
authorship, and it remains the most reliable route to the larger outcome should that become
available.

Assets that distinguish this from a typical proposal: an unfilled and specific gap, a
deployment surface under the proposer's control, regulatory pressure arriving on a published
schedule, and one open door with a closing window.
