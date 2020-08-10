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
    organization: Mozilla
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
contain errors (as an example, TLS 1.3 {{?RFC8446}} currently has >30
errata). However, fixing these errors is prohibitively difficult
as it currently requires publishing a new RFC, which incurs new
delay, at which point new errors will have accumulated and the
cycle begins again.

This document proposes a new approach to RFC publication, "just-in-time"
(JIT) publication, which is designed to address both of these issues. JIT
publication is centered around two basic ideas:

- A series of documents which are intended to be semantically identical,
  even though the text may be different.

- The ability to rapidly publish new documents within a series as soon
  as the requisite level of approval has been obtained.

When put together, these allow us to radically decrease the time to
publish while also cheaply addressing issues as they are discovered.


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
https://ietf.org/documents/D.0.

Once the initial version was published, new versions could be
immediately published based on approval from either the responsible
Area Director (or perhaps the WG Chair), who would be responsible for
confirming that the changes did not change the document semantics.
This allows for errata, etc. to be immediately applied to the document
in place.  Note that this is consistent with AUTH48 process because
the AD can sign off on changes after IESG approval. Moreover, the
stakes here are quite low because we can always publish a new Version
that reverts any change.

## Editing

In this model, V.0 is published without any professional editing other
than what occurs during the normal WG and IESG process. This is
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
but not have any code points in it. The former seems preferable.


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
not. In addition, it would start ot burn through the RFC numbers very
quickly, especially if each erratum creates a new version.


# Security Considerations

This document has no impact on security.

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
