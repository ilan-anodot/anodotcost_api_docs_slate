## Topology
The Topology APIs enable you to load topology data such as Customers, Geographical and Network Information, the data is then presented over Anodot map view. 

You can find more information about the Network Topology Map in our - [Topology documentation](https://support.anodot.com/hc/en-us/articles/4415016305682-Overview)

The Anodot Topology Data ingestion mechanism is made up of 3 APIs.

* [Topology User](#topology-user) - One time creation of a topology entity
* [Topology Data Ingestion](#topology-data-ingestion)
* [Topology Metric Mapping](#topology-metric-mapping)


### Topology User

>End Point prefix is **/api/v2/topology/user**

> Request Example: Create the topology user

```shell
curl -X PUT \
{
 "_id": "625bcc6e4af784000fc3128d"
}
```
Use this API to create a topology user.

<aside class="warning">
Topology User creation:</br>
This user is required only for the first time the topology APIs are used by the customer.
A topology user should be created before you load any topology data. 
</aside>

Argument | Description
---------| -----------
id | customerID [Get CustomerID](#get_customerID) [**Required**]

### Topology Data Ingestion

>End Point prefix is **/api/v2/topology/map/load**

Anodot’s topology data ingestion mechanism operates on a full data set ingestion, partial additions are not supported.

During this process, the ingested data is first validated and only then loaded to overwrite the existing data stored in the topology entities.

The topology data-load APIs consist of three APIs that load the full updated topology data set:

* [Load Start](#load-start)
* [Bulk Entities Load](#bulk-entities-load)
* [Load End](#load-end)

### Load Start

> Request Example: Load start

```shell
curl -X POST \
https://app.anodot.com/api/v2/topology/map/load/start \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to start the topology data load. This API is used to mark the process initiation.

The process retrieves a new “rollupid” to be used in the data ingestion API.</br>
A successful response ("Response 200”) includes a new “rollupid”, which is used by the ‘Bulk entities load’ API.

<aside class="notice">
The call might fail (“Response 401”) if a user does not exist in the topology service.<br/>
In this case, the user should be created using the 'Topology User' Creation API.
</aside>

### Bulk Entities Load

> Request Example: Bulk Entities Load

```shell
curl -X PUT \
https://app.anodot.com/api/v2/topology/map/load/start \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D '{
  "bulkSerNumber": 1,
  "numberOfRows": 7,
  "type": "SITE",
  "rows": {
    "DFWIRV01CORE001": "{"id":"DFWIRV01CORE001","parentRegionId":"DFWIRV01","name":"DFWIRV01CORE001","latitude":"32.907385","longitude":"-97.03882","customer":"DFW"}",
    "DFWIRV01ENB001": "{"id":"DFWIRV01ENB001","parentRegionId":"DFWIRV01","name":"DFWIRV01ENB001","latitude":"32.906672","longitude":"-97.036331","customer":"DFW"}",
  },
  "rollupId": "6"
}'
```

#### Response - successful iteration

> Response 200 example:

```json
{
  "bulkSize": 500,
  "entityCounts": {},
  "insertErrors": [],
  "missingIds": {},
  "validationErrors": [],
  "rollupId": 26
}
```

#### Response - with failed entity records

> Response with failed validations example:

```json
{
    "rollupId": 28,
    "bulkSize": 6,
    "validationErrors": [
        {"type": "CELL","error": "entity failed on validation","json": "{\"relatedNodeId\":\"24722\",\"vendor\":\"Ericsson\",\"domain\":\"RAN;3G\",\"name\":\"U2477\",\"band\":\"2100\",\"id\":\"U2477\",\"lac\":\"333\"}"
        }
    ],
    "insertErrors": [],
    "missingIds": {},
    "entityCounts": {}
}
```

Use this API to load the topology data for each entity.

* Each PUT call can contain up to 500 records.
* Each PUT call should contain only records of the same entity type.
* The record format is a key value pair, where the key is the entity id, and the value is a JSON representation of the entity.
* Every bulk should include:
  * Bulk serial number of entity bulk
  * Number of rows in the bulk
  * rollupId
  * Topology data fields and data corresponding to the entity
* Entity Load validations:
  * Mandatory arguments validation (according to the entity mapping guidelines)
  * Arguments content validation (according to the entity mapping guidelines)

<aside class="notice">
The PUT call can succeed even if some entities fail on validation.
</aside>

#### Request Arguments

Argument | Description
---------| -----------
type | The Entity type, taken from a predefined list: “REGION”, “SITE”, “NODE”, “CARD”, “INTERFACE”, “CELL”, “LINK”, “SERVICE”, “APPLICATION”, “LOGICALEGROUP". [**required**]
rollupId | rollupId retrieved from the load start API. [**required**]
timestamp | Date and time. [*Optional*]
bulkSerNumber | The topology load is divided into bulks, with each bulk stamped with a serial number. [*Optional*]
numberOfRows | Number of entity records in the bulk. [*Optional*]
rows | The topology entity data according to the predefine topology entity fields. [**required**]

In both responses demonstrated, the PUT call was successful.

### Topology entity arguments

* Product Arguments:</br> 
Each topology entity consists of a predefined set of product arguments. These arguments are used in the network topology view by default (Presentation names, Search capability and Info).
* Custom Arguments:</br>
Custom arguments can be added ad-hoc by the customer as additional arguments. Keep in mind these custom fields will not be used in the network topology search and informational view capability by default and will require product customization that should be coordinated with the product team. Please send a mail to support@anodot.com if you would like to start using the extended version or reach out your CSM.

<aside class="success">
A Pro Tip:</br>
Each entity record consists a row which starts with the 'entity id' followed by the mandatory/optional entity fields.</br>
For example:</br>
"rows": {
entity "id": "{"entity field":"value"}
</aside>

#### Region entity

> Request Example: Region entity

```json
{
  "bulkSerNumber": 4,
  "numberOfRows": 2,
  "type": "REGION",
  "rows": {
    "NYC": "{"id":"NYC","name":"NYC","customer":""}",
    "KFC-C1-NYC": "{"id":"KFC-C1-NYC","parentRegionId":"NYC","name":"KFC-C1-NYC","customer":"KFC"}",
  },
  "rollupId": "6"
}

```

* type=”REGION”
* There can be several region levels, for example: District, Customer, Location.

Argument | Unique | Description
---------| ------ | -----------
id | Y | Key field. Unique identifier of the entity. The value shall match one/multiple metrics dimensions combined for uniqueness and identification. [**Required**]
parentRegionId | Y | Used for region relation. Region-> Upper Region level association. In case the region level is the upper level, the field can be Null[**Required**]
name | Y | Region display Name. [**Required**]
type | N | Type of Region and Site. For example: County, City, etc. [*Optional*]

#### Site entity

> Request Example: Site entity

```json
{
  "bulkSerNumber": 4,
  "numberOfRows": 1,
  "type": "SITE",
  "rows": {
"KFC-Site1": "{"id":"KFC-Site1","parentRegionId":"KFC-C1-NYC","name":"KFC-Site1","latitude":"30.907385","longitude":"-90.03882","customer":"KFC"}"  }
  "rollupId": "6"
}

```

* type=”SITE”

* Site will be associated with the Region entity.

Argument | Unique | Description
---------| ------ | -----------
id | Y | Key field. Unique identifier of the entity. The value shall match one/multiple metrics dimensions combined for uniqueness and identification. [**Required**]
parentRegionId | N | The region in which the site is located in, corresponding to REGION entity “id”. [**Required**]
name | Y | Site display Name. [**Required**]
type | N | Site type, such as: RAN Site, Data Center, Central Office, Local Exchange. [*Optional*]
domain | N | Site related Domain. [*Optional*]
latitude | N | Site Latitude coordinate. [**Required**]
longitude | N | Site longitude coordinate. [**Required**]
address | N | Site address. [*Optional*]
zipcode | N | Site Zip Code. [*Optional*]
customer | N | Customer names can be used in cases of enterprise sites, mobile shared sites etc. [*Optional*]

#### Node entity

> Request Example: Node entity

```json
{
  "bulkSerNumber": 4,
  "numberOfRows": 1,
  "type": "NODE",
  "rows": {
 "KFC-Site1-NR1": "{"id":"KFC-Site1-NR1","name":"KFC-Site1-NR1","siteId":"KFC-Site1","keyword":"Cell Site","type":"Cell Site","domain":"5G","customer":"KFC"}"}
  "rollupId": "6"
}
```
* type=”NODE”

Argument | Unique | Description
---------| ------ | -----------
id | Y | Key field. Unique identifier of the entity. The value shall match one/multiple metrics dimensions combined for uniqueness and identification. [**Required**]
name | Y |  Node display name. [**Required**]
siteId | N | The site “id” in which the node is located in, corresponding to ‘SITE’ entity “id”. [**Required**]
keywords | N | The keyword defines the Node classification, and is used in the Map node visualization. Pre-defined keyword list: 'Cell Site', 'Router', 'Switch', ‘Server’. [**Required**]
type | N | Free text describing the equipment type, for example: CSR 1800. [*Optional*]
domain | N | Network Domain used by the map domain filtering. such as: 3G,4G, 5G, Mobile Core, IP, Optical, MW, etc. [*Optional*]
description | N | Free text describing the equipment. [*Optional*]
status | N | Node status, such as: Active, In Use, In Maintenance, Planned. [*Optional*]
vendor | N | Node vendor. [*Optional*]
ip | N | Node IP Address. [*Optional*]
customer | N | Customer related Node. [*Optional*]
relatedNodeId | N | Dimension ID of a related Node, for example: NodeB -> RNC relation. [*Optional*]

#### Cell entity

> Request Example: Cell entity

```json
{
  "bulkSerNumber": 4,
  "numberOfRows": 1,
  "type": "CELL",
  "rows": {
 "KFC-Site1-NR1C1": "{"id":"KFC-Site1-NR1C1","name":"1","relatedNodeId":"KFC-Site1-NR1","azimuth":"270","domain":"5G","status":"Active"}"}
  "rollupId": "6"
}
```
* type=”CELL”

Argument | Unique | Description
---------| ------ | -----------
id | Y | Key field. Unique identifier of the entity. The value shall match one/multiple metrics dimensions combined for uniqueness and identification. [**Required**]
name | N | Cell display name. [**Required**]
relatedNodeId | N | The Cell Site Node “id”, corresponding to ‘NODE’ entity “id”. [**Required**]
azimuth | N | Cell Azimuth, used for the cell position over the map. [**Required**]
band | N | Cell band/frequency. [*Optional*]
domain | N | Network domain Used in the map domain filtering, such as: 3G,4G, 5G, Mobile Core, IP, Optical, MW, etc. [*Optional*]
status | N | Cell status, such as: Active, In Use, In Maintenance, Planned. [*Optional*]
vendor | N | Cell vendor. [*Optional*]
latitude | N | Cell Latitude coordinate. Required only in case in which the cell is located away from the site. [*Optional*]
longitude | N | Cell Longitude coordinate. Required only in case in which the cell is located away from the site. [*Optional*]
lac | N | Cell lac identification. [*Optional*]
rac | N | Cell RAC identification. [*Optional*]
ci | N | Network Domain, such as: 3G,4G, 5G, Mobile Core, IP, Optical, MW, etc.. Used by the map domain filtering. [*Optional*]
neighborcells | N | Cell status, such as: Active, In Use, In Maintenance, Planned. [*Optional*]
tilt | N | Cell tilt. [*Optional*]
description | N | Cell Latitude coordinate. Required only in case in which the cell is located away from the site. [*Optional*]

#### Card entity

> Request Example: Card entity

```json
{
  "bulkSerNumber": 4,
  "numberOfRows": 1,
  "type": "CARD",
  "rows": {
    "RTR01/CPU01": "{"id":"RTR01/CPU01","name":"RTR01/CPU01","relatedNodeId":"RTR01","type":"CPU","status":"Active"}"}
  "rollupId": "6"
}
```
* type=”CARD”

Argument | Unique | Description
---------| ------ | -----------
id | Y | Key field. Unique identifier of the entity. The value shall match one/multiple metrics dimensions combined for uniqueness and identification. [**Required**]
name | N | Card display name. [**Required**]
relatedNodeId  | N | The “id” of the parent Node, corresponding to ‘NODE’ entity “id”. [**Required**]
type | N | Free text describing the card type, for example: PIC. [*Optional*]
status | N | Card status. [*Optional*]
description | N | Free text. [*Optional*]

#### Interface entity

> Request Example: Interface entity

```json
{
  "bulkSerNumber": 4,
  "numberOfRows": 1,
  "type": "INTERFACE",
  "rows": {
 "RTR01/ETH01": "{"id":"RTR01/ETH01","name":"RTR01/ETH01","relatedNodeId":"RTR01","type":"Ethernet","status":"Active"}"}
  "rollupId": "6"
}
```
* type=”INTERFACE”

Argument | Unique | Description
---------| ------ | -----------
id | Y | Key field. Unique identifier of the entity. The value shall match one/multiple metrics dimensions combined for uniqueness and identification. [**Required**]
name | N | Interface display name. [**Required**]
relatedNodeId  | N | The “id” of the parent Node, corresponding to ‘NODE’ entity “id”. [**Required**]
type | N | Free text describing the interface type. for example: optical. [*Optional*]
status | N | Interface status. [*Optional*]
description | N | Free text. [*Optional*]
ip | N | Interface IP Address. [*Optional*]
customer | N | Associated customer. [*Optional*]

#### Link entity

> Request Example: Link entity

```json
{
  "bulkSerNumber": 4,
  "numberOfRows": 1,
  "type": "INTERFACE",
  "rows": {
 "RTR01-RTR02-001": "{"id":"RTR01-RTR02-001"","name":"RTR01-RTR02-001","sourceNodeId":"RTR01","destNodeId":"RTR02","type":"Ethernet","status":"Active"}
  "rollupId": "6"
}
```

* type=”LINK”

Argument | Unique | Description
---------| ------ | -----------
id | Y | Key field. Unique identifier of the entity. The value shall match one/multiple metrics dimensions combined for uniqueness and identification. [**Required**]
name | N | Link display name.[**Required**]
sourceNodeId  | N | The “id” of the source Node, corresponding to ‘NODE’ entity “id”. [**Required**]
destNodeId  | N | The “id” of the destination Node, corresponding to ‘NODE’ entity “id”. [**Required**]
sourceInterfaceId  | N | The “id” of the source Interface, corresponding to ‘INTERFACE’ entity “id”. [*Optional*]
destInterfaceId  | N | The “id” of the destination Interface, corresponding to ‘INTERFACE’ entity “id”. [*Optional*]
type | N | Free text describing the interface type. for example: optical. [*Optional*]
status | N | Interface status. [*Optional*]
description | N | Free text. [*Optional*]
customer | N | Associated customer. [*Optional*]

#### Service entity

> Request Example: Service entity

```json
{
  "bulkSerNumber": 4,
  "numberOfRows": 1,
  "type": "SERVICE",
  "rows": {
 "OSFP-001": "{"id":"OSFP-001"","name":OSFP-001","type":"OSFP Ring","status":"Active","relatedEntityType":"INTERFACE","relatedEntityId":"RTR01/ETH01"}"}
  "rollupId": "6"
}
```

* type=”SERVICE”

Argument | Unique | Description
---------| ------ | -----------
id | Y | Key field. Unique identifier of the entity. The value shall match one/multiple metrics dimensions combined for uniqueness and identification. [**Required**]
name | N | Service display name.[**Required**]
type | N | Service type name. [*Optional*]
status | N | Interface status. [*Optional*]
relatedEntityType | N | Associated entity type, taken from a predefined list: REGION, SITE, NODE, CARD, INTERFACE, LINK, CELL, SERVICE. [*Optional*]
relatedEntityId | N | Dimension ID of the associated entity instance. [*Optional*]
description | N | Free text. [*Optional*]
customer | N | Associated customer. [*Optional*]

### Load End

> Requests Structure:

> POST http://{{app-url}}/api/v2/topology/map/load/{rollupId}/end

> Request Example: Load End

```shell
curl -X POST \
https://app.anodot.com/api/v2/topology/map/load/1/end \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API as a markup to end the topology load process. The API initiates internal enrichment processing and integrity validation between the topology entities. As a result, the execution time for this API may take several minutes.
If the Integrity validation results include a certain amount of crucial mismatched data (according to internal thresholds), the topology data load will be aborted.
{rollupId} is being retrived by the [Load Start](#Load-Start)

<aside class="notice">
Successful responses result in “Response 200”.
</aside>

### Topology Metric Mapping

> Requests Structure:

> PUT http://{{app-url}}/api/v2/topology/user/metric-mapping

> GET http://{{app-url}}/api/v2/topology/user/metric-mapping

> Request Example: Topology Metric Mapping

```shell
curl -X PUT \
curl -X PUT \
https://app.anodot.com/api/v2/topology/user/metric-mapping \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-D 
  "3G_DL_data_Drop_Rate_HSDPA": [
    {
      "delimiter": "_",
      "properties": [
        "BTS_Name"
      ],
      "types": [
        "NODE"]
    }
  ],
  "3G_Download_HS_Data_Throughput_per_user_average": [
    {
      "delimiter": "_",
      "properties": [
        "BTS_Name"
      ],
      "types": [
        "NODE"]
    }
  ]
}’
```

This API loads a metric mapping of the user by mapping between the "Entity id" to the Metric dimension. This API is not used periodically but upon changes to the user metrics.

<aside class="success">
A Pro Tip:</br>
"Entity id” argument:
The mapping provides the option to concatenate the metric dimension in order to create the match to the topology Dimension ID.
For example: 
Metric: ifInUcastPkts, Host:Router1,Interface:GE-1/1/0
Topology Dimension ID: Router1/GE-1/1/0
</aside>

#### Request Arguments

Argument | Description
---------| -----------
Measure name | Measure name as it appears in Anodot’s managed metrics.  [**Required**]
delimiter | This will be used in cases where metric dimension concatenation is required in order to create a matching value between the Anodot metrics and the topology entity “Dimension ID”. The delimiter is irrelevant in cases where there's only one property in the “properties” field. In these cases, the field can be empty or include any type of character. [**Required**]  <br/><br/>For example: <br/>Interface entity dimension ID : Router1/Interface10 <br/><br/>Anodot metric mapping :  <br/>Measure: Traffic  <br/><br/>Dimensions: <br/>Host Name: Router1 <br/>Interface Name: Interface10 <br/><br/>“delimiter”: “/”, <br/>"properties": [“Router1”, “Interface10”] <br/>
properties | The metric dimension that will be used as a matching property between the Anodot metric and the topology entity “Dimension ID”. [**Required**]
types | Topology Entity Type (REGION, SITE, NODE, CARD, INTERFACE, LINK, CELL, SERVICE, APPLICATION, LOGICAL GROUP). [**Required**]


