# Installation (Non-Production)

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

Once installed and started, ensure that ES is up and running:
```sh
curl -X GET "localhost:9200/_cat/health?v&pretty"
```
Status should be yellow for a single-node cluster.

## Mediator

Clone the repository into a directory of choice.
```
git clone https://github.com/openhie/client-registry.git
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

## Troubleshooting


**Symptom**: The app does not run due to an error accessing `client-registry/server/lib/../../resources/SearchParameter`. The solution is to create an empty folder for `client-registry/resources/SearchParameter`.


**Symptom**: There is an error trying to use the Structure Definition for patient. This is because the SD for patient does not exist.
```
{ resourceType: 'OperationOutcome',
  text:
   { status: 'generated',
     div:
      '<div xmlns="http://www.w3.org/1999/xhtml"><h1>Operation Outcome</h1><table border="0"><tr><td style="font-weight: bold;">ERROR</td><td>[]</td><td><pre>Resource StructureDefinition/Patient not found, specified in path: Basic.subject</pre></td>\n\t\t\t\t\t\n\t\t\t\t\n\t\t\t</tr>\n\t\t</table>\n\t</div>' },
  issue:
   [ { severity: 'error',
       code: 'processing',
       diagnostics:
        'Resource StructureDefinition/Patient not found, specified in path: Basic.subject' } ] }
```
The solution is to load an SD for Patient. This can be done directly using the HAPI FHIR Web Testing UI for Structure Definition resource --> CRUD Operations --> UPDATE and indicating the id as Patient, and pasting in the SD for patient. Alternatively, hapi-fhir-cli offers a tool to update all SD and ValueSets. In another terminal:
```
hapi-fhir-cli upload-definitions -v r4 -t http://localhost:8080/baseR4/
```
There may be some errors, but the correct patient SD will be loaded.

After a successful load the correct output is:
```
{ resourceType: 'Basic',
  id: 'patientreport',
  meta:
   { versionId: '1',
     lastUpdated: '2020-01-28T10:58:16.860+00:00',
     profile:
      [ 'http://ihris.org/fhir/StructureDefinition/iHRISRelationship' ] },
  extension:
   [ { url:
        'http://ihris.org/fhir/StructureDefinition/iHRISReportDetails',
       extension: [Array] } ],
  code: { coding: [ [Object] ], text: 'iHRISRelationship' },
  subject: { reference: 'StructureDefinition/Patient' } }
```


