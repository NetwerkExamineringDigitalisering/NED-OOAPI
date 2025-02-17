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
| [flow 1 : Test catalogue](flow1.md) | SIS | Toetsplanning |  |
| [flow 2 : zittings gegevens ](flow2.md) | Toetsplanning | Toetsafname |  |
| [flow 3 : student results ](flow3.md) | Toetsafname | Toetsplanning |  |
| [flow 4 : zittingsverslag ](flow4.md) | Toetsafname | Toetsplanning |  |
| [flow 5 : Test results to SIS](flow5.md) | Toetsplanning | SIS |  |
| [flow 6 : Analisys](flow6.md) | SIS | Toetsafname |  |

## Other relevant standards

TODO: Describe all relevant standards for parties that want to join the ecosystem.
- [OOAPI v5](https://open-education-api.github.io/specification/v5/docs.html) 
- OAUTH2
- We are currently investigating the use of [edukoppeling](https://www.edustandaard.nl/app/uploads/2023/07/2023-07-24-Edukoppeling-Secure-API-OAuth-Client-Credentials-profielen-v0.8.pdf) a question here is do we HAVE to useedu_org_id? Do we really want to alllow for all 4 levels of Data Classification or are we only using 2?
- the [connection to MORA](./connection_MORA.md) has also been made in this project.  



## Information model

TODO: What is the underlying information model used within the ecosystem? See OOAPI, but focus on offering, component, association

[10:52] Jos van der Arend

Let op, in de definitie van de gegevensobjecten zijn genoemde gegevens binnen een object bij aanlevering (PUT of POST) of aflevering (GET) verplicht of optioneel zoals aangegeven in kolom #; de doorgestreepte gegevens (met 0 in kolom #) mogen worden gebruikt maar zullen bij ontvangst worden genegeerd.

Bij aanlevering van een wijziging (PATCH) mogen ongewijzigde verplichte gegevens ontbreken. Het weghalen van een optioneel enkelvoudig gegeven kan door de waarde null mee te geven; deze waarde null is dus hiervoor in PATCH toegestaan, ook wanneer dit element niet nullable is (d.w.z. nullable=false). Het weghalen van een optioneel meervoudig gegeven (lijst of array) kan door een lege lijst mee te geven; de lege lijst is dus hiervoor ook toegestaan al is de lijst gedefinieerd als “non-empty” (d.w.z. minItems=1).

 
