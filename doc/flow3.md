# Flow 3 : Transfer results when test is taken

After a test is taken the results for each participant are returned to the Toetsplanning application. This can be updated many times, also with partial information. for example : first attendance information, a few weeks later the results and finally the final results (as determined by the exam committee).

## Flow 3.1 : Send attendance and result directly (automated scored tests)

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
		"attendance": "present",
		"assessorId": "05035972-0619-4d0b-8a09-7bdb6eee5e6d",
		"assessorCode": "JAJE",
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
- In the OOAPI standard the consumer is not available for result, this is candidate to be added in the next version.
- The result may be send with status "in progress", "postponed", "queued", but must be sent when status changes to "completed". The result in state "completed" can be replaced by another result (for instance when a test item is cancelled), usually also in state "completed". 

- Result attributes and values
	- weight is mandatory for ComponentResult on top of Result), but is ignored in this interface. Value is always 100.
- Consumer attributes and values
	- attendence: indication of presence during the test (mandatory). Possible values are notPresent (niet aanwezig), notStarted (aanwezig maar niet gestart), notFinished (aanwezig en gestart maar niet afgerond/ingeleverd) en present (aanwezig en afgerond/ingeleverd)"
	- assessorId en assessorCode: is the identity resp. code of the assessor (both optional).
	- irregularities: textual information about the student test, such as irregularities: <to be added>
	- final : indicates that the result has been finalised by the exam committee. Can be done in Toetsplanning (or even DR), so most Toetsafname applications will send false. (optional, default false).
	- documents: data group for document specification (optional, multiple times). Document transfer is always via documents OOAPI endpoint because of security issues (TODO: Explain why). All next attributes are mandatory for each document:
		- documentId: unique identifier for the document to be used in the document request.
		- documentTypes: identifies the kind of document; supported values: "assessmentForm" (beoordelingformulier), "assessmentFormWithAnswers" (answers with assessment notes), "assessmentModel" (beoordelingsmodel/-voorschrift), "other" (any document not suitable for the other values). 
		- documentName: name for the document that is specified by Toetsafname.

## Flow 3.2 : Send attendance first, send resultaat later
```mermaid
sequenceDiagram
    participant Toetsplanning
    participant Toetsafname
    loop for each student
      Toetsafname->>Toetsplanning: here is the attendance info for this student
      activate Toetsplanning
      Note right of Toetsplanning: endpoint /a/ooapi/associations/{associationId} (POST)
      Toetsplanning->>Toetsafname: 200 - OK!
      deactivate Toetsplanning
   end
    loop voor elke student
      Toetsafname->>Toetsplanning: Here are the complete test results for this student
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
 
## Flow 3.3 : Retrieve supporting result documents
when a result message contains a document reference the file can be downloaded
```mermaid
sequenceDiagram
    participant Toetsplanning
    participant Toetsafname
    loop for each supporting document
      Toetsplanning->>Toetsafname: give me the supporting document
      activate Toetsafname
      Note right of Toetsplanning: endpoint /a/ooapi/documents/{documentid} GET
      Toetsafname->>Toetsplanning: 200 - Here it is!
      deactivate Toetsafname
   end

```

## Flow 3.4 Read current state of the attendance and results
To see/check the current state of the offering with its associations the following endpoint can be used
```mermaid
sequenceDiagram
    alt for all students
        Toetsplanning->>Toetsafname: Give me the students with their results
        activate Toetsafname
        Note right of Toetsafname: endpoint /a/ooapi/offerings/<id>/associations (GET)
        Toetsafname->>Toetsplanning: 200 - here they all are!
        deactivate Toetsafname
    else just one student
        Toetsplanning->>Toetsafname: And give me the student results
        activate Toetsafname
        Note right of Toetsafname: endpoint /a/ooapi/offerings/<id>/associations/<associationId> (GET)
        Toetsafname->>Toetsplanning: 200 - here it is!
        deactivate Toetsafname
    end
```
