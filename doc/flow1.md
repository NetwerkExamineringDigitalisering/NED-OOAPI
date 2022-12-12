# Flow 1: Plannings information (tests and students)

# Proposal : THIS IS FOR DISCUSSION ONLY. NOT FINAL

## endpoints
Used endpoints for this flow are:
`PUT /geenidee`
`GET /geenidee/{geenideeId}`
`GET /geenidee/{geenideeId}/details`

## Flow 1.1 : Get the to be planned exams (and students)

### Sequence diagram of request Create offering (zitting)	
```mermaid
sequenceDiagram
    participant SIS
    participant Toetsplanning
    SIS-->>SIS: setup studyplan and tests/exams
    Toetsplanning->>SIS : Give list of exams/test that need to be planned soon
    activate SIS
    Note right of SIS: endpoint /a/ooapi/geenidee/{geenideeId} (GET)
    SIS->>Toetsplanning : 200 - please plan this!
    deactivate SIS

    loop for each exam/test
        Toetsplanning->>SIS: Get list of students
        activate SIS
        Note right of SIS: endpoint /a/ooapi/geenidee/{geenideeId}/students/{geenideeId} (GET)
        SIS->>Toetsplanning: 200 - Bedankt!
    end
    deactivate SIS

    loop for each student / teacher
        Toetsplanning-->>SIS: Get details of a student / teacher
        activate SIS
        Note right of SIS: endpoint /a/ooapi/geenidee/{geenideeId}/students/{geenideeId} (GET)
        SIS-->>Toetsplanning: 200 - Bedankt!
    end
    deactivate SIS

```

