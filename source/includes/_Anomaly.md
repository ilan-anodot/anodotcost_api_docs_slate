## Anomaly

### The Anomaly Object

> End Point **/api/v2/anomalies**


Anomalies are deviations from a normal pattern in one or more metrics that signal unexpected behavior. Anodot treats anomalies as distinct objects when have their own attributes, rather than a collection of anomalous data points. 

The anomaly API can be used to get and anlalyze the list of anomalies in your account. 

### List anomalies

> Request Example: Get all anomalies

```shell
curl -X GET 'https://app.anodot.com/api/v2/anomalies' \
-H 'Authorization: Bearer {bearer-token} \
-H 'Content-Type: application/json'
```

Get all anomalies based on a search query. If no query is defined, the call will return all the anomalies in the account.   
Used to fetch multiple group anomalies by their properties.  
All Arguments are passed as query params, there is no body to the call.  

Argument | Description
-------- | -----------
q * (string) | Search Query - include only anomalies of abnormal metrics that are part of the given search query.
startDate (integer) | Filter anomalies that started after this given time (inclusive). Units are epoch in seconds.
endDate (integer) | Relevant only when requesting closed anomalies, only anomalies that started before the given time are retrieved (inclusive). Units are epoch in seconds.
state (string) | Anomaly state {open, closed, both}
resolution (string) | Include anomalies of the listed timescales,<br>currently supported timescales are: {short (1 min), medium (5 min), long (1 hour), longlong (1 day), weekly}.<br>The list passed should be comma separated string list. Available values : short, medium, long, longlong
durationUnit (string) | Can be one of the following: {seconds, minutes, hours, days}.
durationValue (integer) | Minimum anomaly duration. Units are based on durationUnits.
score (number) | Minimum anomaly score in range (0,1) (inclusive).
bookmark (string) | Filter by bookmarks. The following options are available:<br>all - filter bookmarked anomalies.<br>none - filter anomalies with no bookmarks.<br>*user_id* - filter anomalies that are bookmarked by the given user.<br>* - filter anomalies that are not bookmarked by the given user.
sort (string) | Sort by {startDate, endDate, duration, score}.
order (string) | Sort order {asc, desc}.
index (integer) | Anomaly starting index (for paging the results).
size (integer) | Number of anomalies to fetch staring from index given by parameter index (for paging the results).
valueDirection (string) | Direction of the anomaly breach - one of: {down,up,both}.
deltaType (string) | Delta of the metric anomaly peak relative to the baseline. Delta type can be: percentage or absolute.
delta (number) | Delta of the metric anomaly peak relative to the baseline. Units are based on the deltaType.

#### Response

> Response Example

```json
{
    "total": 1146,
    "pagingMax": "50",     
    "anomalies": [
        {
            "id": "2af8dcb64b3520178dd115345af28bc3", 
            "state": "open", 
            "name": "anomaly name given by the user", 
            "resolution": 'small', 
            "startDate": 1409051280, 
            "endDate": 1409051310, 
            "score": 95, 
            "duration": 300,
            "detailedScore": 95, 
            "metrics": [  
                {
                    "compositeId": "",
                    "user": "58e5f6b3ba01b264935d99f9",
                    "score": 0.41,
                    "maxBreach": 12.130026035603969,
                    "maxBreachPercentage": 46.88843542053082,
                    "anomalySumDeltas": 22.427541379084232,
                    "upperAbsoluteDelta": 12.130026035603969,
                    "upperPercentageDelta": 46.88843542053082,
                    "value": 23,
                    "delta": 46.88843542053082,
                    "state": "closed",
                    "correlation": "graph",
                    "lastNormalTime": 1614508200,
                    "timestamp": 1614516760,
                    "anomalies": [
                        [
                            1614508800,
                            1614513600,
                            "closed",
                            0.41,
                            true,
                            38,
                            "transient"
                        ]
                    ],
                    "currentAnomalyIntervals": [
                        {
                            "startDate": 1614508800,
                            "endDate": 1614513600,
                            "state": "closed",
                            "score": 0.41,
                            "directionUp": true,
                            "type": "transient",
                            "peakValue": 38,
                            "absoluteDelta": 12.130026035603969,
                            "percentageDelta": 46.88843542053082,
                            "anomalySumDeltas": 22.427541379084232
                        }
                    ],
                    "otherAnomalyIntervals": [],
                    "id": "bctele.0.SSSoKR5Z9lZ5S.RUNNING.samples-submit",
                    "name": "what=monitoring.module=bc.stream_name=test_BC_in_prod.stream_state=RUNNING.stream_type=Postgres.target_type=counter.type=samples-submit.unit=samples.version=0",
                    "what": "monitoring",
                    "properties": [
                        {
                            "key": "module",
                            "value": "bc"
                        },
                        {
                            "key": "stream_name",
                            "value": "test_BC_in_prod"
                        },
                        {
                            "key": "stream_state",
                            "value": "RUNNING"
                        },
                        {
                            "key": "stream_type",
                            "value": "Postgres"
                        },
                        {
                            "key": "type",
                            "value": "samples-submit"
                        },
                        {
                            "key": "version",
                            "value": "0"
                        },
                        {
                            "key": "target_type",
                            "value": "counter"
                        },
                        {
                            "key": "unit",
                            "value": "samples"
                        }
                    ],
                    "tags": [],
                    "dataPoints": [],
                    "baseline": []    
                }
            ]
        } 
    ]
}
```

Value | Description | Example
------|-------------|--------
total (integer) | Total Anomalies | 1146
pagingMax (integer) | The max numbers of items supported for paging, beyond this limit no items will return. | 100
anomalies (array) | An array of anomalies. See below |   


**GroupAnomaly Array**

Value | Description | Example
------|-------------|--------
id | Anomaly Id | 2af8dcb64b3520178dd115345af28bc3
mergedIds (array) | Ids of anomalies that were merged into this anomaly group. | [ "1111111111111cff9ffb666814ef311b", "2222222222222cff9ffb666814ef311b" ]
state (string) | State of anomaly - Enum: [open, closed] | closed
resolution (string) | Time scale of the anomaly metrics. Enum: [ small, medium, long, longlong, weekly ] | medium
startDate (integer) | Start date of the anomaly. | 1470045775
endDate (integer) | End date of the anomaly. | 1470218575
score (double) | Anomaly score in [0, 1]. (inclusive) | 0.95
duration (integer) | Anomaly duration in seconds. | 300
metrics (array) | See *Metric Array* below |   


**Metric Array**

Value | Description | Example
------|-------------|--------
compositeId (string) | Id of the composite definition generating the metric (null for non composite metrics). | 111111122222222333333
score (double) | Anomaly score in [0, 1]. (inclusive) | 0.2
upperAbsoluteDelta (double) | Absolute breach delta relative to the upper baseline | 30.7
upperPercentageDelta (double) | Percentage breach delta relative to the upper baseline | 70.9
lowerAbsoluteDelta (double) | Absolute breach delta relative to the lower baseline | 20.5
lowerPercentageDelta (double) | Percentage breach delta relative to the lower baseline | 60.4
state (string) | Anomaly state - open or closed | open
lastNormalTime (ineteger) | Last epoch time the metric recieved sample within the baseline band | 1470218575
timestamp (integer) | Last server time (epoch in seconds) of when the abnormal metric was reported | 1470218575
currentAnomalyIntervals (array) | See *AnomalyInterval Array* below | 

**AnomalyInterval Array**

Value | Description | Example
------|-------------|--------
startDate (integer)| Start time of the anomaly interval (epoch in seconds) | 1470045775
endDate (integer) | End of the anomaly interval (epoch in seconds), null for open anomalies | 1470218575
state (string) | Anomaly interval state, open or closed | open
score (double) | Anomaly score in [0, 1]. (inclusive) | 0.67
directionUp (boolean) | Boolean indicating the anomaly interval direction | true
type (string) | Anomaly classification. transient or pattern_change | transient
peakValue (double) | Anomaly interval peak value | 60.8
absoluteDelta (double) | Absolute breach delta relative to the baseline | 45.8
percentageDelta (double) | Percentage breach delta relative to the baseline | 34.12

### Count Anomalies

> Request Example: Get count of anomalies

```shell
curl -X GET 'https://app.anodot.com/api/v2/anomalies/count?startDate=1648512000&endDate=1648598400' \
-H 'Authorization: Bearer {bearer-token} \
-H 'Content-Type: application/json'
```

Get a total number of anomalies identified by Anodot for a time frame.

Argument | Description
-------- | -----------
startDate (integer) | Filter anomalies that started after this given time (inclusive). Units are epoch in seconds.
endDate (integer) | Relevant only when requesting closed anomalies, only anomalies that started before the given time are retrieved (inclusive). Units are epoch in seconds.

#### Response

> Response Example

```json
{
    "total": 1819
}
```

Value | Description 
------|-------------
total (integer) | Total Anomalies

### Get Anomalies token map

> End Point Prefix: **/api/v2/anomalies/tokenMap**

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/anomalies/tokenMap?anomalyId=36862597743148e0b2d70f95a5f56339&resolution=longlong' \
--header 'Authorization: Bearer {bearer-token}
```

Retrieves the token map for an anomaly if exists. Notice that all the below parameters are appended to the URL and the body remains empty. 

Argument | Description
-------- | -----------
q * (string) | Search Query - include only anomalies of abnormal metrics that are part of the given search query (mandatory when no anomalyId is passed)
anomalyId (string) | Filter by specific anomaly Id (mandatory when no q is passed) 
startDate (integer) | Filter anomalies that started after this given time (inclusive). Units are epoch in seconds.
endDate (integer) | Relevant only when requesting closed anomalies, only anomalies that started before the given time are retrieved (inclusive). Units are epoch in seconds.
state (string) | Anomaly state {open, closed, both}
resolution (string) | Include anomalies of the listed timescales,<br>currently supported timescales are: {short, medium, long, longlong, weekly}.<br>The list passed should be comma separated string list. This parameter is mandatory when using anomlayId.
durationUnit (string) | Can be one of the following: {seconds, minutes, hours, days}.
durationValue (integer) | Minimum anomaly duration. Units are based on durationUnits.
score (number) | Minimum anomaly score in range (0,1) (inclusive).
sort (string) | Sort by (startData, endDate, duration, score)
bookmark (string) | Filter by bookmarks. The following options are available:<br>all - filter bookmarked anomalies.<br>none - filter anomalies with no bookmarks.<br>*user_id* - filter anomalies that are bookmarked by the given user.<br>* - filter anomalies that are not bookmarked by the given user.
order (string) | Sort order {asc, desc}.
valueDirection (string) | Direction of the anomaly breach - one of {down, up, both}
delta (number) | Delta of the metric anomaly peak relative to the baseline. Delta type can be: percentage or absolute.
delta (number) | Delta of the metric anomaly peak relative to the baseline. Units are based on the deltaType.
tokenMapClustersLimit (integer) | Number of token map clusters to fetch
toklenMapClusterLimit (integer) | Max number of tokens to include per cluster

> Response Example:

```json
[
  {
    "key": "what",
    "value": "errors",
    "weight": 1.53303,
    "tokens": [
      {
        "key": "server",
        "value": "my_server_name01",
        "weight": 0.7,
        "anomalyOccurrences": 30,
        "totalOccurrences": 309
      }
    ],
    "anomalyOccurrences": 30,
    "totalOccurrences": 309
  }
]
```

#### Response 
An array of key-values with tokens of the specific anomaly.

Argument | Description
-------- | -----------
Key (string) | Token Key.
Value (String) | Token value.
weight (number) | weight of the token
anomalyOccurances (number) | How many abnormal metrics of the cluster measure (what)
totalOccurances (number) | The number of metrics in the cluster measure (what)

### Get Anomaly Metrics

> End Point Prefix: **/api/v2/anomalies/{anomalyId}/metrics

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/anomalies/36862597743148e0b2d70f95a5f56339/metrics?startDate=1614507300' \
--header 'Authorization: Bearer {bearer-token}'
```

Get anomaly data, used in anomaly investigation view. Used to fetch a single anomaly including the abnormal metrics data points.

Argument | Description
-------- | -----------
anomalyId (String) | (Required) The Id of the anomaly
startDate (Integer) | Filter anomalies that started after this given time (inclusive). Units are epoch in seconds. 
endDate (Integer) | Relevant only when requesting closed anomalies. Filter anomalies that started before this given time (inclusive). Units are epoch in seconds. 
q (String) | Search query - include only abnormal metrics what are part of the given search query.
state (String) | Anomaly State {open, closed, both}
resolution (string) | Include anomalies of the listed timescales: {short, medium, long, longlong, weekly}. The list passed should be a comma seperated string list.
durationUnit (String) | Can be one of the following: {seconds, minutes, hours, days}
durationValue (Integer) | Minimum anomaly duration. Units are based on *durationUnits*
score (Number) | Minimum anomaly score (between 0 and 1)
sort (String) | Sort by {startDate, endDate, duration, score}
order (String) | Sort order {asc, desc}
index (integer) | Metrics starting index (for paging the results)
valueDirection (String) | Direction of the anomaly breach - one of {down, up, both}
deltaType (String) | Delta of the metric anomaly peak relative to the baseline. Delta type can be percentage or absolute.
delta (Number) | Delt aof the metric anomaly peak relative to the baseline. Units are based on the *deltaType*

> Response Example:

```json
{{
  "id": "2af8dcb64b3520178dd115345af28bc3",
  "mergedIds": [
    "1111111111111cff9ffb666814ef311b",
    "2222222222222cff9ffb666814ef311b"
  ],
  "state": "closed",
  "resolution": "medium",
  "startDate": 1470045775,
  "endDate": 1470218575,
  "score": 0.95,
  "duration": 300,
  "metrics": [
    {
      "compositeId": "111111122222222333333",
      "score": 0.2,
      "upperAbsoluteDelta": 30.7,
      "upperPercentageDelta": 70.9,
      "lowerAbsoluteDelta": 30.7,
      "lowerPercentageDelta": 70.9,
      "state": "open",
      "lastNormalTime": 1470218575,
      "timestamp": 1470218575,
      "currentAnomalyIntervals": [
        {
          "startDate": 1470045775,
          "endDate": 1470218575,
          "state": "open",
          "score": 0.67,
          "directionUp": true,
          "type": "transient",
          "peakValue": 60.8,
          "absoluteDelta": 45.8,
          "percentageDelta": 34.12
        }
      ]
    }
  ]
}
```

### Get Metric Anomalies

> End Point Prefix: **/api/v2/anomalies/{anomalyId}/metric/{metricId}**

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/anomalies/36862597743148e0b2d70f95a5f56339/metric/bctele.0.SSSY8i9l3GIoH.RUNNING.data-lag?startDate=1614507300' \
--header 'Authorization: Bearer {bearer-token} \
```

Argument | Description
-------- | -----------
anomalyId (String) | (Required) The Id of the anomaly
metricId (String) | (Required) The Name of the metric. Example: *what=cpu.server=ano1.dc=ny*
startDate (Integer) | Filter anomalies that started after this given time (inclusive). Units are epoch in seconds. 
endDate (Integer) | Relevant only when requesting closed anomalies. Filter anomalies that started before this given time (inclusive). Units are epoch in seconds. 
resolution (string) | Include anomalies of the listed timescales: {short, medium, long, longlong, weekly}. The list passed should be a comma seperated string list.

> Response Example:

```json
{
  "compositeId": "111111122222222333333",
  "score": 0.2,
  "upperAbsoluteDelta": 30.7,
  "upperPercentageDelta": 70.9,
  "lowerAbsoluteDelta": 30.7,
  "lowerPercentageDelta": 70.9,
  "state": "open",
  "lastNormalTime": 1470218575,
  "timestamp": 1470218575,
  "currentAnomalyIntervals": [
    {
      "startDate": 1470045775,
      "endDate": 1470218575,
      "state": "open",
      "score": 0.67,
      "directionUp": true,
      "type": "transient",
      "peakValue": 60.8,
      "absoluteDelta": 45.8,
      "percentageDelta": 34.12
    }
  ]
}
```

