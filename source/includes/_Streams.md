## Data Sources & Data Streams

> End Point prefix is **/api/v2/bc**

The two steps to connect to an external data source and stream metrics to Anodot:

1. In Anodot - Create an Anodot data source according to your source type: S3, a database, a Kinesis Stream, Google Ads or any other source from the list of available resources.
2. In Anodot or through an API - Create one or more data streams to relay the needed data as metrics.

Additional information is available in our [Help center documentation](https://support.anodot.com/hc/en-us/articles/360001987453-Using-Data-Collectors)

Use the *data streams API* to:

* Get the list of Data sources in your account
* Get the available data streams in your account
* Create a new data stream
* Get data stream total metric count
* Get data stream connectied entities
* Pause / Resume a data stream
* Edit a data stream
* Delete a data stream

Authentication type: [Access Token Authentication] (#access-tokens).

###  Data Sources

> End Point prefix is **/api/v2/bc/data-sources**

There are several options to get the configured data sources:

* Get all data sources
* Get all data sources of a certain type
* Get all data sources according to names
* Get a data source according to its id

### GET data-sources

> Request Example: Get All Data Sources

```shell
curl -X GET \
"https://app.anodot.com/api/v2/bc/data-sources" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

> Request Example: Get S3 Data Sources

```shell
curl -X GET \
"https://app.anodot.com/api/v2/bc/data-sources?type=s3" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this call to get all data sources defined in your account.</br>
You can optionally get the data sources by type.

#### Request Arguments

Argument | Type | Description
---------|------|------------
query [**Optional**] | String | If left empty, the response returns all data sources.</br>To get data sources of a certain source type, use the relevant ENUM value in the query.

#### List of source types
ENUM | Description
-|-
google_storage| Google Cloud Storage 
google_analytics| Google Analytics
google_ads| Google Ads
bigquery| Google BigQuery DWH
adobe| Adobe Analytics
local_file| File Upload
s3| AWS S3
athena| AWS S3 Parquet data source powered with Athena
salesforce| Salesforce CRM
mysql| MySQL DB
psql| PostgreSQL DB
mssql| Microsoft SQL Server DB
databricks| Databricks
mariadb| Maria DB
redshift| AWS Redshift DWH
snowflake| Snowflake DWH
oracle| Oracle DB
mparticle| mParticle listener
kinesis| AWS Kinesis Stream

> Response Example:

```json
[
    {
        "id": "CLF6EQisl",
        "name": "demo.csv 1584355051733",
        "type": "local_file"
    },
    {
        "id": "CS3Jwrk4gf",
        "name": "Demo S3 1584005873272",
        "type": "s3"
    }
]
```

#### Response Fields

Field | Type | Description / Example
-|-|-
id | String | Data source id. This id can be used in future calls to stream creation.
name | String | Data Source name. Used for human readability.
type | String | The source type. Possible values are listed in source types table above.

### GET data-sources/find

Use this call to get a list of data sources according to their name.</br>
The string you include in the search query will be used for a case insensitive search in the data source names.</br>
The result will be a list of data sources matching the search.

> Request Example: Get Data Sources containing *S3* in their name

```shell
curl -X GET \
"https://app.anodot.com/api/v2/bc/data-sources/find?searchQuery=s3" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

#### Request Arguments

Argument | Type | Description
---------|------|------------
searchQuery | String | The query string is used to search the available the data source names

> Response Example: an S3 data source and its parameters

```json
[
   {
      "path" : "",
      "region" : "us-east-1",
      "modifyTime" : 1584285442,
      "id" : "CS3xxxBfrk4gf",
      "bucket" : "demo",
      "createTime" : 1584005874,
      "type" : "s3",
      "name" : "S3_anodot-bc Data Source 1584005873272"
   }
]
```

#### Response Fields

Response: List of data sources.</br>
There are mutual fields appearing for all source types.</br>
Each type has some fields relevant to that type.

Field | Type | Description / Example
-|-|-
id | string | The data source unique id
type | string [ENUM] | One of the data source types listed
name | string | Data source name as it appears in the Anodot App.
modifyTime | epoch | epoch time the data source was updated
createTime | epoch | epoch time the data source was created
*additional* | Various | Additional fields according to data source type.</br>e.g. authentication type, database name, database host, region, bucket name and more  

### GET data-sources/:id

> Request Example: Get Data Source by id

```shell
curl -X GET \
"https://app.anodot.com/api/v2/bc/data-sources/CPGLrAfdxxxFm" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this call to get a single data source according to its id.</br>
The result will be a data source object matching the id.</br>
In addition, all the data streams linked to the data source will be listed

#### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Data source id to retrieve.

> Response Example: A PostgresDB data source

```json
{
   "name" : "Postgres demo Data Source 1584551534093",
   "dbHost" : "demo.anodot.com",
   "type" : "psql",
   "modifyTime" : 1584551534,
   "id" : "CPGLrAfdxxxFm",
   "verifyServerCertificate" : false,
   "createTime" : 1584551534,
   "dbName" : "demo_api",
   "dbPort" : 5432,
   "useSSL" : true,
   "userName" : "demodbuser",
   "streams": [
        {
            "id": "SSS0uMc43vFkO",
            "name": "stream1",
            "type": "psql"
        },
        {
            "id": "SSShvwKn0mnMa",
            "name": "stream2",
            "type": "psql"
        }
   ]
}
```

#### Response Fields

Response: A data source object, including all fields according to its type.</br>

Field | Type | Description / Example
-|-|-
id | string | The data source unique id
type | string [ENUM] | One of the data source types
name | string | Data source name as it appears in the Anodot App.
modifyTime | epoch | epoch time the data source was updated
createTime | epoch | epoch time the data source was created
*additional* | Various | Additional fields according to data source type.</br>e.g. authentication type, database name, database host, region, bucket name and more
streams | Array | The list of streams linked to the data source

### Divert data-streams

> Request Example: Divert streamX and streamY from source A to source B

```shell
curl -X POST \
"https://app.anodot.com/api/v2/bc/data-sources/divertStreams?fromDataSrc={SourceId}&toDataSrc={DestinationId}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}" \
-d '{ ["streamX-Id", "streamY-Id"] }'
```

Use this request to divert data streams from a one data source to another.</br>
A data source may become invalid when user credentials were revoked and cannot be updated in the same datasource. This is the case when using "Sign in with google", "Sign in with Facebook". Other scenarios may apply too.
Note the two sources are the same data source type.

#### Request Arguments and body

Argument | Type | Description
---------|------|------------
fromDataSrc | String | Id of the datasource streams are taken from.
toDataSrc | String | Id of the datasource streams are linked to.
Body | Array | An array of stream Ids to be diverted between the sources

### Data Streams

> End Point prefix is **/api/v2/bc/data-streams**

* Get the available data streams in your account
* Create a new data stream
* Pause / Resume a data stream
* Edit a data stream
* Delete a data stream

### GET data-streams

> Request Example: Get all data streams basic information

```shell
curl -X GET \
"https://app.anodot.com/api/v2/bc/data-streams" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

> Response Example:

```json
[
    {
        "id": "SSSrlxxxxxxSC",
        "name": "postgres test",
        "type": "psql",
        "dataSourceId": "XXXYYYZZZAYYY"
    },
    {
        "id": "SSSyjxxxxxgfq",
        "name": "S3 Test",
        "type": "s3",
        "dataSourceId": "XXXYYYZZZBYYY"
    },
    {
        "id": "SSSxxxxzYUP0O",
        "name": "Google Ads Test",
        "type": "google_ads",
        "dataSourceId": "XXXYYYZZZCYYY"
    },
    {
        "id": "SSS4xxxxxxQqn",
        "name": "My Data Test",
        "type": "local_file",
        "dataSourceId": "XXXYYYZZZDYYY"
    },
    {
        "id": "SSSxxxxxxxCoR",
        "name": "S3 Parquet Test",
        "type": "athena",
        "dataSourceId": "XXXYYYZZZEYYY"
    }
]
```

Use this call to get all data streams in your account with basic information

#### Response Fields

The response includes a basic list of data streams, the fields for each data stream are:

Field | Type | Decsription
------|------|------------
id | string | Data stream unique ID
name | string | Data stream name.
type | string [*ENUM*] | Data stream type. See [source type list](#list-of-source-types)
dataSourceId | string | Unique identifier of this stream's data source

### GET data-streams/find 

> Request Example: Get Data Streams containing *test* in their name

```shell
curl -X GET \
"https://app.anodot.com/api/v2/bc/data-streams/find?searchQuery=test" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this call to get a list of data streams according to the search query.</br>
The string you include in the search query will be used for a case insensitive search in the data stream names.</br>
The result will be a list of data streams matching the search.

<aside class="notice">
The result is a list of full data stream objects.<br/>
Use these structures in the creation calls to create new streams with proper configuatrion.
</aside>

#### Request Arguments

Argument | Type | Description
---------|------|------------
searchQuery | String | The query string is used to search the available the data stream names

> Response Example: an S3 data stream and its parameters

```json
[
   {
      "timeDefinition" : {
         "timeColumnIdx" : 5,
         "timePattern" : "epoch_seconds"
      },
      "schema" : {
         "columns" : [
            {
               "targetType" : "gauge",
               "id" : "5655-cd1f2cff1c1d",
               "sourceColumn" : "0",
               "name" : "id",
               "hidden" : false,
               "type" : "metric"
            },
            {
               "type" : "metric",
               "hidden" : false,
               "sourceColumn" : "1",
               "name" : "number1",
               "targetType" : "gauge",
               "id" : "0838-fc15de8c75c8"
            },
            {
               "hidden" : false,
               "type" : "metric",
               "sourceColumn" : "2",
               "name" : "number2",
               "id" : "74e6-090be30f786f",
               "targetType" : "gauge"
            },
            {
               "hidden" : false,
               "type" : "metric",
               "id" : "4505-7814afd0f9fa",
               "targetType" : "gauge",
               "sourceColumn" : "3",
               "name" : "number3"
            },
            {
               "name" : "cloths",
               "sourceColumn" : "4",
               "id" : "9472-9a1b7e338ef6",
               "type" : "dimension",
               "hidden" : false
            }
         ],
         "sourceColumns" : [
            {
               "id" : "0",
               "index" : 0
            },
            {
               "index" : 1,
               "id" : "1"
            },
            {
               "index" : 2,
               "id" : "2"
            },
            {
               "index" : 3,
               "id" : "3"
            },
            {
               "index" : 4,
               "id" : "4"
            }
         ]
      },
      "modifyTime" : 1584355552,
      "paused" : false,
      "hasDimreduce" : false,
      "metrics" : [
         0,
         1,
         2,
         3
      ],
      "dataSourceId" : "CS3JwVBfxxxgf",
      "missingDimPolicy" : {
         "action" : "fill",
         "fill" : "unknown"
      },
      "fileNamePattern" : "yyyyMMddHH",
      "state" : "running",
      "path" : "folder6f537479-xxxx-43f2-9ce3-66b0294b7532",
      "maxMissingFiles" : 0,
      "type" : "s3",
      "createTime" : 1584355524,
      "fileNameSuffix" : "_data.csv",
      "fileNamePrefix" : "data3_",
      "name" : "test",
      "timeZone" : "UTC",
      "status" : "ok",
      "pollingInterval" : "daily",
      "fileFormat" : {
         "shouldReplaceEmptyCells" : true,
         "decimalPointChar" : ".",
         "hasHeader" : true,
         "delimiter" : ",",
         "ignoreEmptyLines" : false,
         "quoteChar" : "'"
      },
      "historicalDateRange" : {
         "constRange" : "m3"
      },
      "id" : "SSSq07uxxxyvI",
      "dimensions" : [
         4
      ]
   }
]
```

#### Response Fields

The response contains a list of complete data stream objects

### GET data-streams/:id

> Request Example: Get Data Stream by id

```shell
curl -X GET \
"https://app.anodot.com/api/v2/bc/data-streams/{id}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this call to get a single data stream according to itd id.</br>
The result will be a data stream object matching the id.

#### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Data stream id to retrieve.

> Response Example: A Google Ads data stream

```json
{
    "status" : "ok",
    "createTime" : 1584878085,
    "schema" : {
        "sourceColumns" : [
        {
            "name" : "Cost",
            "id" : "Cost"
        },
        {
            "id" : "Clicks",
            "name" : "Clicks"
        },
        {
            "id" : "Impressions",
            "name" : "Impressions"
        },
        {
            "name" : "Conversions",
            "id" : "Conversions"
        }
        ],
        "columns" : [
        {
            "id" : "32dd-8f1cbb6d3e48",
            "hidden" : false,
            "name" : "Cost",
            "metricTags" : {
                "unit" : "USD"
            },
            "type" : "metric",
            "targetType" : "counter",
            "transform" : {
                "parameters" : [
                    "0.000001"
                ],
                "name" : "scale",
                "input" : [
                    {
                    "sourceColumn" : "Cost"
                    }
                ]
            }
        },
        {
            "name" : "Clicks",
            "sourceColumn" : "Clicks",
            "hidden" : false,
            "id" : "79d8-fa375dd34f90",
            "targetType" : "counter",
            "type" : "metric"
        },
        {
            "sourceColumn" : "Impressions",
            "name" : "Impressions",
            "id" : "8214-d826d1746a93",
            "hidden" : false,
            "type" : "metric",
            "targetType" : "counter"
        },
        {
            "type" : "metric",
            "targetType" : "counter",
            "id" : "5489-090f6c31b882",
            "hidden" : false,
            "sourceColumn" : "Conversions",
            "name" : "Conversions"
        },
        {
            "name" : "Dummy Dimension",
            "hidden" : false,
            "id" : "dfcb-4aaf2c5188b1",
            "transform" : {
                "parameters" : [
                    "1"
                ],
                "name" : "dummy",
                "input" : []
            },
            "type" : "dimension"
        }
        ]
    },
    "timezone" : "America/Los_Angeles",
    "missingDimPolicy" : {
        "action" : "fill",
        "fill" : "unknown"
    },
    "metrics" : [
        "Clicks",
        "Conversions",
        "Cost",
        "Impressions"
    ],
    "pollingInterval" : "daily",
    "paused" : false,
    "clientCustomerId" : "7559118320",
    "type" : "google_ads",
    "state" : "running",
    "id" : "SSSjMxxxfXgdL",
    "hasDimreduce" : false,
    "pollingResolution" : "days",
    "delayMinutes" : 60,
    "modifyTime" : 1584879047,
    "historicalDateRange" : {
        "constRange" : "m1"
    },
    "reportType" : "ACCOUNT_PERFORMANCE_REPORT",
    "name" : "test",
    "dataSourceId" : "CAW5nPxxxxwt4",
    "dimensions" : [],
    "basedOnTemplateId" : "44"
}
```

#### Response Fields

The response contains the complete data stream object.

### POST data streams

> Request Example: Create an AWS Cost Monitoring data Stream

```shell
curl -X POST \
  "https://app.anodot.com/api/v2/bc/data-streams" \
  -H 'Authorization: Bearer ${TOKEN}' \
  -H 'Content-Type: application/json' \
  -d '{
  "id": "your_CUR_stream_id",
  "name": "CUR Stream",
  "dataSourceId": "your_data_source_id",
  "type": "aws_cur",
  "historicalDateRange": {
    "constRange": "m3"
  },
  "pollingInterval": "daily",
  "basedOnTemplateId": "na",
  "state": "new",
  "schema": {
    "columns": [
      {
        "sourceColumn": "lineItem/UsageAmount",
        "id": "lineItem/UsageAmount",
        "name": "UsageAmount",
        "type": "metric",
        "targetType": "counter",
        "hidden": false
      },
      {
        "sourceColumn": "lineItem/BlendedCost",
        "id": "lineItem/BlendedCost",
        "name": "BlendedCost",
        "type": "metric",
        "targetType": "counter",
        "hidden": false
      },
      {
        "sourceColumn": "lineItem/UnblendedCost",
        "id": "lineItem/UnblendedCost",
        "name": "UnblendedCost",
        "type": "metric",
        "targetType": "counter",
        "hidden": false
      },
      {
        "sourceColumn": "savingsPlan/SavingsPlanEffectiveCost",
        "id": "savingsPlan/SavingsPlanEffectiveCost",
        "name": "SavingsPlanEffectiveCost",
        "type": "metric",
        "targetType": "gauge",
        "hidden": false
      },
      {
        "sourceColumn": "savingsPlan/UsedCommitment",
        "id": "savingsPlan/UsedCommitment",
        "name": "savingsPlan_UsedCommitment",
        "type": "metric",
        "targetType": "counter",
        "hidden": false
      },
      {
        "sourceColumn": "product/region",
        "id": "product/region",
        "name": "region",
        "type": "dimension",
        "hidden": false
      },
      {
        "sourceColumn": "product/instanceType",
        "id": "product/instanceType",
        "name": "instanceType",
        "type": "dimension",
        "hidden": false
      },
      {
        "sourceColumn": "lineItem/ProductCode",
        "id": "lineItem/ProductCode",
        "name": "ProductCode",
        "type": "dimension",
        "hidden": false
      },
      {
        "sourceColumn": "lineItem/LineItemType",
        "id": "lineItem/LineItemType",
        "name": "LineItemType",
        "type": "dimension",
        "hidden": false
      },
      {
        "sourceColumn": "lineItem/UsageType",
        "id": "lineItem/UsageType",
        "name": "lineItem_UsageType",
        "type": "dimension",
        "hidden": false
      },
      {
        "sourceColumn": "lineItem/UsageAccountId",
        "id": "lineItem/UsageAccountId",
        "name": "UsageAccountId",
        "type": "dimension",
        "hidden": false
      },
      {
        "sourceColumn": "savingsPlan/OfferingType",
        "id": "savingsPlan/OfferingType",
        "name": "savingsPlan_OfferingType",
        "type": "dimension",
        "hidden": false
      }
    ],
    "sourceColumns": [
      {
        "id": "lineItem/UsageAmount",
        "name": "lineItem/UsageAmount"
      },
      {
        "id": "lineItem/UnblendedCost",
        "name": "lineItem/UnblendedCost"
      },
      {
        "id": "lineItem/BlendedCost",
        "name": "lineItem/BlendedCost"
      },
      {
        "id": "product/region",
        "name": "product/region"
      },
      {
        "id": "product/instanceType",
        "name": "product/instanceType"
      },
      {
        "id": "lineItem/ProductCode",
        "name": "lineItem/ProductCode"
      },
      {
        "id": "lineItem/LineItemType",
        "name": "lineItem/LineItemType"
      },
      {
        "id": "lineItem/UsageType",
        "name": "lineItem/UsageType"
      },
      {
        "id": "lineItem/UsageAccountId",
        "name": "lineItem/UsageAccountId"
      },
      {
        "id": "savingsPlan/SavingsPlanEffectiveCost",
        "name": "savingsPlan/SavingsPlanEffectiveCost"
      },
      {
        "id": "savingsPlan/UsedCommitment",
        "name": "savingsPlan/UsedCommitment"
      },
      {
        "id": "savingsPlan/OfferingType",
        "name": "savingsPlan/OfferingType"
      }
    ]
  },
  "paused": false,
  "missingDimPolicy": {
    "action": "ignore"
  },
  "dimensions": [
    "product/region",
    "product/instanceType",
    "lineItem/ProductCode",
    "lineItem/LineItemType",
  	"lineItem/UsageType",
    "lineItem/UsageAccountId",
    "savingsPlan/OfferingType"
  ],
  "metrics": [
    "lineItem/UnblendedCost",
    "lineItem/BlendedCost",
    "lineItem/UsageAmount",
    "savingsPlan/SavingsPlanEffectiveCost",
    "savingsPlan/UsedCommitment"
  ],
  "filters": [],
  "path": "cur_reports_path",
  "timeZone": "UTC"
}'
```

> Request Example: Create a PostgreSQL data Stream

```shell
curl -X POST \
  "https://app.anodot.com/api/v2/bc/data-streams" \
  -H 'Authorization: Bearer ${TOKEN}' \
  -H 'Content-Type: application/json' \
  -d '{
      "tableName" : "bc_stream",
      "dimensions" : [
         "status",
         "resolution"
      ],
      "timestampColumn" : "create_time",
      "historicalDateRange" : {
         "constRange" : "d3"
      },
      "maxBackFillIntervals" : 3,
      "dataSourceId" : "CPGLrAfd3YpFm",
      "timeZone" : "UTC",
      "metrics" : [
         "is_paused"
      ],
      "schema" : {
         "sourceColumns" : [
            {
               "name" : "is_paused",
               "id" : "is_paused"
            },
            {
               "name" : "status",
               "id" : "status"
            },
            {
               "id" : "resolution",
               "name" : "resolution"
            }
         ],
         "columns" : [
            {
               "name" : "is_paused",
               "targetType" : "counter",
               "type" : "metric",
               "hidden" : false,
               "id" : "b888-6cd7a274c69a",
               "sourceColumn" : "is_paused"
            },
            {
               "sourceColumn" : "status",
               "id" : "8e11-b51f18268970",
               "type" : "dimension",
               "hidden" : false,
               "name" : "status"
            },
            {
               "name" : "resolution",
               "id" : "3ec1-fa87d9bf32a4",
               "sourceColumn" : "resolution",
               "type" : "dimension",
               "hidden" : false
            }
         ]
      },
      "pollingInterval" : "hourly",
      "delayMinutes" : 60,
      "schemaName" : "public",
      "name" : "postgres test api 2",
      "customQuery" : false,
      "missingDimPolicy" : {
         "action" : "ignore",
         "fill" : "unknown"
      },
      "type" : "psql",
      "timestampType" : "TIMESTAMP"
   }
'
```

> Request Example: Create a Kinesis data Stream

```shell
curl -X POST \
  "https://app.anodot.com/api/v2/bc/data-streams" \
  -H 'Authorization: Bearer ${TOKEN}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "kinesis api test 1",
	"dataSourceId": "CKISt8xxxI4Vp",
	"type": "kinesis",
	"pollingInterval": "m5",
	"dimensions": ["serverId.id", "checksum.offset"],
	"metrics": ["load"],
	"schema": {
		"columns": [
			{
				"sourceColumn": "load",
				"id": "b8af-14c99d63d6c1",
				"name": "load",
				"type": "metric",
				"targetType": "gauge",
				"hidden": false
			},
			{
				"sourceColumn": "serverId.id",
				"id": "b8af-14c99d63d6c2",
				"name": "serverId.id",
				"type": "dimension",
				"hidden": false
			},
			{
				"sourceColumn": "checksum.offset",
				"id": "b8af-14c99d63d6c3",
				"name": "checksum.offset",
				"type": "dimension",
				"hidden": false
			}
		],
		"sourceColumns": [
			{"id": "load", "path": "load"},
			{"id": "serverId.id", "path": "serverId.id"},
			{"id": "checksum.offset", "path": "checksum.offset"}
		]
	},
	"recordType": "single_json",
	"filters": [{"path": "clazz", "value": "HeartbeatMessage"}],
	"delayMinutes": 5,
	"timeDefinition": {"path": "checksum.time", "timePattern": "epoch_millis"}
    }'
```

Use this call to create a data-stream.
The call needs to contain the entire data-stream object, including the link to parent data-source.

<aside class="notice">
Remember:</br>The parent data source should be defined within the Anodot App.<br/>
Use the GET data-sources call to retrieve its id and use the id as the datasourceId in the stream you create.
</aside>

#### Request Arguments

The request is a complete data stream object.</br>
See below a partial list of stream components.</br>

* **Context**: Where is the information taken from.
    * All types: Data Source Id, Data stream name, type
    * S3: Path in the bucket, file prefix and suffix, filename pattern
    * RDB : Schema name, Table name
* **Schedule**: Data stream timing properties
    * Collection interval
    * Timezone
    * Delay
    * History span to collect
    * Backfill policy
* **Schema**: The measures and dimensions collected by the stream
    * Measures: name, aggregation type, unit
    * Dimensions: name

<aside class="success">
A Pro Tip:</br>
To get the relevant data stream object structure, do a GET data-stream on an existing live stream of the same type in your account.</br>You will be able to see the complete object structure in the output.
</aside>

#### Schedule Arguments

Field | Description & Values
-|-
pollingInterval | The possible frequencies to collect data:</br> m1, m5, m10, m15, m30, hourly, h2, h3, h4, h6, h8, h12, daily</br> [m=minute, h=hour]
Historical Range | The history to collect:</br> h1, h4, d1, d3, w1, m1, m3, m6, y1</br>[h=last hour, w=last week, m=last month, y=last year]

**Polling interval restrictions per type**:

* **Google Analytics**: 
    * polling interval: hourly, daily
    * pollingResolution: hours, days
    * Allowed combinations hourly&hours OR daily&days
* **BigQuery**: hourly, daily
* **Salesforce**: hourly, daily
* **Adobe Analytics**: hourly, daily
* **Kinesis**: m1, m5, m10, m15, m30, hourly
* **SQL typed**: m1, m5, m15, hourly, h2, h3, h4, h6, h8, h12, daily
* **S3, GCS**: Also consider the fileDataPattern

**Historical time span restrictions per type**:

* **Kinesis**: no history

**Polling interval and Historical span Allowed combinations**:

Polling interval | Historical spans allowed
--|--
m1 | h1, h4, d1, d3, w1
m5, m10, m15, m30 | h1, h4, d1, d3, w1, m1
hourly, h2, h3, h4, h6, h8, h12 | d1, d3, w1, m1, m3, m6, y1
daily | d1, d3, w1, m1, m3, m6, y1, y2, y5, y10, y15, y20

#### Response Fields

The response contains the created data stream object.

### Get data stream total metric count

> Request Example:

```shell
curl -X GET \
"https://app.anodot.com/api/v2/bc/data-streams/metrics/total?streamId={id}&timeRange=day" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this call to get the number of metrics created by an active stream in a designated time range.

#### Request Arguments

Argument | Type | Description
---------|------|------------
streamId | String | Data stream id to retrieve. Alternatively, you can use the stream name.
streamName | String | Full data stream name to retrieve.
timeRange | String [ENUM] | Time range to collect the number of metrics.</br>Allowed values: "day", "week"

> Response Example:

```json
{
    "total" : 100
}
```

#### Response Fields

Field | Type | Description / Example
-|-|-
total | Int | Number of metric created by the data stream in the given time range.

### Get data stream connected entities

> Request Example:

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/bc/data-streams/totals?streamId={{stream-id}}' \
--header 'Authorization: Bearer {{bearer-token}}'
```

Use this call to get the number of entities which are connected to a stream. These entities can be alerts, dashboards or composites. 'Connected' means that when the alerts were defined, they have the reference to this stream (Same goes for dashboards and composites). This is equivalent to the 'Alerts & Dashboards' tab you can see in the stream summary modal inside the app.

#### Request Arguments

Argument | Type | Description
---------|------|------------
streamId | String | Data stream id to retrieve. 

> Response Example:

```json
{
    "alerts": {
        "alerts": [
            {
                "id": "5a1f1886-a92e-4bb5-978b-2781ef81c75d",
                "title": "Alert Views for {{customer_name}}"
            },
            {
                "id": "f2c84112-286c-436d-8316-6beb8186a6b6",
                "title": "Anomaly on events by {{type}}, {{category}}, {{customer_name}} from Alert Views"
            }
        ],
        "total": 2
    },
    "composites": {
        "composites": [],
        "total": 0
    },
    "dashboards": {
        "dashboards": [
            {
                "id": "610fb48151b949000f5a17a1",
                "title": "Alert Views"
            }
        ],
        "total": 1
    }
}
```

#### Response Fields

Field | Type | Description / Example
-|-|-
alerts | Array | An array of alerts. For each alert you will get the id and title. At the end of the array you can get the total number of connected alerts.
composites | Array | An array of composites. For each composite you will get the id and title. At the end of the array you can get the total number of connected composites.
dashboards | Array | An array of dashboards. For each dashboard you will get the id and title. At the end of the array you can get the total number of connected dashboards.

### Pause or Resume a data Stream

> Request Example:

```shell
curl -X PUT \
"https://app.anodot.com/api/v2/bc/data-streams/{id}/state/{action}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this call to pause a working data stream, or resume a paused stream.

#### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Id of the data stream you wish to pause or resume
action | String [ENUM] | The action to perform on the data stream.</br>Valid values: "pause", "resume"

> Response Example:

```json
{
   "name" : "test",
   "id" : "SSSGxxxDegnQEv",
   "paused" : true
}
```

#### Response Fields

Field | Type | Description / Example
-|-|-
name | String | Data stream name
id | String | Data stream id
paused | boolean | true - Data stream is paused, false - Data stream activity is resumed

### Edit data stream

> Request Example:

```shell
curl -X PUT \
"https://app.anodot.com/api/v2/bc/data-streams/{id}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}" \
-d "{stream json from GET request}"
```

To edit stream parameters:

1. Use a GET request to get the stream current settings.
2. Edit the required settings to the new settings (schedule, measures, dimensions) in the structure you've received 
3. Use a PUT request using the stream id and the updated settings as the request body.

<aside class="notice">
Anodot supports two modes of editing a live stream:</br>
A. Using the new settings from the update onwards.</br>
B. Recollecting the metrics using the new settings.</br>
The default option is A.</br>
</aside>

To force recollecting the data. Use:</br>
`https://app.anodot.com/api/v2/bc/data-streams/{id}?shouldRewind=true` 


### Delete data Stream

> Request Example:

```shell
curl -X DELETE \
"https://app.anodot.com/api/v2/bc/data-streams/{id}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this call to delete a stream based on its id.

#### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Id of the data stream you wish to delete
deleteMetrics [**optional**] | boolean | Delete the metrics created by the stream. Default = true.

> Response Example</br>If the operation succeeded - you get no response.

> Failure Response Example:

```json
{
   "path" : "/api/v2/bc/data-streams/SSSGYfxxxxob7",
   "message" : "object not found in underlying storage:  stream SSSGYfxxxxob7 was not found for user {account id}",
   "additionalInfo" : null,
   "name" : "HttpError",
   "andtErrorCode" : 11002,
   "status" : 404
}
```

#### Response

If the operation succeeded - you get no response.</br>
In case the operation failed, you will receive the failure reason
