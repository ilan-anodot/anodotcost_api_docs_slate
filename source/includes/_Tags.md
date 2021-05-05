# Tags

A tag is a property/value pair that can be dynamically added/removed to a metric or set of metrics without effecting the name of the metric.<br/>
For example: a tag can be accountManager=Joe which will be attached to the metrics related to the accounts Joe manages. If the accounts Joe manages change, the API lets you change the assignment of the tags to metrics without affecting the metric name.
Tags can be used to filter and aggregate data.

**Please note:**

1. If a tag already exists then the tag will be removed from metrics that previously matched the search expression. The tag will be added only to the metrics that match the current search expression.
2. Tags MUST NOT contain the following characters:{ . SPACE}
3. It can take up to 20 minutes till the metric tagging operation will be applied to the relevant metrics.


> End Point prefix is **/v1/metrics/tags**

## Create Tags

To create a tag on a set of metrics that match a search expression.
Multiple expressions can be associated to each tag which is treated an OR expression between them.

> Request Structure 

```shell
curl -X POST \
https://app.anodot.com/api/v1/tags?token=<api token> \
-H 'Content-Type: application/json' \
-D '{
"tag": {
    "key": "tagName",
  "value": "tagValue"
  },
  "expressions": [
    {
      "expression": [
        {
          "type": "property",
          "key": "key1",
          "value": "val1"
      },
      {
          "type": "property",
          "key": "key2",
          "value": "val2"
      }
     ]
    },
    {
      "expression": [
        {
          "type": "property",
          "key": "key1",
          "value": "val3"
        },
        {
          "type": "property",
          "key": "key2",
          "value": "val4"
        }
      ]
    }
  ]
    }'
```
### Arguments
Argument | Definition
---------|-----------
tag key | tag key name
tag value | tag key value
expressions |  Array of expressions, each expression can contain several metrics queries.
type |  the type of the search expression, can be of type [*search*] or [*property*] (recommended).
key |  the metric key name
value |  the metric key value 

In the example on the right all metrics returning from the query:
`((key1 == val1) AND (key2 == val2)) OR ((key1 ==val3) AND (key2 ==val4))`  will be tagged with the tag: `tagName:tagValue`

> Example Request

```shell
curl -X POST \
https://api.anodot.com/api/v1/metrics/tags?token=<api token> \
-H "Content-Type: application/json" \
-d '{
   "tag":
      {
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
   "expressions":
   [
      {
         "expression":
            [
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ]
   }'
```

> Example Response

```json
{
   "validation":{
      "passed":true,
      "failures":[

      ]
   },
   "tags":[
      {
         "id":"8b6f6fa8-ba46-4596-8358-ebcb4727f5ee",
         "expressions":[
            {
               "expression":[
                  {
                     "type":"property",
                     "key":"what",
                     "value":"impressions"
                  },
                  {
                     "type":"property",
                     "key":"campaign_id",
                     "value":"12345678ABC"
                  }
               ]
            }
         ],
         "tag":{
            "key":"my_campaigns",
            "value":"12345678ABC"
         }
      }
   ]
}
```

## Get Tag by ID

Returns a tag's configuration based on the tag's ID.

> Sample Request: Get Tag by ID 

```shell
curl -X GET \
https://api.anodot.com/api/v1/metrics/tags?<Id>&token=<api token> \
-H "Content-Type: application/json" 
```

### Arguments
Argument | Definition
-------- | ----------
ID | The ID of the category

> Example Response

```json
[
   {
      "id":"31425ce5-32c1-4cce-9075-28c1748d42a8",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470056853,
      "disabled":false
   },
   {
      "id":"8b6f6fa8-ba46-4596-8358-ebcb4727f5ee",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058877,
      "disabled":false
   },
   {
      "id":"a56259ce-38df-4f0a-87a9-157198c82275",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058253,
      "disabled":false
   },
   {
      "id":"f9c4bec4-632d-4366-b78d-65323fbc4e84",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058449,
      "disabled":false
   }
]
```

## Get Tag by Search Expression

Returns a tag's configuration based on a search expression.

> Sample Request: Get Tag by Search Expression 

```shell
curl -X GET \
"https://app.anodot.com/api/v1/metrics/tags?token=<api token> \
&value-contains-string={{string}}&key-contains-string={{string}}" \
-H "Content-Type: application/json" 
```

### Arguments
Argument | Description
-------- | ----------
value-contains-string (optional)  | The string to search within the value
key-contains-string (optional) | The string to search within the key

**Note**: The relationship is AND between the two parameters. 

> Example Response

```json
[
   {
      "id":"31425ce5-32c1-4cce-9075-28c1748d42a8",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470056853,
      "disabled":false
   },
   {
      "id":"8b6f6fa8-ba46-4596-8358-ebcb4727f5ee",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058877,
      "disabled":false
   },
   {
      "id":"a56259ce-38df-4f0a-87a9-157198c82275",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058253,
      "disabled":false
   },
   {
      "id":"f9c4bec4-632d-4366-b78d-65323fbc4e84",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058449,
      "disabled":false
   }
]
```

## List All tags

List all tag configurations in the account sent via the tag APIs.

> Sample Request: List all tags

```shell
curl -X GET \
https://api.anodot.com/api/v1/metrics/tags?token=<api token> \
-H "Content-Type: application/json" 
```

> Example Responses

```json
[
   {
      "id":"31425ce5-32c1-4cce-9075-28c1748d42a8",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470056853,
      "disabled":false
   },
   {
      "id":"8b6f6fa8-ba46-4596-8358-ebcb4727f5ee",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058877,
      "disabled":false
   },
   {
      "id":"a56259ce-38df-4f0a-87a9-157198c82275",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058253,
      "disabled":false
   },
   {
      "id":"f9c4bec4-632d-4366-b78d-65323fbc4e84",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058449,
      "disabled":false
   }
]
```


## Delete Tags

Delete a tag and remove it from all metrics.

### Arguments
Argument | Definition
-------- | ----------
ID | The tag ID

> Example Request: Delete all tags

```shell
curl \
-X DELETE \
-H "Content-Type: application/json" \
'https://api.anodot.com/api/v1/metrics/tags/31425ce5-32c1-4cce-9075-28c1748d42a8?token=<api token>'
```
### Response
No response body for this API

## Assign Metrics to Tags

> Example Request: Assign Metrics to Tags

```shell
curl \
-X POST \
-H "Content-Type: application/json" \
'https://api.anodot.com/api/v1/metrics/tags/assign-metrics-to-tags?token=<api token>' \
-D '{
  "expressions": [
    {
      "expression": [
        {
          "type": "property",
          "key": "key1",
          "value": "val1"
        },
        {
          "type": "property",
          "key": "key2",
          "value": "val2"
        }
      ]
    }
  ],
  "tags": [
    {
      "key": "tagkey1",
      "value": "tagval1"
    },
    {
      "key": "tagkey1",
      "value": "tagval2"
    }
  ]
}'
```

Assign one or more metrics to one or more tags.

The user will send an array of expressions that define the metrics he wants to assign the tags to.

**Note:** It can take up to 20 minutes until the metric tagging operation will be applied to the relevant metrics.

**Important**
If a tag already exists then the tag will be removed from the metrics that previously matched the search expression; a tag will be added only to the metrics that match the current search expression.


### Arguments
Argument | Definition
---------|-----------
tag key | tag key name
tag value | tag key value
expressions |  Array of expressions, each expression can contain several metrics queries.
type |  the type of the search expression, can be of type [*search*] or [*property*] (recommended).
key |  the metric key name
value |  the metric key value 

> Sample Response

```json
[
    {
        "id": "7ecc44e1eab4c1a649776ebf4c6fc49b",
        "expressions": [
            {
                "expression": [
                    {
                        "type": "property",
                        "key": "pos1",
                        "value": "raanana"
                    },
                    {
                        "type": "property",
                        "key": "pos2",
                        "value": "shared"
                    }
                ]
            },
            {
                "expression": [
                    {
                        "type": "property",
                        "key": "pos2",
                        "value": "coconut"
                    },
                    {
                        "type": "property",
                        "key": "pos1",
                        "value": "netanya"
                    }
                ]
            }
        ],
        "tag": {
            "key": "service",
            "value": "a",
            "metricTagRepresentation": "#service=a"
        }
    },
    {
        "id": "548e8537d14613bf94b458afa12e921a",
        "expressions": [
            {
                "expression": [
                    {
                        "type": "property",
                        "key": "pos1",
                        "value": "raanana"
                    },
                    {
                        "type": "property",
                        "key": "pos2",
                        "value": "shared"
                    }
                ]
            }
        ],
        "tag": {
            "key": "service",
            "value": "b",
            "metricTagRepresentation": "#service=b"
        }
    }
]
```

## Add Metrics to Tags

> Example Request: Add Metrics to Tags

```shell
curl \
-X PUT \
-H "Content-Type: application/json" \
'https://api.anodot.com/api/v1/metrics/tags/add-metrics-to-tags?token=<api token>' \
-D '{
  "expressions": [
    {
      "expression": [
        {
          "type": "property",
          "key": "key1",
          "value": "val1"
        },
        {
          "type": "property",
          "key": "key2",
          "value": "val2"
        }
      ]
    }
  ],
  "tags": [
    {
      "key": "tagkey1",
      "value": "tagval1"
    },
    {
      "key": "tagkey1",
      "value": "tagval2"
    }
  ]
}'
```

This method works similarly to Assign Metrics to Tags. Instead of removing metrics that were previously assigned to the requested tags, it adds the new metrics to the previous ones.

If a tag does not exist then it will create a new tag and assign the metrics to it.

**Note:** It can take up to 20 minutes until the metric tagging operation will be applied to the relevant metrics.

### Arguments
Argument | Definition
---------|-----------
tag key | tag key name
tag value | tag key value
expressions |  Array of expressions, each expression can contain several metrics queries.
type |  the type of the search expression, can be of type [*search*] or [*property*] (recommended).
key |  the metric key name
value |  the metric key value 

> Sample Response

```json
[
    {
        "id": "7ecc44e1eab4c1a649776ebf4c6fc49b",
        "expressions": [
            {
                "expression": [
                    {
                        "type": "property",
                        "key": "pos1",
                        "value": "raanana"
                    },
                    {
                        "type": "property",
                        "key": "pos2",
                        "value": "shared"
                    }
                ]
            },
            {
                "expression": [
                    {
                        "type": "property",
                        "key": "pos2",
                        "value": "coconut"
                    },
                    {
                        "type": "property",
                        "key": "pos1",
                        "value": "netanya"
                    }
                ]
            }
        ],
        "tag": {
            "key": "service",
            "value": "a",
            "metricTagRepresentation": "#service=a"
        }
    },
    {
        "id": "548e8537d14613bf94b458afa12e921a",
        "expressions": [
            {
                "expression": [
                    {
                        "type": "property",
                        "key": "pos1",
                        "value": "raanana"
                    },
                    {
                        "type": "property",
                        "key": "pos2",
                        "value": "shared"
                    }
                ]
            }
        ],
        "tag": {
            "key": "service",
            "value": "b",
            "metricTagRepresentation": "#service=b"
        }
    }
]
```