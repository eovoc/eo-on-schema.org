
# 2 Best Practices

## 2.1 Two Step Search

One serious hurdle to overcome in searching for data is the great number of data items to account
for in responses, as well as the expected number of successful “hits” for a query. In ordinary web
searches, the searcher is usually looking for a small number of web pages or documents.
Relevance ranking typically does a good job of presenting these successful hits near the top of
the returned list, followed by single point-and-click retrievals. However, when searching for Earth
science data covering large time periods or spatial areas, a user will often specify a set of
constraints to find an appropriate data collection together with space-time criteria for files within
that data collection. Often, the precision of the data collections returned for the search is low, with
many spurious hits. However, the space-time precision of the files is often quite high: that is, the
user truly wants to use all the data files of a desirable data collection set that fall within the spacetime region of interest. Thus, searching for all data satisfying both dataset content and space-time
region at the same time can produce a great many spurious hits, i.e., all the files for data
collections that are not desired.

> **CEOS-STAC-BP-001 - Support of two step search [Recommended]**<a name="BP-001"></a>
> 
> Support for a two-step search consisting of a collection level search followed by a corresponding granule level search is recommended.

The two-step search consists of a collection level search and the subsequent granule level search
(or file-level search).


| **STAC**                            | **OpenSearch** | 
| --------                   | --------- | 
| Request for Landing Page | Request for OSDD (collection level) | 
| Identify rel=`data` endpoint providing access to collections.  Typically /collections. | Extract collection search URL template rel=`collection` |  
| Optionally drill-down the STAC collection/catalog hierachy following the rel=`child` links. |  | 
| Check whether collection search is supported by rel=`search` endpoint (referring to rel=`data` and type=`application/json`) or rel=`queryables` |  | 
| Perform search request (collection level) and obtain search response (JSON).  Search parameters allowed are STAC search parameters or the parameters explicitly returned by `/queryables`. | Perform search request (collection level) and obtain search response (e.g. Atom Feed or GeoJSON FeatureCollection). Search parameters allowed are all parameters explicitly declared in the OSDD URL template as optional or mandatory. | 

After identiying the collection of interest in the above search response, the client can perform a granule search.

| **STAC**                            | **OpenSearch** | 
| --------                   | --------- | 
| Check whether granule search is supported, i.e. by rel=`search` endpoint or rel=`items` endpoint or rel=`queryables` available in collection representation (JSON)|  Check whether granule search is supported. i.e. rel=`search` in collection representation (e.g. Atom) refers to OSDD to be used for granule search. | 
| Extract search endpoint from Collection representation, i.e. by rel=`search` endpoint or rel=`items` endpoint. Extract list of search parameters (for /items endpoint) from rel=`queryables` endpoint. |  Extract URL template i.e. rel=`results` from OSDD, which provides the search endpoint and optional and mandatory search parameters. | 
| Perform search request (granule level) and obtain search response (GeoJSON FeatureCollection).  Search parameters allowed are STAC search parameters or the parameters explicitly returned by rel=`queryables`. | Perform search request (granule level) and obtain search response (e.g. Atom Feed or GeoJSON FeatureCollection). Search parameters allowed are all parameters explicitly declared in the OSDD URL template as optional or mandatory. | 


| ![Two step search](./figures/two-step.png "Two step search") |
|:--:| 
| *Two Step Search* |



In order to provide a well-defined search path from a collection of interest to granules associated
with that collection, we advocate the use of two-step searching leveraging the following:

1. Link elements of relation items (rel=’items’) within collection entries. These links point
to a granule-level endpoint specific to the collection entry.
1. Link elements of relation queryables (rel=’queryables’) within collection entries. These links point
to available granule-level search parameters specific to the collection entry.
2. Granule level interface descriptions (i.e. endpoints and sets of search parameters) that can be tailored to a specific collection.

The advantages of this approach are as follows:

- A client can navigate from collection to granule with only an understanding of the STAC specification.
- A server links between collections and granules exploiting the relation between a STAC Collection and a STAC Item.
- It allows the client to determine what search parameters are available to the user at the
granule level using the /queryables response.

---
**NOTE/QUESTION 1**

To be clarified what assumptions a STAC client is allowed to make regarding available (granule) search parameters.  Are STAC item-search parameters always all available at /search and /items endpoints, or only at the /search endpoint ?  Are STAC clients assumed to "parse" an OpenAPI specification to find parameters ?  Not possible to indicate optional versus mandatory parameters ?

---

---
**NOTE/QUESTION 2**

What STAC collection ìdentifiers can be used to perform searches at the /search endpoint ?  All the ones available at the rel=`data` path, all the ones in the hierachical structure starting from the landing page following the rel=`child` links ?

---

---
**NOTE/QUESTION 3**

What is the relation between the collections appearing in the hierarchy rel=`child` and the response from the /collections (rel=`data`) endpoint ?  One is a subset of the other or both can be unrelated ?   /collections to return a flat list ?  and how is it "filtered" when a query is applied ?  Flat list of all matches ?

---

## 2.3 Interface Descriptions 

> **CEOS-STAC-BP-002-1 - Collections endpoint [Requirement]**<a name="BP-002-1"></a>
>
> CEOS implementations shall advertise an endpoint for listing available collections in the landing page rel="data", type="application/json", href="/collections". 

> **CEOS-STAC-BP-002-2 - Collection search endpoint [Recommended]**<a name="BP-002-2"></a>
>
> CEOS implementations should advertise an endpoint for collection search  in the landing page rel="search", type="application/json", href="/collections".


> **CEOS-STAC-BP-002-3 - Granule search endpoints [Recommended]**<a name="BP-002-3"></a>
>
> CEOS implementations should advertise an endpoint for granule search which is valid for all the (underlying) collections (in case of a hierarchy) in the STAC Catalog or STAC Collection representation (typically the Landing Page) with rel="search" and type="application/geo+json".  TBD: valid for all in the hierarchy, or all in the list at /collections ?


> **CEOS-STAC-BP-002-4 - Granule search endpoints [Recommended]**<a name="BP-002-4"></a>
>
> CEOS implementations should advertise an endpoint for granule search which is valid for a single collection in the STAC Collection representation as a Link object rel="items" and type="application/geo+json".




> **CEOS-STAC-BP-002-5 - Support of Queryables [Recommended]**<a name="BP-002-5"></a>
>
> CEOS recommends the use of a rel=`queryables` endpoint to return the list of available search parameters in JSON Schema format according to §6.2 of [OGC API-Features - Part 3:Filtering](#AD6).

JSON Schema [AD7](#AD7) explicitly advertises the valid values and ranges of search parameters.  Thus, it reduces the risk of ambiguity and error.

While clients might analyse the OpenAPI Definition available at the `/api` endpoint for obtaining information about search parameters, the rel=`queryables` endpoint provides a simpler description which is easier to parse by client applications and can be specialised for individual collections.

.Example: /queryables response
```
:[example-1](.\examples\queryables.json)
```

> **CEOS-STAC-BP-002 - Parameter Descriptions [Recommended]**<a name="BP-002"></a>
>
> The GET response for the rel=`queryables` endpoint in application/schema+json representation shall provide additional information about search parameters including:
- whether the search parameter is mandatory (`required` property).
- `type` of the parameter (e.g. array, string, number, ...) 
- `format` of the string parameter (e.g. "uri")
- `enum` to enumerate valid (string) values
- `minItems`, `maxItems` to constrain the size of arrays
- `minimum`, `maximum` to constrain the range of a numerical parameter


### 2.2.1 Free Text Keyword

> **CEOS-STAC-BP-003 - Free text search [Recommended]**<a name="BP-003"></a>
>
> For supporting free text searches, the server
shall advertise support for the HTTP query parameter `q`.


### 2.2.1 Geometry

For conveying the server’s support condition of a geometry (`intersects`) parameter, the server should declare the supported subset in the JSON Schema object included in the rel=`queryables` response.

In the snippet below, the server supports any GeoJSON geometry, including Point, LineString, Polygon, MultiPoint, MultiLineString and MultiPolygon.

```
"intersects": {
			"$ref": "https://geojson.org/schema/Geometry.json"
}
``` 

In the example below, only a Point search is supported.

```
:[example-geometry](.\examples\geometry-queryables.json)
```



## 2.3 Search Request


> **CEOS-STAC-BP-005 - Supported search parameters [Requirement]**<a name="BP-005"></a>
>
> The STAC-API, OGC API-Features and OGC API-Records specifications define a list of fundamental search parameters.  From these specifications, a CEOS STAC implementation shall support the following
minimum set of search parameters for both “collection” and “granule” levels:
- `limit`  
- `q` (optional for "granule level" search)
- ...
- `ids`
- `bbox` 
- `intersects`
- `datetime` 


## 2.4 Search Response


> **CEOS-STAC-BP-10-1 - Collection search response representation [Requirement]**<a name="BP-010-1"></a>
>
> A Collection search response (or /collections response in case "search" is not supported) shall be represented as `application/json` as defined by [OGC API-Features](#AD8) and extended by [STAC API-Collections](https://github.com/radiantearth/stac-api-spec/tree/master/collections).


> **CEOS-STAC-BP-10-2 - Item search response representation [Requirement]**<a name="BP-010-2"></a>
>
> An Item search response shall be represented as a GeoJSON FeatureCollection according to version v1.0.0-rc1 of the ["STAC API ItemCollection Specification"](https://github.com/radiantearth/stac-api-spec/blob/master/fragments/itemcollection/README.md).


> **CEOS-STAC-BP-XXX-1 - Item representation [Recommended]**<a name="BP-XXX-1"></a>
>
> Individual granules in a search response shall be represented as STAC Items according to version v1.0.0 of the ["STAC Item Specification"](#AD5).


Search-by-id is supported by default when adhering to the applicable STAC specifications.  The CEOS Best Practices below are provided for completeness, but do not represent additional requirements.

> **CEOS-STAC-BP-011-1 - Allow for collection search-by-id [Requirement]**<a name="BP-011-1"></a>
>
> The $.collections[].id property in a collection search response shall allow to navigate to a single collection using the `ids` search parameter (rel='search') or as a path parameter (rel='items') /collections/{id}. 

> **CEOS-STAC-BP-011-2 - Allow for granule search-by-id [Requirement]**<a name="BP-011-2"></a>
>
> The $.features[].id property in a granule search response shall allow to navigate to a single granule using the `ids` search parameter (rel='search') or as a path parameter (rel='items') /collections/{collection-id}/items/{id}. 

Search-by-id makes the following use cases possible:

- Put a link on a Web page pointing to a single catalog item (using a URL) to
illustrate a particular event (e.g. an earthquake in the Himalaya).
- The ability to bookmark and retrieve a single item.


> **CEOS-STAC-BP-011B-1 - Result set navigation collections [Recommended]**<a name="BP-011B-1"></a>
>
> The $.links array in a collection search response shall include Link objects for navigating the search result set or collection list when the result set is too large to fit a single response using hyperlinks rel='self', rel='next', rel='prev', rel='first', rel='last' and type=`application/json`. 


> **CEOS-STAC-BP-011B-2 - Result set navigation granules [Requirement]**<a name="BP-011B-2"></a>
>
> The $.features\[\].links array in an item search response shall include Link objects for navigating the search result set when the result set is too large to fit a single response using hyperlinks rel='self', rel='next', rel='prev', rel='first', rel='last' and type=`application/geo+json`. 


> **CEOS-STAC-BP-011B-3 - Result set navigation [Requirement]**<a name="BP-011B-3"></a>
>
> Implementations may decide to only implement forward traversal via navigation/paging links.  Implementations shall implement the navigation links for a number of border cases as defined in the table below.


| **Use case**   | **first** |  **prev** |  **self** | **next** | **last** |
| --------   | --------- | --------- | --------- | --------- | --------- |
| First page | Optional |  Not allowed |  Mandatory  | Mandatory  |  Optional |
| Middle pages | Optional |  Optional | Mandatory  | Mandatory  | Optional  |
| Last page | Optional |  Optional | Mandatory  |  Not allowed  | Optional  |
| Empty result set | Not allowed  |  Not allowed |  Mandatory  | Not allowed  |  Not allowed |
| Single page | Optional |  Not allowed |  Mandatory  | Not allowed  |  Optional |



### Item assets



> **CEOS-STAC-BP-012-1 - Reference to metadata [Recommended]**<a name="BP-012-1"></a>
>
> STAC implementations are
encouraged to include Link objects in Items or Collections with rel="alternate" or rel=”via” for detailed
representation of the metadata. (The “via” relation should be preferred to convey the authoritative resource or the source of the information from where the Item or Collection is made.)


> **CEOS-STAC-BP-012-2 - Metadata assets [Requirement]**<a name="BP-012-2"></a>
>
> STAC implementations are
encouraged to include Asset objects in Items or Collections with role="metadata" and type identifying the media type representation of the metadata. 

| **role**                   | **type** |  **description** | 
| --------                   | --------- | --------- |
| `metadata` | `application/vnd.iso.19139+xm` |  Granule metadata file in ISO 19139 format. |
| `metadata` | `application/gml+xml;profile=http://www.opengis.net/spec/EOMPOM/1.1` |  Granule metadata file in OGC 10-157r4 format. |





> **CEOS-STAC-BP-012B - Provenance information [Optional]**<a name="BP-012B"></a>
>
> STAC implementations using syndication/aggregation of search results provided by third-
party catalogs are recommended to include provenance information (at the item search response level) using
a Link object with rel=”via” and an href attribute referring to the original result set (See http://www.openarchives.org/rs/1.0/resourcesync#RePub).


> **CEOS-STAC-BP-012C - Reference to documentation [Recommended]**<a name="BP-012C"></a>
>
> STAC implementations should use a Link object with rel="describedby" to reference from a collection to its documentation or from an item to its documentation.

Note: although some implementations use rel="about" for the same purpose, rel="describedby" is recommended by https://docs.ogc.org/DRAFTS/20-024.html.



> **CEOS-STAC-BP-012D - Relation and role attribute values [Recommended]**<a name="BP-012D"></a>
>
> CEOS STAC implementations are recommended to use the following relation attribute
values for Link objects and role values for Asset objects when describing artifacts associated with a resource.  When the information can be represented as an Asset or a Link object, the object indicated by (*) is preferred. 


. Relation attribute values
| **Artifact**   | **description** |  **relation** (Link) |  **role** (Asset) |
| --------                   | --------- | --------- | --------- |
| Metadata | Metadata file in format indicated by media type. | `via` or `alternate` (*) |  `metadata` (*) |
| Thumbnail | Thumbnail image | `preview`  | `thumbnail` (*) | 
| Browse |  Quicklook or browse image. Image of the data typically used for making data request decisions. | `icon` | `overview` (*) |
| Data |  Data file or other science data resource; may be large in size. |  `enclosure` | `data` (*) |
|  |   |  `search` |  |
|  |   |  `items` |  |
|  |   |  `canonical` |  |
|  |   |  `self` |  |
| STAC Catalog or Collection |   |  `root` |  |
| STAC Catalog or Collection |   |  `parent` |  |
| STAC Collection |   |  `collection` |  |
| STAC Item |   |  `derived_from` |  |


> **CEOS-STAC-BP-012E - Link and Asset type attributes [Recommended]**<a name="BP-012E"></a>
>
> CEOS STAC implementations shall specify the media (MIME) type of the artifact
associated with a resource by specifying the "type" attribute of the Link object or Asset object.

This requirement allows a client to determine which resource type a HATEOAS Atom link refers
to. E.g. a "describedby" link may point to a human readable representation or to an ISO 19139
service metadata file of the INSPIRE Download service. Clients can make this distinction by
checking the media type provided in the "type" attribute.


The table below list some frequently used formats and their corresponding media type(`type`).

| **Format**                   | **type** |   
| --------                   | --------- | 
| [ISO19139](https://www.iso.org/standard/32557.html)      | `application/vnd.iso.19139+xml` |  
| [ISO19139-2](https://www.iso.org/standard/57104.html)      | `application/vnd.iso.19139-2+xml` | 
| [ISO19115-3](https://www.iso.org/standard/32579.html)      | `application/vnd.iso.19115-3+xml` | 
| [ISO19157-2](https://www.iso.org/standard/66197.html)      | `application/vnd.iso.19157-2+xml` | 
| [OGC 10-157r4](https://docs.opengeospatial.org/is/10-157r4/10-157r4.html)  | `application/gml+xml;profile=http://www.opengis.net/spec/EOMPOM/1.1`  |
| [OGC 17-003r2](https://docs.opengeospatial.org/is/17-003r2/17-003r2.html)  | `application/geo+json;profile=http://www.opengis.net/spec/eo-geojson/1.0`  |
| [Dublin Core](http://www.loc.gov/standards/sru/recordSchemas/dc-schema.xsd)  | `application/xml`  |
| [Markdown](https://datatracker.ietf.org/doc/html/rfc7763)  | `text/markdown`  |





---
**ISSUE**

Although the `datetime` search parameters can represent a single time or a time interval, the STAC `datetime` Item property is limited to single dates and cannot represent a time intervale according to [its description](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#datetime) and the corresponding [STAC Best Practices](https://github.com/radiantearth/stac-spec/blob/master/best-practices.md#datetime-selection).  Instead, [STAC proposes two separate properties to represent a time range](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#date-and-time-range): `start_datetime` and `end_datetime`.  

---

> **CEOS-STAC-BP-013B - Temporal extents [Recommended]**<a name="BP-013B"></a>
>
> STAC implementations should represent temporal extents in Items with the `start_datetime` and `end_datetime` properties and include the value for `start_datetime` also as `datetime` property.


> **CEOS-STAC-BP-014 - Geographical extents [Requirement]**<a name="BP-014"></a>
>
> STAC implementations shall represent geographical extents of Items with the `geometry` property (GeoJSON Geometry object or null if not available).

Geographical extents of Items are represented using GeoJSON geometry objects [RFC7946](#AD4) in STAC item search responses.  This representation can natively represent multi-point, multi-line and multi-polygon geometries, thus no additional guidance similar to `CEOS-BP-014B`, `CEOS-BP-014C` and `CEOS-BP-014D` is required.


> **CEOS-STAC-BP-014E - Minimum-bounding rectangle [Requirement]**<a name="BP-014E"></a>
>
> CEOS implementations should render spatial extents using a minimum-bounding
rectangle (MBR) with a GeoJSON `bbox` property [RFC7946](#AD4) in addition to the native more accurate
representation of that extent with the `geometry` property. The value of the bbox
element must be an array of length 4 (two long/lat pairs), with the southwesterly point followed by the northeasterly point.

The `bbox` item property is mandatory according to the STAC Item specification unless `geometry` is null.


> **CEOS-STAC-BP-015 - Browse image [Recommended]**<a name="BP-015"></a>
>
> We recommend that CEOS implementations provide a URL to the granule’s browse image when available, via an Asset object with role=`overview`.


> **CEOS-STAC-BP-016 - Data access [Recommended]**<a name="BP-016"></a>
>
> We recommend that CEOS implementations provide data access URL for the granule via an Asset object with role=`data`.


> **CEOS-STAC-BP-016B - Data access to multiple files [Optional]**<a name="BP-016B"></a>
>
> When data access to a granule in a granule search response is to be provided in multiple physical files, each file should be linked to via a separate Asset object with role=`data`.



.Example: Asset object for Cloud Optimized GeoTIFF data
```
"assets": {
        "analytic": {
            "href": "https://storage.googleapis.com/sample-cogs/cog/20210515_145754_03_245c_3B_AnalyticMS.tif",
            "type": "image/tiff; application=geotiff; profile=cloud-optimized",
            "title": "4-Band Analytic",
            "roles": [
                "analytic", "data"
            ]
        }
```

.Example: Asset object for Zarr data
```
 "assets": {
    "zmetadata": {
      "roles": [
        "data",
        "metadata",
        "zarr-consolidated-metadata"
      ],
      "description": "Consolidated metadata file for Zarr store",
      "href": "https://storage.sbg.cloud.ovh.net/v1/AUTH_d40770b0914c46bfb19434ae3e97ae19/hdsa-public/prisma_v2/20200410/.zmetadata",
      "type": "application/json"
    },
```


> **CEOS-STAC-BP-017 - Exception codes [Recommended]**<a name="BP-017"></a>
>
> CEOS STAC API implementations are recommended to use the HTTP status codes as required by the underlying [OGC API-Features - Part 1:Core](#AD8) specification (§7.5).




