# Anomaly

## The Anomaly Object

> End Point

> https://api.anodot.com/api/v2/anomalies - Access Token Authentication

Anomalies are deviations from a normal pattern in one or more metrics that signal unexpected behavior.

The anomaly API can be used to get metrics with anomalous information.

## List anomalies

> Request Structure: GET https://api.anodot.com/api/v2/anomalies

```shell
curl "https://api.anodot.com/api/v2/anomalies"
  -H "Authorization: meowmeowmeow"
```

Get all anomalies, used by anomaly dashboard.  
Used to fetch multiple group anomalies by their properties.  
All Arguments are passed as query params, there is no body to the call.  

Argument | Description
-------- | -----------
q * (string) | Search Query - include only anomalies of abnormal metrics that are part of the given search query.
startDate (integer) | Filter anomalies that started after this given time (inclusive). Units are epoch in seconds.
endDate (integer) | Relevant only when requesting closed anomalies, only anomalies that started before the given time are retrieved (inclusive). Units are epoch in seconds.
state (string) | Anomaly state {open, closed, both}
resolution (string) | Include anomalies of the listed timescales,<br>currently supported timescales are: {short, medium, long, longlong, weekly}.<br>The list passed should be comma separated string list. Available values : short, medium, long, longlong
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

### Response

```json
{
  "errors": []
}
```

> Response Example

```json
{
    "total": 1146,
    "pagingMax": the max numbers of items supported for paging, beyond this limit no items will return //NEW
    "anomalies": [
        {
            "id": "2af8dcb64b3520178dd115345af28bc3", //<string>
            "type": 'business', //<string> 'business', 'application', 'system'
            "user": '', //<string> the customer Id the anomaly is associated with (for NOC)
            "state": "open", //<string> 'open' or 'closed'
            "name": "anomaly name given by the user", //<string> currently not in use
            "resolution": 'small', //<string> 'small', 'medium', 'large'
            "startDate": 1409051280, //epoch
            "endDate": 1409051310, //epoch, optional
            "score": 95, //<int 0-100>
            "currentScore": 95, //<int 0-100> for future use
            "hiddenByUsers": [ //used to be likes
                {
                    'date': 1409051280, //epoch
                    'userId': '', //<string> client-user
                }
            ],
            "metricsCount": {
                "total": 56,
                "system": 20,
                "application": 18,
                "business": 18
            },
            "activities": [
                {
                    'date': 1409051280, //epoch
                    'userId': '', //<string> client-user,
                    'type': '', //<string> 'resolve', 'root-cause', 'none', 'star'
                    'text': '' //<string>
                }
            ],
            "tokenCloud": [
                {
                    "text": "customer=test",
                    "weight": 31.637347752404885
                }
            ],
            "metrics": [  //should return All metrics and not just the descriptive ones
                {
                    "id": 'metric Id',
                    "name": "yonatan.anomaly5...m.role.monitored.iowait", //does not really matter
                    "compositeId": 'compositeId',
                    "maxDeviation": 5,
                    "score": 90,
                    "representative": true, //not in use any more
                    "type": 1,// 0-not set, 1-business, 2-application, 3-system
                    "dataPoints": [
                        //empty - without actual points data
                    ],
                    "baseline": [
                        //empty - without actual points data
                    ],
                    "anomalies": [
                        //empty - without actual points data
                    ]
                }
                //more metrics...
            ]
        }
        //more anomalies...
    ]
}
```

Value | Description | Example
------|-------------|--------
total (integer) | Total Anomalies | 1146
pagingMax (integer) | The max numbers of items supported for paging, beyond this limit no items will return. | 100
anomalies (array) | See *GroupAnomaly Array* below |   

*GroupAnomaly Array*

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
metrics (array) | See *AbnormalMetric Array* below |   

*AbnormalMetric Array*

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

*AnomalyInterval Array*

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

## Get Anomalies token map

## Get Anomaly Metrics

## Get Metric Anomalies
