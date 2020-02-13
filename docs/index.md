# Overview

*Welcome! Thank you for taking an interest in the Open Client Registry. This is a community project and meant for others to adopt to their use cases as they wish.*

## What Does It Do?

A Client Registry holds patient identifers and may include patient demographic information. It is a necessary tool for public health use cases that require managing patients, monitoring outcomes, and conducting case-based surveillance.

This Client Registry is an open source and open standards-based implementation that offers the ability to:

* Assign and look-up unique identifiers,
* Allow connections from diverse point of service (POS) systems, such as electronic medical record (EMR) systems, that can submit messages in FHIR, and
* Configure decision rules around patient matching.

!!! caution
    This implementation does not allow point-of-service systems to get patient demographic information stored in the Client Registry. This is also not a Shared Health Record, nor does it contain patient clinical data.

## Use Cases

The Client Registry is one component in a more complex HIS architecture needed to accomplish important use cases, such as:

* **Deduplicating patients**: Sometimes patients have multiple diagnostic results stored within a POS. The Client Registry will link patients based on configurable decision rules so multiple test results for the same patient can be found. 

* **Tracking patients lost to clinical care**: EMRs are often not interoperable with one another, resulting in difficulty tracking patients as they move between facilities to seek care. A Client Registry will help data managers to track patients, decreasing instances of duplicate and incomplete records, patients LTFU, and sub-optimal care. 

!!! caution
    The Client Registry is not deduplicating or even touching patient clinical and demographic records within point-of-service systems. Instead, it provides a way to enable use cases like deduplication - which must be an external process. 

## Workflows

At its core, the Client Registry provides a unique identifier (UID) that also links to all other already matched records from submitting systems. This means that the Client Registry stores an identifier from submitting systems so that it can uniquely identify according to however the submitting systems store their records, but it also produces a UID for the entire domain using the service.

Several workflows are supported out-of-the-box depending on the POS-Client Registry use case. For example:

* A specimen is received by a laboratory. Demographic data and requesting location data is entered into the LMIS. The LMIS queries the Client Registry for a UID. The Client Registry provides the UID if one did not exist and stores limited patient demographic information but **does not** store test results. A use that this enables (but the Client Registry **does not** provide) is the ability to track persons lab results over time.

* A patient is registered at a clinic. The clinician recommends a viral load test. The specimen is sent for processing to the laboratory. The Client Registry receives the UID and specimen and returns a diagnostic result that is then stored in the EMR. 

* A patient is registered at a clinic and has been assigned a UID. In the course of their clinical encounter, a sentinel event occurs, triggering the EMR to send limited clinical information to the Health Information Exchange (HIE). The HIE sends the data to a data analysis warehouse for population analysis and case-based surveillance.

!!! warning
    It is important to note that in the above workflows the Client Registry does not store or provide clinical data. Such processes are external to the Client Registry and must be separately created, governed, and enabled. 

## Architecture

The Client Registry is not one application, instead it's a set of applications that work together in the [Open Health Information Exchange (OpenHIE)](http://ohie.org) architecture to serve point-of-service systems, like EMRs, insurance mechanisms, and labs.

!!! caution
    This is not an OpenHIE product. It is a prototypical client registry to facilitate discussion among a broad set of stakeholders. 

The architecture is made up of both a lite and a production version. In the production version it includes:

* The [**Open Health Information Mediator (OpenHIM)**](http://openhim.org): The OpenHIM is the entrypoint for POS systems, and includes authentication (are you who you say you are?), authorization (what roles do you have permission to fulfill?), and auditing of all transactions.
* The [**HAPI FHIR Server**](http://hapifhir.io): HAPI is the reference FHIR server in Java and scalable into production environments.
* The [**ElasticSearch**](http://elastic.co/products/elasticsearch): Elasticsearch is a powerful search engine that is highly performant.
* An **optional UI** to view and break matches between records, and to select and chain together decision rules around matching algorithms.

![Alt text](images/production_architecture.png "Production Architecture")

The lite architecture is for non-production and does not require OpenHIM or ElasticSearch.

![Alt text](images/lite_architecture.png "Lite Architecture")

