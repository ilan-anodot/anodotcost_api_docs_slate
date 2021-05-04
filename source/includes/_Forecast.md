# Forecast

> Endpoint: **GET /api/v2/forecast/**

Anodot Autonomous Forecast is an additional product on top of Anodot, enabling the customer to get continous forecast for specific metrics. The forecast product is based on **Tasks** which are running in Anodot. The tasks are providing the projected values of metrics based on previous behaviour. You can consider tasks as a kind of stream which sends forecast metrics into your account, enabling you to build dashboards on top of them.

The data hierarchy as as follows: Forecast Tasks --> Each task has metrics --> Each metric has forecast values (or results). So a good way to traverse all the information would be:<br>

* Get list of tasks --> [GET Forecast Tasks](#get-forecast-tasks)
* for each task - get list of metrics --> [GET Task Metrics](#get-task-metrics)
* fer each metric - get list of results. --> [GET results for a specific metric](#get-results-for-a-specific-metric)


Authentication type: [Access Token Authentication] (#access-tokens).

## GET forecast tasks

> Request Example: Getting all tasks in the account

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/tasks' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

> Response Example:

```json
[
    {
        "forecastTaskId": "565fda42-d3ab-46da-9fe2-d9a57bcf43cf",
        "forecastTaskName": "Forecast_20210411_aws"
    }
]
```

### Response Fields

The response is an array of forecast tasks. Each one of them has the following fields:

Field | Type | Description / Example
-|-|-
forecastTaskId | String ($uuid) | Unique identifier of the forecast task.
forecastTaskName | String | Readable name of the forecast task (to be used going forward in creating dashboards, etc.)


## GET forecast results 

> Request Example: Get forecast results for a specific task

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/tasks/{{forecast-taskid}}/results?startTimestamp=1620119209' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

This call gets all the results for a specific task. This returns the results for all the metrics in the task. 

### Request Fields

Field | Type | Description / Example
-|-|-
forecastTaskId | String ($uuid) | Unique identifier of the forecast task (you can get this from the [GET Forecast Tasks](#get-forecast-tasks) call).
startTimestamp | timestamp (Optional) | Will retrieve forcast results **after** this timestamp. 

> Response Example:

```json
{
  {
    "forecastResults": [
        {
            "last_forecast_ts": "1620000000",
            "last_updated_ts": "1620119209",
            "metric_id": "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSBackup.UnblendedCost",
            "points": [
                {
                    "lowerBand": 0.4523230493,
                    "timestamp": 1620086400,
                    "upperBand": 0.9068104625,
                    "value": 0.5088179708
                },
                {
                    "lowerBand": 0.5381199121,
                    "timestamp": 1620172800,
                    "upperBand": 1.0131766796,
                    "value": 0.6004434824
                },
                {
                    "lowerBand": 1.136085391,
                    "timestamp": 1620259200,
                    "upperBand": 1.7545015812,
                    "value": 1.2390322685
                },
                {
                    "lowerBand": 1.1384156942,
                    "timestamp": 1620345600,
                    "upperBand": 1.7573906183,
                    "value": 1.2415208817
                }
            ]
        },
        {
            "last_forecast_ts": "1620000000",
            "last_updated_ts": "1620119208",
            "metric_id": "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSCloudTrail.UnblendedCost",
            "points": [
                {
                    "lowerBand": 110.9317855835,
                    "timestamp": 1620086400,
                    "upperBand": 114.520149231,
                    "value": 113.305770874
                },
                {
                    "lowerBand": 215.4238891602,
                    "timestamp": 1620172800,
                    "upperBand": 220.5315246582,
                    "value": 218.9199981689
                },
                {
                    "lowerBand": 112.3378067017,
                    "timestamp": 1620259200,
                    "upperBand": 115.9466094971,
                    "value": 114.726890564
                },
                {
                    "lowerBand": 206.5173797607,
                    "timestamp": 1620345600,
                    "upperBand": 211.495513916,
                    "value": 209.9178314209
                }
            ]
        }
    ]
}
```

### Response Fields

The response is an array of **forecast results**. A forecast result will be a set of predicted data points per metric. A data point has a value and a lower band and upper band of forecast, all at a given time stamp. The response itself looks as following: 

Field | Type | Description / Example
-|-|-
last_forecast_ts | timestamp | timestamp of when the last forecast was calculated.
last_updated_ts | timestamp | timestamp of when the forecast was calculated.
metric_id | String | Name of forecast metric
points | Array | An array of forecast data points. 


## GET task metrics

> Request Example: Get forecast metrics for a specific task

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/tasks/{{forecast-taskid}}/metrics?startTimestamp=1620119209' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

This call gets all the metrics for a specific task. 

### Request Fields

Field | Type | Description / Example
-|-|-
forecastTaskId | String ($uuid) | Unique identifier of the forecast task (you can get this from the [GET Forecast Tasks](#get-forecast-tasks) call).
startTimestamp | timestamp (Optional) | Will retrieve forcast results **after** this timestamp. 

> Response Example:

```json
[
    {
        "forecastMetrics": [
            "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSBackup.UnblendedCost",
            "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSCloudTrail.UnblendedCost",
            "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSConfig.UnblendedCost",
            "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSCostExplorer.UnblendedCost",
            "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSELB.UnblendedCost",
            "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSEvents.UnblendedCost",
            "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSGlue.UnblendedCost"
        ],
        "forecastTaskId": "565fda42-d3ab-46da-9fe2-d9a57bcf43cf"
    }
]
```

### Response Fields

The response is list of metric IDs covered by this task.


## GET results for a specific metric

> Request Example: Get forecast results for a specific metric

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/tasks/{{forecast-taskid}}/metrics/{{forecast-metricId}}/results' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

This call gets all the forecast results for a specific metric.
### Request Fields

Field | Type | Description / Example
-|-|-
forecastTaskId | String ($uuid) | Unique identifier of the forecast task (you can get this from the [GET Forecast Tasks](#get-forecast-tasks) call).
metricID | tString ($uuid) | Unique identifier of the forecast metric (you can get this from the [GET Task  Metrics](#get-task-metrics) call).


> Response Example:

```json
{
    "forecastResults": {
        "last_forecast_ts": "1620000000",
        "last_updated_ts": "1620119209",
        "metric_id": "92fe84ca-97c7-4b27-b993-c53fbb7be33a.AWSBackup.UnblendedCost",
        "points": [
            {
                "lowerBand": 0.4523230493,
                "timestamp": 1620086400,
                "upperBand": 0.9068104625,
                "value": 0.5088179708
            },
            {
                "lowerBand": 0.5381199121,
                "timestamp": 1620172800,
                "upperBand": 1.0131766796,
                "value": 0.6004434824
            },
            {
                "lowerBand": 1.136085391,
                "timestamp": 1620259200,
                "upperBand": 1.7545015812,
                "value": 1.2390322685
            },
            {
                "lowerBand": 1.1384156942,
                "timestamp": 1620345600,
                "upperBand": 1.7573906183,
                "value": 1.2415208817
            }
        ]
    }
}
```

### Response Fields

The response is an array of **forecast results**. A forecast result will be a set of predicted data points per metric. A data point has a value and a lower band and upper band of forecast, all at a given time stamp. See [GET Forecast Results](#get-forecast-results) for the details.