# Production Considerations


This page is under construction.

!!! warning
    Server and network hardening and production best practices are out of scope. This document only attempts to capture aspects relevant to the Client Registry.

Hardening and production best practices include:

* Removing unnecessary services, software, network protocols
* Backup and recovery
* Patches
* Vulnerability scanning
* Limiting remote administration
* Managing open internal and external ports
* Auditing, logging software

See the Guide to General Server Security: Recommendations of the National Institute of Standards and Technology
(Karen Scarfone Wayne Jansen Miles Tracy, July 2008, NIST Special Publication 800-123)

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

