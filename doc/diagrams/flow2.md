# Flow 2 : registreren van de zitting bij de toetsafname applicatie

## optie 1 : Aanmaken van zitting  met studenten

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: hier is mijn zitting met details
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/ziting (POST)
    Toetsafname->>Toetsplanning: Bedankt!
    deactivate Toetsafname
```

## optie 2 : Aanmaken van zitting  met studenten

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: hier is mijn zitting met details
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/ziting (POST)
    Toetsafname->>Toetsplanning: Bedankt!
    deactivate Toetsafname
    activate Toetsplanning
    Toetsplanning->>Toetsafname: En dit zijn de studenten die mee doen
    deactivate Toetsplanning
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/participants (POST)
    Toetsafname->>Toetsplanning: Bedankt!
    deactivate Toetsafname
```

## optie 3 : toevoegen van studenten aan aan zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: En dit zijn nieuwe studenten die mee doen
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/participants (POST)
    Toetsafname->>Toetsplanning: Bedankt!
    deactivate Toetsafname
```

## optie 4 : verwijderen van studenten aan aan zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: En deze studenten doen niet meer mee
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/participants (DELETE)
    Toetsafname->>Toetsplanning: Bedankt!
    deactivate Toetsafname
```


## optie 5 verwijderen van een zitting

```mermaid
sequenceDiagram
    Toetsplanning->>Toetsafname: Deze zitting mag weg, gaan we niet meer gebruiken
    activate Toetsafname
    Note right of Toetsafname: endpoint /a/ooapi/ziting (DELETE)
    Toetsafname->>Toetsplanning: Bedankt!
    deactivate Toetsafname
```
