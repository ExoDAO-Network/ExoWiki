# Lucene vs. re-Isearch

Lucene is a very popular engine with a massive installed base. It is the motor behind ElasticSearch, Solr, Neo4j and many other products. To compare Lucene with re-Isearch is really to compare potatoes with fish. They have quite different histories, design considerations and goals. The following short sketch attempts, however, to outline a few points since we've been asked.

### Design Target

* Lucene was designed to be a reasonably flexible fulltext search engine with support for fields. Its a more or less traditional unstructured text search system using an optimized inverted index.&#x20;
* re-Isearch was designed to be an object (neither text nor data centric) oriented search engine for abstract objects and structural paths (including overlays). Its been designed to try to manage wildly heterogeneous formats and extract also implicit structure. re-Isearch supports XML, SGML and other formats as-if native.
* Lucene is packaged in a number of solutions like ElasticSearch, Solr and Neo4j. They all have applications that are nearly turn-key to make it easy to get running. Solr is bundled as the built-in search in many applications like CMSs and ECMs, Cloudera’s Hadoop,...
* Re-Isearch is not a complete solution. Its not a turn-key application. Its like Lucene merely enabling technology. Despite the command line tools supplied and the available information servers (SRU/W, ISO23950/Z.2950 etc.) its really just a library and some scaffolding

### Platform

* Lucene can run on platforms providing Java. Each operating platform has to provide a JVM. There are some open source takes on Java: primarily OpenJDK variants and Jikes Research VM.
* Re-Isearch can be compiled and run on nearly any POSIX compatible operating system. It uses a simplified subset of C++ and has minimal dependencies.

### Data Import (Document Formats)

* Lucene’s tokenizer does not really support any document formats natively. It depends upon analyzers and these too don’t really support any formats. Instead Lucene (Solr) depends upon external libraries such as Apache Tika.
* Re-Isearch lets one load data as is. There is no need to define a schema in advance. Re-Isearch’s, to use Lucene’s nomenclature, tokenizer and analyers (which we call doctypes) are firmly intertwined into the engine’s architecture.  It natively indexes a large number of formats like SGML/XML, emails, BibTeX, Medline and over 60 other formats of which many dozens are fundamental such as token seperated values, paragraph text which allow through simple configuration their applications to many 100s of more exotic and proprietary formats.
* Lucene/Solr works with filters to convert or extract metadata.
* Re-Isearch also supports filters to convert formats or extract metadata. It can user any filter as well as, naturally, those based upon Apache Tika or any other conversion filter, including custom one-off scripts. Out-of-the-box it supports, for example, in a number of classes the Pandoc tool for a vast array of formats, filters like catdoc for older Microsoft Word files and xls2ssv for older Microsoft Spreadsheets.
* One adds new Analzers to Lucene by extending Lucene in Java. There is no fool proof protection of intellectual property. Java can always be reverse engineered.
* One can add new Doctypes to re-Isearch easily either directly via a contribution of source code to its built-in core or via a plug-in. Plugins are C or C++ language compiled loadable objects. They can be closed source and proprietary. They don’t need to adhere to the re-Isearch license.
* Heterogenous Collections
  1. Lucene has no direct model. Instead one needs to map schemas together.
  2. Re-Isearch has been designed for just this.

### Market Share

* Lucene has a sizeable market pressence. Lucene has an infrastructure of consultants. ElasticSearch is publically traded and has a market capitalization of more than 14 billion USD.
* Re-Isearch, despite its more than 20 year history, is the “new kid on the block”.

### Java

* Lucene is typically pure Java. Its 100% written in Java. Its more or less Java thread safe but not completely.
* re-Isearch is written in C++. Java is provided via a SWIG created JNI (Java Native Interface) module. Since re-Isearch is written in C++ and interfaces via SWIG, application development is not limited to Java but Python (perhaps the most popular choice and by far the best developed re-Isearch interface), Tcl, Ruby and a number of other languages.
* Lucene needs Java to run (although there are a few forks rewriting the algorithms and structures into other languages). Java is, in our educated opinion, a fine language for developing many kinds of applications (especially given the availability of Java developing talent) but is less than ideal for database, search and retrieval especially on appliances. The Java runtime needs not only at not only at least 128 MB to run but it does not return allocated memory to the host operating system.
* re-Isearch allows for applications to use its algorithms to be written in Java but does not require the use of Java. Our favorite language for writing applications to use re-Isearch is, in fact, Python and not Java.
* Java continues to have some copyright and license issues. Java SE 8 and newer public updates will no longer be available for "Business, Commercial or Production use" without a commercial license.

### Other Scripting Languages

* Lucene is Java and its extended by Java.&#x20;
* Re-Isearch has APIs in a number of languages including C#, C++, D, Go, Guile, Lua, Java, JavaScript (Node.js), Perl. PHP, Python, R, Ruby, Tcl/Tk,…&#x20;

### Search Term Length

* Lucene places limits on the lengths of terms
* re-Isearch is designed to handle terms/words of any length.

### Search Terms/Wild cards/Truncated Search Terms

* Lucene expands wildcards to terms before even searching. Queries are re-written into a more basic form consisting of a set of logically combined TermQuery's. The standard limit on the total number of terms in a query is 1024. For example, if the indexed documents contain the terms "car" and "cars" the query "ca\*" will be expanded to "car OR cars" before the search takes place.Lucene "pre-processes" steps:
  1. A sorted term enumeration is started from the term alphabetically closest to/after the given prefix (the term characters on the left). This enumerates all terms from all existing fields in the index.
  2. For each term, Lucene checks if the term actually starts with the prefix and belongs to the given field. If so the term is added to a BooleanQuery as a TermQuery with OR logic.
  3. The process produced a constructed BooleanQuery which contains exactly as many clauses as there were terms matching the prefix.
* For a WildcardQuery, the process is similar in that the term value string containing wildcard(s) is also expanded to all matching terms for the given field and OR-combined using a BooleanQuery.
* re-Isearch places no similar limits but allows for wildcards in paths, terms etc. Its really a different model. The query ""ca\*" will search for all the terms that start with "ca". If there is a limit defined in the search for time (which may be set to a limit or allowed to be unlimited) or number of records (which can be set to an absolute number, a number as a proportion of the total number of records in the index or unlimited) the search will stop when that mark is passed (default is unlimited number of records).&#x20;
  1. re-Isearch does not expand the query into a boolean OR'd expression but searches directly for the query. This is more direct and allows for query structures to be re-used. With Lucene for each change in an index the Query must be re-parsed (Note: the Lucene query parser is NOT thread safe).
  2. re-Isearch supports not just wildcards but full glob (POSIX 1003.2, 3.13) including some extensions. These can be applied to the entire search expression including path and term components.
* Lucene normally supports only wildcards to the right (prefix queries).
* re-Isearch supports both right and left truncation as well as generic glob expressions.
* Lucene does not normally allow for wildcards in field names
* re-Isearch allows for wildcards (glob expressions) in field names and paths.
* Lucene does not support combinations of phrase and wildcard as in the right truncated phrase search "search optim"\*
* re-Isearch allows for wildcards.

### Query Languages/Interfaces

* Lucene does not per say have a query language since it contains only terms and modifiers (+,-, weight). These may be processed in any order. There are a number of classes to try to convert other languages into Lucene's model.&#x20;
* re-Isearch uses a vector/boolean information retrieval model. Queries are driven by an object oriented automata. These may be created as program objects or parses from any of a number of query languages. At the heart of re-Isearch's model is an RPN stack based language and there are a number a number of classes to convert from other query languages into RPN.&#x20;
* Lucene's boolean query class limits the number of clauses (typ. 1024). This makes sense since Lucene too limits the number of terms in a query (typ. 1024).
* re-Isearch's boolean query class places not limits on the number of clauses, terms, operators etc.
* Lucene is best when not finding records. Since there are no operators and just terms and predicates that apply to them (either weight or modality) they can without worry be easily rearranged. The query: A -B C D +E for instance can be quickly optimized by the constraints "must have" E and "can't have B.&#x20;
* re-Isearch is run by a query automata. It can perform many optimizations but it can't just build upon short circuits and programs will need to run their course except in some simple cases such as all terms "ANDed".

### Structure Search

* Lucene is a traditional inverted index fulltext engine. Its quite good at handling a limited number of fields but is inappropriate for use with arbitary trees.
* re-Isearch is not based on "Inverted file indexes" and uses other algorithms. It has no limits on the length of terms, their frequency and and can support arbitary structures and paths, including overlap.
* The granularity of Lucene (unit of retrieval) is the record as defined at the time of indexing.
* re-Isearch allows for search-time dynamic granularity. The scale of grain (sentence, paragraph, document, chapter, book, collection, source, community, hub, inter-hub bridge, sphere, inter-sphere bridge) is defined by the result of the search and by the query.&#x20;
* The product of a search in Lucene is a identification for the record. To extract elements one must load the document (parse etc.) into an object model that supports addressing elements.
* re-Isearch has no no need for a “middle layer” of content manipulation code. Instead of getting IDs, fetching documents, parsing them, and navigating the DOMs to find required elements, re-Isearch lets you simply request the elements you need and they are returned directly.
