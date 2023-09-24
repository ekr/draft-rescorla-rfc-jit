---
title: "Just-In-Time RFC Publication"
abbrev: "RFC JIT"
docname: draft-rescorla-rfc-jit-latest
category: info

ipr: trust200902
area: General
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: E. Rescorla
    name: Eric Rescorla
    organization: Windy Hill Systems, LLC
    email: ekr@rtfm.com

normative:
  RFC2119:

informative:



--- abstract

This document describes a new approach to RFC publication that is
intended to decrease publication time while also allowing easy
revisions without re-running the entire RFC publication process.

--- middle

# Introduction

The current RFC publication process is unwieldy and slow. This is a
poor match for an environment where protocol specifications routinely
see wide deployment well in advance of IESG approval, let alone RFC
publication. Despite the long publication time, RFCs also routinely
contain errors (as an example, TLS 1.3 {{?RFC8446}} currently has >40
errata). However, fixing these errors is prohibitively difficult
as it currently requires publishing a new RFC, which incurs new
delay, at which point new errors will have accumulated and the
cycle begins again.

This document proposes a new approach to RFC publication, termed
"just-in-time" (JIT) publication, which is designed to address both of
these issues. JIT publication is centered around two basic ideas:

- A series of documents which are intended to be semantically identical,
  even though the text may be different.

- The ability to rapidly publish new documents within a series as soon
  as the requisite level of approval has been obtained.

When put together, these allow us to radically decrease the time to
publish while also cheaply addressing issues as they are discovered.
This document focuses on the IETF Stream as that is the dominant
source of RFCs. However, the contents potentially apply to other
streams as well.

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 {{RFC2119}} {{!RFC8174}}
when, and only when, they appear in all capitals, as shown here.

# Requirements

The method in this document is intended to balance a number of requirements.

- Rapid publication: It MUST be possible to publish documents rapidly once their content
has been approved. This allows for immediate deployment as soon as the
IETF part of the process is complete.

- Easy updating: Once a document is published it MUST be easy to make
non-semantic changes (editorial fixes, clarifications, etc.) without
re-running the whole process.

- Referential Integrity: It MUST be possible to reference a specific version
of a document (i.e., the exact bits).

- Semantic References: It MUST be possible to reference a document as a
semantic entity, including whatever editorial updates have been made.

- Professional Editing: It MUST be possible to have professional editing
for documents.


# Generic Structure

This section describes a generic publication structure that does not
take into account the RFC Series. In {{mapping}} we describe how it
might be mapped onto the existing RFC Series. It makes use of a two-level
structure of Document and Version.

- Documents refer to the semantics of a given specification and
  thus may appear in multiple Versions. Each document has
  a document identifier D which is persistent through the life of the document
  and refers to the latest Version.
- Version refer to specific instances of a Document. All versions
  of a given document have the same semantics. Each version has
  a unique version identifier under the document, as in D.V.

In this design, documents would proceed through the IETF process as
usual until they got to IESG approval. At the point where they were
approved by the RFC they would receive a document identifier and the
initial version would be published, e.g., at
https://rfc-editor.org/documents/D.0.

Once the initial version was published, new versions could be
immediately published based on approval from either the responsible
Area Director (or perhaps the WG Chair), who would be responsible for
confirming that the changes did not change the document semantics.
This allows for errata, etc. to be immediately applied to the document
in place.  Note that this is consistent with AUTH48 process because
the AD can sign off on changes after IESG approval (see
{{consensus-rfc}} for more on this). Moreover, the stakes here are
quite low because we can always publish a new Version
that reverts any change.

## Editing

In this model, V.0 can be published without any professional editing
other than what occurs during the normal WG and IESG process. This is
intended to favor rapid publication over perfection, but is still
compatible with editing. There are two primary options:

- Post-publication editing: The document can be edited after
  publication with the edits being folded into new versions. This
  will happen naturally to some extent with active documents
  as readers find issues, but it also provides a way for
  professional editing to be fed in.

- Pre-publication editing: In cases where the IESG deems a document
  to be particularly hard to read, it can be sent for initial editing
  prior to publication.

It is also possible to do a combination of these, with some level
of pre-publication editing followed by subsequent volunteer and
professional post-publication editing. Because it is easy to mint
new versions, getting the first version perfect is less important.


## IANA Interaction

Because documents contain IANA considerations sections, these
considerations MUST be accurate. This can be achieved in one of
two ways (1) the document can be held until IANA actions are
complete or (2) the document can refer to an external registry
but not have any code points in it. The former seems preferable,
as it avoids having a published version with incorrect code
points and the IANA actions can be finished during IETF review.
Note that it is not necessary for the IANA actions to be perfect,
as the IANA considerations section and the IANA registry can
be adjusted after the initial version is published.


# Mapping onto the RFC Series {#mapping}

There are at least two ways to map this onto the existing RFC Series,
which only has one level with each RFC being unchanging.

- Add a new level of Document identifiers with RFCs as versions
  (e.g., D1234.1 -> RFC 123456)

- Add a new level of Version below RFC (e.g., RFC8446.0, RFC8446.1, etc.),
  with the main RFC# pointing to the most current version.

Either of these would probably work, and which people prefer to some
extent depends on their priors. However, it's worth noting that the
former approach would create a new reference that people would
generally be pointed to that wasn't RFCs, whereas the second would
not. In addition, it would start to burn through the RFC numbers very
quickly, especially if each erratum creates a new version.

# Implications for Existing Organizational Structures

The primary impact of what is proposed here is to lower the stakes
for decisions we make about the RFC series because it is possible
to publish revised versions that embody different decisions.
For instance, stylistic questions around word choice or layout
are more easily changed if we later determine that the wrong
decision was made. This has some implications for the organizational
structures we need.


## The RS(Editor,Advisor)

This proposal does not necessarily speak one way or the other to
whether we have an RFC Series Editor (or Advisor). However,
if post-publication editing -- which is enabled by this proposal --
is used, then some of the traditional functions of the RSE
become less salient. In particular, at present Internet-Drafts
do not really follow the RFC Style Guide except to the extent
to which (1) either authors do so or (2) the tools do so automatically.
Thus, if documents are published once they clear the IESG they
may not conform to the style guide, though later revisions
might do so. This somewhat reduces the importance of a unified
style guide.

Similarly, because it detaches the question of what makes a document
a standard (in this case, first publication as an RFC) from long
term strategy for the RFC Series, this proposal would reduce the
dependency on the IETF side on extensive RS(E,A) involvement,
with the expectation that the IETF would decide on what it needed
prior to first publication. A separate process could then be used
to address broader RFC Series issues that crossed all streams.


## The RPC

In the structure contemplated here, the RPC would probably do
less. Specifically:

- Some documents might not be processed by the RPC at all
  and would simply be published as-is.

- We would attempt to minimize the amount of semi-mechanical
  changes that the RPC made so that it was easy to re-publish.
  For instance, in some cases references in the RFC are
  different from those in the I-D even if both are generated
  from the DOI. We would want to automate these transformations.

- The RPC would not need to manage AUTH48 in post-publication
  review; in some versions their proposed edits could be handled
  like any other edits.

- The RPC would not need to do cluster 238-style dependency
  management and we could just publish documents as soon as
  they were approved by the IESG with whatever references
  the IESG considered successful.

In addition, because the RPC was not on the critical path
to standards availability, it would allow for more flexible
arrangements with the LLC. For example, currently when the
RPC falls behind their SLA it causes disruption of the whole
process. However, in a post-publication editing setting,
the LLC can decide how much editing it wants and tune SLAs
and RPC cost appropriately. Note that it is still possible
to run entirely in a pre-publication editing mode, simply
not necessary.


## Toolchains

Currently we have an odd mix of toolchains in which authors use
a variety of input formats (Word, XML, Markdown) but then the RPC
works only with XML (this used to be nroff). A continuous publication
structure like this puts pressure on this structure.
We already have this problem now with documents
which started in markdown where the effort to product a bis is
very large as either the authors need to work in XML or backport
RFC Ed changes into markdown (an imperfect process) but if
revisions are common then that clearly makes things worse.

There are a number of possible options, including:

- A single mandatory toolchain
- A small set of allowed toolchains

In the latter case, the amount of editing that the RPC could
do in practice would be somewhat limited.


# Security Considerations

This document has no impact on security.

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
