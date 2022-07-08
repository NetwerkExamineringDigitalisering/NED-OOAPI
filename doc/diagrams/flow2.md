# Flow 2 : registreren van de zitting bij de toetsafname applicatie

## optie 1 : Aanmaken van zitting  zonder studenten

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: hier is mijn zitting met details
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/offerings (PUT)
    Toetsafname->>Toetsplanning: 200 - Bedankt!
    deactivate Toetsafname
```

- id worden door sender aangemaakt.
- er moet een put endpoint gemaakt worden
- velden: 
- primaryCode-codeType ? hoeft niet unqiue zijn, moet herkenbaar zijn voor afname leider
- nl-nl word ondersteund , rest /dev/null
- primarycode, naam en description (not used) zijn allemaal verplicht (afhankelijk van afname systeem wat ze er mee doen)
- teachingLanguage (standaard NLD, not used)
- modeOfDelivery : situated, online, oncampus (beter omschrijving)
- resultExpected verplicht op true
- offeringState: cancelled, active

```
{
   "offeringId": "123e4567-e89b-12d3-a456-134564174000",
   "primaryCode": {
      "codeType": "offeringCode",
      "code": "Remindo_rekenen_MBO-3_op_woendag_middag_21-jun-22_om_13:00_in_lokaal_13"
   },
   "offeringType": "component",
   "component":"112-333-mooieguid-999-888",
   "name": [
      {
         "language": "nl-NL",
         "value": "20220621-12:45-Remindo rekenen MBO-3"
      }
   ],
   "abbreviation": null,
   "description": [
      {
         "language": "nl-NL",
         "value": "20220621-12:45-Remindo rekenen MBO-3"
      }
   ],
   "teachingLanguage": "nld",
   "modeOfDelivery": [
      "situated"
   ],
   "resultExpected": true,
   "consumers": [
      {
         "consumerKey": "MBO-toetsafname",
				 #je hebt duration nodig als je flexibele periodes hebt.
				 "duration": 60,
				 "safety": "schoolYear",
				 "offeringState": "active",
             "locationCode":"A-22"
      }
   ],
   "startDateTime": "2022-06-21T12:45:00",
   "endDateTime": "2022-06-21T13:45:00"
}
```



## optie 2 : Aanmaken van zitting  met studenten

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: hier is mijn zitting met details
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/offerings (PUT)
    Toetsafname->>Toetsplanning: 200 -Bedankt!
    deactivate Toetsafname
    loop voor elke student/medewerker
        Toetsplanning->>Toetsafname: En dit is een student/medewerker die mee doen
        activate Toetsafname
        Note right of Toetsafname: endpoint /a/ooapi/offerings/<id>/associations (PUT)
        Toetsafname->>Toetsplanning: 200 - Bedankt!
    end
    deactivate Toetsafname
```

- bij meerdere associaties : het vervangt de complete lijst
- - 
- voorstel attributen: personid, primaryCode (beter omschrijven - sso), givenName, surName, surnamePrefix, mail
- voor standaard bevrediging : displayname (goed gevuld), activeEnrollment (true) , affilliations (guest)
- gedrag : mag gebruiken om te updaten (geen verplichting ivm provisioning)
 - perosn is zelfde voor alle rollen 
 - rollen: student, invilgator, coordinator, assessor (wat als er meerdere rolen zijn?)
- remoteState : zelfde als State
- offeringId is impliciet

```
{
   "associationId": "123e4567-e89b-12d3-a456-426614174000",
   "associationType": "componentOfferingAssociation",
   "role": "student",
   "state": "associated",
   "remoteState": "associated",
   "consumers": [
      {
         "consumerKey": "MBO-toetsafname",
				 "userName": "1234321@student.roc.nl",
				 #je hebt extra time nodig om te weten hoeveel extra tijd.
				 "extraTimeInMin": 30,
				 #https://www.imsglobal.org/sites/default/files/spec/afa/3p0/information_model/imsafa3p0pnp_v1p0_InfoModel.html
				 "personalNeeds": [
				 	"extraTime",
					"spoken", 
					"spell-checker-on-screen"
				 ]
      }
   ],
   "person": <expanded>
}
```
## optie 3 : later tijdstip: toevoegen van studenten aan aan zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: En dit is een  student/medewerker die mee doen
    activate Toetsafname
    Note right of Toetsafname: endpoint endpoint /a/ooapi/offerings/<id>/associations (PUT)
    Toetsafname->>Toetsplanning: 200 Bedankt!
    deactivate Toetsafname
```

used status van association : associated, cancelled

## optie 4 : later tijdstip: verwijderen van studenten aan aan zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: En deze studenten doen niet meer mee
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/offerings/<id>/associations (PUT status cancelled)
    Toetsafname->>Toetsplanning: 200 -Bedankt!
    deactivate Toetsafname
```


## optie 5 later tijdstip: verwijderen van een zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: Deze zitting mag weg, gaan we niet meer gebruiken
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/offerings (PUT status cancelled)
    Toetsafname->>Toetsplanning: 200 Bedankt!
    deactivate Toetsafname
```
