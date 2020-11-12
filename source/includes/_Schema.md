# Schema

## The Schema Object

> End Point

> https://api.anodot.com/api/v1/stream-schemas - Access Token Authentication

The Schema API enables you to create schemas as a preliminary step to sending Metrics to Anodot using Metrics 3.0 protocol.

* The schema object defines the required measures and their attributes:
  * Units
  * Aggregation
  * Counting method
* The schema object also defines the required dimensions and attributes:
  * Name
  * Fill policy

## Create Stream Schema

> Request Structure: `POST https://api.anodot.com/api/v1/stream-schemas`

> Example Request

```json

Content-Type: application/json
{
  "version": "1",
  "name": "schema name",
  "dimensions": [
    "OS",
    "GEO"
  ],
  "measurements": {
    "m1": {
      "units": "sec",
      "aggregation": "average",
      "countBy": "none"
    },
    "m2": {
      "units": "sec",
      "aggregation": "average",
      "countBy": "none"
    }
  },
  "missingDimPolicy": {
    "action": "fill",
    "fill": "dummy_val"
  }
}
```
Use this request to create a new schema

Argument | Type | Description
------|------|------------
version | String | (Optional) Describe the schema version
name | String | (Required) max 200 chars Schema name, will be later used as the stream name
Dimensions | Array | (Required) List of dimensions, max 30. Each dimension must be unique. Each dimension name is up to 30 chars. 
measurements | Array | (Required) List of measurements, max 200
For each measurement: | |
units | String | (Optional) Provide the unit for the measure, this unit will be used in Anodot display 
aggregation | String | (Required) Aggregation function for the measure. Valid values: average , sum
countby | String | (Required) How to count the data points. Valid value: currently “None” is the only value supported (each bucket counts as “1”)
missingDimPolicy |  | How to treat missing dimension values in the posted data.
action | String | Valid values:\n Fill - the empty dimension value will be filled with the value in the “fill” field (see below).\n Fail - data point will be rejected
fill | String | Fill value. Applicable if action field is set to fill

## Get Stream Schema

## Delete Stream Schema

## Get User Stream Schemas

