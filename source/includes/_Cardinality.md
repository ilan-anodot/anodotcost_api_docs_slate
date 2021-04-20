# Metrics Operations

> End Point **GET /api/v2/metrics/cardinality**

Use this API to find metrics cardinality per dimension. The query returns dimensions sorted by their values caridnality with metrics examples.

## Get cardinality

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
                "what=active_streams.source_type=GOOGLE_ADS.user_name=BC_test",
                "what=active_streams.source_type=GOOGLE_ANALYTICS.user_name=BC_test.#source=athena",
                "what=active_streams.source_type=MYSQL.user_name=BC_test",
                "what=active_streams.source_type=KINESIS.user_name=BC_test",
                "what=active_streams.source_type=S3.user_name=BC_test",
                "what=active_streams.source_type=S3.user_name=BC_test",
                "what=active_streams.source_type=GOOGLE_ANALYTICS.user_name=BC_test",
                "what=active_streams.source_type=REDSHIFT.user_name=BC_test",
                "what=active_streams.source_type=SNOWFLAKE.user_name=BC_test",
                "what=active_streams.source_type=BIGQUERY.user_name=BC_test"
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
                "what=active_streams.source_type=GOOGLE_ADS.user_name=BC_test",
                "what=active_streams.source_type=GOOGLE_ANALYTICS.user_name=BC_test.#source=athena",
                "what=active_streams.source_type=MYSQL.user_name=BC_test",
                "what=active_streams.source_type=KINESIS.user_name=BC_test",
                "what=active_streams.source_type=S3.user_name=BC_test",
                "what=active_streams.source_type=S3.user_name=BC_test",
                "what=active_streams.source_type=GOOGLE_ANALYTICS.user_name=BC_test",
                "what=active_streams.source_type=REDSHIFT.user_name=BC_test",
                "what=active_streams.source_type=SNOWFLAKE.user_name=BC_test",
                "what=active_streams.source_type=BIGQUERY.user_name=BC_test"
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
