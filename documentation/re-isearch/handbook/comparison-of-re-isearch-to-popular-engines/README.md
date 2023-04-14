# Comparison of re-Isearch to Popular Engines

## Abstract

There are a vast number of engines on the market. On the commerical side there is, for example, MarkLogic; there are RBMSs with fulltext support like SQLlite and the extraordinary PostgreSQL; and on the “open source” side there engines like ElasticSearch, Solr and Neo4j. The last 3, interestingly, are not really engines but are applications based around the Lucene engine. This study thus compares re-Isearch to Lucene.

### Apache Solr

Apache Solr is an open-source search server built on top of Apache Lucene that provides all of Lucene’s search capabilities through HTTP requests. It has been around for almost a decade and a half, making it a mature product with a broad user community.

Solr uses request handlers to ingest data from XML files, CSV files, databases, Microsoft Word documents, and PDFs. It provides native support for the Apache Tika library.

A Solr configuration requires at least 512 MB of HEAP memory to allocate to instances.

Solr is licensed with the Apache 2.0 License. It is an official Apache project.

### ElasticSearch

Elasticsearch is also an open-source search engine built on top of Apache Lucene. It extends Lucene’s indexing and search functionalities using RESTful APIs, and it archives the distribution of data on multiple servers using the index and shards concept. Elasticsearch is completely based on JSON.  It supports data ingestion from multiple sources using the Beats family (lightweight data shippers available in the ELK Stack) and Logstash.

The default Elasticsearch configuration requires 1GB of HEAP memory.&#x20;

Elastic started off under the Apache 2.0 license but is now dual licensed under both the Elastic License and SSPL. While the former disallows providing the software to third parties as a hosted or managed service, SSPL allows it but requires anyone who wants to offer ElasticSearch as a service to either release all surrounding infrastructure as SSPL or get a commercial license. &#x20;

### MarkLogic

MarkLogic is a commerical search system originally developed for XML. It indexes the content and structure of documents including words, phrases, relationships, and values in over 200 languages with tokenization, collation, and stemming for core languages. It supports search across its data and metadata using a word or phrase and incorporates Boolean logic, stemming, wildcards, case sensitivity, punctuation sensitivity, diacritic sensitivity, and search term weighting.

### Rust Based

1\) Tantivy: https://github.com/quickwit-oss/tantivy

Tantivy is a full text search engine library written in Rust.

It is closer to Apache Lucene than to Elasticsearch or Apache Solr in the sense it is not an off-the-shelf search engine server, but rather a crate that can be used to build such a search engine.

Tantivy is, in fact, strongly inspired by Lucene's design.

2\) Bayard: [https://github.com/bayard-search/bayard](https://github.com/bayard-search/bayard)

Bayard is a full-text search and indexing server written in Rust built on top of Tantivy that implements Raft Consensus Algorithm and gRPC.

3\) Toshi: [https://github.com/toshi-search/Toshi](https://github.com/toshi-search/Toshi)

Toshi is meant to be a full-text search engine similar to Elasticsearch. Toshi strives to be to Elasticsearch what Tantivy is to Lucene.

4\) Sonic: [https://github.com/valeriansaliou/sonic](https://github.com/valeriansaliou/sonic)

Sonic is a fast, lightweight and schema-less search backend. It ingests search texts and identifier tuples that can then be queried against in a microsecond's time.

Sonic can be used as a simple alternative to super-heavy and full-featured search backends such as Elasticsearch in some use-cases. It is capable of normalizing natural language search queries, auto-completing a search query and providing the most relevant results for a query. Sonic is an identifier index, rather than a document index; when queried, it returns IDs that can then be used to refer to the matched documents in an external database.

5\) Vector [https://vector.dev/](https://vector.dev/)  and [https://github.com/vectordotdev/vector](https://github.com/vectordotdev/vector)

Alternative to Elastic’s Logstash

### Angolia

[https://www.algolia.com/blog/engineering/full-text-search-in-your-database-algolia-versus-elasticsearch/](https://www.algolia.com/blog/engineering/full-text-search-in-your-database-algolia-versus-elasticsearch/)

[https://en.wikipedia.org/wiki/Algolia](https://en.wikipedia.org/wiki/Algolia)

Algolia was founded in 2012 by Nicolas Dessaigne and Julien Lemoine, both originally from Paris, France. It was originally a company focused on offline search on mobile phones. Later it was selected to be part of Y Combinator's Winter 2014 class.

The Algolia model provides search as a service, offering web search across a client's website using an externally hosted search engine

“Subscribe for free, pay based on your usage”

### Typesense

[https://github.com/typesense/typesense](https://github.com/typesense/typesense)

## Core Overview

| <p><br></p>            | Typesense                                                                                                                           | Algolia                                                                   | ElasticSearch                                                                     | Meilisearch                                                                                                                                    |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Source Code            | Fully open source                                                                                                                   | Proprietary closed source                                                 | Source-available, licensed under SSPL                                             | Fully open source                                                                                                                              |
| First Commit           | 2015                                                                                                                                | 2012                                                                      | 2010                                                                              | 2018                                                                                                                                           |
| Built Using            | C++                                                                                                                                 | C++                                                                       | Java                                                                              | Rust                                                                                                                                           |
| Core Search Algorithm  | Built from the ground-up                                                                                                            | Built from the ground-up                                                  | Built on top of Lucene                                                            | Built from the ground-up                                                                                                                       |
| Best Suited For        | Instant Search-as-you-type Experiences for data sets that can fit in RAM, up to 24 TB (or current commercially available RAM size). | Instant Search-as-you-type Experiences for datasets up to 128 GB in size. | General-purpose search & aggregations over petabyte-scale datasets (eg: log data) | Instant Search-as-you-type Experiences for up to a few hundred thousand records, that don't require a production-grade highly-available setup. |
| Primary Index Location | RAM                                                                                                                                 | RAM                                                                       | Disk, with RAM cache                                                              | Disk with Memory Mapped files                                                                                                                  |

\


## Re-Isearch

Re-Isearch is a reborn open-source search engine built on the basis of the Isearch open-source engine and IB (a proprietary fork).  It indexes the content and structure of documents including words, phrases, relationships, and values. It supports search across its data and metadata using a word or phrase and incorporates Boolean logic, wildcards, case sensitivity, punctuation sensitivity, diacritic sensitivity, and search term weighting.&#x20;

The default configuration requires a minimum heap size 3x the size of the largest document it intends to index. It has run well on machines with virtual memory and as little as 8 MB RAM.

The engine has, in various forms (and using differing algorithms), been available since the 1990s and was originally developed to  provide structured field search via the NISO Z39.50 protocol (an international standard client–server, application layer communications protocol for searching and retrieving information from a database that dominates, for example, the library world) . Its well established user community was primarily in the public sector and included many high profile projects such as the U.S. Patent and Trademark Office (USPTO) patent search, the Federal Geographic Data Clearinghouse (FGDC), the NASA Global Change Master Directory, the NASA EOS Guide System, the NASA Catalog Interoperability Project, the astronomical pre-print service based at the Space Telescope Science Institute, The PCT Electronic Gazette at the World Intellectual Property Organization (WIPO), the SAGE Project of the Special Collections Department at Emory University, Eco Companion Australasia (an environmental geospatial resources catalog), Australian National Genomic Information Service (ANGIS), the Open Directory Project and numerous governmental portals.

The engine has a number of extensions that enables search functionality via ISO23950/Z39.50, OASIS SRU/W and OpenSearch REST API.

Re-Isearch is licensed under the Apache 2.0 license. The Z39.50, SRU/W and other servers are licensed under highly permissive MIT style licenses. Basically these means anyone has permission to use, copy, modify, distribute, and sell the software and its documentation, in whole or in part, for any purpose without fee.\


| re-Isearch                                                    | MarkLogic                                               | Elasticsearch                                                                    | Apache Solr                                                              |
| ------------------------------------------------------------- | ------------------------------------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| NoSQL search engine                                           | Operational and transactional Enterprise NoSQL database | A distributed, RESTful modern search and analytics engine based on Apache Lucene | A widely used distributed, scalable search engine based on Apache Lucene |
| NativeXML DBMS, RDF Store, search engine                      | NativeXML DBMS, RDF Store, search engine                | Search engine                                                                    | Search engine                                                            |
| Object DBMS including Spatial                                 | <p><br></p>                                             | <p>Document store</p><p>Spatial DBMS</p>                                         | Spatial DBMS                                                             |
| 1994-2011,  reborn 2021                                       | Since 2001                                              | Since 2010                                                                       | Since 2006                                                               |
| C++                                                           | C++                                                     | Java                                                                             | Java                                                                     |
| Free                                                          | Commerical                                              | Partially Free                                                                   | Free                                                                     |
| Open Source                                                   | Proprietary                                             | Open Source                                                                      | Open Source                                                              |
| XML support                                                   | XML support                                             | JSON Only                                                                        | XML support                                                              |
| Foreign keys, Join                                            | No foreign keys                                         | No foreign keys                                                                  | No foreign keys                                                          |
| Schema-Free                                                   | Schema-Free                                             | Schema-Free                                                                      | Schema                                                                   |
| Multi-language API, Z39.50, SRU/W. CQL, IB Query Language,... | Multi-language API, Xquery, SPARQL,                     | <p>Java API</p><p>RESTful HTTP/JSON API</p>                                      | <p>Java API</p><p>RESTful HTTP/JSON API</p>                              |
| Search during index                                           | Search during index                                     | No                                                                               | No                                                                       |
| Own algorithms                                                | Own algorithms                                          | Based on Lucene (inverted index)                                                 | Based on Lucene (inverted index)                                         |

## Relation DB based

### MantiCore Search

[https://github.com/manticoresoftware/manticoresearch](https://github.com/manticoresoftware/manticoresearch)

Manticore Search is a multi-storage database designed specifically for search, including full-text search.&#x20;

Craigslist, Socialgist, PubChem and many others use Manticore for efficient searching and stream filtering.

Manticore Search was forked from Sphinx 2.3.2 in 2017.

### Sphinx

[https://sphinxsearch.com/](https://sphinxsearch.com/)

Sphinx can be used either as a stand-alone server or as a storage engine ("SphinxSE") for the MySQL family of databases. When run as a standalone server Sphinx operates similar to a DBMS and can communicate with MySQL, MariaDB and PostgreSQL through their native protocols or with any ODBC-compliant DBMS via ODBC. MariaDB, a fork of MySQL, is distributed with SphinxSE.

### Lucene



