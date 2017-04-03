# Justifications

Below are justifications for some major differences between qremis and premis

## Mandated use of a "qremis" wrapper element

The mandated use of a wrapper element serves multiple purposes.

It standardizes, conceptually, the organization of the qremis data model below
a single node, facilitating the development of a _single_ validatable serialization structure for
complete records.

It provides a standard place in which to deposit serialization specific metadata, as well as 
potentially meta-metadata.

It provides an easily recognizable "envelope" for recognition of qremis records coming over the wire
in channels which may contain multiple record formats.

It allows qremis to be sensibly serialized both in "rooted" serializations (xml) as well as serializations
which begin with an iterable (json).


## Lack of strong ordering

Not requiring strong ordering of elements serves to reinforce the requirement for each qremis element
to be fully self describing by prohibiting reliance on "previous" elements. It also aligns qremis record
specifications with modern popular technologies for storing key value pairs, 
[hash tables](https://en.wikipedia.org/wiki/Hash_table).

This requirement also reinforces qremis' focus on machine readability and tooling, rather than imposing
arbitrary constraints for the sake of human readability.


## Elimination of linkingIdentifiers

LinkingIdentifier elements in the standing PREMIS specification (v3), are implemented with an inconsistant
interface - some have linkingXRole nodes and some do not. Additionally, each entity requires it's own 
linkingXIdentifier element in each other potentially linked entities.

Additionally, in the case of objects the distinguishing characteristics between a relatedEventIdentifier and
a linkingEventIdentifier invites ambiguity and arbitrary divisions.

For this reason, rather than implementing linkingIdentifier elements for each entity, _all linkages in
qremis are considered the simplest sort of relationship_. 

This eliminates the inconsistancy of linkingXIdentifier element interfaces in the specification, 
relying on the standardized interface of the Relationship entity (see next point) to handle inter-element
linkages. Additionally, this change provides the extensability and elasticity of a freestanding top 
level element to describe the nuances of a linkage, if required.


## Promotion of "Relationship" to a top level entity

Promoting "Relationship" to a top level entity provides a convienant method for utilizing qremis as
an actual semi-relational database, rather than just a series of loosely connected (through references)
metadata records.

Altering a standing record (eg, adding a relationship subelement to an existing object record) requires
a significantly more complex operation (which is significantly more difficult to scale) than appending
an additional free standing "Relationship" record. This, in combination with changes to linkingIdentifiers,
allow for some of the most basic relational operations performed on archival data: linking objects to other
objects, linking objects to events, linking agents to rights, etc to be performed _without altering standing
records_, necessitating only (hopefully atomic) "append" operations to the database, which scales very well
and sidesteps the complex problem of scalable record locking.


## rightsStatementIdentifier --> rightsIdentifier

In the current PREMIS specification (v3) the rightsElement is formatted as such:

```
rights
    rightsStatement (O, R)
        rightsStatementIdentifier (M, NR)
            rightsStatementIdentifierType (M, NR)
            rightsStatementIdentifierValue (M, NR)
        ...
    rightsExtension (O, R)
```

The usage notes of rightsStatement are:

> This semantic unit is optional because in some cases Rights
> may be unknown. Institutions are encouraged to record Rights
> information when possible. Either rightsStatement or rightsExtension
> must be present if the Rights entity is included. The rightsStatement
> should be repeated when the act(s) described has (have) more than one 
> basis, or when different acts have different bases.

Note that in order for a valid Rights entity to be produced either a rightsExtension
_or_ a rightsStatement may be present. However, there is no formal way to produce an
identifier for a rightsExtension, and additionally all linking elements in other entities
reference linkingRightsStatementIdentifier's.

Thus, it is impossible to utilize the rightsExtension element and link that rightsExtension
element to another relevant element as the standard currently stands.

In addition to the inability to link rights entities which are composed of the rightsExtension element
there is also the matter of the incongruity of _all_ other linking entity nodes (with the exception of
"relationships") which are referenced in other elements being "top level" elements of their respective
entities.

In order to remedy these issues the top level of the rights entity in qremis has been reformatted as such:

```
rights
    rightsIdentifier (M, R)
        rightsIdentifierType (M, NR)
        rightsIdentifierValue (M, NR)
    rightsStatement (O, R)
    rightsExtension (O, R)
    ...
```

All linking relations have been reformatted as "linkingRightsIdentifier" rather 
than "linkingRightsStatementIdentifier." Additionally, both rightsStatement and 
rightsExtension are optional, but they are no longer the only two elements at the top level
of the rights element (and thus are truly optional, both can be omitted without producing
and empty element). This accounts for the case where rights may be referenced by identifier, and linked
via qremis metadata while necessitating that they be resolved against an external database by use
of an included identifier.


## Bidirectional linking (usually)

Resolving links when "inbound" links are unknown is an O(n) operation, where n = number
of potentially linked records. Resolving links when inbound links are known is O(1) per link.
For this reason linking, whenever possible, is recommended to be bidirectional and symmetric.
This differs from the PREMIS specification's assertion that link direction is not mandated.


## Identifier Cardinality Changes

In the standing PREMIS specification (v3) objectIdentifiers and agentIdentifiers are repeatable, while
rightsStatementIdentifiers and eventIdentifiers are not. This different results in inconsistancy in the
interface of the data model when it comes to identifiers - arguably the most critical part of the entirety
of the specification.

The current PREMIS specifications defend this inconsistancy by hypothesizing about the nature and purpose
of the identifiers applied to these entities.

Qremis opts isntead to make no assumptions about the identifiers applied to entities, the purpose of those
identifiers, the origin of those identifiers, etc. For this reason the interface for identifiers in every
element can be standardized, and any and all relevant identifiers can be specified for the relevant entities.
