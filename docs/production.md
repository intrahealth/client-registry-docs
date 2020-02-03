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

See, for example, the [Guide to General Server Security: Recommendations of the National Institute of Standards and Technology](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-123.pdf) by Karen Scarfone, Wayne Jansen, Miles Tracy, July 2008, (NIST Special Publication 800-123).

## IT Resource Planning

Benchmarking will be completed in future phases to make recommendations for medium to heavy workloads. The below resource suggestions should be revised based on benchmarking for the particular context into which the Client Registry is being deployed.

### Servers

For an MVP in a production environment where potential data loss is acceptable, a single large server can be used. ES has high memory requirements. 

### CPU

* Local development: PC with multiple cores
* Production: Use 2-8 cores.

### Memory

Memory usage depends on the number of records and the performance required.

Local development on a PC: 8GB of RAM should be available

Production, at minimum: 32GB with 24GB free for the Client Registry is recommended for light loads if using one VM.

* 16GB minimum for ElasticSearch with 32GB preferred or 64GB for high volume: Follow the guidelines provided by the maintainers [here](https://www.elastic.co/guide/en/elasticsearch/guide/current/hardware.html#_memory). 
* 8GB for OpenHIM, mediator, Postgres, and HAPI FHIR Server.


### Disk Space

This depends heavily on the workload. Expect 200GB at minimum per node.