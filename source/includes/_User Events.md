## User Events

> End Point prefix is **/api/v2/user-events**

The Event API enables you to create and edit periodic events such as deployments and holidays. Events can assist in the anomaly investigation process, by correlating them to alerts/anomalies/metrics in dashboards and alerts. 

Each event has a **category** and a **source**. There is a specific set of APIs to enable to you manage all three entities.

* [Events](#events)
* [Categories](#categories)
* [Sources](#sources)

Additional information is available in our [Help center documentation](https://support.anodot.com/hc/en-us/articles/209776765-Events-Overview)

<aside class="success">
Updates to the user event object</br>
We added support to user event type, to be used in advanced alert conditions.
The added fields allow linking the user event to a specific event condition.
</aside>

You can find more information about the event conditions in our [Alert configuration documentation](https://support.anodot.com/hc/en-us/articles/360015501820-Defining-Advanced-Alert-Conditions)

### Create Event

> Request Example - Create a User Event 

```shell
curl -X POST \
https://app.anodot.com/api/v2/user-events \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{
      "event": {
        "title":"deployment started on myServer",
        "description":"my description",
        "source":"chef",
        "category":"deployments",
        "startDate": "1468326864",
        "endDate":null,
        "properties":[
          {
             "key":"service",
             "value":"myService"
          }
        ]
      }
    }'

```

> Request Example - Create a suppress event

> This example shows an "end suppress" event. Note the type and action fields

```shell
curl -X POST \
https://app.anodot.com/api/v2/user-events \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{
      "event": {
        "title":"deployment started on myServer",
        "description":"my description",
        "source":"chef",
        "category":"deployments",
        "startDate": "1651140507",
        "endDate":"",
        "type":"Suppress",
        "action":"End",
        "properties":[
          {
             "key":"service",
             "value":"myService"
          }
        ]
      }
    }'

```


> Response Example

```json
{
   "id":"b1229900-1a49-4b9e-a2e4-b9d21240793f",
   "title":"deployment started on myServer",
   "description":"my description",
   "source":"chef",
   "category":"deployments",
   "startDate":1468326864,
   "endDate":null,
   "type":"Suppress",
   "action":"End",
   "properties":[
      {
         "key":"service",
         "value":"myService"
      }
   ]
}
```


To add more information to the event use the *properties* field and add additional data with key-value objects.

**Request Arguments**

Argument | Description
---------| -----------
Title | The event's title [**required**]
Description | A description of the event. [*Optional*]
Category | The name of the pre-defined category that the event belongs to (e.g deployments, marketing_campaigns, holidays) [**required**]. If a relevant category does not exist, create one. To create a new category, see [create category](#create-event-category).
Source | The pre-defined source of the request (e.g rest_api, Chef, Jenkins) [**required**]. If the source does not exist, add it. To add a new source, see [create source](#create-event-source).
Properties | Key-value pairs, can be whatever the user wants to add as additional data except the ones that are already part of the body arguments like title, description, category and so on. E.g: severity: high, publisher: nyt, exchange: rubicon. [*Optional*]
StartDate |The start time of the event. Needs to be in epochtime (seconds)[**required**]
EndDate | The end time of the event. Needs to be in epochtime (seconds) [*Optional*]
Type (New) | The user event type. Possible values:</br>Display - Used for Display Only</br>Influence - Used for influencing events</br>Suppress - Used to suppress specific metrics from an alert. Can also be used as Display and influencing events </br>OfficeHours - Used to pause an alert altogether. Can also be used as Display and influencing events</br>This field is relevant for Suppress and OfficeHours events. [*Optional*]
Action (New) | Stating if the user event is the starting or ending time of the event.</br>Possible values: Start, End.</br> This field is relevant for Suppress and OfficeHours events. [*Optional*]

<aside class="success">
Note the enhanced validation:</br>
If the event type is "OfficeHours", the event should include both "startTime" and "endTime".</br>
If the event type is "Suppress", the event should either include both "startTime" and "EndTime"</br>
OR, Have the "Action" field filled with start or end.
</aside>

**Response Fields**
The response is an object similar to the request, enabling you to verify that the object was created successfully.  

### Edit Event

> Request Example - Editing an Event

```shell
curl -X PUT \
https://app.anodot.com/api/v2/user-events \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{
"event": {
    "id":"event id"
    "title": "<event title>",
    "description": "<event description>",
    "category": "<category name>",
    "source": "<source name>",
    "properties":[
     {
        "key":"<key1>",
        "value": "<value1>"
      },
      {
        "key": "<key2>",
        "value": "<value2>"
      }
    ],
    "startDate": "<epoch start date>",
    "endDate": "<epoch end date>"
  }
} 
```


> Response Example

```json
{
   "id":"b1229900-1a49-4b9e-a2e4-b9d21240793f",
   "title":"deployment started on myServer",
   "description":"my description",
   "source":"chef",
   "category":"deployments",
   "startDate":1468326864,
   "endDate":null,
   "properties":[
      {
         "key":"service",
         "value":"myService"
      }
   ]
}
```

Use this call to edit an existing event. This can be done by event with the ID and all the event details - the same as in the add event API.

Argument | Description
---------| -----------
id | The event ID
Title | The event's title [**required**]
Description | A description of the event. [*Optional*]
Category | The name of the pre-defined category that the event belongs to (e.g deployments, marketing_campaigns, holidays) [**required**]. If a relevant category does not exist, create one. To create a new category, see [create category](#create-event-category).
Source | The pre-defined source of the request (e.g rest_api, Chef, Jenkins) [**required**]. If the source does not exist, add it. To add a new source, see [create source](#create-event-source).
Properties | Key-value pairs, can be whatever the user wants to add as additional data except the ones that are already part of the body arguments like title, description, category and so on. E.g: severity: high, publisher: nyt, exchange: rubicon. [*Optional*]
StartDate | The start time of the event. Needs to be in epochtime (seconds)[**required**]
EndDate | The end time of the event. Needs to be in epochtime (seconds) [*Optional*]
Type (New) | The user event type. Possible values:</br>Display - Used for Display Only</br>Influence - Used for influencing events</br>Suppress - Used to suppress specific metrics from an alert</br>OfficeHours - Used to pause an alert altogether.</br>This field is relevant for Suppress and OfficeHours events. [*Optional*]
Action (New) | Stating if the user event is the starting or ending time of the event.</br>Possible values: Start, End.</br> This field is relevant for Suppress and OfficeHours events. [*Optional*]

### Retrieve Event by ID

> Request example - Getting an Event by ID

```shell
curl -X GET \
https://app.anodot.com/api/v2/user-events/9b0cfa33-8a78-416e-84b7-1bff5628deaf \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

> Response Example - Event content with its id

```json
{
   "id":"9b0cfa33-8a78-416e-84b7-1bff5628deaf",
   "title":"Independence Day",
   "description":"Happy Holiday",
   "source":"calendar",
   "category":"holidays",
   "startDate":1468307241,
   "endDate":1468307241,
   "properties":[

   ]
}
```

> Response Example - EventID not found (HTTP Status 404)

```json
{
    "errors": [
        {
            "index": null,
            "error": 3016,
            "description": "event with this id does not exit"
        }
    ]
}
```

Use this call to retrieve an existing event definition.

Argument | Definition
-------- | ----------
ID | The ID of the event

### Retrieve Events by Search Expression

> Request example - Getting events using a timestamp and an empty expression

```shell
curl -L -X POST \
'https://app.anodot.com/api/v2/user-events/execute?fromDate=1609517291' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer ${TOKEN}' \
--data-raw '{"filter":{"categories":[]},"aggregation":null}'
```

> Request example - Getting events using a search sxpression

```shell
curl -X POST \
https://app.anodot.com/api/v2/user-events/execute?fromDate=1468326864 \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{
{
  "filter": {
    "categories": [
      "deployment"
    ],
    "q": {
      "expression": [
        {
          "type": "property",
          "key": "host",
          "value": "anodot13",
          "isExact": true
        }
      ]
    },
    "aggregation": {
      "aggregationField": "source",
      "topEventSize": 20,
      "maxBuckets": 100,
      "resolution": "longlong"
    }
  }
}
```

> Response Example 

```json
{
  "resolution": "long",
  "totalEvents": 91,
  "events": [
    {
      "date": 1513760272,
      "totalEvents": 1,
      "topEvent": [
        {
          "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
          "title": "deployment started on myServer",
          "description": "my description",
          "category": "deployments",
          "source": "chef",
          "properties": [
            {
              "key": "service",
              "value": "myService"
            }
          ],
          "startDate": 1468326864,
          "endDate": null
        }
      ],
      "topGroups": [
        {
          "title": "deployments",
          "total": 1
        }
      ]
    }
  ]
}
```

> Response Example - Request is in bad format (HTTP Status 400)

```json
{
  "error": {
    "error": 3015,
    "description": "field properties must be unique"
  }
}
```

Use this call to retrieve events by search expression.

Argument | Description
-------- | ----------
fromDate | (integer) Start time for the event retrieval [**Required**]
toDate | (integer) End time for the event retrieval.</br>Default value: the request activation time (current time).
order | (string) Result order. Default: asc. Values: asc, desc.
size | (integer) Number of results per page. Default: 10
index | (integer) Page index, used for paged results.

### Delete Event by ID

> Request Example - Delete event by Event ID

```shell
curl -X DELETE \
https://app.anodot.com/api/v2/user-events/9b0cfa33-8a78-416e-84b7-1bff5628deaf \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this call to delete a single event by its ID.

Argument | Definition
-------- | ----------
ID | The ID of the event

### Delete Events

> Request Example - Delete multiple events using expression

```shell
curl -X DELETE \
https://app.anodot.com/api/v2/user-events/ \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{ 
   "expression": [ 
   { 
      "type": "property", 
      "key": "category", 
      "value": "deployments" 
   }, 
   { 
      "type": "property", 
      "key": "ver", 
      "value": "!*" 
   } 
   ]   
}

```

> Request Example - Delete multiple events using an eventID array

```shell
curl -X DELETE \
https://app.anodot.com/api/v2/user-events/ \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{ 
    "ids": [
      "event_ID_1",
      "event_ID_2"
  ]  
}

```

Use this call to delete multiple events, located by a search expression, or by their ids.

Argument | Definition
-------- | ----------
expression[] | An array of search expressions to locate a single or multiple events.</br>Note that all properties are provided as a flat list, regardless of the event hierarchy.

Argument | Definition
-------- | ----------
ids[] | An array of event ids to locate a single or multiple events


### *Categories*

> End Point Prefix is **/api/v2/bc/user-events/categories**

A category is one of the properties of an event.
Anodot provides default categories to help in event classification.
These default categories are **Deployments**, **Alerts**, **Holidays** and **Other**.
The default categories cannot be removed or modified, user defined categories can be added, edited and removed.

### Create Event Category

> Request example - Creating an event category

```shell
curl -X POST \
https://app.anodot.com/api/v2/user-events/categories \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{
  "category": 
  {
    "name":"myCategory",
    "imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-anodot.png"
    }  
}'
```

> Response example

```json
 { 
   "id":"430f648e-f5e3-40ca-a7e5-50b773ede81b",
   "imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-anodot.png",
   "name":"myCategory",
   "owner":"user"
}
```

This call allows you to create a new event category.

Argument | Definition
-------- | ----------
name | The category name and the way it will be displayed. Name must be unique [**required**]
imageUrl | A url to the image that will be displayed as an icon for the category [*optional*]. The ImageUrl must be located under a secure server - for example  https://myServer/myImage.png. The recommended image size is 32px by 32px.

**Response Fields**
The response is an object similar to the request, enabling you to verify that the object was created successfully.
### Retrieve all Categories

> Request Example - Get all Categories

```shell
curl -X GET \
https://app.anodot.com/api/v2/user-events/categories \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Retrieve all the categories in the account.

Parameters: None.

Response: An array of all the categories defined in the account

> Response example

```json
[
  {
    "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
    "name": "deployment",
    "imageUrl": "http://www.pic.com",
    "owner": "user"
  }
]
```

### Delete Event Category by ID

> Request Example - Delete Event Category by ID

```shell
curl -X DELETE \
https://app.anodot.com/api/v2/user-events/categories/9b0cfa33-8a78-416e-84b7-1bff5628deaf \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Delete Event Category by ID

Argument | Definition
-------- | ----------
Id | (string) The category Id [**required**]

### *Sources*

A source is one of the properties of an event.

> End Point Prefix is **/api/v2/bc/user-events/sources**

### Create event Source

> Request Example - Create Event Source

```shell
curl -X POST \
https://app.anodot.com/api/v2/user-events/sources \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{
  "source": {
    "name": "<source name>",
    "imageUrl": "<image url>"
    }  
}'
```

> Response example

```json
 { 
   "id":"430f648e-f5e3-40ca-a7e5-50b773ede81b",
   "imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-anodot.png",
   "name":"mySource",
   "owner":"user"
}
```

Create a new event source

Argument | Definition
-------- | ----------
name | The source name and the way it will be displayed. Name must be unique [**required**]
imageUrl | A url to the image that will be displayed as an icon for the source [*optional*]. The ImageUrl must be located under a secure server - for example  https://myServer/myImage.png. The recommended image size is 32px by 32px.

### Retrieve all event Sources

> Request Example - Get all event sources

```shell
curl -X GET \
https://app.anodot.com/api/v2/user-events/sources \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Retrieve all the event sources in the account.

Parameters: None.

Response: An array of all the sources defined in the account

> Response example

```json
[
  {
    "id":"0",
  "imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-anodot.png",
  "name":"anodot",
  "owner":"anodot"
  },
  {
    "id":"1",
    "imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-chef.png",
    "name":"chef",
    "owner":"anodot"
    },
    {
      "id":"2",
      "imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-jenkins.png",
      "name":"jenkins",
      "owner":"anodot"
      }
    ]
```

### Delete event Source by ID

> Request Example - Delete Event Source by ID

```shell
curl -X DELETE \
https://app.anodot.com/api/v2/user-events/sources/9b0cfa33-8a78-416e-84b7-1bff5628deaf \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Delete Event Source by ID

Argument | Definition
-------- | ----------
Id | (string) The source Id [**required**]
