# Flow 1: Plannings information (tests and persons)

Proposal : We have several flow 1's : very structured and very ad-hoc

## flow 1a : Structured

We request all components/offerings from the SIS that need to be planned

## Flow 1b : Ad-hoc

We request a selection of students that a teacher can use to create detailed plannings, results do not go back to the SIS automaticly.

## endpoints
Used endpoints for this flow are:
`GET /ooapi/offerings?componentType=TEST&Since=..&Until=&planner=
`PUT /geenidee`
`GET /geenidee/{geenideeId}`
`GET /ooapi/association/{associationId} `

Planner kan al beperkt worden door security
Boolean IsLineItem (we verwachten ook een resultaat te loggen)
ComponentType = Test, Lecture, Consultation, SkillTraining..... Niet beperken.

# Flow 1a : Get the to be planned exams (and students)

### Sequence diagram of request Create offering (zitting)	
```mermaid
sequenceDiagram
    participant SIS
    participant Toetsplanning
    SIS-->>SIS: setup studyplan and tests/exams
    Toetsplanning->>SIS : Give list of exams/test that need to be planned soon by me
    activate SIS
    Note right of SIS: endpoint /a/ooapi/offerings/?selectioncriteria=.. (GET)
    SIS->>Toetsplanning : 200 - please plan this!
    deactivate SIS

    loop for each exam/test
        Toetsplanning->>SIS: Get list of associations (students & staff)
        activate SIS
        Note right of SIS: endpoint /a/ooapi/offerings/{geenideeId}/associations (GET)
        SIS->>Toetsplanning: 200 - Bedankt!
    end
    deactivate SIS

    loop for each student / staff
        Toetsplanning-->>SIS: Get details of a student / staff
        activate SIS
        Note right of SIS: endpoint /a/ooapi/association/{associationId} (GET)
        SIS-->>Toetsplanning: 200 - Bedankt!
    end
    deactivate SIS

```

### Class diagram of request B. Add student to created offering (zitting)	
```mermaid
classDiagram
    class Association {
    	associationId : UUID
	associationType : associationType
	role : associationRole
	state : state
	consumers : Consumer
#	result : Result
	person : personId or Person
	offering : offeringId
    }
    class Consumer {
    	consumerKey : string = "MBO-toetsafname"
	    additionalTimeInMin : int
	    personalNeeds : string[]
        attempt : int
        attemptLeft : int
    }
    class Person {
	personId : UUID
	primaryCode : identifierEntity
	givenName : string
	surnamePrefix : string 
	surname : string
	displayname : string
	activeEnrollment : boolean 
	affiliations : personAffiliations
	mail : string
    	consumers : Consumer2
    }
    class Consumer2 {
    	consumerKey : string = "MBO-toetsafname"
	    programAssociation : string
	    enrollmentAssociation : string
    }
    class Offering {
       offeringId : UUID
    }
    Offering  o-- Association
    Association o-- Consumer
    Association -- Person
    Person o-- Consumer2

```

