# Installation

## Default ports:

* **3000**: Standalone/mediator application
* **9200**: ElasticSearch
* **8080**: HAPI FHIR Server

## Memory Usage

Memory usage depends on the number of records and the performance required.

For local development on a PC:

* PC with multiple cores
* 8GB of RAM should be available

For production, at minimum:

32GB with 24GB free for the Client Registry is recommended for light loads if using one VM.

* 16GB minimum for ElasticSearch with 32GB preferred or 64GB for high volume: Follow the guidelines provided by the maintainers [here](https://www.elastic.co/guide/en/elasticsearch/guide/current/hardware.html#_memory). Use 2-8 cores.
* 8GB for OpenHIM, mediator, Postgres, and HAPI FHIR Server.

Benchmarking will be completed in future phases to make recommendations for heavy workloads.

## How to Install

Under construction.

![](https://media.giphy.com/media/905GG7MjDw61q/giphy.gif)


