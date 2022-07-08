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
- er moet een post gemaakt worden

## optie 2 : Aanmaken van zitting  met studenten

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: hier is mijn zitting met details
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/offerings (PUT)
    Toetsafname->>Toetsplanning: 200 -Bedankt!
    deactivate Toetsafname
    loop voor elke student
        activate Toetsplanning
        Toetsplanning->>Toetsafname: En dit is een student/medewerker die mee doen
        deactivate Toetsplanning
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
 - 
## optie 3 : toevoegen van studenten aan aan zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: En dit zijn nieuwe studenten die mee doen
    activate Toetsafname
    Note right of Toetsafname: endpoint endpoint /a/ooapi/offerings/<id>/associations (PUT)
    Toetsafname->>Toetsplanning: 200 Bedankt!
    deactivate Toetsafname
```

## optie 4 : verwijderen van studenten aan aan zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: En deze studenten doen niet meer mee
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/offerings/<id>/associations (PUT)
    Toetsafname->>Toetsplanning: 200 -Bedankt!
    deactivate Toetsafname
```


## optie 5 verwijderen van een zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: Deze zitting mag weg, gaan we niet meer gebruiken
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/ziting (DELETE)
    Toetsafname->>Toetsplanning: 200 Bedankt!
    deactivate Toetsafname
```
