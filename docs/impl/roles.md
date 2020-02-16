# Roles and Responsibilities

In actual practice, there are specific roles.

* **Point-of-service systems users**: POS systems use the Client Registry to obtain a CRUID. This process is mostly invisible. Users of EMRs and other systems submitting queries may only see that there is a CRUID for a patient. The Client Registry is invisible to them.
* **POS developers**: Client Registry integration must be added into POS systems for them to be able to query for a CRUID. Thus, software developers of POS systems should review the Developer Manual and understand how to implement the proper FHIR query for obtaining a CRUID.
* **Matching administrators** There may be situations in which the Client Registry implementation uses the UI for viewing and breaking matches. This is a privileged role that should be restricted to few individuals.
* **Client Registry systems administrator**: People managing the network, servers, backups and other aspects of the Client Registry should be very familiar with the Developer Guide, particularly security of the system, how to perform upgrades, and recovery procedures.
* **Management team**: Governance of the system should be handled by a management team familiar with the implications of decisions, strategy, roll-out, and other aspects.


