# Full Text Search - Investigating Lucene, Solr, and Elasticsearch

## Introduction

I was recently working on a project that was having some performance issues with search. The team lead asked me to look into whether using a dedicated full text search engine would help. After spending some time reading articles and watching videos, I came across three main options - Lucene, Solr, and Elasticsearch. In this paper, I will explain what each of these tools does, how they are different from each other, and which one I think would be the best fit for our project.

Before going into the tools, it is worth understanding what full text search actually means. In a normal database like MySQL, if you search for a word, it scans through every row to find a match. This gets very slow when the data grows. Full text search engines solve this problem by building something called an inverted index, which basically maps every word to the documents that contain it. This makes searching much faster.

## Lucene

Lucene is a Java library for search. It is not a server or an application you can just install and run. You have to add it to your Java code and use it from there. Both Solr and Elasticsearch are built on top of Lucene, so it is kind of the base layer for all three tools.

I found Lucene a bit hard to get into at first because there is no UI or API. You just work with it through code. Here is a simple example of how indexing works in Lucene:

Java
Directory index = new RAMDirectory();
IndexWriterConfig config = new IndexWriterConfig(new StandardAnalyzer());
IndexWriter writer = new IndexWriter(index, config);

Document doc = new Document();
doc.add(new TextField("title", "Full text search with Lucene", Field.Store.YES));
writer.addDocument(doc);
writer.close();

Lucene is powerful but requires a lot of setup. I would not recommend it unless your team is comfortable with Java and wants very low-level control over how search works.

### Key Points

* It is a library, not a server.
* You need Java knowledge to use it.
* No built-in HTTP API or clustering support.
* Very fast and flexible if used correctly.

## Solr

Solr is built on top of Lucene and wraps it into a proper search server. You can send documents to it using HTTP requests and get search results back in JSON or XML format. This makes it much easier to work with compared to raw Lucene.

One thing I noticed about Solr is that it is very popular in enterprise settings. Companies that deal with large document libraries, like news websites or e-commerce platforms, tend to use it. It has good support for things like filtering, faceting (which means grouping results by category), and spell checking.

Solr also has a mode called SolrCloud which allows you to run it across multiple servers for better performance and availability.

### Key Points

* Runs as a server with an HTTP API.
* Good for structured document search.
* Has faceting, spell check, and autocomplete built in.
* SolrCloud supports distributed setups.
* Schema management is more manual compared to Elasticsearch.


## Elasticsearch

Elasticsearch is also built on Lucene, but it feels more modern compared to Solr. It stores data as JSON documents and has a very clean REST API. Setting it up is straightforward, and it automatically handles distributing data across multiple nodes.

What I liked most about Elasticsearch is that you do not need to define a schema before indexing. It figures out the data types on its own. It is also widely used for logging and monitoring, which is why it is often paired with Kibana (for visualization) and Logstash (for collecting logs), together called the ELK Stack.

Here is a simple example of indexing a document using the Elasticsearch REST API:

POST /products/_doc/1
{
  "name": "Running Shoes",
  "description": "Lightweight shoes for long distance running"
}

And searching for it:

GET /products/_search
{
  "query": {
    "match": {
      "description": "running"
    }
  }
}

This is much simpler to work with compared to writing Java code for Lucene.

### Key Points

* Distributed by default with automatic sharding.
* Schema-free JSON document storage.
* Simple and powerful REST API.
* Great for real-time search and log analytics.
* Large community and good documentation.


## Which One Should We Use?

After going through all three, here is my understanding:

1. Lucene is the base for everything but requires too much manual work. It is not practical for our case.
2. Solr is good but feels more suited for document-heavy enterprise applications with fixed schemas.
3. Elasticsearch fits best for our project because it scales easily, the API is simple to use, and it handles real-time search well.

My recommendation is to go with Elasticsearch. It would solve the performance issue we are facing and also leaves room for future growth without needing major changes.


## References

* [Apache Lucene Official Website](https://lucene.apache.org/)
* [Apache Solr Official Website](https://solr.apache.org/)
* [Elasticsearch Official Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
* [What is Full Text Search - Elastic Blog](https://www.elastic.co/what-is/full-text-search)
* [Solr vs Elasticsearch Comparison - Sematext](https://sematext.com/blog/solr-vs-elasticsearch-differences/)
* [Getting Started with Lucene - Baeldung](https://www.baeldung.com/lucene)
* [ELK Stack Overview - Elastic](https://www.elastic.co/what-is/elk-stack)
* [Inverted Index Explained - GeeksforGeeks](https://www.geeksforgeeks.org/inverted-index/)
