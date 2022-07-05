# Flow 3 : Sturen van resultaten when toets/examen is afgenomen


## optie 1 : Sturen van aan/afwezigheid en resultaat

```mermaid
sequenceDiagram
    loop voor elke student
      Toetsafname->>Toetsplanning: hier is het resultaat van de student voor deze examenzitting
      activate Toetsplanning
      Note right of Toetsplanning: endpoint /a/ooapi/resultaat (POST)
      Toetsplanning->>Toetsafname: 200 - Bedankt!
      deactivate Toetsplanning
    end
```
   
## optie 2 : Sturen van aan/afwezigheid als resultaat nog niet bekend is, gevolgd door resultaat

```mermaid
sequenceDiagram
    loop voor elke student
      Toetsafname->>Toetsplanning: hier is aanwezigheid/afwezigheid van de student voor deze examenzitting
      activate Toetsplanning
      Note right of Toetsplanning: endpoint /a/ooapi/resultaat (POST)
      Toetsplanning->>Toetsafname: 200 - Bedankt!
      deactivate Toetsplanning
   end
    loop voor elke student
      Toetsafname->>Toetsplanning: hier is het resultaat van de student voor deze examenzitting
      activate Toetsplanning
      Note right of Toetsplanning: endpoint /a/ooapi/resultaat (PUT)
      Toetsplanning->>Toetsafname: 200 - Bedankt!
      deactivate Toetsplanning
   end
```

 
