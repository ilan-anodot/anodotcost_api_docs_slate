## Feedback

> End Point **GET /api/v2/feedbacks**

Our users and trigger recipients give feedback (Not Interesting/Good Catch) to anomaly alert triggers via:

* The Alert console
* The trigger investigation page
* Directly from the received trigger, via Email, Slack and other channels that support it.

Use the feedback API endpoint to get the feedback instances given by your organization members, tune your alerts, map your anomalies to business cases and improve the use of Anodot by your teams.

Authentication type: [Access Token Authentication] (#access-tokens).

### Get Feedback

<aside class="notice">
We've recently introduced feedback also on noData and Static alerts. This means that you can call the Get Feedback API with /static or /noData to get the feedback for those types. Calling without any parameter will return only the anomaly feedbacks.</aside>

> Request Example: Getting Feedback on anomaly alerts

```shell
curl -X GET \
https://app.anodot.com/api/v2/feedbacks?startDate=1647388810&endDate=1647860710' \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"

```

> Request Example: Getting Feedback on static alerts

```shell
curl -X GET \
https://app.anodot.com/api/v2/feedbacks/static' \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"

```

> Request Example: Getting Feedback on No-Data alerts

```shell
curl -X GET \
https://app.anodot.com/api/v2/feedbacks/noData' \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"

```

**Request Arguments**

Argument | Type | Description
---------|------|------------
startTime [**Required**] | Epoch | Time the feedback was given.<br/>Default value is "Now minus 24 hours"
endTime [**Required**] | Epoch | Time the feedback was given.<br/>Default value is "Now".

> Response Example:

```json
{
    "total": 1,
    "feedbacks": [
        {
            "id": "b6deff91-2d8a-41c3-9605-e6d1e9edb0cf",
            "type": "GOOD_CATCH",
            "comment": "ok",
            "createdTime": 1637142124,
            "userName": "ran",
            "anomalyId": "https://app.anodot.com/#!/anomalies?ref=pd&tabs=main;0&activeTab=1&anomalies=;0(6b1ffc765925491c85c2206d05246e0f)&duration=;1(1)&durationScale=;minutes(minutes)&delta=;0(0)&deltaType=;percentage(percentage)&resolution=;longlong(longlong)&score=;0(0)&state=;both(both)&direction=;both(both)&bookmark=;()&alertId=;(1696e800-541a-41e5-b22b-b1ee220e9b81)&sort=;significance(significance)&q=;()&constRange=;1h(c)&startDate=;0(0)&endDate=;0(0)",
            "origin": "slack",
            "alerts": [
                {
                    "Id": "https://app.anodot.com/#!/r/alert-manager/edit/a038cba3-a76e-4edb-8e32-86655f7f6c12/settings",
                    "emailId": "6b1ff",
                    "alertName": "{{customer}}: possible risk [3 consecutive days!]",
                    "startTime": 1636675200,
                    "status": "OPEN",
                    "alertOwner": "CS"
                }
            ],
            "anomalyGroupId": "6b1ffc765925491c85c2206d05246e0f",
            "score": 48
        }
    ]
}
```

**Response Fields**

Field | Type | Description / Example
-|-|-
total | Number | Number of feedback instances included in the response
feedbacks[] | Array | An array of feedback instances
id | String ($uuid) | Feedback id
type | String | Type of feedback. Possible values:<br/>* Good Catch<br/>* Not Interesting
comment | String | Optional reason and comment, if provided by the user while giving the feedback.
createdTime | Epoch | Feedback creation time.
username | String | Email of the user who provided the feedback
anomalyId | String | Link to the anomaly *Investigate* page in the Anodot platform.
alerts[] | Array | An array of alerts related to the feedback instance.
anomalyGroupID | String | A unique identifier of alert group related to the feedback instance.
score | Number | Score of the anomaly for which the feedback was given.

**Alerts Array Fields** 

Field | Type | Description / Example
-|-|-
Id | String | Link to the *alert settings* in the Anodot platform.
emailId | String | 5 characters used to identify the email.<br/>These are also the first 5 chars of the AnomalyId.
alertName | String | Alert name in Anodot
startTime | Epoch | Alert start time
endTime | Epoch | [**Optional**] Alert end time, relevant if the alert is closed.
status | String | The alert status. Possible values:<br/>* OPEN<br/>* CLOSE.
alertOwner | String | The alert owner in Anodot. Possible values are:<br/>* User first and Last name.<br/>* Group name.

### Create Feedback

> Request Example:

```shell
curl -X POST \
https://app.anodot.com/api/v2/feedbacks \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-d '{
  "userId":"5e662ecxxxx61e000eaa0de8",
  "userName":"demo user",
  "triggerIds":["9e701a4b-xxxx-42e6-8799-7b6cde40b69e"],
  "type":"GOOD_CATCH",
  "origin":"api"
  }'
```

Feedback can be submitted in the Anodot app from The alert console, the investigation page, the insights widget and from the triggers you recieve to your inbox, slack or other channels.
Use the POST call to provide your feedback using an API.

**Request Arguments**

Argument | Type | Description
---------|------|------------
userId | String ($uuid)| The ID of the user giving the feedback. Find this ID from your browser developer window.
userName | String | The first and last names of the user giving the feedback.
triggerIds | Array of Strings ($uuid) | A list of trigger IDs the feedback is given on, provide a single value.
type | Enum | Possible values:</br>GOOD_CATCH</br>NOT_INTERESETING
origin | Enum | Possible value:</br>api</br>

> Response Example

```json
{
    "id": "ef66297a-48e8-4899-xxyy-d6b1612b1392",
    "meta": {
        "userId": "58e5f7893xxxyyyc736e701",
        "modifiedTime": 1600020701,
        "createdTime": 1600020701,
        "origin": "api"
    }
}
```

**Response Fields**

Field | Type | Description / Example
-|-|-
id | string | The specific feedback entry id, generated by Anodot.
meta (Object)| JSON Object | the fields related to the feedback entry
userId | string | ID of the user giving the feedback.
modifiedTime | epoch | Feedback entry modification time.
createdTime | epoch | Feedback entry creation time.
origin | Enum | The method this feedback entry was provided. For API calls, the value is "api"


### Update Feedback

> Request Example

```shell
curl -X PUT \
https://app.anodot.com/api/v2/feedbacks/{feedbackId} \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-d '{
  "userId":"5e662ecxxxx61e000eaa0de8",
  "userName":"demo user",
  "triggerIds":["9e701a4b-xxxx-42e6-8799-7b6cde40b69e"],
  "type":"GOOD_CATCH",
  "comment": "demo comment",
  "reason": "demo reason",
  "origin":"api"
  }'
```

To update the feedback entry with a reason and a comment:

* Use the PUT call with the feedback id in the URL</br>

To find the feedback id you with to update:

* Use the feedback id you have received in the response to the original POST call
* Or, find the relevant feedback entry using the GET feedback call.

**Request Fields**

Two additional fields can be used in the PUT call:

Argument | Type | Description
---------|------|------------
reason | string (Enum)| Relevant only in NOT_INTERESTING feedback entries. Possible values:</br>* No Business Impact</br>* Incorrect Data</br>* Not an Anomaly
comment | string | Optional free text comment.

> Response Example

```json
{
    "id": "9e253017-741a-xxxx-913e-5ea250172921",
    "meta": {
        "userId": "58e5f7893xxxx2bc736e701",
        "modifiedTime": 1600021808,
        "createdTime": 1600020257,
        "origin": "api"
    }
}
```

**Response Fields**

The same fields are returned.</br>The modifiedTime field is updated with the PUT time.

### Set Comment

> Request Example:

```shell
curl --location --request POST 'https://app.anodot.com//api/v2/timeline/comments' \
--header 'Authorization: Bearer {{data-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "alertGroupId": "0f0748cfcacc42c89fbd45edce1f6dcf",
    "text": "This looks like a serious issue",
    "userId": "60d19b3148b5bf0010cdc3f4"    
}'
```

Use this API to add a comment to the timeline of an existing incident. This is similar to the comment being sent as part of the [Update Feedback](#update-feedback) call, but without a specific feedback. 

**Request Arguments**

Argument | Type | Description
---------|------|------------
alertGroupId | String ($uuid)| The ID of the anomaly group (a.k.a. anomalyId) to which the comment should be attached. 
text | String | The comment text
userId | String | (optional) the user giving the feedback. Find this ID from your browser developer window.

> Response Example

```json
{
    "id": "61505cb6e3896c6ff4b75f24",
    "comment": "This looks like a serious issue",
    "createdTime": 1632656566
}
```

**Response Fields**

Field | Type | Description / Example
-|-|-
id | string | The specific comment entry id, generated by Anodot.
comment | string | Text which was added to the timeline 
createdTime | epoch | Comment entry creation time.

