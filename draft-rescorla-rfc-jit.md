---
title: "Just-In-Time RFC Publication"
abbrev: "RFC JIT"
docname: draft-rescorla-rfc-jit-latest
category: info
stream: editorial
ipr: trust200902
keyword: Internet-Draft
workgroup: "RFC Series Working Group"

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
intended to allow easy revisions without re-running the entire RFC
publication process.

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
"just-in-time" (JIT) publication, which is designed to address this
issue. JIT publication is centered around two basic ideas:

- A series of documents which are intended to be semantically identical,
  even though the text may be different.

- The ability to rapidly publish new documents within a series as soon
  as the requisite level of approval has been obtained.

When put together, these allow us to cheaply address issues as they
are discovered. This document focuses on the IETF Stream as that is
the dominant source of RFCs. However, the contents potentially apply
to other streams as well.

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 {{RFC2119}} {{!RFC8174}}
when, and only when, they appear in all capitals, as shown here.

# Requirements

The method in this document is intended to balance a number of requirements.

- Easy updating: Once a document is published it MUST be easy to make
non-semantic changes (editorial fixes, clarifications, etc.) without
re-running the whole process.

- Referential Integrity: It MUST be possible to reference a specific version
of a document (i.e., the exact bits).

- Semantic References: It MUST be possible to reference a document as a
semantic entity, including whatever editorial updates have been made.


# Generic Structure

This section describes a generic publication structure that does not
take into account the RFC Series. In {{mapping}} we describe how it
might be mapped onto the existing RFC Series. It makes use of a two-level
structure of Document and Version.

- Documents refer to the semantics of a given specification and
  thus may appear in multiple Versions. Each document has
  a document identifier D which is persistent through the life of the document
  and refers to the latest Version.
- Versions refer to specific instances of a Document. All versions
  of a given document have the same semantics. Each version has
  a unique version identifier under the document, as in D.V, indicating
  version V of document D.

In this design, documents would proceed through the process as
usual until they were first published as an RFC. At the point where they were
published by the RPC they would receive a document identifier and the
initial version would be published, e.g., at
https://rfc-editor.org/documents/D.0.

Once the initial version was published, new versions could be
immediately published based on approval from the
Area Director (or perhaps the WG Chair), who would be responsible for
confirming that the changes did not change the document semantics.

This process allows for errata, etc. to be immediately applied to the document
in place. Note that this is consistent with AUTH48 process because
the AD can sign off on changes after IESG approval (see
{{consensus-rfc}} for more on this). Moreover, the stakes here are
quite low because we can always publish a new Version
that reverts any change which has been inappropriately applied.


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

# IETF Consensus {#consensus-rfc}

All IETF Stream documents require IETF rough consensus {{?RFC8789}}.
As with the existing publication process, the combination of IETF Last
Call and IESG review is intended to ensure that the initial published
document as reviewed by the IESG has IETF Consensus. The JIT
publication process in principle allows for subsequent revisions to
incorporate material that does not have consensus. This would
be a process violation because ADs are supposed to only approve
non-semantic changes, but of course ADs might make mistakes.

However, it is _already_ possible to introduce non-consensus
changes in RFCs during AUTH48. Authors routinely make changes to RFCs
during the AUTH48 process; the RPC does not prevent these changes but
just requires that substantive changes be approved by the appropriate
AD. This proposal extends the period when changes can be made past
initial RFC publication but the risk of non-consensus changes
being mistakenly made is mitigated by several factors:

1. Changes are explicitly restricted to those without any semantic
   content, whereas AUTH48 changes can be semantically meaningful
   at the ADs discretion. The RPC could detect at least some
   of these cases, as they do now.

1. Any such changes are clearly visible as a diff from the previous
   version, so this situation is readily detectable. One could
   imagine having a short "objection period" between approval
   and publication to allow for incorrect changes to be detected.

1. It is trivial to publish a new version reverting any non-consensus
   changes.

Together, these changes minimize the risk of semantic changes being
introduced to published RFCs.

As a side effect, JIT publication may also make AUTH48 faster,
as authors would not need to worry so much about having every
single sentence be perfect.


# Version Publication Logistics

Once the RPC has published the initial version of an RFC, it should be
trivial to make a change and re-spin the document without significant
work by the RPC.  Ideally, there would be a simple process in
which the changes were proposed, approved by the ADs, vetted by the
RPC and then mechanically published as a new RFC version. This should only require
changing the specific text, rather than metadata, etc., which should
all change automatically.

For readers familiar with GitHub, one could imagine this a workflow
powered by GitHub actions:

1. Once the RFC is published, the XML is published on GitHub
  (e.g., in a repo containing the XML for every RFC).
1. The proposed change is submitted as a pull request to
   the XML.
1. A GitHub action automatically generates the resulting
    publication formats (HTML, TXT, PDF) for review.
1. The AD approves the pull request.
1. Upon AD approval and verification that no inappropriate changes
  have been made, the RPC merges the pull request.
1. A GitHub action publishes the RFC Version.

Obviously, these processes need not be based on GitHub, but this example
illustrates the desired level of automation.

# Published Version Adjustments

## Published Versions

With JIT RFC publication, we will have multiple versions of the
same semantic document, which means that we need some way to
help readers keep track of the state of the documents. Minimally:

* There needs to be a semantic reference that points to the most recent
  Version of the document. This reference is stable but the
  target is updated whenever a new version is published.

* There should be an associated document/page which lists
  each Version of the document along with a brief summary
  of the changes (think "git log").

* Each Version of the document should have an affordance which
  allows the reader to (1) find previous Versions of the document
  and (2) see what changes were made in this Version

For instance, if we were to use RFC number as the stable reference, then:

- A reader could always get the most recent Version of a document
  by going to the semantic reference at https://rfc-editor.org/documents/RFC12345

- The individual versions of the documents would be at
  https://rfc-editor.org/documents/RFC12345.0, https://rfc-editor.org/documents/RFC12345.1,
  etc.

- The list of all Versions might live at  https://rfc-editor.org/versions/RFC12345

## XML

If we expect people to make changes directly to the published XML,
it's important that that the XML be as legible as possible. Currently,
the XML produced by the preptool is significantly harder to read than
the editorial XML that people work on. Simplifying that XML would
be valuable in terms of user ergonomics, but can be pursued
in parallel to the changes proposed here.


# Security Considerations

This document in theory might make it possible to make semantically
relevant changes to RFCs post-publication, which could have
security implications. In the event that this
happens, it is straightforward to revert these changes.

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
