The [Cardstack Hub](https://medium.com/cardstack/what-is-the-cardstack-hub-1c9a9e3df343) provides a “data fusion” layer for decentralized applications built using Cardstack. Using the Hub, each Cardstack application can use off-the-shelf plugins in order to ingest data from some particular data source — be it a cloud service or a blockchain — into the Cardstack Hub’s index.

## CQRS architecture

Cardstack plugins follow a [CQRS](https://martinfowler.com/bliki/CQRS.html) architectural pattern.

CQRS stands for Command Query Responsibility Segregation.
To summarize, in CQRS, the "source of truth" database and API is separate from the data source that serves information to the outside world.

For example, many large companies have a huge, slow database that contains information from all their customers, ever. Querying that database is painfully slow, and access to it is limited. So, instead, in a CQRS architecture, you would create a caching layer that retrieves and stores the data that is most likely to be used, so it can be returned in milliseconds. That data can also be serialized to be a good fit for the front end. The serialization can also account for variations in the data as changes were made across time to how the company stored and used data.

Cardstack plugins offer these kind of benefits.
A plugin defines where to make a request, what information to get, and how long to keep the cache before requesting updated information.

## Hub Plugins

Examples of plugins for different data sources that we have built so far are:

* [PostgreSQL plugin](https://github.com/cardstack/cardstack/tree/master/packages/postgresql) that can ingest content from a PostgreSQL database,
* [Git plugin](https://github.com/cardstack/cardstack/tree/master/packages/git) that can ingest content from a Git repository (e.g. GitHub)
* Drupal plugin that can ingest content from a Drupal content management system
* [Ethereum plugin](https://github.com/cardstack/cardstack/tree/master/packages/ethereum) that can ingest content from smart contracts (including Oracles) running on Ethereum
* [S3 plugin](https://github.com/cardstack/cardstack/tree/master/packages/s3) for saving files to AWS

Additionally, we have a development kit that can be used to create new data source plugins for ingesting content from other sources.

When the Cardstack Hub is booted, the very first thing it will do is to start crawling the directory structure of the application to discover the various plugins that are installed and the different features available for the plugin. The most basic feature that any data source plugin must have is an indexing feature.

The **_indexing_ feature’s** main job is to ingest content from a data source and place the content in the Cardstack _index_. To accomplish this, the _indexing_ feature describes to the Cardstack Hub the schema of the data source: what are the different entities contained by the data source, what are the attributes of the various entities, and how are those entities related to one another.

Additionally, the indexing feature contains logic that knows how to ingest the content held by the data source, and to ingest the content in such a way that we are not doing wasted work, so that if we have previously ingested the content, we won’t waste time trying to re-ingest the information that we already know about.

The information that we ingest into the Cardstack Hub is placed in the **Cardstack Hub’s _index_**. The _index_ is a fast means to query the content based on the schema of the content that was indexed. This means that once the content is indexed we can now search on the data and explore its relationships in a manner that is more sophisticated and performant than if an application were to talk to the original data source directly.

The index can then be exposed to the outside world via our **RESTful API** which utilizes a system of permissions and grants to ensure that users and groups are only authorized to view and manipulate resources for which they have been granted access.

Plugins can have [other features](https://medium.com/cardstack/what-is-the-cardstack-hub-1c9a9e3df343) too. A _writer_ feature allows the Cardstack Hub to write information back to the data source. A _searcher_ feature allows the Cardstack Hub to perform a search against the backing data source. A _middleware_ feature allows the plugin to register API endpoints to perform specialized tasks for the plugin. An _authenticator_ features allows the Cardstack Hub to authenticate sessions. 
