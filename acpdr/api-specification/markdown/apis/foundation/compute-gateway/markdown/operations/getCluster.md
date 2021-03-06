
<a name="getcluster"></a>
### Gets the cluster details
```
GET /data/foundation/compute/clusters/{clusterId}
```


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**clusterId**  <br>*required*|Cluster id of the cluster|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Cluster info retrieved|[Cluster Details](../definitions/Cluster_Details.md#cluster-details)|
|**401**|Unauthorized access|No Content|
|**404**|Cluster ID not found|No Content|
|**406**|Non Acceptable Response Type.Only application/json allowed|No Content|
|**422**|Input validation failure|No Content|
|**503**|Unable to fetch cluster details|No Content|


#### Produces

* `application/json`


#### Tags

* Describes operations performed on clusters


#### Example HTTP request

##### Request path
```
/data/foundation/compute/clusters/4b3c4fae-212c-404f-9565-9b3fa0a9cb4f
```


#### Example HTTP response

##### Response 200
```
json :
{
  "id" : "8a475fbd-4a58-4b90-bb9b-bb23b6c873ff",
  "name" : "Reporting Jobs Cluster",
  "status" : "string",
  "tags" : {
    "batch" : "123"
  },
  "workerType" : "ML_SIZE_LARGE",
  "minInstance" : 4,
  "maxInstance" : 16,
  "livy" : "{\"url\" : \"http://8a475fbd-livy.ethos.adobe.com\", \"username\" : \"mluser\", \"password\" : \"gJMGq*7Uzg8X3\"",
  "userId" : "[MCDP_HARVESTER@AdobeID]",
  "createdAt" : "2017-10-02T18:17:19Z",
  "updatedAt" : "2017-10-03T18:17:19Z",
  "property" : "ethos_role:8a475fbd-4a58-4b90-bb9b-bb23b6c873ff"
}
```



