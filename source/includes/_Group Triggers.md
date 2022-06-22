### Get Triggered Alert Groups

> Example Request: Get list of alert groups by search parameters

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/alerts/trigger-group?startTime=1616929887&sort=startTime&order=asc&severity=HIGH' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkYXRhIjoiMDgwNjVlYjA3ZTI4ZTRiZGVlZjkxZGJlNGZlM2I1YjA1MzZlMjc4NTA1NjhjOTdmYmQwNWMwMDI4ZThhMWFiMDFhZDA3MWIxZDIiLCJpYXQiOjE2MTgxMzUyOTgsImV4cCI6MTYyMDcyNzI5OH0.WlxxH0ekbldZyAtvHjCckDsBARJlig6hHlXfE6DCsXM' \
--data-raw ''
```

Fetch alert triggers for a specific time frame based on a given set of filters.
Arrays can be passed as comma seperated list of strings.
#### Request Arguments

Argument | Type | Description
---------|------|------------
channels | array string | Channel Id of the alert.<br/>Only alerts that were triggered to the specified list of channels are included in the result. User Ids may be specified as well to filter alerts that have specific subscribers.
searchQuery | string | Filter alerts that include the given search query in the alert title or description.
metricQuery | string | Filter alerts that include the metrics specified by the given search query.
startTime | integer | Time the alert was opened (epoch in seconds). Only alerts that occured after this given time are included in the result. **required**
endTime | integer | Time the alert was closed (epoch in seconds). Only alerts that started before this given time are included in the result.
snoozed | boolean | Include alerts that contain at least one snoozed metric. Default is snoozed.
updatedStartTime | integer | Time the alert was last updated (epoch in seconds). Only alerts that were updated after the given time are included in the result.
updatedEndTime | integer | Time the alert was last updated (epoch in seconds). Only alerts that were updated before the given time are included in the result.
ackType | integer | TBD akcnowlage
type | array string | Array of the following strings 'no_data', 'static', 'anomaly'. By default alert of all types are returned. Available values : no_data, static, anomaly
status | array string | Array of the following strings: 'open', 'closed'. By default alert of all statuses are returned. Available values : open, closed
severity | array string | Array of the following strings 'critical', 'high', 'medium', 'low', 'info'.<br/>By default alert of all serverities are returned. Available values : critical, high, medium, low, info.
sort | string | Sorting by one of the given fields: 'startTime', 'updatedTime', 'severity'.<br/>Default value is 'updatedTime'. When non time-based field is selected a secondary sorting by updatedTime is performed.<br/> When using grouping, the sorting of the group is perforned accordingly:<br/>* startTime - min of startTime cross all alerts in the group.<br/>* updatedTime - max of updatedTime cross all alerts in the group.<br/>* severity - highest severity cross all alerts in the group.<br/>Available values : startTime, updatedTime, severity
order | string | Sorting order, one of: 'asc', 'desc'.<br/>Default value is 'desc'.<br/>Available values : asc, desc
groupBy | string | Group the anomaly instances. There are few supported groupings:<br/>* relatedAlerts - group related alerts together.<br/>* alertConfiguration - group alert instances by the alert configuration.<br/>* none - no grouping. When none is selected the grouping level of the response is ommited.<br/>Default value is 'relatedAlerts'.<br/>Available values : relatedAlerts, alertConfiguration, none.
index | integer | Number of records to skip for pagination
size | integer | Maximum number of records to return per page.

> Example Response:

```json
{
  "total": 121,
  "alertGroups": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
      "name": "Jetty servers availability",
      "alerts": [
        {
          "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
          "timeScale": "5m",
          "type": "anomaly",
          "status": "open",
          "groupId": "20262fb3-00ba-412c-a515-aec74c4824ca",
          "alertConfigurationId": "20262fb3-00ba-412c-a515-aec74c4824ca",
          "title": "Jetty servers availability",
          "description": "This alerts monitors the availablity of the jetty servers",
          "severity": "critical",
          "startTime": 1512882919,
          "updatedTime": 1512882919,
          "endTime": 1512982919,
          "channels": [
            {
              "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
              "type": "email",
              "name": "NOC Channel"
            }
          ],
          "metrics": [
            {
              "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
              "name": "what=cpu.metric=cpuIdle.server=anodot.dc=ny",
              "what": "cpu",
              "properties": [
                {
                  "key": "server",
                  "value": "anodot32"
                }
              ],
              "tags": [
                {
                  "key": "server",
                  "value": "anodot32"
                }
              ],
              "snooze": {
                "userId": "20262fb3-00ba-412c-a515-aec74c4824ca",
                "snoozeTime": 1525586116,
                "resumeTime": 1525586116
              },
              "origin": {
                "type": "alert",
                "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
                "title": "GA sessions of my application"
              },
              "status": "closed",
              "updatedTime": 1512982919,
              "displayStartTime": 1512982919,
              "displayEndTime": 1512982919,
              "intervals": [
                {
                  "status": "closed",
                  "startTime": 1512882919,
                  "endTime": 1512982919,
                  "duration": 300,
                  "anomalyId": "20262fb3-00ba-412c-a515-aec74c4824ca",
                  "direction": "up",
                  "score": 0.99,
                  "peak": 45.99,
                  "deltaAbsolute": 3.76,
                  "deltaPercentage": 10.42
                },
                {
                  "status": "closed",
                  "startTime": 1512882919,
                  "endTime": 1512982919,
                  "duration": 300,
                  "lastSeenTime": 1512382919
                },
                {
                  "status": "closed",
                  "startTime": 1512882919,
                  "endTime": 1512982919,
                  "duration": 300,
                  "threshold": 40.3,
                  "direction": "up",
                  "peak": 45.99
                }
              ]
            }
          ]
        }
      ],
      "stars": [
        "string"
      ]
    }
  ]
}
```

### Get Alert Trigger by Group Id

> Request Structure: GET Anomaly Group by ID

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/alerts/trigger-group/b0f67e314de54360862132977579ab32' \
--header 'Authorization: Bearer {{bearer-token}}
```

Find trigger by group Id
Fetch a single alert grouptrigger instance.

#### Request Arguments

Argument | Type | Description
---------|------|------------
groupId | string | ID of the alert group **required**

> Example Response:

```json
{
  "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
  "timeScale": "5m",
  "type": "anomaly",
  "status": "open",
  "groupId": "20262fb3-00ba-412c-a515-aec74c4824ca",
  "alertConfigurationId": "20262fb3-00ba-412c-a515-aec74c4824ca",
  "title": "Jetty servers availability",
  "description": "This alerts monitors the availablity of the jetty servers",
  "severity": "critical",
  "startTime": 1512882919,
  "updatedTime": 1512882919,
  "endTime": 1512982919,
  "channels": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
      "type": "email",
      "name": "NOC Channel"
    }
  ],
  "metrics": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
      "name": "what=cpu.metric=cpuIdle.server=anodot.dc=ny",
      "what": "cpu",
      "properties": [
        {
          "key": "server",
          "value": "anodot32"
        }
      ],
      "tags": [
        {
          "key": "server",
          "value": "anodot32"
        }
      ],
      "snooze": {
        "userId": "20262fb3-00ba-412c-a515-aec74c4824ca",
        "snoozeTime": 1525586116,
        "resumeTime": 1525586116
      },
      "origin": {
        "type": "alert",
        "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
        "title": "GA sessions of my application"
      },
      "status": "closed",
      "updatedTime": 1512982919,
      "displayStartTime": 1512982919,
      "displayEndTime": 1512982919,
      "intervals": [
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "anomalyId": "20262fb3-00ba-412c-a515-aec74c4824ca",
          "direction": "up",
          "score": 0.99,
          "peak": 45.99,
          "deltaAbsolute": 3.76,
          "deltaPercentage": 10.42
        },
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "lastSeenTime": 1512382919
        },
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "threshold": 40.3,
          "direction": "up",
          "peak": 45.99
        }
      ]
    }
  ]
}
```
### Get Triggered Alerts by TriggerId

> Request Example: Get Triggered Alert by TriggerID

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/alerts/triggers?triggerId=012887cb-77b1-4360-ac44-1fac4fc8a2bb' \
--header 'Authorization: Bearer {{bearer-token}'
```

#### Request Arguments

Argument | Type | Description
---------|------|------------
triggerId **Required** | string | Id of the alert trigger to fetch
metricQuery | string | Filter alerts that include the metrics specified by the given search query
sort | string | Sorting by one of the given fields: 'startTime', 'updatedTime'.<br/>Default value is 'updatedTime'.<br/>Available values : startTime, updatedTime.
order | string | Sorting order, one of: 'asc', 'desc'.<br/>Default value is 'desc'.<br/>Available values : asc, desc

> Response Example:

```json
{
  "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
  "timeScale": "5m",
  "type": "anomaly",
  "status": "open",
  "groupId": "20262fb3-00ba-412c-a515-aec74c4824ca",
  "alertConfigurationId": "20262fb3-00ba-412c-a515-aec74c4824ca",
  "title": "Jetty servers availability",
  "description": "This alerts monitors the availablity of the jetty servers",
  "severity": "critical",
  "startTime": 1512882919,
  "updatedTime": 1512882919,
  "endTime": 1512982919,
  "channels": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
      "type": "email",
      "name": "NOC Channel"
    }
  ],
  "metrics": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
      "name": "what=cpu.metric=cpuIdle.server=anodot.dc=ny",
      "what": "cpu",
      "properties": [
        {
          "key": "server",
          "value": "anodot32"
        }
      ],
      "tags": [
        {
          "key": "server",
          "value": "anodot32"
        }
      ],
      "snooze": {
        "userId": "20262fb3-00ba-412c-a515-aec74c4824ca",
        "snoozeTime": 1525586116,
        "resumeTime": 1525586116
      },
      "origin": {
        "type": "alert",
        "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
        "title": "GA sessions of my application"
      },
      "status": "closed",
      "updatedTime": 1512982919,
      "displayStartTime": 1512982919,
      "displayEndTime": 1512982919,
      "intervals": [
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "anomalyId": "20262fb3-00ba-412c-a515-aec74c4824ca",
          "direction": "up",
          "score": 0.99,
          "peak": 45.99,
          "deltaAbsolute": 3.76,
          "deltaPercentage": 10.42
        },
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "lastSeenTime": 1512382919
        },
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "threshold": 40.3,
          "direction": "up",
          "peak": 45.99
        }
      ]
    }
  ]
}
```
