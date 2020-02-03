# Installation (Production)

!!! warning
    Installing and maintaining a production installation is not trivial. This installation method requires strong familiarity with the command line and expertise administering Linux environments. 

The production stack consists of 10 components:

* OpenHIM core and OpenHIM admin console. Requires MongoDB and Nginx or another reverse proxy.
* Client Registry Service as an OpenHIM mediator.
* HAPI FHIR Server with a database backend.
* ElasticSearch (>=7.5)
* Client Registry UI
* Kibana Dashboard

The expected operating system is Linux for production. Only Ubuntu versions have been tested.

It is critical that systems administrators note the version compatibilities outlined below. This guide does not cover most aspects of enterprise systems administration, rather it attempts to cover this Client Registry platform. If there are key areas missing, please open an [issue on GitHub](https://github.com/intrahealth/client-registry/issues/new).

## Prerequisites

* If entities outside of your LAN are connecting to the Client Registry, you will need a public-facing domain name. This is necessary for a certificate which is required for any interactions.
* See [production considerations]()

## OpenHIM

OpenHIM supports the last 2 versions of NodeJS LTS and requires MongoDB.

* Follow the [instructions](http://openhim.org/docs/installation/npm) to install OpenHIM core and admin console. The maintainers use the NPM PPA installation method.
* Note the important step to obtain a certificate immediately after installation. The configuration should be that any client must have a certificate and the server has a certificate (mutual TLS).
* Follow the instructions including console configuration. 
* Note the important step to change the console password. It is also recommended that the console only be accessible on a local subnet and not to the WAN.

## HAPI FHIR Server and Postgres

HAPI FHIR should use a database backend in production. HAPI FHIR stores the patient demographic data from queries. If the data is lost, then the Client Registry is unrecoverable.

* Follow the [JPA Server information](https://hapifhir.io/hapi-fhir/docs/server_jpa/get_started.html) and [instructions](https://github.com/hapifhir/hapi-fhir-jpaserver-starter) for how to customize the hapi.properties file and build the server using maven.
* The ES integration is separate from HAPI FHIR Server, so there is not need to use it as an indexer. ES only works with an old version of ES.
* Install and configure the preferred database. Postgres has been tested by the maintainers but any database should work that HAPI supports. Change default passwords on the database.
* Database replication should be encrypted.
* Confirm that HAPI accepts requests. 
* The web interface for HAPI should be disabled for privacy reasons.

!! In production, Postgres should run on multiple nodes with replication. This is to ensure high availability and backups of the data.

## ElasticSearch

* Follow the instructions for [installation](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html)
* Systemd is the preferred system and service manager. There are commands to initiate systemd and journalctl.

!! warning
    ES is not production-ready when run one a single node. It is recommended to run ES on several nodes. Those nodes can also run followers of Postgres.

## Client Registry UI

Under construction.

## Kibana Dashboard

Under construction.