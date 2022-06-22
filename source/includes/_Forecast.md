## Forecast

> Endpoint: **GET /api/v2/forecast/**

Anodot Autonomous Forecast is an additional product on top of Anodot, enabling the customer to get continous forecast for specific metrics.</br>The forecast product is based on **Tasks** which are running in Anodot. The tasks are providing the projected values of metrics based on previous behaviour. You can consider tasks as a kind of stream which sends forecast metrics into your account, enabling you to build dashboards on top of them.

The data hierarchy is as follows: Forecast Tasks --> Each task has metrics --> Each metric has forecast values (or results). So a good way to traverse all the information would be:<br>

* Get list of tasks --> [GET Forecast Tasks](#get-forecast-tasks)
* for each task - get list of metrics --> [GET Task Metrics](#get-task-metrics)
* fer each metric - get list of results. --> [GET forecast results per tasks](#get-forecast-results-per-task)

**New!** We recently added a way to get the metrics directly regardless of tasks. For this please use the following:

* Get all the forecast metrics in the account --> [Get forecast metrics](#get-forecast-metrics)
* Get forecast metric results -->  [GET forecast results](#get-forecast-results)


Authentication type: [Access Token Authentication] (#access-tokens).

### GET forecast tasks

> Request Example: Getting all tasks in the account

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/tasks' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

Get the list of forecast tasks

#### Response Fields

> Response Example:

```json
[
    {
        "forecastTaskId": "565fda42-d3ab-46da-9fe2-d9a57bcf43cf",
        "forecastTaskName": "Forecast_20210411_aws"
    }
]
```

The response is an array of forecast tasks. Each one of them has the following fields:

Field | Type | Description / Example
-|-|-
forecastTaskId | String ($uuid) | Unique identifier of the forecast task.
forecastTaskName | String | Readable name of the forecast task (to be used going forward in creating dashboards, etc.)


### GET forecast results per task

#### Request Fields

> Request Example: Get forecast results for a specific task

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/tasks/{{forecast-taskid}}/results' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

This call gets all the results for a specific task. This returns the results for all the metrics in the task. 

Field | Type | Description / Example
-|-|-
forecastTaskId | String ($uuid) | Unique identifier of the forecast task (you can get this from the [GET Forecast Tasks](#get-forecast-tasks) call).

#### Response Fields

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

<aside class="notice">
The response is an array of forecast results. A forecast result will be a set of predicted data points per metric.
Each data point (per timestamp) is comprised of a the predicted value and 'confidence band' for the value. The confidence band is represented by range between lowerBand and upperBand.
</aside>

The response itself looks as following: 

Field | Type | Description / Example
-|-|-
last_forecast_ts | timestamp (in epoch) | The last timestamp which the forecast was done from, meaning the date from which the forecast is done forward. For example, for a daily forecast of 7 days, the last_forecast_ts can be the epoch of 5.5.2021, and then the resulting 7 days will be of 6.5.2021, 7.5.2021 etc. 
last_updated_ts | timestamp (in epoch) | The time that this record was updated.
metric_id | String | Name of forecast metric
points | Array | An array of forecast data points. 

### GET forecast metrics 

#### Request Fields

> Request Example: Get forecast metrics 

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/metrics' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

This call gets all the forecast metrics in the account

The request has no input fields

#### Response Fields

> Response Example:

```json
{
    "forecastMetrics":
        [
            "3afa10d8-ba60-45aa-aef1-e5b9e5edd01b.0014P00002QZllCQAT.VOLUME_SUM", "ff761ee7-74db-4f26-9293-c242fe55e7b4.sumSeries", 
            "3afa10d8-ba60-45aa-aef1-e5b9e5edd01b.0014P00002QZljTQAT.VOLUME_SUM" 
        ]
}
``` 

Field | Type | Description / Example
-|-|-
metric_id | Array | An array of forecast metric

### GET task metrics

This call gets all the metrics for a specific task.

#### Request Fields

> Request Example: Get forecast metrics for a specific task

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/tasks/{{forecast-taskid}}/metrics' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

Field | Type | Description / Example
-|-|-
forecastTaskId | String ($uuid) | Unique identifier of the forecast task (you can get this from the [GET Forecast Tasks](#get-forecast-tasks) call).

#### Response Fields

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

The response is list of metric IDs covered by this task.


### GET forecast results per task

#### Request Fields

> Request Example: Get forecast results for a specific metric per task

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/tasks/{{forecast-taskid}}/metrics/{{forecast-metricId}}/results' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

This call gets all the forecast results for a specific metric of a given task.

Field | Type | Description / Example
-|-|-
forecastTaskId | String ($uuid) | Unique identifier of the forecast task (you can get this from the [GET Forecast Tasks](#get-forecast-tasks) call).
metricID | String ($uuid) | Unique identifier of the forecast metric (you can get this from the [GET Task  Metrics](#get-task-metrics) call).

#### Response Fields

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

The response is an array of **forecast results**. A forecast result will be a set of predicted data points per metric. A data point has a value and a lower band and upper band of forecast, all at a given time stamp. See [GET Forecast Results](#get-forecast-results) for the details.

### GET forecast results

#### Request Fields

> Request Example: Get forecast results for a specific metric 

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/metrics/{{forecast-metricId}}/results' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

This call gets all the forecast results for a specific metric regardless of the task.

Field | Type | Description / Example
-|-|-
metricID | String ($uuid) | Unique identifier of the forecast metric (you can get this from the [GET Task  Metrics](#get-task-metrics) call or from the [GET Metrics](#get-metrics) call).
forecastFunction (Optional) | enum | A function which enables aggregation of forecast metrics. Possible values: DAILY_SUM, MONTHLY_SUM, QUARTERLY_SUM, YEARLY_SUM. 

#### Response Fields

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

The response is an array of **forecast results**. A forecast result will be a set of predicted data points per metric. A data point has a value and a lower band and upper band of forecast, all at a given time stamp. See [GET Forecast Results](#get-forecast-results) for the details.

### GET forecast results history

#### Request Fields

> Request Example: Get forecast results for a specific metric 

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/forecast/metrics/{{forecast-metricId}}/history?for_date=1541618139' \
--header 'Authorization: Bearer {{data-token}}' \
--data-raw ''
```

This call gets all the forecast results for a specific metric regardless of the task.

Field | Type | Description / Example
-|-|-
metricID | String ($uuid) | Unique identifier of the forecast metric (you can get this from the [GET Task  Metrics](#get-task-metrics) call or from the [GET Metrics](#get-metrics) call).
for_date | timestamp (epoch) | The date for which to get the historical forecast results.

#### Response Fields

> Response Example:

```json
{
    "Actual": {
        "points": [
            {
                "timestamp": 1560384000,
                "value": 107.8399963379
            },
            {
                "timestamp": 1565827200,
                "value": 0
            },
            {
                "timestamp": 1565913600,
                "value": 0
            },
            {
                "timestamp": 1566086400,
                "value": 0
            },
            {
                "timestamp": 1566172800,
                "value": 98.3499984741
            },
            {
                "timestamp": 1566345600,
                "value": 0
            },
            {
                "timestamp": 1570752000,
                "value": 0
            },
            {
                "timestamp": 1571270400,
                "value": 0
            },
            {
                "timestamp": 1571443200,
                "value": 0
            },
            {
                "timestamp": 1572220800,
                "value": 0
            }
        ]
    },
    "Forecast": {
        "last_forecast_ts": "1634688000",
        "last_updated_ts": "1636378064",
        "metric_id": "3afa10d8-ba60-45aa-aef1-e5b9e5edd01b.0014P00002QZfjeQAD.VOLUME_SUM",
        "points": [
            {
                "lowerBand": 9870080,
                "timestamp": 1541635200,
                "upperBand": 9879956,
                "value": 9875018
            },
            {
                "lowerBand": 9960603,
                "timestamp": 1541721600,
                "upperBand": 9970569,
                "value": 9965586
            },
            {
                "lowerBand": 10056923,
                "timestamp": 1541808000,
                "upperBand": 10066985,
                "value": 10061954
            },
            {
                "lowerBand": 9589967,
                "timestamp": 1541894400,
                "upperBand": 9599561,
                "value": 9594764
            },
            {
                "lowerBand": 11604541,
                "timestamp": 1541980800,
                "upperBand": 11616151,
                "value": 11610346
            },
            {
                "lowerBand": 11557702,
                "timestamp": 1542067200,
                "upperBand": 11569266,
                "value": 11563484
            },
            {
                "lowerBand": 11614124,
                "timestamp": 1542153600,
                "upperBand": 11625744,
                "value": 11619934
            },
            {
                "lowerBand": 13145074,
                "timestamp": 1542240000,
                "upperBand": 13158226,
                "value": 13151650
            }
        ]
    }
}
```

The response is built of two arrays: 

* An array of Actual values - the actual values of the metric split into the relevant time stamps. 
* An array of **forecast results**. A forecast result will be a set of predicted data points per metric. A data point has a value and a lower band and upper band of forecast, all at a given time stamp. See [GET Forecast Results](#get-forecast-results) for the details.
