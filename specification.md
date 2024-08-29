# Shape index

## Abstract
The open web and linked data are often seen as complementary, yet this doesn't imply that data publication should be disorganized.
 While the web as a whole lacks a clear structure, specific structures can be imposed on web segments through hypermedia descriptions and precise specifications. 
This document introduces the _Shape Index_ specification, designed to 
facilitate data quality and query execution time in decentralized unindexed networks of linked data. The shape index leverages RDF data shapes to provide lightweight yet powerful structural and statistical data.


## Aim, scope, and intended audience

The present document outlines how  to annotate networks of datasets with a shape index and proposes methods
by which the query engines can optimize traversal queries using this index. 
This document does not propose a mechanism for data validation, particularly continuous data validation during the insertion of data and update of the data model or structure of publication implying changes in the index. 
The voice of the document is mostly from the perspective of developers who want to use the shape index for query optimization.

## Document conventions
All assertions, diagrams, examples, and notes are non-normative, as are all sections explicitly marked non-normative. Everything else is normative. 

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” are to be interpreted as described in BCP 14 [RFC2119] [RFC8174] when, and only when, they appear in all capitals, as shown here. 

(copied from https://solid.github.io/type-indexes/)

```turtle
PREFIX si: <http://www.shapeindex.com#>
PREFIX ex: <http://www.example.com#>   
```

### Terminology

__Subweb__

A subsection of the web, defined by a set of IRIs or IRI patterns.

__RDF Data Shape__

A schema definition for an RDF subgraph, independent of any specific shape language. Typically, this MAY be formalized in ShEx or SHACL.

__Complete Index__

An index that fully describes every resource within its associated subweb.

## Shape index

A shape index is a scoped mapping between sets of RDF resources and RDF data shapes.
In this context, a scope means that the index is characterized by a pair identifying a subweb `si:subweb` and the completeness of the index `si:complete`.
A subweb is a set of IRI, which can be also simplified by using IRI patterns, the completeness of a 
shape index indicates if every resource in the subweb is associated with a shape.
This characterization of the scope of the index is useful for two reasons.
First, query optimizations related to the shape index are local, hence it is interesting for query engines
to know which subwebs are affected by the optimization when performing traversal queries.
Second, it is OPTIONAL to describe within a shape index every resource of a subweb, however, it is RECOMMENDED.
We define an index characterizing every resource in its subweb as complete `si:complete`.
Data providers might not want to describe every resource in its subweb to reduce potential computational time associated
with keeping the mapping valid or to give themselves more flexibility in their data publication paradigm in a section of their subweb.

The core feature of the shape index is its mappings.
A shape index can have multiple entries identified by the term`si:hasEntry`.
Each entry includes a target set identified by the term `si:target`.
Target sets are bound by a shape identified by the term `si:bindByShape`.
The target is a set of RDF resources where every triple within them MUST be validated by the shape linked to the target through a `si:bindByShape` annotation. 

```turtle
<http://mySubweb.com/index1> a si:ShapeIndex ;
  si:subweb <http://mySubweb.com/> ;
  si:subweb "http://mySubweb.com/0/data/.*/*" ;
  si:hasEntry [
    si:bindByShape <ex:profile#ProfileShape> ;
    si:target <http://mySubweb.com/profile>
  ], [
    si:bindByShape <ex:movieReview#movieReviewShape> ;
    si:target "http://mySubweb.com/0/data/movie_review/.*"
  ] , [
    si:bindByShape <http://mySubweb.com/readingList#readingListShape> ;
    si:target <http://mySubweb.com/0/data/personal_reading_list>
    si:target <http://mySubweb.com/0/data/research/paper_list>
    si:target "http://mySubweb.com/0/data/reading_list/*"
  ] ;
  si:isComplete true .
```

Furthermore, every set of triples respecting a shape with a close world assumption of `si:bindByShape` MUST be inside a resource of the target.
It is RECOMMENDED to use shapes with close-world assumptions.
If shapes with open-world assumptions are used then, triples respecting the shape SHOULD be inside a resource of the target.

Given shapes contained with each other that are used as binding for targets (see the example below and notice that the shapes are open) then a set of triples respecting the most restrictive shape (`ex:Citizen` in the example) SHOULD be located inside the resources of this target.
In a similar case but with closed shapes, the set of triples MUST be located at the target of the most restrictive shape.
If a set of triples can be validated by an open and a closed shape, the set MUST be inside the target of the closed shape. A resource MUST only be bound to a maximum of one shape.
 

```shexc
PREFIX ex: <http://example.com/#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX school: <http://school.example/#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

ex:Human {
  foaf:name xsd:string ;
}

ex:Citizen {
  foaf:name xsd:string ;
  ex:socialSecurityNumber xsd:string ;
}
```

## Negative entries

## How to discover a Shape Index
The discovery of the shape index is left to the data provider.
However, two approaches are RECOMMENDED.

The data provider can use the predicate`si:shapeIndexLocation` with as an object the IRI of the shape index. This triple can be placed at convenient locations in the data provider subweb. 
Examples include at the root, at the location where other dataset information is provided, or in every file of the subweb.

Another RECOMMENDED approach is to advertise the location of the shape index in the HTTP header.
This SHOULD be in the form `Link: <iri-of-the-shape-index>; rel="http://www.shapeindex.com#shapeIndexLocation"`.

## Shape Index for source selection

## Vocabulary

### si:ShapeIndex
A shape index

#### domain

#### range
`si:subweb`, `si:complete`, `si:hasEntry`, `si:doesNotContain`

### si:shapeIndexLocation

### si:subweb
The section of the web that the shape index caracterizes.
It can be a single IRI or IRI pattern in the form of a regex or a set of IRI and/or regex IRI pattern.
To represent a set multiple `si:subweb` predicate MUST be defined, thus the subweb does not have to be
in a tree structure.

#### domain 

`si:ShapeIndex`

#### range 
`xsd:string`

### si:complete
A flag indicating if a shape index is complete.

#### domain 

`si:ShapeIndex`

#### range 
`xsd:boolean`

### si:hasEntry

### si:Entry

### si:bindByShape

### si:target

### si:doesNotContain

## References

https://solid.github.io/type-indexes/

https://linkeddatafragments.org/specification/triple-pattern-fragments/#document-conventions