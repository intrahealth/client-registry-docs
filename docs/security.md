# Security

It is difficult (and irresponsible) to try to explain all of the best practices in computer security. This page focuses on how security is addressed in the Open Client Registry. For links to information on server and network hardening and production best practices see the [production](production.md) page.

Several areas are addressed, user authentication, node authentication, ATNA auditing, and non-production (demos, tests) configuration.

Secure Node
Any IHE actor which is capable of authenticating itself to other nodes and transmitting data securely.
Audit Repository
Responsible for the storage of audit events

[IHE ITI](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_TF_Vol2a.pdf)

The IHE IT Infrastructure (ITI) domain addresses the implementation of standards-based interoperability solutions to improve information sharing, workflow and patient care.

## User Authentication


## Node Authentication

Authenticate Node [ITI-19]


## ATNA Logging

Audit Trail and Node Authentication (ATNA) Integration Profile is 


## Non-Production

```sh
# create a private key with pem extension
openssl req -newkey rsa:4096 -keyout dhis2_key.pem -out dhis2_csr.pem -nodes -days 365 -subj "/CN=dhis2"
# generate a 
openssl x509 -req -in dhis2_csr.pem -CA ../certificates/server_cert.pem -CAkey ../certificates/server_key.pem -out dhis2_cert.pem -set_serial 01 -days 36500
# 
openssl pkcs12 -export -in dhis2_cert.pem -inkey dhis2_key.pem -out dhis2.p12
```



The `.p12` filename extension is for [PKCS #12](https://en.wikipedia.org/wiki/PKCS_12) file archives. PKCS #12 is for storing several cryptographic objects together including user-defined values. In the provided example .p12 files in the Client Registry repository, they include an X.509 (public key) certificate and a private key, and some information about the owner, including, for the Client Registry, the `subject`, meaning the name of the system (such as a specfic EMR POS system) that it was issued for.

A useful way to understand this better is to open one of the .p12 files using no password (hit enter) for the Import Password, and the subject name for the PEM pass phrase (in this example 'openmrs' without quotes and in lowercase).

When certificate is self-signed, then issuer and subject field contains the same value

```sh
# from client-registry/server/sampleclientcertificates
$ openssl pkcs12 -info -in openmrs.p12
Enter Import Password:
MAC Iteration 2048
MAC verified OK
PKCS7 Encrypted data: pbeWithSHA1And40BitRC2-CBC, Iteration 2048
Certificate bag
Bag Attributes
    localKeyID: 36 7A 50 BC 1C 0E 69 93 22 7F CC FB 4D 07 C2 BE B2 37 02 C6
subject=/CN=openmrs
issuer=/CN=localhost/O=Client Registry
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
PKCS7 Data
Shrouded Keybag: pbeWithSHA1And3-KeyTripleDES-CBC, Iteration 2048
Bag Attributes
    localKeyID: 36 7A 50 BC 1C 0E 69 93 22 7F CC FB 4D 07 C2 BE B2 37 02 C6
Key Attributes: <No Attributes>
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----BEGIN ENCRYPTED PRIVATE KEY-----
...
-----END ENCRYPTED PRIVATE KEY-----
```


the p12 is normally protected with a password and is not shared. It is imported in an application (e.g. a browser or a password manager) When a authentication must take place, the browser sends the identification information and its public key. The server then offers a challenge only the owner of the private key can solve. The browsers then sends back the solution of the challenge and the user is both identified and authenticated. Anyone getting access to the p12 will be able to impersonate the real owner.



