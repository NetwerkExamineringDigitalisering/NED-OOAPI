# Flow 5: Result information (students)

This flow can only be used for componentOfferingAssociations originating from flow 1 where the Deelnemerregistratie has indicated that it expects results by setting the attribute "resultsExpected" to true. Only in these situations results can be unambiguously related to a componentOfferingAssociation within the Deelnemerregistratie.

## Flow 5.1 : Return attendance and results combined

After Toetsplanning has received the result from Toetsafname and done some additional processing like checking whether there is still an active componentOfferingAssociation for the test and the score to be provided to the Deelnemerregistratie fits into the resultValueType provided by the Deelnemerregistratie, the result is sent back to the Deelnemerregistratie using this flow.

### Sequence diagram of request Send attendance and result combined
```mermaid
sequenceDiagram
    participant Deelnemerregistratie
    participant Toetsplanning
    loop for each student
      Toetsplanning->>Deelnemerregistratie: Send attendance and result combined
      activate Deelnemerregistratie
      Note right of Deelnemerregistratie: endpoint /ooapi/associations/{associationId} (PATCH)
      Deelnemerregistratie->>Toetsplanning: 200 - OK!
      deactivate Deelnemerregistratie
    end
```

### Class diagram of request Send attendance and result directly
```mermaid
classDiagram
    class Association {
        consumers : NL-TEST-ADMIN-Association
    	result : Result
    }
    class `NL-TEST-ADMIN-Association` {
        consumerKey : string
        attempt: integer
    }
    class Result {
    	state : string
        pass : string
        comment : string
        score : string
        resultDate : date
        consumers : NL-TEST-ADMIN-Result
        weight : integer
    }
    class `NL-TEST-ADMIN-Result` {
        consumerKey : string
        attendance : string
        executedOfferingName: string
        assessorId : string
        assessorCode : string 
        irregularities : string
        final : boolean 
        rawScore : integer 
        maxRawScore : integer
        testDate: string
        documents : Document[]
    }
    class Document {
        documentId : string
        documentType : string
        documentName : string
    }
    Association o-- Result
    Association o-- `NL-TEST-ADMIN-Association`
    Result o-- `NL-TEST-ADMIN-Result`
    `NL-TEST-ADMIN-Result` o-- Document
```
TO DO:

Vanuit de openstaande vragen in deze flow moet bij resultaten terug naar de deelnemerregistratie nog het volgende uitgewerkt worden:
- attempt/poging
- poging vergeven indicator
- daadwerkelijk toetsmoment als label om in ieder geval naam/code beschikbaar the hebben.

Important attributes:

- associationId: ID for the componentOfferingAssociation that has been provided by the Deelnemerregistratie
- resultDate: Date on which the candidate has performed the test/handed in the documents.
- testDate: Date on which the assessment has taken place/has been finalized.
- attempt: sequence number of the attempt. There are two scenario's:
    - Deelnemerregistratie creates an association per attempt
    - Deelnemerregistratie creates an association per test an indicates how many attempts are allowed. In this  scenario the Toetsplanning has to indicate per result for which attempt this result is.

