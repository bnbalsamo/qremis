# Justifications

Below are justifications for some major differences between qremis and premis

## Mandated use of a "qremis" wrapper element

TODO


## Lack of strong ordering

TODO


## Alterations to relatedXIdentifier names

TODO


## Promotion of "Relationship" to a top level entity

TODO


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


## Elimination of linkingIdentifiers

TODO


## Identifier Cardinality Changes

TODO


## Identifier Structure Changes

TODO
