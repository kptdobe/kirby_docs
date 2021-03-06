
<a name="update_batch_by_id"></a>
### Updates an existing Batch by ID.
```
PUT /batches/{id}
```


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**if-match**  <br>*optional*|Set to verify the right version of document to be modified by matching the updated date.|string|
|**Header**|**x-api-key**  <br>*required*|The API key belonging to the calling client.|string|
|**Header**|**x-gw-ims-org-id**  <br>*required*|The owning IMS organization identifier.|string|
|**Path**|**id**  <br>*required*|Object ID|string|


#### Body parameter
Data set field(s) to be updated.

*Name* : batch  
*Flags* : required  
*Type* : [batchRequest](../definitions/batchRequest.md#batchrequest)


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Array[ @/batches/batchId ]|< string > array|
|**400**|Bad request|No Content|
|**403**|Forbidden|No Content|
|**404**|Not found|No Content|
|**500**|Internal server error|No Content|
|**default**|Unexpected error|No Content|


#### Consumes

* `application/json`


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**apiKey**|**[Bearer](security.md#bearer)**|


#### Example HTTP request

##### Request path
```
/batches/string
```


##### Request header
```
json :
"string"
```


##### Request body
```
json :
{
  "started" : 0,
  "completed" : 0,
  "status" : "string",
  "recordCount" : 0,
  "failedRecordCount" : 0,
  "errors" : [ {
    "code" : "string",
    "rows" : [ "string" ],
    "description" : "string"
  } ],
  "size" : 0,
  "availableDates" : "object",
  "relatedObjects" : [ {
    "type" : "string",
    "id" : "string",
    "tag" : "string",
    "status" : "string",
    "errors" : [ {
      "code" : "string",
      "rows" : [ "string" ],
      "description" : "string"
    } ],
    "metrics" : {
      "string" : 0
    }
  } ],
  "metrics" : {
    "string" : 0
  },
  "tags" : {
    "string" : [ "string" ]
  }
}
```


#### Example HTTP response

##### Response 200
```
json :
[ "string" ]
```



