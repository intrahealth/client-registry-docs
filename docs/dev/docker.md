# Test and Demo using Docker

!!! Time to complete
    10 Minutes

The easiest way to get started with OpenCR is to use Docker to launch ElasticSearch and HAPI FHIR Server and run the OpenCR Service directly. By running the OpenCR Service directly, it is easy to revise and reload decision rules.

These instructions have been tested on Linux and macOS.

## Prerequisites

* Docker.
* Java runtime version 8.
* Node and NPM.

## Instructions

* Clone the repository and change directory into the root folder.
```
https://github.com/intrahealth/client-registry.git
cd client-registry
```

* Ensure that Docker is installed and running.
```
docker --version
```

* Start ElasticSearch and HAPI FHIR Server using Docker. 

!!! warning 
    You cannot use the existing hosted ElasticSearch image because OpenCR requires a plugin to be installed. The docker-compose file provided uses the Dockerfile-es which builds an ES image with the plugin.
```
docker-compose -f docker-compose.dev.yml up fhir es
```

* Switch to a new terminal window. Install the requirements for the OpenCR Service.
```
cd server
npm install
```

* Copy a configuration for Docker for the OpenCR Service to use.
```
cp config/config_docker_template.json config/config_docker.json
```

* Run the server using the docker config for NODE_ENV.
```
# from client-registry/server
sudo NODE_ENV=docker node lib/app.js
```

Visit the UI at: [https://localhost:3000/crux](https://localhost:3000/crux)

Default username: root@intrahealth.org
Default password: intrahealth

OpenCR may require access to /var/log for logging. This requirement may be changed in the future.