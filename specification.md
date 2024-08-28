# Shape index
https://solid.github.io/type-indexes/

https://linkeddatafragments.org/specification/triple-pattern-fragments/#document-conventions

## Abstract
Open web and linked data go pair to pair, however, it does not mean that the publication of linked data cannot have precise structures.
This document specifies the _shape index_ specification, designed for query optimization of linked data in networks of structured information.
A shape index maps an RDF data shape to a set of linked data resources, to provide lightweight yet powerful structural and statistical information about the content of the data within a defined region of a linked data network.

## Aim, scope, and intended audience


## Document conventions
All assertions, diagrams, examples, and notes are non-normative, as are all sections explicitly marked non-normative. Everything else is normative. 

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” are to be interpreted as described in BCP 14 [RFC2119] [RFC8174] when, and only when, they appear in all capitals, as shown here. 

(copied from https://solid.github.io/type-indexes/)

### Terminology

Subweb

    - A subsection of web defined by a set of IRI or IRI patterns.

RDF data shape

    - Schema definition of RDF subgraph, independant from a shape language.

Complete index

    - An index that describe every ressource within its subweb


### Conformance

## Shape index

A shape index is a mapping between sets of documents and RDF data shapes of any language with information about the subweb that the data publisher operates in and an indication of whether or not the shape index reports every file published in the subweb of the data publisher.
Information about the absence in the subweb of dependent shapes can also be encoded in the index.


The purpose of a shape index is to organise and control the data quality of a small decentralized environments. 

## Negative entries

## How to discover a Shape Index

## Shape index Data Validation

## Shape Index for source selection

