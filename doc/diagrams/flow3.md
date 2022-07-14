# Flow 3 : Sturen van resultaten when toets/examen is afgenomen

After a test is taken the results for each participant are returned to the plannings application. This can be updated many times, also with partial information. for example : first attendance information, a few weeks later the results.

## Flow 3.1 : Send attendance en resultaat directly (automated scored tests)

```mermaid
sequenceDiagram
    participant Toetsplanning
    participant Toetsafname
    loop voor elke student
      Toetsafname->>Toetsplanning: here are the complete results for this student
      activate Toetsplanning
      Note right of Toetsplanning: endpoint /a/ooapi/associations/{associationId} (PATCH)
      Toetsplanning->>Toetsafname: 200 - OK!
      deactivate Toetsplanning
    end
```
   
### example of result message for one student
```
PATCH /associations/{associationId}

{
	

   "result": {
      "state": "completed",
      "pass": "unknown",
      "comment": "string",
      "score": "9",
      "resultDate": "2020-09-28T00:00:00.000Z",
      "weight": 100,
      "consumers": [
	     {
		"consumerKey": "MBO-toetsafname",
		"assessorId": "05035972-0619-4d0b-8a09-7bdb6eee5e6d",
		"assessorCode": "JAJE",
		"attendance": "present",
		"irregularities": "Jantje heeft gespiekt."
		"final": true,
		"documents": [
		 {
		   "documentId": "123454",
		   "documentType": "assessmentForm",
		   "documentName": "Assessment form for Jake Doe.pdf"
		 }
		]
	      }
      ],
    }
}
```
Remarks:
- In the standard is the consumer not available for result, this is candidate to be added.
- the result may be send with status "in progress", "postponed", "queued", but must be send when status changes to "completed".

- Fields
	- weight is mandatory, but is ignored in this interface. set to 100.
- Consumer values
	- final : is vast gesteld door examen commissie. Can be done in toetsplanning or SIS. so most systems would send false. (optional, default false)
	- assessorId en assessorCode is the identity of the assessor (optional)
	- documents: always via documents ooAPI endpoint. (TODO Explain why)
	- documentTypes: supported : "assessmentForm", "assessmentFormWithAnswers", "assessmentModel", "other"
	- attendence: "notPresent| notStarted | notFinished | present" (mandatory)
	- irregularities: <to be added>

## Flow 3.2 : Send attendance first, send resultaat later
```mermaid
sequenceDiagram
    participant Toetsplanning
    participant Toetsafname
    loop voor elke student
      Toetsafname->>Toetsplanning: here is first the attendance for this student
      activate Toetsplanning
      Note right of Toetsplanning: endpoint /a/ooapi/associations/{associationId} (POST)
      Toetsplanning->>Toetsafname: 200 - OK!
      deactivate Toetsplanning
   end
    loop voor elke student
      Toetsafname->>Toetsplanning: here are the complete results for this student
      activate Toetsplanning
      Note right of Toetsplanning: endpoint /a/ooapi/associations/{associationId} (PUT)
      Toetsplanning->>Toetsafname: 200 - OK!
      deactivate Toetsplanning
   end
```

### example of a message with only attendance information
```json
PATCH /associations/{associationId}

{
   "result": {
      "state": "in progress",
      "resultDate": "2020-09-27T00:00:00.000Z",
      "weight": 100,
      "consumers": [
	     {
		"consumerKey": "MBO-toetsafname",
		"attendance": "present",
	     }
      ]
    }
}
```
 
## Flow 3.3 : Retrieve supporting results files
when a result message contains a file reference the file can be downloaded
```mermaid
sequenceDiagram
    participant Toetsplanning
    participant Toetsafname
    loop for each supporting file
      Toetsplanning->>Toetsafname: give me the supporting file
      activate Toetsafname
      Note right of Toetsplanning: endpoint /a/ooapi/documents/{documentid} GET
      Toetsafname->>Toetsplanning: 200 - Here it is!
      deactivate Toetsafname
   end

```

## Flow 3.4 Read current state of the attendance and results
To see the current state of the offering with its assocoations and endpoints the following endpoint can be used
```mermaid
sequenceDiagram
    alt for all students
        Toetsplanning->>Toetsafname: Give me the students and results
        activate Toetsafname
        Note right of Toetsafname: endpoint /a/ooapi/offerings/<id>/associations (GET)
        Toetsafname->>Toetsplanning: 200 - here they all are!
        deactivate Toetsafname
    else just one student
        Toetsplanning->>Toetsafname: And give me the students results
        activate Toetsafname
        Note right of Toetsafname: endpoint /a/ooapi/offerings/<id>/associations/<associationId> (GET)
        Toetsafname->>Toetsplanning: 200 - here it is!
        deactivate Toetsafname
    end
```
