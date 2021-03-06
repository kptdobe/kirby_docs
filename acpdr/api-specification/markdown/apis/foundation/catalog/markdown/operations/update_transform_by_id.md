
<a name="update_transform_by_id"></a>
### Updates an existing Transform by ID.
```
PUT /transforms/{id}
```


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**if-match**  <br>*optional*|Set to verify the right version of document to be modified by matching the updated date.|string|
|**Header**|**x-api-key**  <br>*required*|The API key belonging to the calling client.|string|
|**Header**|**x-gw-ims-org-id**  <br>*required*|The owning IMS organization identifier.|string|
|**Path**|**id**  <br>*required*|Object ID|string|


#### Body parameter
Transform field(s) to be updated.

*Name* : transform  
*Flags* : required  
*Type* : [transformRequest](../definitions/transformRequest.md#transformrequest)


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Array[ @/transforms/transformId ]|< string > array|
|**400**|Bad request|No Content|
|**403**|Forbidden|No Content|
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
/transforms/string
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
  "inputs" : [ "object" ],
  "outputs" : [ "object" ],
  "name" : "string",
  "body" : "string",
  "language" : "string",
  "codeUrl" : "string",
  "vehicleUrl" : "string",
  "args" : [ "string" ]
}
```


#### Example HTTP response

##### Response 200
```
json :
[ "string" ]
```



