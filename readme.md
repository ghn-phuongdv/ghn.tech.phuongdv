# Git
- URL: https://g.ghn.vn/inside-system/inside-api-packaging
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

2) The requests require a **X-Auth**, you can find and use it at [https://g.ghn.vn/inside-system/inside-api-account](https://g.ghn.vn/inside-system/inside-api-account).

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
		"orderCode":"2F5VAXZ2",
		"currentWarehouseId":1047,
		"deliveryWarehouseId":1233,
		"weight": 56,
		"orderDate":"2017-06-05 14:55:53.000",
		"shippingAddress":"23 Nguyễn Chí Thanh, phường 16, Quận 11, Hồ Chí Minh, Việt Nam",
		"expectedDeliveryTime":"2017-06-06 19:00:00.000",
		"fromDistrictId":1442,
		"toDistrictId":1453,
		"currentStatus":"Storing",
		"pickWarehouseId":1047,
		"returnHubId":0,
		"returnWarehouseId":0
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
				"orderCode":"3X7ALXZ2",
				"currentWarehouseId":1047,
				"deliveryWarehouseId":1233,
				"weight": 56,
				"orderDate":"2017-06-05 14:55:53.000",
				"shippingAddress":"23 Nguyễn Chí Thanh, phường 16, Quận 11, Hồ Chí Minh, Việt Nam",
				"expectedDeliveryTime":"2017-06-06 19:00:00.000",
				"fromDistrictId":1442,
				"toDistrictId":1453,
				"currentStatus":"Storing",
				"pickWarehouseId":1047,
				"returnHubId":0,
				"returnWarehouseId":0
			},
			{
				"orderCode":"6C7ALXZ2",
				"currentWarehouseId":1047,
				"deliveryWarehouseId":1233,
				"weight": 56,
				"orderDate":"2017-06-05 14:55:53.000",
				"shippingAddress":"23 Nguyễn Chí Thanh, phường 16, Quận 11, Hồ Chí Minh, Việt Nam",
				"expectedDeliveryTime":"2017-06-06 19:00:00.000",
				"fromDistrictId":1442,
				"toDistrictId":1453,
				"currentStatus":"Storing",
				"pickWarehouseId":1047,
				"returnHubId":0,
				"returnWarehouseId":0
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
		"packageCode": "MAOBG12AWCO",
		"currentWarehouseId": 1295,
		"toWarehouseId": 1060,
		"fromWarehouseId": 1295,
		"packageType": "Thùng lớn 1",
		"weight": 0,
		"height": 80,
		"width": 60,
		"length": 60,
		"packList": [
			"2FBAA47K",
			"2FB9RLX7"
		],
		"errorList": [
			{
				"orderCode": "2FB2VA9N",
				"description": "Không thuộc chuyến luân chuyển"
			},
			{
				"orderCode": "2F51VA9I",
				"description": "Không thuộc chuyến luân chuyển"
			},
			{
				"orderCode": "2FBAA47K",
				"description": "Không thuộc chuyến luân chuyển"
			}
		],
		"status": "DRAFTS"
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
		"packageCode": "MAOBG12AWCO",
		"currentWarehouseId": 1295,
		"toWarehouseId": 1060,
		"fromWarehouseId": 1295,
		"packageType": "Thùng lớn 1",
		"weight": 0,
		"height": 80,
		"width": 60,
		"length": 60,
		"packList": [
			"2FBAA47K",
			"2FB9RLX7"
		],
		"unpackList": [
			"2FBAA47K",
			"2FB9RLX7"
		],
		"errorList": [
			{
				"orderCode": "2FB2VA9N",
				"description": "Không thuộc chuyến luân chuyển"
			},
			{
				"orderCode": "2F51VA9I",
				"description": "Không thuộc chuyến luân chuyển"
			},
			{
				"orderCode": "2FBAA47K",
				"description": "Không thuộc chuyến luân chuyển"
			}
		],
		"note": "Test",
		"status": "FINISHED"
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
				"packageCode": "QEARUJW7I9C",	
				"status": "RECIEVED",
				"startTime": "dd/MM/yyyy HH:mm a",
				"expectedTime": "dd/MM/yyyy HH:mm a"
			},
			{		
				"packageCode": "K2FWTNSDF4B",	
				"status": "RECIEVED",
				"startTime": "dd/MM/yyyy HH:mm a",
				"expectedTime": "dd/MM/yyyy HH:mm a"
			},
			{		
				"packageCode": "ZJCLGTL6GCG",	
				"status": "RECIEVED",
				"startTime": "dd/MM/yyyy HH:mm a",
				"expectedTime": "dd/MM/yyyy HH:mm a"
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

q = {"packageCode": "ZJCLGTL6GCG","currentWarehouseId": 1295,"toWarehouseId": 1220,"fromWarehouseId": 1060,"packageType": "Thùng lớn 1","weight": 0,"height": 80,"width": 60,"length": 60,"status": "DRAFTS","expectedDate": "2017-7-12","startDate": "2017-7-12","endDate": "2017-7-14","createdDate": "2017-7-14"}

| Name                   | Description                                                                |
| ---------------------- | ---------------------------------------------------------------------------|
| packageCode            | The barcode of a package                                                   |
| currentWarehouseId     | Current warehouse id                                                       |
| fromWarehouseId        | From warehouse id                                                          |
| toWarehouseId          | To warehouse id                                                            |
| packageType            | Package type                                                               |
| weight                 |                                                                            |
| height                 |                                                                            |
| width                  |                                                                            |
| length                 |                                                                            |
| status                 | Package status, support is (DRAFTS,DELIVERING,RECIEVED,FINISHING,FINISHED) |
| expectedDate           | Expected date fortmat yyyy-mm-dd                                           |
| startDate              | Start date fortmat yyyy-mm-dd                                              |
| endDate                | End date fortmat yyyy-mm-dd                                                |
| createdDate            | Created date fortmat yyyy-mm-dd                                            |

- Body parameters: none

- Response
Content-Type: application/json
```json
	{
		"status": "OK",
		"data": [
			{
				"packageCode": "MAOBG12AWCO",
				"currentWarehouseId": 1295,
				"toWarehouseId": 1060,
				"fromWarehouseId": 1295,
				"packageType": "Thùng lớn 1",
				"weight": 0,
				"height": 80,
				"width": 60,
				"length": 60,
				"packList": [
					"2FBAA47K",
					"2FB9RLX7"
				],
				"unpackList": [
					"2FBAA47K",
					"2FB9RLX7"
				],
				"errorList": [
					{
						"orderCode": "2FB2VA9N",
						"description": "Không thuộc chuyến luân chuyển",
						"createdById": "210737",
						"created": "Sep 13, 2017 3:36:00 PM",
						"updatedById": "209937",
						"updated": "Sep 18, 2017 10:00:19 AM"
					},
					{
						"orderCode": "2F51VA9I",
						"description": "Không thuộc chuyến luân chuyển",
						"createdById": "210737",
						"created": "Sep 13, 2017 3:36:00 PM",
						"updatedById": "209937",
						"updated": "Sep 18, 2017 10:00:19 AM"
					},
					{
						"orderCode": "2FBAA47K",
						"description": "Không thuộc chuyến luân chuyển",
						"createdById": "209937",
						"created": "Sep 18, 2017 10:00:19 AM",
						"updatedById": "209937",
						"updated": "Sep 18, 2017 10:00:19 AM"
					}
				],
				"note": "Test",
				"startTime": "Sep 15, 2017 10:19:00 AM",
				"endTime": "Sep 18, 2017 10:00:19 AM",
				"history": [
					{
						"createdById": "123123",
						"created": "Sep 15, 2017 10:19:01 AM",
						"action": "DELIVERING"
					},
					{
						"createdById": "209937",
						"created": "Sep 18, 2017 9:58:33 AM",
						"action": "RECIEVED"
					},
					{
						"createdById": "209937",
						"created": "Sep 18, 2017 9:59:06 AM",
						"action": "FINISHING",
						"unpackList": [
							"2FBAA47K"
						]
					},
					{
						"createdById": "209937",
						"created": "Sep 18, 2017 10:00:19 AM",
						"action": "FINISHED",
						"unpackList": [
							"2FB9RLX7"
						]
					}
				],
				"createdById": "210737",
				"status": "FINISHED",
				"date": "Sep 13, 2017 3:36:00 PM",
				"id": "59b8edf0e5b075d5dc936eab",
				"createdTime": "Sep 13, 2017 3:36:00 PM",
				"lastUpdatedTime": "Sep 18, 2017 10:00:19 AM"
			}]
	}
```
