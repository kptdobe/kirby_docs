
<a name="batch"></a>
### Batch

|Name|Description|Schema|
|---|---|---|
|**availableDates**  <br>*optional*|Describes what date range of data is available in the Batch. Null if dates aren't relevant for data related to this Batch.  <br>**Example** : `"object"`|[availableDates](#batch-availabledates)|
|**completed**  <br>*optional*|The Unix timestamp (in milliseconds) when the Batch processing action was completed. Completed - Started should yield the total processing time.  <br>**Example** : `0`|integer|
|**created**  <br>*optional*  <br>*read-only*|The Unix timestamp (in milliseconds) when this Batch was persisted.  <br>**Example** : `0`|integer|
|**createdClient**  <br>*optional*  <br>*read-only*|The ID of the IMS client that created this Batch.  <br>**Example** : `"string"`|string|
|**createdUser**  <br>*optional*  <br>*read-only*|The ID of the user who created this object.  <br>**Example** : `"string"`|string|
|**errors**  <br>*optional*|**Example** : `[ "[errors](#errors)" ]`|< [errors](errors.md#errors) > array|
|**failedRecordCount**  <br>*optional*|The number of records that could not be processed in this Batch.  <br>**Example** : `0`|integer|
|**id**  <br>*optional*|The batch identifier.  <br>**Example** : `"string"`|string|
|**imsOrg**  <br>*required*|The owning IMS organization identifier.  <br>**Example** : `"string"`|string|
|**metrics**  <br>*optional*|Contains metrics related to this Batch.  Metric names are determined by the producer of the object since each Batch may want to record metrics that are relevant to the process  <br>**Example** : `"object"`|object|
|**recordCount**  <br>*optional*|The total number of data records (rows/documents) processed in this Batch.  <br>**Example** : `0`|integer|
|**relatedObjects**  <br>*optional*|A list of the objects involved with this Batch.  <br>**Example** : `[ "[relatedobjects](#relatedobjects)" ]`|< [relatedObjects](relatedObjects.md#relatedobjects) > array|
|**size**  <br>*optional*|Number of bytes processed in this Batch.  <br>**Example** : `0`|integer|
|**started**  <br>*optional*|The Unix timestamp (in milliseconds) when the Batch processing action was started.  <br>**Example** : `0`|integer|
|**status**  <br>*required*|The current (mutable) status of this Batch.  <br>**Example** : `"string"`|enum (processing, success, failure, queued, retrying, stalled)|
|**tags**  <br>*optional*|Tags are values associated with a particular object,  these are generally used by external systems for marking an object in a way that it understands.  Normally tags are not used for internal Catalog business logic  <br>**Example** : `"object"`|object|
|**updated**  <br>*optional*  <br>*read-only*|The Unix timestamp (in milliseconds) of last modification.  <br>**Example** : `0`|integer|
|**updatedUser**  <br>*optional*  <br>*read-only*|The ID of the user who changed this object.  <br>**Example** : `"string"`|string|
|**version**  <br>*optional*  <br>*read-only*|The Semantic version of the Batch. Updated when the Batch is modified.  <br>**Example** : `"string"`|string|

<a name="batch-availabledates"></a>
**availableDates**

|Name|Description|Schema|
|---|---|---|
|**endDate**  <br>*optional*|The Unix timestamp (in seconds) for the oldest data available in this Batch.  <br>**Example** : `0`|integer|
|**startDate**  <br>*optional*|The Unix timestamp (in seconds) for the most recent data available in this Batch.  <br>**Example** : `0`|integer|



