# Metrics Operations

## Snooze Learning

When should you use the *Snooze the learning API* ?

On occassions, metrics become very noisy, or out of their regular bounds. This may create several effects:

* The number of alerts sent on account of such metrics might increase, resulting in alert overflow, and possibly hiding alerts occuring on other metrics.
* The baseline learnt by Anodot may be convoluted due to the different values the metric gets.

To handle these scenarios, the API allows you to:

* Snooze the learning of noisy metrics, thus prevening the side effects.
* Resume the learning when the metrics resume close to normal values.

> Request Example: Stopping the learning on two metrics.

```shell
curl --location --request POST 'https://app.anodot.com/api/v1/metrics/stl/add?token={{data-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userId": "5e3bc704bd8089000d1fbee2",
  "timeScale": "5m",
  "duration": 3600,
  "metricIds": "[m1,m2]"
}'
```

Use this request to snooze the learning of the metrics to a designated time.

### Request Arguments

Argument | Type | Description
---------|------|------------
userId | string | The user requesting to snooze the learning
timescale | Enum |  The timescale of the metrics. e.g. 5m
duration | number | The requested snooze duration in seconds.
metricIds | Array of strings | The array of metric ids we wish to snooze. 


## Resume Learning

> Request Example: Resuming the learning on two metrics.

```shell
curl --location --request POST 'https://app.anodot.com/api/v1/metrics/stl/remove?token=58e5f6b3ba01b264935d99f8' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userId": "58e5f6b3ba01b264935d99f9",
  "timeScale": "5m",
  "duration": 3600,
  "metricIds": "[m1,m2]"
}'
```

Use this request to resume the learning of the metrics you have snoozed earlier.

### Request Arguments

Argument | Type | Description
---------|------|------------
userId | string | The user requesting to resume the learning
timescale | Enum | The timescale of the metrics. e.g. 5m
duration | number | The requested resume duration in seconds.
metricIds | Array of strings | The array of metric ids we wish to resume. 

## Get cardinality

> End Point **GET /api/v2/metrics/cardinality**


Use this API to find metrics cardinality per dimension. The query returns dimensions sorted by their values caridnality with metrics examples.

> Request Example: Get the cardinality of all metrics in a given time frame

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/metrics/cardinality?startTime=1614771550&endTime=1614785550' \
--header 'Authorization: Bearer {bearer-token}'
```

### Request 

Name | Description
-----|------------
startTime | (Mandatory) featch metrics from startTime (Unix time in seconds). Maximum 90 days back. 
endTime | (Mandatory) featch metrics up to endTime (Unix time in seconds)
size | size of dimensions to return
dims | A comma-seperated list of dimensions to return in result.
stream | Name of stream to return results for
from | starting index to fetch data from (0 based integer).

### Response 

> Response Example: 

```json
{
    "totalMetrics": 233,
    "startIndex": 0,
    "nextIndex": 25,
    "metricsCardinality": {
        "user_name": {
            "cardinality": 72,
            "examples": [
                "what=active_streams.source_type=GOOGLE_ADS.user_name=Kenshoo_prod",
                "what=active_streams.source_type=GOOGLE_ANALYTICS.user_name=BC_test.#source=athena",
                "what=active_streams.source_type=MYSQL.user_name=Meliuz",
                "what=active_streams.source_type=KINESIS.user_name=Nordstrom-Nonprod",
                "what=active_streams.source_type=S3.user_name=browsi",
                "what=active_streams.source_type=S3.user_name=PayU",
                "what=active_streams.source_type=GOOGLE_ANALYTICS.user_name=Magyar_Telekom",
                "what=active_streams.source_type=REDSHIFT.user_name=Adevinta",
                "what=active_streams.source_type=SNOWFLAKE.user_name=Ridewithvia",
                "what=active_streams.source_type=BIGQUERY.user_name=BeenVerified"
            ],
            "what": [
                "active_streams"
            ]
        },
        "Page": {
            "cardinality": 27,
            "examples": [
                "what=Unique_Pageviews.Page=/hc/en-us/articles/360010785800-Databricks.Page_Title=Databricks_–_Anodot",
                "what=Pageviews.Page=/hc/en-us/search?utf8=✓.Page_Title=Search_results_–_Anodot",
                "what=Pageviews.Page=/hc/en-us/articles/207878935-Anomalies-Overview.Page_Title=Anomalies_Overview_–_Anodot",
                "what=Pageviews.Page=/hc/en-us/articles/115001227989-No-Data-Alert-Webhook-Formats.Page_Title=No_Data_Alert_Webhook_Formats_–_Anodot",
                "what=Unique_Pageviews.Page=/hc/en-us/articles/115002813254-Group.Page_Title=Group_–_Anodot",
                "what=Unique_Pageviews.Page=/hc/en-us/articles/360023727754-Anodot-Agent-Overview-.Page_Title=Anodot_Agent_Overview_–_Anodot",
                "what=Pageviews.Page=/hc/en-us/articles/207214979-Delta.Page_Title=Delta_–_Anodot",
                "what=Entrances.Page=/hc/en-us/articles/207498789-Using-Statsd.Page_Title=Using_Statsd_–_Anodot",
                "what=Entrances.Page=/hc/en-us/articles/360001267433-Managing-Events-Categories.Page_Title=Managing_Events_Categories_–_Anodot",
                "what=Pageviews.Page=/hc/en-us/articles/209776765-Events-Overview.Page_Title=Events_Overview_–_Anodot"
            ],
            "what": [
                "Unique_Pageviews",
                "Entrances",
                "Pageviews"
            ]
        }
    }
}
```

If successfull the API returns an array of cardinality objects as described below in additional to high level metadata. 

Paramater | Description
----------|------------
totalMetrics | Number of metrics which meet the criteria
startIndex | initial index of this call
nextIndex | index of next batch of metrics (useful if you are using pagination)
cardinality | The actual cardinality of the metric (Number)
examples | An array containing some examples of the metric
what | The measure to which this metric belongs to. 

## Get cardinality Per Stream


Use this API to find metrics cardinality per stream. 
otice that this API still uses the V1 authentication (e.g. Data token on the URL) and and not the bearer-authentication. 

> Request Example: Get the cardinality of all metrics in a given time frame

```shell
curl --location --request GET 'https://app.anodot.com/api/v1/metrics/cardinality/streams?sort=name&token={{data-token}}'
```

### Request 

Name | Description
-----|------------
token | (Mandatory) Data token of the account
sort | (Mandatory) How to sort the results. Available values : name, size

### Response 

> Response Example: 

```json
{
    "streams": [
        {
            "name": "Alert_origin_4",
            "numMetrics": 66
        },
        {
            "name": "All permanently stopped streams [HOURLY]",
            "numMetrics": 1
        },
        {
            "name": "App Usage - Hourly",
            "numMetrics": 88
        },
        {
            "name": "BC Streams Stopped Aggregation",
            "numMetrics": 1
        }
    ]
}
```

If successfull the API returns an array of stream names and the number of metrics for each one of them. 


## Get EPS (Events Per Second)

> Request Example: Get the EPS of the top metrics in the account

```shell
curl --location --request GET 'https://app.anodot.com/api/v1/metrics/eps?token={{data-token}}&rollup=medium' 
```

Use this API in order to get the EPS (Events per second) of the top metrics in the account. The main use case for this is to see which metrics cause a load (if at all) at a given time. 
Notice that this API still uses the V1 authentication (e.g. Data token on the URL) and and not the bearer-authentication. 

### Request 

Name | Description
-----|------------
token | (Mandatory) Data token of the account
rollup | (Mandatory) Requested time scale for the EPS. Available values : short, medium, long, longlong, weekly

### Response 

> Response Example: Array of metric names and their EPS. 

```json
[
    {
        "metric": "bctele.0.SSS3702beLUpo.RUNNING.data-lag",
        "eps": 0.0588235
    },
    {
        "metric": "bctele.0.SSS3702beLUpo.RUNNING.samples-created",
        "eps": 0.0588235
    },
    {
        "metric": "bctele.0.SSS3702beLUpo.RUNNING.samples-invalid",
        "eps": 0.0588235
    },
    {
        "metric": "bctele.0.SSS3702beLUpo.RUNNING.rows-fetched",
        "eps": 0.0588235
    },
    {
        "metric": "bctele.0.SSS3702beLUpo.RUNNING.samples-submit",
        "eps": 0.0588235
    },
    {
        "metric": "bc.SSSvPErF5rIQT.bit.val",
        "eps": 0.0208333
    },
    {
        "metric": "bc.SSSAD7GDMwN9X.1.heartbeat",
        "eps": 0.0208333
    },
    {
        "metric": "bctele.0.SSSAD7GDMwN9X.RUNNING.data-lag",
        "eps": 0.0200803
    },
    {
        "metric": "bctele.0.SSSAD7GDMwN9X.RUNNING.rows-fetched",
        "eps": 0.0200803
    },
    {
        "metric": "bctele.0.SSSvPErF5rIQT.RUNNING.samples-invalid",
        "eps": 0.0200803
    },
    {
        "metric": "bctele.0.SSSAD7GDMwN9X.RUNNING.samples-invalid",
        "eps": 0.0200803
    }
]
```

If successfull the API returns an array of cardinality objects as described below in additional to high level metadata. 

Paramater | Description
----------|------------
metric | Metric name
eps | Events per second for the metric.
