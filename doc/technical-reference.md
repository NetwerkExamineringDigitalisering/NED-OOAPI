# Technical reference documentation

## Key technical principles

The NED implementation is based on these technical principles. 
* Based on OOAPI
* direct communication between all parties, no central hub
* minimal data exposure of student information
* no vendor lock-in
* use proven standards for authorization and authentication (OAUTH2)

## Architecture


A simple overview of a generic exchange is as follows, note that while this may look complex at first glance, it is a common pattern used in the B2B software space and is straight forward to implement on both sides of the exchange:

![architecture](diagrams/Saas_Vendor_Infrastructure-Big_Picture_Gateways.drawio.svg)

## APIs

Each Party in the NED Ecosystem provides and consumes APIs according to their roles. The table outlines these APIs.

| API Definition | Service Provider | Services Consuming | Remarks |
|---|---|---|---|
| [flow 0 : Test catalogue ](flow0.md) | Toetsafname | SIS & Toetsplanning |  |
| flow 1 : Test catalogue  | SIS | Toetsplanning |  |
| [flow 2 : zittings gegevens ](flow2.md) | Toetsplanning | Toetsafname |  |
| [flow 3 : student results ](flow3.md) | Toetsafname | Toetsplanning |  |
| [flow 4 : zittingsverslag ](flow4.md) | Toetsafname | Toetsplanning |  |
| flow 5 : Test results to SIS  | Toetsplanning | SIS |  |
| flow 6 : Analisys | ? | ? |  |

## Other relevant standards

TODO: Describe all relevant standards for parties that want to join the ecosystem.
- [OOAPI v5](https://open-education-api.github.io/specification/v5/docs.html) 
- OAUTH2

## Information model

TODO: What is the underlying information model used within the ecosystem? See OOAPI, but focus on offering, component, association


