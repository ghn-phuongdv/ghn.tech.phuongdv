# Git
- URL: https://g.ghn.vn/datbq/front-end-packaging
- Install: config nginx, deploy

# Format repo gitlab: 
- |__ conf: Configuration host, database, urls, 
- |__ dist: Contains executable file, libraries
- |__ dist/lib: Library requirements
- |__ src: The source code

# Library requirements: 
- bson-3.4.2.jar
- gson-2.2.2.jar
- java.common.api-lib.jar
- java.mongodb-lib.jar
- log4j-1.2.17.jar 
- mongo-client-mapper.jar
- mongodb-driver-3.4.2.jar
- mongodb-driver-core-3.4.2.jar
- NLib.jar
- PermissionService.jar

# Server infomation

- **Staging**

Doamin: [api.staging.insidev2.ghn.vn](api.staging.insidev2.ghn.vn)

Server address: 45.118.151.34

Port: 5054

Database: Mongodb

Database name: INSIDE_PACKAGE

Database port: 27018

- Config nginx
```nginx
upstream staging_packaging_service {
   server 127.0.0.1:5054 fail_timeout=0;
}
server {
	listen 80;
	server_name api.staging.insidev2.ghn.vn;
	client_max_body_size 10M;		
	
	location /packaging {	
		proxy_pass http://staging_packaging_service;
		proxy_set_header Host $http_host;
		add_header Access-Control-Allow-Origin *;
		add_header Access-Control-Allow-Methods "GET, POST, PUT, DELETE, OPTIONS";
		add_header Access-Control-Allow-Headers "X-Auth";
	}
}
```

- **Production**

Commitsoon

# Deployment
Run script to deploy 

./runserver start || restart || stop

# General Information
Base-URL for our API is http://api.staging.insidev2.ghn.vn

All requests to the API shall be HTTP/1.1 GET

Please make sure to use the API with http only.

**Most requests require 1 or 2:**

1) The requests require a **X-APIKEY** & **X-APISECRET**, you can find both in the User Model at the fields (ssoId,secret).

2) The requests require a **X-Auth**, you can find and use it at [https://g.ghn.vn/phuongdv/account-login-service](https://g.ghn.vn/phuongdv/account-login-service).

Response is json, structure is as follows:
```json
{"status":"<status-code>","message":"<informational message. might vary, use the status code in your code!>","result": "<result of the request. varies depending on the request>"}
```
### status
- **OK**: Everything is OK. Request succeeded
- **NOT_FOUND**: File not found
- **INVALID**: The parameter is incorrect
- **ERROR**: Server errors. You should not see this, but be prepared.

### message
This message gives more detailed information in case there is an error.
You can use this for displaying it to the user, but please don't use it for checking if the request succeeded. That's what the status code is for.

### data
Holds the response of the request if succeeded. Might hold an array of data or just a boolean true/false, depending on the request

# API support:
- [Add a shipping order](#add-a-shipping-order)
- [Add many shippings order](#add-many-shippings-order)
- [Add transition session](#add-transition-session)
- [Update transition session](#update-transition-session)
- [Update many transition session, using for update transition session status](#update-many-transition-session-using-for-update-transition-session-status)
- [Delete transition session](#delete-transition-session)
- [Get a transition session](#get-a-transition-session)
- [Get list transition session](#get-list-transition-session)

## Add a shipping order:
- Request
```url
/**
 * URL
 * Method: POST
 */
http://api.staging.insidev2.ghn.vn/packaging/shipping-order
```

- Header parameter
```
X-Auth: {{X-Auth}}
```

- Body parameters
```json
	{		
		"OrderCode":"2F5VAXZ2",
		"CurrentWarehouseID":1047,
		"DeliveryWarehouseID":1233,
		"OrderDate":"2017-06-05 14:55:53.000",
		"ShippingAddress":"23 Nguyễn Chí Thanh, phường 16, Quận 11, Hồ Chí Minh, Việt Nam",
		"ExpectedDeliveryTime":"2017-06-06 19:00:00.000",
		"FromDistrictID":1442,
		"ToDistrictID":1453,
		"CurrentStatus":"Storing",
		"PickWarehouseID":1047,
		"ReturnHubId":0,
		"ReturnWarehouseId":0
	}
```

- Response
Content-Type: application/json
```json
	{
		"status":"OK",
		"message":"Successfull.", 
		"data": []
	}
```

## Add many shippings order:
- Request
```url
/**
 * URL
 * Method: POST
 */
http://api.staging.insidev2.ghn.vn/packaging/shipping-order/multi
```

- Header parameter
```
X-Auth: {{X-Auth}}
```

- Body parameters
```json
	{	
		"ShippingOrders":[
			{				
				"OrderCode":"2F5VAXZ2",
				"CurrentWarehouseID":1047,
				"DeliveryWarehouseID":1233,
				"OrderDate":"2017-06-05 14:55:53.000",
				"ShippingAddress":"23 Nguyễn Chí Thanh, phường 16, Quận 11, Hồ Chí Minh, Việt Nam",
				"ExpectedDeliveryTime":"2017-06-06 19:00:00.000","FromDistrictID":1442,
				"ToDistrictID":1453,
				"CurrentStatus":"Storing",
				"PickWarehouseID":1047,
				"ReturnHubId":0,
				"ReturnWarehouseId":0
			},
			{				
				"OrderCode":"2FBX393Q",
				"CurrentWarehouseID":1220,
				"DeliveryWarehouseID":1233,
				"OrderDate":"2017-06-05 16:49:00.000",
				"ShippingAddress":"70 Lữ Gia, Hồ Chí Minh, Việt Nam",
				"ExpectedDeliveryTime":"2017-06-23 05:00:00.000",
				"FromDistrictID":1453,
				"ToDistrictID":1453,
				"CurrentStatus":"Storing",
				"PickWarehouseID":1220,
				"ReturnHubId":0,
				"ReturnWarehouseId":0
			}
		]
	}	
```

- Response
Content-Type: application/json
```json
	{
		"status":"OK",
		"message":"Successfull.", 
		"data": []
	}
```

## Add transition session
- Request
```url
	/**
	 * URL
	 * Method: POST
	 */
	http://api.staging.insidev2.ghn.vn/packaging/transition-session
```

- Header parameter
```
X-Auth: {{X-Auth}}
```

- Body parameters
```json
	{		
		"CurrentWarehouseID": 1295,
		"ToWarehouseID": 1220,
		"FromWarehouseID": 1060,
		"PackageType": "Thùng lớn 1",
		"Weight": 90,
		"Height": 80,
		"Width": 60,
		"Length": 60,
		"ErrorList": [
			{
				"OrderCode": "2FPR3L94",
				"Description": "Không thuộc chuyến luân chuyển"
			}
		],
		"PackList": [
			"2F043FQA"
		],
		"Note": "Test",
		"ExpectedDate": "2017-7-12"
	}
```

- Response
Content-Type: application/json
```json
	{
		"status":"OK",
		"message":"Successfull.", 
		"data": []
	}
```

## Update transition session
- Request
```url
	/**
	 * URL
	 * Method: PUT
	 */
	http://api.staging.insidev2.ghn.vn/packaging/transition-session/{{ID}}
```

- Header parameter
```
X-Auth: {{X-Auth}}
```

- Body parameters
```json
	{		
		"CurrentWarehouseID": 1295,
		"ToWarehouseID": 1220,
		"FromWarehouseID": 1060,
		"PackageType": "Thùng lớn 1",
		"Weight": 90,
		"Height": 80,
		"Width": 60,
		"Length": 60,
		"ErrorList": [
			{
				"OrderCode": "2FPR3L94",
				"Description": "Không thuộc chuyến luân chuyển"
			},
			{
				"OrderCode": "2FB3RA57",
				"Description": "Không ở trong trạng thái luân chuyển"
			}
		],
		"ExpectedDate": "2017-7-12",
		"StartDate": "2017-7-12",
		"EndDate": "2017-7-14",		
		"Status": "FINISHED"
	}
```

- Response
Content-Type: application/json
```json
	{
		"status":"OK",
		"message":"Successfull.", 
		"data": []
	}
```

## Update many transition session, using for update transition session status
- Request
```url
	/**
	 * URL
	 * Method: POST
	 */
	http://api.staging.insidev2.ghn.vn/packaging/transition-session/multi
```

- Header parameter
```
X-Auth: {{X-Auth}}
```

- Body parameters
```json
	{
		"TransitionSessions": [
			{		
				"PackageCode": "QEARUJW7I9C",	
				"Status": "RECIEVED",
				"StateDate": "yyyy-mm-dd",
				"EndDate": "yyyy-mm-dd"
			},
			{		
				"PackageCode": "K2FWTNSDF4B",	
				"Status": "RECIEVED",
				"StateDate": "yyyy-mm-dd",
				"EndDate": "yyyy-mm-dd"
			},
			{		
				"PackageCode": "ZJCLGTL6GCG",	
				"Status": "RECIEVED",
				"StateDate": "yyyy-mm-dd",
				"EndDate": "yyyy-mm-dd"
			}
		]
	}	
```

- Response
Content-Type: application/json
```json
	{
		"status":"OK",
		"message":"Successfull.", 
		"data": []
	}
```

## Delete transition session
- Request
```url
	/**
	 * URL
	 * Method: DELETE
	 * Just delete the transition session has status is `DRAFTS`
	 */
	http://api.staging.insidev2.ghn.vn/packaging/transition-session/{{ID}}
```

- Header parameter
```
X-Auth: {{X-Auth}}
```

- Body parameters: none

- Response
Content-Type: application/json
```json
	{
		"status":"OK",
		"message":"Successfull.", 
		"data": []
	}
```

## Get a transition session
- Request
```url
	/**
	 * URL
	 * Method: GET
	 * Parameters is id or packageCode
	 */
	http://api.staging.insidev2.ghn.vn/packaging/transition-session/{{ID}}
```

- Header parameter
```
X-Auth: {{X-Auth}}
```

- Body parameters: none

- Response
Content-Type: application/json
```json
	{
		"status":"OK",
		"message":"Successfull.", 
		"data": []
	}
```

## Get list transition session
- Request
```url
	/**
	 * URL
	 * Method: GET	 
	 */
	http://api.staging.insidev2.ghn.vn/packaging/transition-session
```

- Header parameter
```
X-Auth: {{X-Auth}}
```

- Parameters:

offset = 0 | default is 0

limit = 20 | default is 20

reverse = true/false | default is false

q = {"PackageCode": "ZJCLGTL6GCG","CurrentWarehouseID": 1295,"ToWarehouseID": 1220,"FromWarehouseID": 1060,"PackageType": "Thùng lớn 1","Weight": 0,"Height": 80,"Width": 60,"Length": 60,"Status": "DRAFTS","ExpectedDate": "2017-7-12","StartDate": "2017-7-12","EndDate": "2017-7-14",}

| Name                   | Description                                                      |
| ---------------------- | ---------------------------------------------------------------- |
| PackageCode            | The barcode of a package                                         |
| CurrentWarehouseID     | Current warehouse id                                             |
| FromWarehouseID        | From warehouse id                                                |
| ToWarehouseID          | To warehouse id                                                  |
| PackageType            | Package type                                                     |
| Weight                 |                                                                  |
| Height                 |                                                                  |
| Width                  |                                                                  |
| Length                 |                                                                  |
| Status                 | Package status, support is (DRAFTS,DELIVERING,RECIEVED,FINISHING,FINISHED) |
| ExpectedDate           | Expected date fortmat yyyy-mm-dd                                 |
| StartDate              | Start date fortmat yyyy-mm-dd                                    |
| EndDate                | End date fortmat yyyy-mm-dd                                      |

- Body parameters: none

- Response
Content-Type: application/json
```json
	{
		"status":"OK",
		"total": 1,
		"message": "Query TransitionSession successfully." 
		"data": [{
            "PackageCode": "AQGCGTASLSH",
            "CurrentWarehouseID": 1295,
            "ToWarehouseID": 1220,
            "FromWarehouseID": 1060,
            "PackageType": "Thùng lớn 1",
            "Weight": 0,
            "Height": 80,
            "Width": 60,
            "Length": 60,
            "PackList": [
                "2FP53Y37"
            ]
            "Note": "Test",
            "ExpectedTime": "Jun 12, 2017 12:00:00 AM",
            "CreatedByID": 210737,
            "Status": "DRAFTS",
            "orderNumber": 14,
            "id": "598ca22de7ed3f02e77c727d",
            "createdTime": "Aug 11, 2017 1:13:01 AM",
            "lastUpdatedTime": "Aug 11, 2017 1:13:01 AM"
        }]
	}
```
