BulkData User document(Beta)
===
# Summary
IDX BulkData provides a convenient, programmable resource for users who want to access, store, and manage MLS data on their own. You can use the BulkData API to access over <a href="https://idxbroker.com/idx_mls_coverage" target="_blank">600+ MLS’</a> data with multiple usable endpoints. The data has been sanitized to follow industry standard, so users can be confident they are getting consistent data even across different MLS systems. The data is updated hourly, but it is also possible to do full and/or incremental updates of the resources as needed.

# API Endpoints (Beta)

| Endpoints                      | Description                                                  |
| :----------------------------- | ------------------------------------------------------------ |
| [Token](#Token endpoint)       | This endpoint is used to obtain an **AccessToken**, which is required for downloading listings, images, agents, and offices. This token can also be used to check MLS permission and is different than **x-api-key**. This token will **expire hourly**. |
| [MLSInfo](#MLS info endpoint)  | This endpoint is used to obtain overall MLS feed info including property count, property statuses, property types, property sub-types, agent count, office count, and last updated. |
| [Listings](#Listings endpoint) | This endpoint provides data for individual listings, such as acres, square feet, price, bedrooms, and bathrooms. |
| [Images](#Images endpoint)     | This endpoint provides image data such as high resolution url, thumbnail URL, and captions. |
| [Agents](#Agents endpoint)     | This endpoint provides agent data such as agent name, agent ID, office ID, email, and phone number. |
| [Offices](#Offices endpoint)   | This endpoint provides office data such as office name, office ID, and phone number. |
## Token endpoint

https://data-services.idxbroker.com/beta/live/token 

The Token endpoint requires a **POST** request as well as **username** and **password** in the request body, and **x-api-key** in the header. This will return an **AccessToken**, which is required for the mlsinfo and property resource endpoints. The AccessToken is set to expire every hour and must be replaced in order to continue using mlsinfo and property resource endpoints.
### Request Headers
| Parameter | Required |
| --------- | :------: |
| **x-api-key** |    ✅    |
### Request Body parameters
| Parameter | Required |
| --------- | :------: |
| **username** |    ✅    |
| **password** |    ✅    |

### Response
```json
{
    "accessToken": "eyJraWQiOiJ1cGlT....."
}
```

## MLS info endpoint

https://data-services.idxbroker.com/beta/live/mlsinfo

The Mlsinfo endpoint requires **AccessToken** and **x-api-key**  in the request headers. The mlsinfo provides an overview of MLS data, such as listing count, agent count, office count, and last updated date.

### Request Headers
| Parameter | Required |
| --------- | :------- |
|**AccessToken**|    ✅    |
|**x-api-key**  |    ✅    |
### Request URL parameters
| Parameter | Required | Addtional|
| --------- | :------: | -------- |
|idxID|❌|Allow  one or multiple<br />e.g. idxID=a001 or idxID=a001,a002|
|limit|❌|Only allow one and maximum is 10.<br />e.g. limit=5 |
|offset|❌|e.g. offset=20|
### Response
```json
{
    "a001": {
        "idxID": "a000",
        "listingCount": 999,
        "propTypes": [ 
            {
                "mlsPtID": 1,
                "mlsName": "RES",
                "subTypes": {
                    "active": [
                        "Single Family Residence",
                        "..."
                    ],
                    "sold": [], // Optional
                }
            }
        ],
        "propStatuses": [  
            "Active",
            "..."
        ],
        "agentCount": 999,
        "officeCount": 999,
        "lastUpdated": "2017-10-03T22:49:23.000Z"
    }
}
```
## Property Resource endpoints

All property resource endpoints (**listings**, **images**, **agents**, and **offices**) require an **AccessToken** and **x-api-key** in the request headers. 

### Listings endpoint

https://data-services.idxbroker.com/beta/live/listings

#### Request Headers

| Parameter       | Required |
| --------------- | -------- |
| **AccessToken** | ✅        |
| **x-api-key**   | ✅        |

#### Request URL parameters

| Parameter  | Required | Addtional                                                    |
| :--------- | :------- | ------------------------------------------------------------ |
| **idxID**  | ✅        | Only allow one<br /> e.g. [https://data-services.idxbroker.com/beta/live/listings?idxID=a001](https://data-services.idxbroker.com/beta/live/listings?idxID=a001) |
| listingID  | ❌        | Allow one or muliple listingID<br />e.g. listingID=x0000001 or listingID=x0000001,x0000002 |
| propStatus | ❌        | Allow one or muliple propStatus<br />e.g. propStatus=active or propStatus=active,pending |
| mlsPtID    | ❌        | Allow one or muliple mlsPtID<br />e.g. mlsPtID=1 or mlsPtID=1,2 |
| updated    | ❌        | (**gt**, **gte**, **lt**, **lte**):YYYY-MM-DDThh:mm:SS<br />Allow one or muliple updated<br />e.g. updated=gt:2017-01-01 or updated=gt:2017-01-01,lt:2017-02-01 |
| limit      | ❌        | Only allow one and maximum is 500.<br />e.g. limit=499       |
| offset     | ❌        | e.g. offset=1000                                             |
#### Response
```json
{
    "total": 999,
    "first": "...",
    "last": "...",
    "next": "...",
    "previous": "...",
    "data": [
        {
          	"docType": "listing",
            "idxID": "a000",
            "mlsPtID": 1,
            "listingID": "33672",
            "metaFields": {
                
            },
            "coreFields": {
                "acres": 0.08,
                "address": {
                    // ...
                },
                "bedrooms": 3,
                "cityName": "Hillsboro",
                "countyName": "Washington",
                "fullBaths": 1,
                "listingPrice": 450000,
                "partialBaths": 0,
                "status": "Active"
                "type": "Residential"
            },
            "advancedFields": {
                // ...
            },
            "mutatedFields": {
                // ...
            },
            "lastUpdated": "2018-09-17T11:00:00+00:00" 
        },
        ...
    ]
}
```

### Images endpoint

https://data-services.idxbroker.com/beta/live/images

#### Request Headers

| Parameter       | Required |
| --------------- | -------- |
| **AccessToken** | ✅        |
| **x-api-key**   | ✅        |

#### Request URL parameters

| Parameter     | Required | Addtional|
| ------------- | -------- | ---------|
| **idxID**    | ✅        | Only allow one, e.g. idxID=a001|
| listingID | ❌        | Allow one or muliple listingID<br />e.g. listingID=x0000001ororlistingID=x0000001,x0000002 |
| updated   | ❌        | (**gt, gte, lt, lte**):YYYY-MM-DDThh:mm:SS<br />Allow one or muliple updated<br />e.g. updated=gt:2017-01-01 or updated=gt:2017-01-01,lt:2017-02-01 |
| limit     | ❌        | Only allow one and maximum is 500.e.g.limit=499|
| offset    | ❌        | e.g. offset=1000 |

#### Response

```json
{
    "total": 999,
    "first": "...",
    "last": "...",
    "next": "...",
    "previous": "...",
    "data": {
        "33672": { // listingID
            "1": { // priority
                "caption": "Main View",
                "hires": {
                    "url": "https://..."
                }
            }
        }
     }
}
```

Images data uses listing ids and priorties as keys.

### Agents endpoint

https://data-services.idxbroker.com/beta/live/agents

#### Request Headers

| Parameter   | Required |
| ----------- | -------- |
| AccessToken | ✅        |
| x-api-key   | ✅        |

#### Request URL parameters

| Parameter | Required | Addtional                                                    |
| :-------- | :------- | ------------------------------------------------------------ |
| **idxID** | ✅        | Only allow one, e.g. idxID=a001                              |
| agentID   | ❌        | Allow one or muliple agentID<br />e.g. agentID=V211511918 or agentID=V211511918,V211512197 |
| officeID  | ❌        | Allow one or muliple officeID<br />e.g. officeID=V3600 or officeID=V3600,V6241 |
| updated   | ❌        | (**gt**, **gte**, **lt**, **lte**):YYYY-MM-DDThh:mm:SS<br />Allow one or muliple updated<br />e.g. updated=gt:2017-01-01 or updated=gt:2017-01-01,lt:2017-02-01 |
| limit     | ❌        | Only allow one and maximum is 500.<br />e.g. limit=499       |
| offset    | ❌        | e.g. offset=1000                                             |

#### Response

```json
{
    "total": 999,
    "first": "...",
    "last": "...",
    "next": "...",
    "previous": "...",
    "data": [
        {
            "idxID": "a000",
            "agentID": "123456",
            "officeID": "654321",
            "name": "Demo Developer",
            "email": "developers@idxbroker.com",
            "primaryPhone": "800-421-9668"
            "lastUpdated": "2018-9-17T11:00:00+00:00"
        },
        ...
    ]
}
```

### Offices endpoint

https://data-services.idxbroker.com/beta/live/offices

#### Request Headers

| Parameter   | Required |
| ----------- | -------- |
| AccessToken | ✅        |
| x-api-key   | ✅        |

#### Request URL parameters

| Parameter | Required | Addtional|
| :-------- | :------- | ---------|
| idxID     |✅| Only allow one, e.g. idxID=a001|
| officeID  | ❌| Allow one or muliple officeID<br />e.g. officeID=V3600 or officeID=V3600,V6241 |
| updated|❌| (**gt**, **gte**, **lt**, **lte**):YYYY-MM-DDThh:mm:SS<br />Allow one or muliple updated<br />e.g. updated=gt:2017-01-01 or updated=gt:2017-01-01,lt:2017-02-01 |
| limit|❌| Only allow one and maximum is 500.<br />e.g. limit=499 |
| offset| ❌| e.g. offset=1000 |

#### Response

```json
{
    "total": 999,
    "first": "...",
    "last": "...",
    "next": "...",
    "previous": "...",
    "data": [
        {
            "idxID": "a001",
            "officeID": "654321",
            "email": "developers@idxbroker.com",
            "primaryPhone": "800-421-9668"            
            "name": "IDX Broker",
            "lastUpdated": "2018-9-17T11:00:00+00:00"
        },
        ...
    ]
}
```

### Status Codes

### 200 OK

Good request.

### 401 Unauthorized

Missing **AccessToken** or Invalid **AccessToken**

### 403 Forbidden

The HTTP resource may not be supported (URL may be incorrect).

### 410 Gone

The MLS has undergone migration. If there is an equivalent MLS, the response will return the new idxid. If there is no equivalent MLS, the idxid will be null and you should contact <developers@idxbroker.com>.

### 422 Unprocessable entity
The query value is invalid. The response will include the specific value.

### 500 Internal Server Error

The backend service is down. Please contact <developers@idxbroker.com>.

## Recommendation

We recommend beginning with a full update, and then running incremental updates every hour rather than hourly full updates.

# Resources


## Postman

The BulkData API <a href="https://github.com/idxbroker/bulkdata" target="_blank">postman collection</a> is an easy way to look at data.

## Starter
- <a href="https://github.com/idxbroker/bulkdata-php-starter" target="_blank">bulkdata-php-starter</a>

## Changelog

#### 2020-08-17

- Support `mlsPtID` query parameter on listings endpoint
- Remove` propType` query parameter on listings endpoint
#### 2019-06-17

- Change image endpoint limit from 500 to 100.

