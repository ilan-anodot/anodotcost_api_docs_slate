# User Events

## The event object

> End Points

> https://api.anodot.com/api/v1/user-events - Basic Authentication

> https://api.anodot.com/api/v2/user-events - Access Token Authentication

The Event API enables you to create and edit periodic events such as deployments and holidays. Events can assist in the anomaly investigation process, by correlating them to alerts/anomalies/metrics in dashboards and alerts.

## Create Event

> Request Structure: POST https://api.anodot.com/api/v1/user-events

```json
Content-type=application/json

{
"event": {
    "title": "<event title>",
    "description": "<event description>",
    "category": "<category name>",
    "source": "<source name>",
    "properties":[
     {
        "key":"<key1>",
        "value": "<value1>"
      },
      {
        "key": "<key2>",
        "value": "<value2>"
      }
    ],
    "startDate": "<epoch start date>",
    "endDate": "<epoch end date>"
  }
} 
```

> Example Request

```json
{
   "id":"b1229900-1a49-4b9e-a2e4-b9d21240793f",
   "title":"deployment started on myServer",
   "description":"my description",
   "source":"chef",
   "category":"deployments",
   "startDate":1468326864,
   "endDate":null,
   "properties":[
      {
         "key":"service",
         "value":"myService"
      }
   ]
}
```

```shell
curl \
-X POST \
-d '{"event":{"title":"deployment started on myServer","description":"my description","category":"deployments","source":"chef","properties":[{"key":"service","value":"myService"}],"startDate":"1468326864"}}' \
-H "Content-Type:application/json" \
'https://api.anodot.com/api/v1/user-events?token=<API TOKEN>'
```

```python
create_event.py
#!/usr/bin/env python
# -*- coding: utf-8 -*-

###########################
#######   LICENSE   #######
###########################

# Copyright (c) 2014-2016, Anodot Ltd. <info@anodot.com>
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without modification, are permitted provided that the
# following conditions are met:
#
# 1. Redistributions of source code must retain the above copyright notice, this list of conditions and the
#    following disclaimer.
#
# 2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the
#    following disclaimer in the documentation and/or other materials provided with the distribution.
#
# 3. Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote
#    products derived from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES,
# INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED.
# IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
# EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
# WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

import json
import logging
import argparse
import csv
import datetime
import time
from calendar import timegm
import requests
requests.packages.urllib3.disable_warnings()

#########################################################
# Function name: print_license                          #
# Params: None                                          #
# Returns: Anodot's license                             #
#########################################################
def print_license():
    text = '''
Copyright (c) 2014-2016, Anodot Ltd. <info@anodot.com>
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the
following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this list of conditions and the
   following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the
   following disclaimer in the documentation and/or other materials provided with the distribution.

3. Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote
   products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES,
INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED.
IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
    '''

    print(text)

#########################################################
# Function name: msg                                    #
# Params: None                                          #
# Returns: Returns the help message                     #
#########################################################
def msg(name=None):
    return '''create_event.py
         Create an event.

         - Print Anodot's license: python create_event.py --license
         - Create an event: python create_event.py --filename <The events CSV file> --token <the API's token> --timezone_shift <the timezone shift. Default is 0 (UTC). Value should be in seconds with the  +/- sign>
           Example: python create_event.py --filename myfile.csv --token 54673544263 --timezone_shift -5000
           Example: python create_event.py --filename myfile.csv --token 54673544263 --timezone_shift +3000
        '''


def main():
    logging.basicConfig(filename='create_event.log',level=logging.INFO, format='%(asctime)s - %(levelname)s: %(message)s', datefmt='%m/%d/%Y %H:%M:%S')
    logger = logging.getLogger(__name__)
    logger.info('Starting a run')

    parser = argparse.ArgumentParser(description='', usage=msg())
    parser.add_argument('--token', required=False, default=None, help="The Anodot's API token")
    parser.add_argument('--filename', required=False, default=None, help="The events CSV file")
    parser.add_argument('--timezone_shift', required=False, default="+0", help="Start time and end time timezone shift in seconds. Must be in the format +6000 or -6000. Default is UTC")
    parser.add_argument('--license', required=False, action='store_true', help="Print Anodot's license")

    args = parser.parse_args()

    if(args.license):
        logger.info('user asked to print the license')
        print_license()
        exit()

    if(args.token is None):
        logger.error('--token is mandatory when creating a new event')
        raise Exception ("--token is mandatory when creating a new event")
    elif(args.filename is None):
        logger.error('--filename is mandatory when creating a new event')
        raise Exception ("--filename is mandatory when creating a new event")
    elif(args.timezone_shift == "" or args.timezone_shift is None):
        logger.error('--timezone_shift is mandatory. timezone shift is in seconds. Must be in the format +6000 or -6000. Default is UTC')
        raise Exception ("--timezone_shift is mandatory. timezone shift is in seconds. Must be in the format +6000 or -6000. Default is UTC ")
    elif(str(args.timezone_shift)[0] != "+" and str(args.timezone_shift)[0] != "-"):
        logger.error('--timezone_shift is mandatory. timezone shift is in seconds. Must be in the format +6000 or -6000. Default is UTC')
        raise Exception ("--timezone_shift is mandatory. timezone shift is in seconds. Must be in the format +6000 or -6000. Default is UTC ")

    timezone_sign = str(args.timezone_shift)[0]
    timezone_shift = str(args.timezone_shift)[1:]
    row_counter = 0
    with open(args.filename, "rb") as source_file:
        reader = csv.reader(source_file, delimiter=",")
        reader.next()
        row_counter += 1
        for row in reader:
            title =  row[0]
            description = row[1]
            category =  row[2]
            source = row[3]
            start_time =  row[4]
            end_time = row[5]
            start_utc_time = None
            start_epoch_time = None
            end_utc_time = None
            end_epoch_time = None

            row_counter += 1
            if(title == "" or title is None):
                logger.error("Row " + str(row_counter) + ": title is missing")
                raise Exception ("Row " + str(row_counter) + ": title is missing")
            if(description == "" or title is None):
                description = "missing"
            if(category == "" or category is None):
                logger.error("Row " + str(row_counter) + ": category is missing")
                raise Exception ("Row " + str(row_counter) + ": category is missing")
            if(source == "" or source is None):
                logger.error("Row " + str(row_counter) + ": source is missing")
                raise Exception ("Row " + str(row_counter) + ": source is missing")
            if(start_time == "" or start_time is None):
                logger.error("Row " + str(row_counter) + ": start_time is missing")
                raise Exception ("Row " + str(row_counter) + ": start_time is missing")
            else:
                try:
                    start_utc_time = time.strptime(start_time, "%Y/%m/%d %H:%M")
                    start_epoch_time = timegm(start_utc_time)
                    if(timezone_sign == "+"):
                        start_epoch_time = int(start_epoch_time) + int(timezone_shift)
                    elif(timezone_sign == "-"):
                        start_epoch_time = int(start_epoch_time) - int(timezone_shift)
                except ValueError:
                    logger.error("Row " + str(row_counter) + ": start_time needs to be in the format: YYYY/MM/DD HH:MM")
                    raise Exception("Row " + str(row_counter) + ": start_time needs to be in the format: YYYY/MM/DD HH:MM")
            if(end_time != "" and end_time is not None):
                try:
                    end_utc_time = time.strptime(end_time, "%Y/%m/%d %H:%M")
                    end_epoch_time = timegm(end_utc_time)
                    if(timezone_sign == "+"):
                        end_epoch_time = end_epoch_time + int(timezone_shift)
                    elif(timezone_sign == "-"):
                        end_epoch_time = end_epoch_time - int(timezone_shift)

                except ValueError:
                    logger.error("Row " + str(row_counter) + ": end_time needs to be in the format: YYYY/MM/DD HH:MM")
                    raise Exception("Row " + str(row_counter) + ": end_time needs to be in the format: YYYY/MM/DD HH:MM")
            else:
                end_time = "missing"



            properties_arr = row[6:]
            properties = []
            for property in properties_arr:
                if (property is not None and property != ""):
                    if("=" in property):
                        tmp_property = property.split("=")
                        properties.append({"key": tmp_property[0], "value":tmp_property[1]})
                    else:
                        logger.error("Row " + str(row_counter) + ": property " + str(property) + " is not in the format of 'key=value'")
                        raise Exception("Row " + str(row_counter) + ": property " + str(property) + " is not in the format of 'key=value'")

            body = {"event": {"title": title, "category": category, "source": source, "startDate": str(start_epoch_time)}}
            if(description != "missing"):
                body["event"].update({"description": description})
            if(end_time != "missing"):
                body["event"].update({"endDate": end_epoch_time})
            if(properties != []):
                body["event"].update({"properties": properties})


            logger.info('Sending http request to create an event: ' +  json.dumps(body))
            response = requests.post('https://api.anodot.com/api/v1/user-events?token=' + args.token, headers={'Content-Type': 'application/json'}, data=json.dumps(body))
            if(response.status_code == 200):
                logger.info('Event was created: ' +  response.content)
                print "Event was created: " + response.content
            else:
                logger.error('Event creation failed: ' + 'status code: ' + str(response.status_code) + ' response body: ' + response.content)
                print 'Event creation failed: ' + 'status code: ' + str(response.status_code) + ' response body: ' + response.content

if __name__ == "__main__":
    main()
```

> Example Response

```json
{
   "id":"b1229900-1a49-4b9e-a2e4-b9d21240793f",
   "title":"deployment started on myServer",
   "description":"my description",
   "source":"chef",
   "category":"deployments",
   "startDate":1468326864,
   "endDate":null,
   "properties":[
      {
         "key":"service",
         "value":"myService"
      }
   ]
}
```

To create a new event.
To add more information to the event use the *properties* field and add additional data with key-value objects.

Argument | Description
---------| -----------
Title | The event's title [**required**]
Description | A description of the event. [*Optional*]
Category | The name of the pre-defined category that the event belongs to (e.g deployments, marketing_campaigns, holidays) [**required**]. If a relevant category does not exist, create one. To create a new category, see Create Category.
Source | The pre-defined source of the request (e.g rest_api, Chef, Jenkins) [**required**]. If the source does not exist, add it. To add a new source, see Create Source.
Properties | Key-value pairs, can be whatever the user wants to add as additional data except the ones that are already part of the body arguments like title, description, category and so on. E.g: severity: high, publisher: nyt, exchange: rubicon. [*Optional*]
StartDate |The start time of the event. Needs to be in epochtime (seconds)[**required**]
EndDate | The end time of the event. Needs to be in epochtime (seconds) [*Optional*]

## Edit Event

> Request Structure: PUT https://api.anodot.com/api/v1/user-events

```json
Content-type=application/json

{
"event": {
    "id":"event id"
    "title": "<event title>",
    "description": "<event description>",
    "category": "<category name>",
    "source": "<source name>",
    "properties":[
     {
        "key":"<key1>",
        "value": "<value1>"
      },
      {
        "key": "<key2>",
        "value": "<value2>"
      }
    ],
    "startDate": "<epoch start date>",
    "endDate": "<epoch end date>"
  }
} 
```

```shell
curl \
-X PUT \
-d '{"event":{"id":"b1229900-1a49-4b9e-a2e4-b9d21240793f","title":"deployment started on myServer","description":"my description","category":"deployments","source":"chef","properties":[{"key":"service","value":"myService"}],"startDate":"1468326864"}}' \
-H "Content-Type:application/json" \
'https://api.anodot.com/api/v1/user-events?token=<API TOKEN>'
```

> Example Response

```json
{
   "id":"b1229900-1a49-4b9e-a2e4-b9d21240793f",
   "title":"deployment started on myServer",
   "description":"my description",
   "source":"chef",
   "category":"deployments",
   "startDate":1468326864,
   "endDate":null,
   "properties":[
      {
         "key":"service",
         "value":"myService"
      }
   ]
}
```

To edit an existing event.
The user can edit an existing event by sending the event with the ID and all the event details - the same as in the add event API.

Argument | Description
---------| -----------
id | The event ID
Title | The event's title [**required**]
Description | A description of the event. [*Optional*]
Category | The name of the pre-defined category that the event belongs to (e.g deployments, marketing_campaigns, holidays) [**required**]. If a relevant category does not exist, create one. To create a new category, see Create Category.
Source | The pre-defined source of the request (e.g rest_api, Chef, Jenkins) [**required**]. If the source does not exist, add it. To add a new source, see Create Source.
Properties | Key-value pairs, can be whatever the user wants to add as additional data except the ones that are already part of the body arguments like title, description, category and so on. E.g: severity: high, publisher: nyt, exchange: rubicon. [*Optional*]
StartDate | The start time of the event. Needs to be in epochtime (seconds)[**required**]
EndDate | The end time of the event. Needs to be in epochtime (seconds) [*Optional*]

## Retrieve Event by ID

> Request Structure: GET https://api.anodot.com/api/v1/user-events/

> Example request

```json
Content-type=application/json

```

```shell
-X GET  \
-H "Content-Type: application/json"  \
'https://api.anodot.com/api/v1/user-events/9b0cfa33-8a78-416e-84b7-1bff5628deaf?token=<api token>'
```

> Example Response

```json
Status 200 OK
{
   "id":"9b0cfa33-8a78-416e-84b7-1bff5628deaf",
   "title":"Independence Day",
   "description":"Happy Holiday",
   "source":"calendar",
   "category":"holidays",
   "startDate":1468307241,
   "endDate":1468307241,
   "properties":[

   ]
}
```

```json
Status 404 Not Found
{
    "errors": [
        {
            "index": null,
            "error": 3016,
            "description": "event with this id does not exit"
        }
    ]
}
```

Use this call to retrieve an existing event definition.

Argument | Definition
-------- | ----------
ID | The ID of the event

## Retrieve Events

> Request Structure: POST https://api.anodot.com/api/v1/user-events/execute

> Example Request

```json
Content-type=application/json

{
  "filter": {
    "categories": [
      "deployment"
    ],
    "q": {
      "expression": [
        {
          "type": "property",
          "key": "host",
          "value": "anodot13",
          "isExact": true
        }
      ]
    },
    "aggregation": {
      "aggregationField": "source",
      "topEventSize": 20,
      "maxBuckets": 100,
      "resolution": "longlong"
    }
  }
}
```

> Example Response

```json
Status 200 OK

{
  "resolution": "long",
  "totalEvents": 91,
  "events": [
    {
      "date": 1513760272,
      "totalEvents": 1,
      "topEvent": [
        {
          "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
          "title": "deployment started on myServer",
          "description": "my description",
          "category": "deployments",
          "source": "chef",
          "properties": [
            {
              "key": "service",
              "value": "myService"
            }
          ],
          "startDate": 1468326864,
          "endDate": null
        }
      ],
      "topGroups": [
        {
          "title": "deployments",
          "total": 1
        }
      ]
    }
  ]
}
```

```json
Status 400 request in bad format

{
  "error": {
    "error": 3015,
    "description": "field properties must be unique"
  }
}
```

Use this call to retrieve events by search expression.

Argument | Description
-------- | ----------
fromDate | (integer) Start time for the event retrieval [**Required**]
toDate | (integer) End time for the event retrieval
order | (string) Result order. Default: asc. Values: asc, desc.
size | (integer) Number of results per page. Default: 10
index | (integer) Page index, used for paged results.

### Response Codes

Code | Description
---- | -----------
200 | Successful Operation
400 | Request in bad format
401 | Unauthorized request
500 | General Error

## Delete Event by ID

> Request Structure: DELETE https://api.anodot.com/api/v1/user-events/{event-id}

Use this call to delete a single event by its ID.

Argument | Definition
-------- | ----------
ID | The ID of the event

## Delete Events

> Request Structure: DELETE https://api.anodot.com/api/v1/user-events/

> Example Request

```json
Content-type=application/json

{ 
   "expression": [ 
   { 
      "type": "property", 
      "key": "category", 
      "value": "deployments" 
   }, 
   { 
      "type": "property", 
      "key": "ver", 
      "value": "!*" 
   } 
   ]   
}
```
Use this call to delete multiple events, located by a search expression.

Argument | Definition
-------- | ----------
expression | Search expression to locate a single or multiple events

# Categories

> End Points

> https://api.anodot.com/api/v1/user-events/categories - Basic Authentication

> https://api.anodot.com/api/v2/user-events/categories - Access Token Authentication

A category is one of the properties of an event.
Anodot provides default categories to help in event classification.
These default categories are **Deployments**, **Alerts**, **Holidays** and **Other**.
The default categories cannot be removed or modified, user defined categories can be added, edited and removed.

## Create Event Category

> Request Structure: POST https://api.anodot.com/api/v1/user-events/categories

```json
Content-type=application/json

{
  "category": {
    "name": "<category name>",
    "imageUrl": "<image url>"
    }  
}
```
> Example Request

```shell
curl \
-X POST 
-d '{"category":{"imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-anodot.png","name":"myCategory"}}' \
-H "Content-Type: application/json" \
'https://api.anodot.com/api/v1/user-events/categories?token=<API TOKEN>'
```

> Example Response

```shell
 { 
   "id":"430f648e-f5e3-40ca-a7e5-50b773ede81b",
   "imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-anodot.png",
   "name":"myCategory",
   "owner":"user"
}
```

This call allows you to create a new event category.

Argument | Definition
-------- | ----------
name | The category name and the way it will be displayed. Name must be unique [**required**]
imageUrl | A url to the image that will be displayed as an icon for the category [*optional*]. The ImageUrl must be located under a secure server - for example  https://myServer/myImage.png. The recommended image size is 32px by 32px.

## Retrieve all Categories

> Request Structure: GET https://api.anodot.com/api/v1/user-events/categories?token=<API_TOKEN>

Retrieve all the categories in the account.

Parameters: None.

## Delete Event Category by ID

> Request Structure: DELETE https://api.anodot.com/api/v1/user-events/categories/{categoryId}

Delete Event Category by ID

Argument | Definition
-------- | ----------
Id | (string) The category Id [**required**]

# Sources

## Create event Source

> Request Structure: POST https://api.anodot.com/api/v1/user-events/sources

```json
Content-type=application/json

{
  "source": {
    "name": "<source name>",
    "imageUrl": "<image url>"
    }  
}
```

> Example Request

```shell
curl \
-X POST 
-d '{"source":{"imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-anodot.png","name":"mySource"}}' \
-H "Content-Type: application/json" \
'https://api.anodot.com/api/v1/user-events/sources?token=<API TOKEN>'
```

> Example Response

```shell
 { 
   "id":"430f648e-f5e3-40ca-a7e5-50b773ede81b",
   "imageUrl":"https://s3.amazonaws.com/anodot-images-common/logo-anodot.png",
   "name":"mySource",
   "owner":"user"
}
```

Create a new event source

Argument | Definition
-------- | ----------
name | The source name and the way it will be displayed. Name must be unique [**required**]
imageUrl | A url to the image that will be displayed as an icon for the source [*optional*]. The ImageUrl must be located under a secure server - for example  https://myServer/myImage.png. The recommended image size is 32px by 32px.

## Retrieve all event Sources

> Request Structure: GET https://api.anodot.com/api/v1/user-events/sources?token=<API_TOKEN>

Retrieve all the sources in the account.

Parameters: None.

## Delete event Source by ID

> Request Structure: DELETE https://api.anodot.com/api/v1/user-events/sources/{sourceId}

Delete Event Source by ID

Argument | Definition
-------- | ----------
Id | (string) The source Id [**required**]
