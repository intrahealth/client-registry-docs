# Installation (Only for Testing CR Service app)

!!! warning
    This guide is for demonstrations or tests only, not for production environments.

!!! note
    This installation method requires familiarity with the command line.

## HAPI FHIR Server CLI

For non-production environments, the HAPI maintainers provide a simple CLI-based tool to run it.

The only required dependency is Java >= 8 (1.8).

See [HAPI FHIR CLI](https://smilecdr.com/hapi-fhir/docs/tools/hapi_fhir_cli.html) for instructions for the OS of choice.

The Client Registry requires FHIR version R4 and HAPI must be started for this version. To run HAPI:
```
hapi-fhir-cli run-server -v r4
```

The HAPI Web Testing UI is available at [http://localhost:8080/](http://localhost:8080/) The Web Testing UI should be disabled for production. It allows the viewing of any resource on the server.

The FHIR Base URL is at [http://localhost:8080/baseR4/](http://localhost:8080/baseR4/)

Visit [http://localhost:8080/](http://localhost:8080/) to ensure HAPI is up and running or
```sh
curl -X GET "localhost:8080/baseR4/Patient?"
```

## ElasticSearch (Optional)

By default, the configuration does not require ES but including it will enable ES-based matching.

Install and start ES for the intended OS. See the [ES install instructions](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)

The required version is >=7.5.

The phonetic analysis package must be installed.

```
/usr/share/elasticsearch/bin/elasticsearch-plugin install analysis-phonetic
```

Once installed and started, ensure that ES is up and running:
```sh
curl -X GET "localhost:9200/_cat/health?v&pretty"
```
Status should be yellow for a single-node cluster.

## Client Registry Service (Standalone)

Clone the repository into a directory of choice.
```
git clone https://github.com/intrahealth/client-registry.git
```

Enter the server directory, install node packages.
```
cd client-registry/server
npm install
```

Copy and edit the configuration file to your liking.
```
cp config/config_development_template.json config/config_development.json
# edit the servers...
```

The minimum changes to start a running standalone system are:

* Change `fhirServer.baseURL` to "http://localhost:8080/baseR4/"

Run the server from inside client-registry/server:
```
node lib/app.js
```

Congratulations! Now it's time to run a [query](queries.md).
