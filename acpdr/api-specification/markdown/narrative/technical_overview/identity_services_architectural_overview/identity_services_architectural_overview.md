# Identity Services - Technical Overview
<!-- The majority of the documentation here came from: https://wiki.corp.adobe.com/display/DMSArchitecture/Unified+Identity+Service+-+API+Specification-->

## 1. Overview

Delivering right and relevant digital experiences requires a correct and complete identification of your End User. Identity Service provides API based access to consumer Identity Graphs which resolves a known or anonymous identity to all other known and anonymous identities of a person. Identity Graph solves the fundamental challenge posed by the fragmentation of identities and thus profiles, across multiple devices owned by a consumer, and is needed for improving the accuracy and reach of digital marketing campaigns.

![Identity Services in the ACP](identity_services_architecture.png)

### 1.1 Audience

This document is aimed at technical personas and should be a useful tool for all users that need to:

* Consume Adobe Cloud Platform APIs
* Understand Adobe Cloud Platform Architecture
* Understand how to differentiate IDs from varying systems within the Experience Cloud
* Establish compliance with GDPR
* Architect integrations between customer-owned and 3rd party systems and Adobe Cloud Platform

### 1.2 Version Information

*Version* : Preview

### 1.3 License Information

*Terms of service* : https://www.adobe.com/legal/terms.html

### 1.4 URI Scheme

*Host* : __platform.adobe.io__  
*BasePath* : __/data/core/identity/__   
*Schemes* : __HTTPS__ 

### 1.5 About the Docs

The HTML rendition of this documentation is kept up-to-date on a per commit basis and can therefore change without announcement. If you require a persistent version of the documentation, it is recommended that you seek out the PDF rendition.

### 1.6 Using the API

This document describes interacting with Identity Namespace Services using Adobe's Platform APIs. See the [Adobe I/O Authentication Overview](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html) for information on how to access these services.

![](lightbulb.jpg) Before you start using the APIs, please read though these notes.

* All variations of Cluster and Mapping APIs support both XID and NID in their requests and response. One of the parameters is required - `xid` or combination of (`nsid`, `id`) to use these APIs
* To limit the payload in response, APIs are adapt their responses to `xid` or `uid`. That is, if you pass XID your responses will have XIDs, if you pass NID responses will have NID's
* The below examples don't cover all usages of XIDs and NID's. For the complete API, see [the Swagger](https://git.corp.adobe.com/pages/experience-platform/api-specification) by selecting Core Services/Identity Services from the __Select a spec__ drop down menu on the top right

#### 1.6.1 Required Headers

All APIs in this document require the following headers unless otherwise indicated:

|Header|Value|Description|
|---|---|---|
|content-type|application/json|The input content type (Only for POST)|
|Authorization|Bearer {TOKEN}|The IMS service token used for authenticating the caller, prefixed with the string "Bearer "|
|Accept|application/vnd.adobe.identity+json;version=1.2|The version of the resource's representation|
|x-gw-ims-org-id|{imsOrgId}, Eg: 17FA2AFD56CF35747F000101@AdobeOrg|The IMS Org ID of client|
|x-api-key|API key|The Client ID/API key of whitelisted client|

---

## 2. Definition of Terms

### 2.1 XID

Experience ID (XID) is the identity object for Experience Cloud, serving as the encapsulation of the Namespace ID (NID) and the identity in the Namespace. There are multiple representations of XIDs - including the existing solution specific representations such as AMO Id, Analytics Id, Target ID, AAM ID and the Marketing Cloud ID. All types of identities can be expressed as an XID, including a consumer identity or a physical device id.

An AMO cookie-id (for example `Was_2AAAAFZ7q3xO`) is an XID with a Namespace ID of 411.  Similarly an AAM cookie-id (for example `64077380566994308300716050799446918829`) is an XID with a namespace of 0. 
<!-- CORE-13607 - Please explain: The native serialized format also supports one way hashing of PII type of identities through the SDK. -->
Identity Services provide a native serialized format of XID. A native format of XID can be obtained either using an SDK or using a REST API. The native serialized format also supports one way hashing of PII type of identities through the SDK.

### 2.2. Namespace

Profile data in the ACP is an aggregate of data from varying solutions, creating the need to qualify a given identifier with the system from which it comes. Namespaces are a cornerstone of the Identity Services. For more information, see the [Identity Namespace Overview](../identity_namespace_overview/identity_namespace_overview.md). 

### 2.3 NID

NID is the qualified Namespace version of an ID, as in `nsid` and `id` tuple represents `nid`. 

### 2.4 IMS Org Id

Customer/Partner IMS Org Id as created in Adobe's customer onboarding process. For more information, see the [Adobe I/O Authentication Overview](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html).

---

## 3. Working with Identity Services

As the single source of truth for identity resolution in the Adobe Cloud Platform, Identity Services provide the following behavior:
<!-- CORE-13607 - Does this accurately and completely describe the overall behavior provided by this release of Identity Services? -->
* __Generation of ID__ - establishment of links between namespaces
* __Computation of identity clusters__ - internal device graph resolves device specific identities
* __Access to identify clusters and mappings through APIs__ - this document covers the Identity Services API

The following are examples meant to describe the way the Identity Services APIs behave. For details on the complete API, see [the Swagger](https://git.corp.adobe.com/pages/experience-platform/api-specification) by selecting Core Services/Identity Services from the __Select a spec__ drop down menu on the top right.

### 3.1 Get XID

Given the Namespace (identified by Namespace ID (e.g. "411") or Namespace code (e.g. "AMO") as `nsId` or `ns` respectively) and `id` in that Namespace, returns XID string.
<!-- CORE-13607 - I don't understand this on `isMCID`	integer	Optional - 1 or 0	Conversion from MCID to Core Namespace. -->
```
GET https://platform.adobe.io/data/core/identity?nsId=411&id=WTCpVgAAAFq14FMF HTTP/1.1
or
GET https://platform.adobe.io/data/core/identity?ns=AMO&id=WTCpVgAAAFq14FMF HTTP/1.1
```

__Example cURL request__

```
# Request for stubbed data
curl -X GET \
  'https://platform.adobe.io/data/core/identity/identity?nsId=411&id=WTCpVgAAAFq14FMF' \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-uis-cst-ctx: stub' \
  -H 'x-gw-ims-org-id: 111'
 
# Real API
curl -X GET \
  'https://platform.adobe.io/data/core/identity/identity?nsId=411&id=WTCpVgAAAFq14FMF' \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-gw-ims-org-id: 111'
```

<!-- CORE-13607 - Include this? Reword at all? -->
![](lightbulb.jpg) NOTE: Usage of `x-uis-cst-ctx: stub` header will return a stubbed response. This is only a stop gap solution for our consumers to facilitate early integration development progress, while services are being completed. We will keep our consumers advised to when this is no longer needed or supported.

__Example Response__

```
Returns an HTTP 200 OK on success.

Expected response

{
    "xid": "CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8"
}
```

### 3.2 Cluster Members API

Given an XID return all XIDs, in the same or other Namespaces, that are linked to it by the device graph type. The related XIDs are considered to be part of the same "cluster". 

<!-- CORE-13607 - I don't understand this enough to elaborate -->
![](lightbulb.jpg) NOTE: Use optional `graph-type` parameter to indicate the output type to get the cluster from. Options are:

* __coop__ - graph built by using coop data
* __pdg__ - private device graph  
* __psr__ - propriatery stiched rules

```
GET https://platform.adobe.io/data/core/identity/cluster/members?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8 HTTP/1.1
or
GET https://platform.adobe.io/data/core/identity/cluster/members?nsId=411&id=WTCpVgAAAFq14FMF HTTP/1.1
or
GET https://platform.adobe.io/data/core/identity/cluster/members?ns=AMO&id=WTCpVgAAAFq14FMF HTTP/1.1
```

OR, use POST as a batch equivalent of GET method. Returns the XIDs that belong to the same cluster, for multiple XIDs.

![](lightbulb.jpg) NOTE: Caller should limit their requests to 1000 XIDs. Request exceeding 1000 XIDs will result in 400 status code.

```
POST https://platform.adobe.io/data/core/identity/clusters/members HTTP/1.1

Body: 

{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"]
}

or

{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "coop"
}

or

{
    "compositeXids": [{
            "nsid": 411,
            "id": "WRbM7AAAAJ_PBZHl"
        },
        {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        }
    ],
    "graph-type": "coop"
}
```

__Example cURL requests__

```
## Stub
curl -X POST \
  https://platform.adobe.io/data/core/identity/clusters/members \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'content-type: application/json' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-uis-cst-ctx: stub' \
  -H 'x-gw-ims-org-id: B3349894589501FE0A494034@AdobeOrg' \
  -d '{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"]
}'
  
## Real Call - Using XIDs
curl -X POST \
  https://platform.adobe.io/data/core/identity/clusters/members \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'content-type: application/json' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-gw-ims-org-id: B3349894589501FE0A494034@AdobeOrg' \
  -d '{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "coop"
}' | json_pp
  
## Real Call - Using UID's
curl -X POST \
  https://platform.adobe.io/data/core/identity/clusters/members \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'content-type: application/json' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-gw-ims-org-id: B3349894589501FE0A494034@AdobeOrg' \
  -d '{
    "compositeXids": [{
            "nsid": 411,
            "id": "WRbM7AAAAJ_PBZHl"
        },
        {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        }
    ],
    "graph-type": "coop"
}' | json_pp
```

<!-- CORE-13607 - Include this? Reword at all? -->
![](lightbulb.jpg) Usage of `x-uis-cst-ctx: stub` header will return a stubbed response. This is only a stop gap solution for our consumers to facilitate early integration development progress, while services are being completed. We will keep our consumers advised to when this is no longer needed or supported.

__Example response__

```
## Stubbed Response
    
{
   "version": 1,
   "clusters": [{
           "xid": "GZsBQnHQaGtL46ZKSvO9bNRE1DcUyQA",
           "compositeXid": {
               "nsid": 411,
               "id": "WRbM7AAAAJ_PBZHl"
           },
           "members": ["e8138f65-d3d3-4485-a7e1-6712e047349d", "21312343536983537571245438594"],
           "members": [{
                   "nsid": 0,
                   "id": "27064814400205787570627663430729680462"
               },
               {
                   "nsid": 411,
                   "id": "86826386186182763871263871263876128612"
               }
           ]
       },
       {
           "xid": "CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8",
           "compositeXid": {
               "nsid": 411,
               "id": "WRbM7AAAAJ_PBZHl"
           },
           "members": [],
           "members": []
       }
   ],
   "unprocessedXids": ["cb0665db616f49758713252d8a335c1e"],
   "unprocessedNids": [{
       "nsid": 411,
       "id": "WY-RNgAAArI4rGBo"
   }]
}
 
## Real Response

{
   "unprocessedXids": [],
   "unprocessedNids": [],
   "version": "1.0.0",
   "clusters": [{
           "xid": "411|WRbM7AAAAJ_PBZHl",
           "members": [
               "411|WRbM7AAAAJ_PBZHl",
               "0|47713142741924778930324734610798294416"
           ],
           "compositeXid": {
               "nsid": 411,
               "id": "WRbM7AAAAJ_PBZHl"
           },
           "members": [{
                   "nsid": 411,
                   "id": "WRbM7AAAAJ_PBZHl"
               },
               {
                   "nsid": 0,
                   "id": "47713142741924778930324734610798294416"
               }
           ]
       },
       {
           "xid": "411|WY-RNgAAArI4rGBo",
           "compositeXid": {
               "nsid": 411,
               "id": "WY-RNgAAArI4rGBo"
           },
           "members": [
               "411|WY-RNgAAArI4rGBo",
               "411|WY-RNgAAArI4rGGy"
           ],
           "members": [{
                   "nsid": 411,
                   "id": "WY-RNgAAArI4rGBo"
               },
               {
                   "nsid": 411,
                   "id": "WY-RNgAAArI4rGGy"
               }
           ]

       }
   ]
}
```
   
![](lightbulb.jpg) The response will always have one entry for each XID provided in the request regardless of whether a request's XIDs belong to the same cluster or if one or more have any cluster associated at all.

### 3.3 Cluster History API

Given an XID, return all cluster associations with that XID. Cluster Associations - XIDs can move clusters over the course of various device graph runs, this API provides cluster id associations of a given XID over time.

<!-- CORE-13607 - I don't understand this enough to elaborate -->
![](lightbulb.jpg) NOTE: Use optional `graph-type` parameter to indicate the output type to get the cluster from. Options are:

* __coop__ - graph built by using coop data
* __pdg__ - private device graph  
* __psr__ - proprietary stitched rules

```
GET https://platform.adobe.io/data/core/identity/cluster/history?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8 HTTP/1.1
or
GET https://platform.adobe.io/data/core/identity/cluster/history?nsId=411&id=WTCpVgAAAFq14FMF HTTP/1.1
or
GET https://platform.adobe.io/data/core/identity/cluster/history?ns=AMO&id=WTCpVgAAAFq14FMF HTTP/1.1
```

OR, use POST as a batch equivalent of GET method. Returns the cluster history for each of the given XIDs.

![](lightbulb.jpg) Caller should limit their requests to 1000 XIDs. Request exceeding 1000 XIDs will result in 400 status code.

```
POST https://platform.adobe.io/data/core/identity/clusters/history HTTP/1.1

Body: 

{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "coop"
}

or

{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "coop"
}

or

{
    "compositeXids": [{
            "nsid": 411,
            "id": "WRbM7AAAAJ_PBZHl"
        },
        {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        }
    ],
    "graph-type": "coop"
}
```

__Example cURL requests__

```
#Stub Response
curl -X POST \
  https://platform.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'content-type: application/json' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-gw-ims-org-id: B3349894589501FE0A494034@AdobeOrg' \
  -H 'x-uis-cst-ctx: stub' \
  -d '{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "coop"
}'
 
 
# Using XIDs
curl -X POST \
  https://platform.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'content-type: application/json' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-gw-ims-org-id: B3349894589501FE0A494034@AdobeOrg' \
  -d '{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "coop"
}' | json_pp
 
# Using UID's
curl -X POST \
  https://platform.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'content-type: application/json' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-gw-ims-org-id: B3349894589501FE0A494034@AdobeOrg' \
  -d '{
    "compositeXids": [{
            "nsid": 411,
            "id": "WRbM7AAAAJ_PBZHl"
        },
        {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        }
    ],
    "graph-type": "coop"
}' | json_pp
```

<!-- CORE-13607 - Include this? Reword at all? -->
![](lightbulb.jpg) Usage of `x-uis-cst-ctx: stub` header will return a stubbed response. This is only a stop gap solution for our consumers to facilitate early integration development progress, while services are being completed. We will keep our consumers advised to when this is no longer needed or supported.

__Example Response__

```
{
    "version": 1,
    "xidsClusterHistory": [{
            "xid": "GZsBQnHQaGtL46ZKSvO9bNRE1DcUyQA",
            "compositeXid": {
                "nsid": 411,
                "id": "WY-RNgAAArI4rGBo"
            },
            "clusterHistory": [{
                    "clusterId": "4c686f23-0871-41c2-b4f4-adef89f6bd2c",
                    "cRecordedTS": "1504741401382"
                },
                {
                    "clusterId": "29bf066c-971a-11e7-abc4-cec278b6b50a",
                    "cRecordedTS": "1502063001629"
                },
                {
                    "clusterId": "aeb2f60c-b0f1-446a-91dd-d28ab6a44ff9",
                    "cRecordedTS": "1499384601763"
                }
            ]
        },
        {
            "xid": "CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8",
            "compositeXid": {
                "nsid": 411,
                "id": "WY-RNgAAArI4rGBo"
            },
            "clusterHistory": [{
                "clusterId": "4c686f23-0871-41c2-b4f4-adef89f6bd2c",
                "cRecordedTS": "1504741401937"
            }]
        }
    ],
    "unprocessedXids": ["cb0665db616f49758713252d8a335c1e"],
    "unprocessedNids": [{
        "nsid": 411,
        "id": "WY-RNgAAArI4rGBo"
    }]
 
}
```

![](lightbulb.jpg) The response will always have one entry for each XID provided in the request regardless of whether a request's XIDs belong to the same cluster or if one or more have any cluster associated at all.

### 3.4 Mapping API

Given an XID, returns all XID mappings in the requested Namespace (targetNs).

```
GET https://platform.adobe.io/data/core/identity/mapping?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8 HTTP/1.1
or
GET https://platform.adobe.io/data/core/identity/mapping?nsId=411&id=WTCpVgAAAFq14FMF HTTP/1.1
or
GET https://platform.adobe.io/data/core/identity/mapping?ns=AMO&id=WTCpVgAAAFq14FMF HTTP/1.1
```

OR, use POST as a batch equivalent of GET method. Returns mappings for multiple identities.

![](lightbulb.jpg) Caller should limit their requests to 1000 XIDs. Request exceeding 1000 XIDs will result in 400 status code.

```
POST https://platform.adobe.io/data/core/identity/mappings HTTP/1.1

Body:

{
 "xids" : ["GesCQXX0CAESEE8wHpswUoLXXmrYy8KBTVgA"],
 "targetNs": "0",
 "graph-type": "coop"
}

or 

{
    "compositeXids": [{
            "nsid": 411,
            "id": "WRbM7AAAAJ_PBZHl"
        },
        {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        }
    ],
 "targetNs": "0",
 "graph-type": "coop"
}
```

__Example cURL requests__

```
# Using XIDs
curl -X POST \
  https://platform.adobe.io/data/core/identity/mappings \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'content-type: application/json' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-gw-ims-org-id: 111111@AdobeOrg' \
  -d '{
 "xids" : ["GesCQXX0CAESEE8wHpswUoLXXmrYy8KBTVgA"],
 "targetNs": "0",
 "graph-type": "coop"
}' | json_pp
  
# Using UID's
curl -X POST \
  https://platform.adobe.io/data/core/identity/mappings \
  -H 'authorization: CALLERS_IMS_SERVICE_TOKEN' \
  -H 'content-type: application/json' \
  -H 'x-api-key: CALLERS_API_KEY/CLIENT_ID' \
  -H 'x-gw-ims-org-id: 111111@AdobeOrg' \
  -d '{
    "compositeXids": [{
            "nsid": 411,
            "id": "WRbM7AAAAJ_PBZHl"
        },
        {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        }
    ],
 "targetNs": "0",
 "graph-type": "coop"
}' | json_pp
```

If no related identities were found with the provided input, an `HTTP 204` response code is returned with no content.

<!-- CORE-13607 - Include this? Reword at all? -->
![](lightbulb.jpg) Usage of `x-uis-cst-ctx: stub` header will return a stubbed response. This is only a stop gap solution for our consumers to facilitate early integration development progress, while services are being completed. We will keep our consumers advised to when this is no longer needed or supported.

__Example response__
<!-- CORE-13607 - Duplicate key: mapping... Should this be broken out into valid JSON objects? -->

```JSON
{
    "version": 1,
    "mappings": [{
        "xid": "CAESEPl1uYyma1kMDWxx7dhbwGo",
        "mapping": [{
            "xid": "81218968060697815473313992060878182012",
            "lastAssociationTime": "1493310475047"
        }],
        "compositeXid": {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        },
        "mapping": [{
            "compositeXid": {
                "nsid": 411,
                "id": "WY-RNchvdsTSJS"
            },
            "lastAssociationTime": "1493310475047"
        }],
 
        "regions": [{
            "regionId": "10",
            "lastAssociationTime": "1493310475047"
        }]
    }],
    "unprocessedXids": ["cb0665db616f49758713252d8a335c1e"],
    "unprocessedNids": [{
        "nsid": 411,
        "id": "WY-RNgAAArI4rGBo"
    }]
}
```

Where `lastAssociationTime` represents the timestamp when the input identity was last associated with this this identity and a `region` is a tuple of `regionId` and `lastAssociationTime` for where the identity was seen.

<!-- CORE-13607 - Do we document the SDK here, too? Or link to documentation...? -->

<!-- CORE-13607 - I need more information if I should include documentation on Cluster Changelog...? -->
