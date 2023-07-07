
# 1 Introduction


## 1.1 Purpose

This document provides server implementation Best Practices for STAC-based discovery services that allow for standardized and harmonized access to metadata and data for CEOS agencies.

The diagram below depicts the various STAC specifications.  The Best Practices proposed in the current document relate to all components of the specification tree except the ones shown in pink.

In addition, a number of extensions are required to allow for federated search.  These are documented in a separate document.

| ![STAC specification tree](./figures/stac-tree.png "STAC Specification Tree") |
|:--:| 
| *STAC specification tree* |

@startwbs
+ STAC Ecosystem
++ STAC Catalog\nSpecification
++ STAC Item\nSpecification
++ STAC Collection\nSpecification
++ STAC API\nSpecification
+++ STAC API - Core Specification
+++ STAC API - Collections Specification
+++ STAC API - Features Specification
+++ STAC API - Item Search Specification
+++[#pink] STAC API - Children Specification
+++[#pink] STAC API - Browseable Specification
+++[#pink] STAC Extensions (Various)
@endwbs

## 1.4 Terms, Definitions and Abbreviated Terms

### 1.4.1 Terms and Definitions 

|  |   |  
| -------- | --------- | 
| `Granule` | A granule is the finest granularity of data that can be independently managed. A granule usually matches the individual file of EO satellite data.  | 
| `Collection` | A collection is an aggregation of granules sharing the same product specification. A collection typically corresponds to the series of products derived from data acquired by a sensor on board a satellite and having the same mode of operation.  | 


### 1.4.2 Acronyms


|  |   |  
| -------- | --------- | 
| `CEOS` | Committee on Earth Observation Satellites   | 
| `CWIC` | CEOS WGISS Integrated Catalog  | 
| `FedEO` | Federated Earth Observation Missions  | 
| `HATEOAS` | Hypertext As The Engine Of Application State  |
| `IDN` |  International Directory Network  | 
| `JSON` | JavaScript Object Notation  | 
| `OGC` | Open Geospatial Consortium  | 
| `OSDD` | OpenSearch Description Document  |
| `STAC` | Spatiotemporal Asset Catalog  |
| `WGISS` | Working Group on Information Systems and Services  | 


## 1.5 References



### 1.5.1 Applicable Documents

| **ID**  | **Title** | 
| -------- | --------- | 
| `AD1` <a name="AD1"></a> | STAC Catalog Specification, https://github.com/radiantearth/stac-spec/blob/master/catalog-spec/catalog-spec.md | 
| `AD2` <a name="AD2"></a> | STAC Collection Specification, https://github.com/radiantearth/stac-spec/blob/master/collection-spec/collection-spec.md  | 
| `AD3` <a name="AD3"></a>| STAC API Specification, https://github.com/radiantearth/stac-api-spec  | 
| `AD4` <a name="AD4"></a> | RFC 7946 - The GeoJSON Format, https://datatracker.ietf.org/doc/html/rfc7946 | 
| `AD5` <a name="AD5"></a> | STAC Item Specification, https://github.com/radiantearth/stac-spec/tree/master/item-spec   | 
| `AD6` <a name="AD6"></a> | OGC API - Features - Part 3: Filtering, https://docs.opengeospatial.org/DRAFTS/19-079r1.html  | 
| `AD7` <a name="AD7"></a>| JSON Schema: A Media Type for Describing JSON Documents              draft-handrews-json-schema-02, https://datatracker.ietf.org/doc/html/draft-handrews-json-schema-02 |
| `AD8` <a name="AD8"></a> | OGC API - Features - Part 1: Core, https://docs.opengeospatial.org/is/17-069r3/17-069r3.html | 


### 1.5.2 Reference Documents

| **ID**  | **Title** | 
| -------- | --------- | 
| `RD1` <a name="RD1"></a> | CEOS OpenSearch Best Practice Document, Version 1.0, CEOS-OPENSEARCH-BP-V1.3, https://github.com/radiantearth/stac-spec/blob/master/catalog-spec/catalog-spec.md | 

