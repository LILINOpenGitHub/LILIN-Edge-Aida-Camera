# LILIN-Edge-Aida-Yolo-Camera
LILIN edge Aida camera running Yolo is able to perform object classification, color detection, [behavior detection](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/behaviorID/behaviorID.json), and [number plate recognition](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/countryCode/numberplateCountry.json).  This document explains how to obtain above recognition results.  The SDK is able to set a detection zone and tripwire via HTTPAPI.

![image](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/images/cameras.jpg)

# Tracking AI PTZ demo

![image](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/images/r7084-me5kh.gif)
# Overview
This document, HTTPAPI, specifies the HTTP-based application-programming interface (API) for AI/Deep Learning of Merit LILIN Aida cameras.  Application developers can use this document to develop applications for Aida deep learning camera.  

# HTTP status returned codes
The built-in Web server uses the standard HTTP status codes.  The syntax of returned HTTP status is as following format:

![image](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/images/edgeai5.jpg)

HTTP code and text meanings are described as the followings:
| HTTP Code	  | HTTP Text	| Description | 
| ------  | ------ | --- | 
| 200		  |  OK		| The request has succeeded. | 
| 204		  | No Content	| Server has received the request but there is no information returned, and the client should stay in the same document view. This is mainly to allow inputting scripts without changing the document at the same time. | 
| 400		  | Bad Request	| The request had bad syntax or was inherently impossible to be satisfied.   | 
| 401		  | nauthorized	| The parameter to this message gives a specification of authorization schemes that are acceptable. The client should retry the request with a suitable Authorization header. <BR><BR> | 
| 403		  | Forbidden	| The request is for an action that is forbidden. <BR><BR> | 
| 404		  | Not Found	| The server has not found anything matching the given URL. <BR> | 

## Default Port Number
The default camera’s HTTP port number is at http://192.168.0.200:80.  Use LILIN [IPScan tool](https://www.meritlilin.com/index.php/en/support/file/type/6) can find the camera via Windows PC.  The default HTTP communication port number is 8592.

# HTTP API      
### License keys

### Set license key
```
Syntax:
http://<serverIP:8592>/setconfig?ch=about_box&<unlocking key>
```
Example: 
Get license key

http://192.168.0.200:8592/getconfig?ch=about_box&unlocking_key=all

![image](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/images/edgeai3.jpg)
	
### Query Aida server status
Get Aida engine status including system ID, version, and unlocking key.

```
Syntax:
http://<serverIP:8592>/server
```

Return:
DeviceName=GYNet 
Language=English 
LicenseType= Home Security, LPR (L)-Taiwan, 
LicenseStatus=Licensed(17 days left) 
UnlockingKey=s7cHLj54kGRJlhDALKaR5xyIXfR4ddDYLxBkEPRmfe9Hsswj+LIpiGLqKiJPeBwlsFnGs5pISVjD6aGs041tRnC+dieyVdw0SoyH7DNAVfs= 
--boundary END

Parameter
| Parameter | Description | Note |
| --- | --- | --- |
| LicenseType 	| 1: No license |  			|
| 		| 2: Traffic detection  		| GYNet_Traffic_Label.names |
| 		| 3: Poker pattern recognition 		| (reserved) |
| 		| 4: Human: Mask detection 		| GYNet_Human_Label.names |
| 		| 5: LED digits recognition 		| (reserved) |
| 		| 6: Sport shoes detection  		| (reserved) |
| 		| 7: Pose:  Human detection 		| (reserved) |
|		| 8: ANPR AI Number plate recognition | |
| 		| 9: Car logos | GYNet_LOG_Label.names	|
| LicenseStatus	| 1: Invalid license 			| 	|
| 		| 2: Licensed				| 	|
| 		| 3: License mismatch the system ID 	| 	|
| 		| 4: License not initialized 		| 	|
| 		| 5: License expired			| 	|

# Set and get configurations for Aida engine 
The database of the camera, config.json, can be set and get via HTTP CGI file.  See the CGIs below:
In general, there are number plate recognition and 4 alarm detection zones for AI.

### Confidence rate

Parameters:
| Parameter 	| Value  	| Description 	|
|--- 		| ---  		|---  		|
| min_characters | 4 		|		|
| max_characters | 10 		|		|
| confidence 	| 0~99 		| Confident rate of ANPR engine |
| confidence2 	| 0~99 		| Confident rate of human & car engine |
| confidence3 	| 0~99 		| Confident rate of logo engine |
| confidence4 	| 0~99 		| Confident rate of human, mask, helmet engine |
| enable_lpr_db | Yes/No 	| Enable LPR database and snapshots |

### Alarm detection zones
For alarm detection zone, the view setting is at 889 x 500.  The detection zone is based on the tab_view_size setting 889x500 and number of zones are in active is in  count_zone.

![image](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/images/edgeai2.jpg)
	
### Get configurations for Aida engine 
```
Syntax: 
http://<serverIP:8592>/getconfig?ch=all
```

```
Syntax: 
http://<serverIP:8592>/getconfig?ch=<ch_id>&detection_zone=<zone ID>
```
Example:
http://192.168.0.200:8592/getconfig?ch=1&detection_zone=0&trigger_events=0

Example:
http://192.168.0.200:8592/getconfig?ch=1&detection_zone=0

```
Syntax:
http://<serverIP:8592>/getconfig?ch=<ch_id>&detection_zone=all
```

```
Syntax:
/getconfig?detection_zone=all
```
The setting of detection_zone

| Parameter	 	| Value  	| Description			 |
| --- 			| ---	 	| --- 				| 
| enable_direction1	|Yes / No	| Enable directional detection |
| enable_direction2	| 		| Reserve for future use 	|
| direction1		|1, 2, 3, 4	| 				| 
| direction2		| 		| Reserve for future use 	|
| enable_tripwire	|Yes / No	 	| 				| 
| enable_traffic_light	| 		| Reserve for future use 	|
| enable_social_distance | 		| 	Reserve for future use |
| set_distance		| 		| Reserve for future use |
| world_distance_unit	| 		| 	Reserve for future use |
| metadata1		| 		| 	Car, Truck for detection purpose |
| Metadata2		|		| 	Reserve for future use |
| no_parking_time 	| 		| 		 |
| queuing_count		| 		| 	Reserve for future use |
| distance_violation_count | 		| 	Reserve for future use |
| detection_time	 | 		|  				| 	
| link_to_counter	 	| 		| 				 | 

###  Set configurations for Aida engine 
```
Syntax: 
http://<serverIP:8592>/setconfig?ch=all
```
```
Syntax: 
http://<serverIP:8592>/setconfig?ch=<ch_id>&detection_zone=<zone ID>
```
Example:
http://192.168.0.200:8592/setconfig?ch=1&detection_zone=0&trigger_events=1&checked=1
```
Syntax: 
http://<serverIP:8592>/setconfig?ch=<ch_id>&detection_zone=all
```
Example:
http://192.168.0.200:8592/setconfig?ch=1&detection_zone=0&enable_direction1=No

/setconfig?detection_zone&zone=1&x1=290&y1=100&x2=290&y2=250&x3=290&y3=400&x4=581&y4=400&x5=581&y5=250&x6=581&y6=100&
zone=2&x1=290&y1=100&x2=290&y2=250&x3=290&y3=400&x4=581&y4=400&x5=581&y5=250&x6=581&y6=100&
zone=3&x1=290&y1=100&x2=290&y2=250&x3=290&y3=400&x4=581&y4=400&x5=581&y5=250&x6=581&y6=100&
zone=4&x1=290&y1=100&x2=290&y2=250&x3=290&y3=400&x4=581&y4=400&x5=581&y5=250&x6=581&y6=100

# Cold Zones
###  Get cold object zones
```
Syntax:
http://<serverIP:8592>/getconfig?coldobjects=all
```
Example:
http://192.168.0.200:8592/getconfig?coldobjects=all

```
{
	"res_height":1080,
	"res_width":1920,
	"cold_objects":[
		{"X":0,"Y":0,"W":0,"H":0,"Object":"car","Index":1},
		{"X":0,"Y":0,"W":0,"H":0,"Object":"people","Index":2},
		{"X":0,"Y":0,"W":0,"H":0,"Object":" car ","Index":3},
		{"X":0,"Y":0,"W":0,"H":0,"Object":" car ","Index":4},
		{"X":0,"Y":0,"W":0,"H":0,"Object":" car ","Index":5},
		{"X":0,"Y":0,"W":0,"H":0,"Object":" car ","Index":6},
		{"X":0,"Y":0,"W":0,"H":0,"Object":" car ","Index":7},
		{"X":0,"Y":0,"W":0,"H":0,"Object":" car ","Index":8}
	]
}
```

| Parameter	| Value  | 	Description| 
| ---		| --- 	| --- | 
| res_height	| 	| 	| 
| res_width	| 	| 	| 

###  Set cold object areas 
If you experience false detection of an object, you are able to decrease the recognition rate of the object at the specific area.  We name it as cold object areas.  There are up to 8 objects can be stored in the configuration for suppress the recognition of the object.
```
Syntax:
http://<serverIP:8592>/setconfig?coldobjects&x=1900&y=4&w=2&h=2&Object=car
```
Example:
http://192.168.0.200:8592/setconfig?coldobjects&x=3&y=3&w=2&h=2&Object=bicycle

### Tripwire
For setting about the tripwire, please set “enable_direction1” and “direction1”.
 
| Parameter	 	| Value 	| 	Description | 
| ---  |  ---  |  ---  |  
| enable_direction1  	| 		| 	Enable directional detection | 
| enable_direction2 	| 	 	| 	Reserve for future use | 
| direction1		| 1, 2, 3, 4  	| 	 		| 
| direction2	 	| 	 	| Reserve for future use | 
| enable_tripwire 	| 		| Reserve for future use | 

###  Save and reload configurations for Aida engine (GYNet.exe) 
Once the configurations are set, call the reload CGI to make the settings active.
```
Syntax: 
http://<serverIP:8592>/getconfig?reload=1
```
### Factory default
Set default settings of a camera including event setting, detection zone setting and HTTP post.
```
Syntax: 
http://<serverIP:8592>/system?default=1
```
Example: 
Manufacture default without license key

http://192.168.0.200:8592/system?default=1

Example: 
Manufacture default including license key 

http://192.168.0.200:8592/system?default=all

#  Get recognition results
Get run-time recognition results.  Both HTTP based and websocket are supported.
```
Syntax: 
http://<serverIP:8592>/getalarmmotion
```

```
Syntax: 
ws://<serverIP:8592>/getalarmmotion
```
	
```
--myboundary
\r\n
Content-Length: <CLength>
\r\n
Content-Type: text/html; charset=utf-8
\r\n
\r\n
CamTime: <TimeStamp>\r\n
{
"AiEngine":<JSON>
}
--myboundary
\r\n
Content-Length: <CLength>
\r\n
Content-Type: text/html; charset=utf-8
\r\n
\r\n
CamTime: <TimeStamp>\r\n
{
"AiEngine":<JSON>
}
```

| Parameter	| Value  | Description | 
| --- |  --- |  --- | 
| CLength	| Integer| HTTP content length| 
| TimeStamp	| String | Date & time info, eg. 2019-09-27 00:53:48| 

Example: 
```
{"AiEngine":[{"id":0,"channel_id":1,"camera_name":"","res_height":1080,"res_width":1920,"confidence":99,"engine_type":256,"label_name":"L._Plate_TWN",
"class_id":0,"obj_type":0,"obj_tracking_id":1401,"obj_dwell_time":0,"color_id":9,"color":"","linked_plate":"White","x":1407,"y":174,"w":226,"h":141,
"parent_idx":-1,"properties":{"plate":"RAT6232","country":"TWN","area":"","area_id":0,"logo":""},"detection_zone_id":0,"behavior_id":0},{"id":1,"channel_id":1,"camera_name":"","res_height":1080,"res_width":1920,"confidence":99,"engine_type":256,"label_name":"L._Plate_TWN",
"class_id":0,"obj_type":0,"obj_tracking_id":64,"obj_dwell_time":1129,"color_id":9,"color":"","linked_plate":"White","x":595,"y":305,"w":217,"h":128,"parent_idx":-1,"properties":{"plate":"9197MB","country":"TWN","area":"","area_id":0,"logo":""},
"detection_zone_id":0,"behavior_id":0}],
"counter_count":[0,0,0,0,0,0,0,0],
"something_vanish_in_zone1":"No","something_vanish_in_zone2":"No","something_vanish_in_zone3":"No","something_vanish_in_zone4":"No",
	
"detection_zone":[
{"x1":354,"y1":60,"x2":203,"y2":570,"x3":639,"y3":751,"x4":1241,"y4":954,"x5":1235,"y5":540,"x6":1470,"y6":84},
{"x1":606,"y1":216,"x2":606,"y2":540,"x3":606,"y3":864,"x4":1235,"y4":864,"x5":1235,"y5":540,"x6":1235,"y6":216},
{"x1":606,"y1":216,"x2":606,"y2":540,"x3":606,"y3":864,"x4":1235,"y4":864,"x5":1235,"y5":540,"x6":1235,"y6":216},
{"x1":606,"y1":216,"x2":606,"y2":540,"x3":606,"y3":864,"x4":1235,"y4":864,"x5":1235,"y5":540,"x6":1235,"y6":216}],
"Count":15}

###  Meta data of Aida engine
LILIN defines meta data as JSON profile which contains:

Profile: Object Name, Color, Behavior, [Speed], [Number Plate], [Count]

To describe a behavior of an object, it could explain the following situation:

1. Three people in red color, blue color, and white color turned left in the paste.  In short, they are:

Person #1, red, turned left, 
Person #2, blue, turned left,
Person #3, white, turned left

2. A Car in blue color parked there with number plate ABC123 over 3 minutes.  In short, they are:

Car, blue color, parked, plate ABC123, 3 minutes

LILIN meta data is determined by JSON protocol for describing a behavior of an object and explained below:

![image](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/images/edgeai4.jpg)
	
#  [Number plates recognition](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/countryCode/numberplateCountry.json) and logo recognition results by Aida engine
```
--myboundary
Content-Type: text/plain
Content-Length: 971
CamTime: 2021-09-17 13:11:02 ms:546

{"AiEngine":[{"id":0,"channel_id":1,"camera_name":"","res_height":1080,"res_width":1920,"confidence":74,"engine_type":1,
"label_name":"car","class_id":2,
"obj_type":0,
"obj_tracking_id":206,
"obj_dwell_time":2135,
"color_id":3,"color":"Yellow",
"linked_plate":"NCJ4338","x":255,"y":473,"w":1130,"h":594,"parent_idx":-1,
"detection_zone_id":0,
"behavior_id":0},

{"id":1,"channel_id":1,"camera_name":"","res_height":1080,"res_width":1920,"confidence":89,"engine_type":256,
"label_name":"L._Plate_TWN","class_id":0,
"obj_type":0,
"obj_tracking_id":1898,"obj_dwell_time":2,
"color_id":1,"color":"Red",
"linked_plate":"NCJ4338","x":742,"y":190,"w":208,"h":83,"parent_idx":-1,
"properties":{"plate":"NCJ4338","country":"TWN","area":"California","area_id":0,"logo":"Toyota"},
"detection_zone_id":0,"behavior_id":0}],"counter_count":[0,0,0,0,0,0,0,0],
"something_vanish_in_zone1":"No","something_vanish_in_zone2":"No","something_vanish_in_zone3":"No","something_vanish_in_zone4":"No","Count":9}
--myboundary
```
#  Traffic object detection results by Aida engine
{"AiEngine"
	 [
		{
			"id": 1,
			"confidence": 94,
			“engine_type”: 1,
			“class_id”: 10,
			“obj_tracking_id”: 237
			“dwell_time”: 23
			“obj_type”: 0,
			“label_name": “truck”,
			“color_id”: “8”,
			“color_text”: “black”,
			"x": 5,
			"y": 3,
			"w": 351,
			"h": 293,
			“parent_idx”:-1
		},
		{
			"id": 2,
			"confidence": 97,
			“engine_type”: 1,
			“class_id”: 19,
			“obj_tracking_id”: 238
			“dwell_time”: 23
			“obj_type”: 0,
			“label_name": “human”,
			“color_id”: “8”,
			“color_text”: “black”,
			"x": 20,
			"y": 387,
			"w": 455,
			"h": 129,
			“parent_idx”:-1
		},
		{
			"id": 3,
			"confidence": 49,
			“engine_type”: 1,
			“class_id”: 17,
			“zone_id”: 1
			“obj_tracking_id”: 239
			“obj_type”: 0,
			“label_name": “trailer”,
			“behavior_id”: “0x00000001”,
			“behavior”: “Parking”,
			“color_id”: “8”,
			“color_text”: “black”,
			"x": 476,
			"y": 391,
			"w": 343,
			"h": 105,
			“parent_idx”:-1
		},
	],
	“Count”:6
}

#  Behavior detection results by Aida engine
```
{
	"AiEngineBeh":
	 [
		{
			"id": 3,
			"confidence": 49,
			“engine_type”: 1,
			“class_id”: 17,
			“obj_tracking_id”: 239
			“obj_type”: 0,
			“label_name": “trailer”,
			“behavior_id”: “0x00000001”,
			“behavior”: “Parking”,
			“color_id”: “8”,
			“color_text”: “black”,
			"x": 476,
			"y": 391,
			"w": 343,
			"h": 105,
			“parent_idx”:-1
		},
		{
			"id": 4,
			"confidence": 42,
			“engine_type”: 1,
			“class_id”: 18,
			“obj_tracking_id”: 239
			“obj_type”: 0,
			“label_name": “human”,
			"x": 570,
			"y": 347
			"w": 135,
			"h": 42,
			“parent_idx”:-1
		},
		{
			"id": 5,
			"confidence": 82,
			“engine_type”: 1,
			“class_id”: 0,
			“obj_tracking_id”: 240
			“obj_type”: 0,
			“label_name": “person”,
			"x": 579,
			"y": 298,
			"w": 381,
			"h": 202,
			“parent_idx”:-1
		},			
	],
	“Count”:6
}
```
	
Parameter	Value (integer)	Description
confidence
engine_type                                         class_id
obj_tracking_id
dwell_time
obj_type                                          label_name
behavior_id
behavior
color_id
color_text	

Confidence of the recognition
Engine type
The object ID
The object tracking ID
The stay time of the object

The name of the object
The ID of the behavior
The name of the behavior
The color ID
The color text


# Color detection ID
| color_id | Color text | 
| ---  | --- | 
| 1  | Red | 
| 2 | Orange | 
| 3 | Yellow | 
| 4 | Green  | 
| 5 | Blue | 
| 6 | Indigo | 
| 7 | Purple | 
| 8 | Black | 
| 9 | White | 
| 10 | Silver | 

# [Behavior detection ID](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/behaviorID/behaviorID.json)
| behavior_id | Hex | behavior_name | 
| --- | --- | --- | 
| 1  | 0x0000000 |  Zone violation for car or pedestrian  | 	 
| 2  | 0x00000002 | Parking spot detection  | 
| 4  | 0x00000004 | Parking violation  | 	 
| 8 | 0x00000008 | Traffic flow  | 	 
| 16  | 0x00000010 | Turn left  | 	 
| 128  | 0x00000080 | Turn left violation  | 	 
| 32  | 0x00000020 | Turn right  | 	 
| 256  | 0x00000100 | Turn right violation  | 	 
| 512  | 0x00000200 | U-turn violations  | 	 
| 64  | 0x00000040 | Wrong way  | 	 
| 1677216  | 0x10000000 | Queuing  | 	 
| Reserved |  | Speeding   | 
| 1024 | 0x00000400 | Run the red light (Straight)  | 	
| 2048 | 0x00000800 | Run the red light (left)	  | 
| 4096 | 0x00001000 | Run the red light (right)  | 	
| 65536 | 0x00010000 | No mask detection  | 
| Reserved |   | Loitering detection | 	 

Parameters of Counting:
behavior_id	behavior_name	Description
CountCar		
CountHuman		

## Mask detection results by Aida engine
```
{"AiEngine":
[
	{
		"id":0,
		"confidence":45,
		"engine_type":4,
		"label_name":"mask",
		"class_id":82,
		"obj_type":0,
		"obj_tracking_id":90,
		"obj_dwell_time":8,
		"color_id":3,
		"color":"Yellow",
		"behavior_id":65536,
		"x":229,"y":310,"w":109,"h":188,
		"parent_idx":-1,
		"world_distance":[]
	},
	{
		"id":1,
		"confidence":98,
		"engine_type":4,
		"label_name":"face",
		"class_id":80,
		"obj_type":0,
		"obj_tracking_id":92,
		"obj_dwell_time":1,
		"color_id":3,
		"color":"Yellow",
		"behavior_id":0,
		"x":534,"y":453,"w":229,"h":245,
		"parent_idx":-1,
		"world_distance":[]
	},
	{
		"id":2,
		"confidence":99,
		"engine_type":4,
		"label_name":"mask",
		"class_id":82,"obj_type":0,
		"obj_tracking_id":89,
		"obj_dwell_time":9,
		"color_id":10,
		"color":"Silver",
		"behavior_id":65536,"x":635,"y":135,"w":115,"h":136,
		"parent_idx":-1,"world_distance":[[90,5.26]]
	},
	{
		"id":3,
		"confidence":47,
		"engine_type":4,
		"label_name":"half_mask",
		"class_id":81,
		"obj_type":0,
		"obj_tracking_id":66,
		"obj_dwell_time":47,
		"color_id":3,
		"color":"Yellow",
		"behavior_id":0,
		"x":1040,"y":196,"w":187,"h":168,
		"parent_idx":-1,
		"world_distance":[]
	},
	{
		"id":4,
		"confidence":98,
		"engine_type":4,
		"label_name":"mask",
		"class_id":82,"obj_type":0,
		"obj_tracking_id":91,
		"obj_dwell_time":5,
		"color_id":4,"color":"Green",
		"behavior_id":65536,
		"x":1460,"y":214,"w":93,"h":72,
		"parent_idx":-1,
		"world_distance":[[90,7.16],[89,3.58]]
	},
	{
		"id":5,
		"confidence":97,
		"engine_type":4,
		"label_name":"face",
		"class_id":80,
		"obj_type":0,
		"obj_tracking_id":84,
		"obj_dwell_time":16,
		"color_id":3,"color":"Yellow",
		"behavior_id":0,
		"x":1598,"y":402,"w":163,"h":175,
		"parent_idx":-1,
		"world_distance":[]
	},
	{
		"id":6,
		"confidence":100,
		"engine_type":8388608,
		"label_name":"counter count","class_id":0,
		"obj_type":7,"obj_tracking_id":0,"obj_dwell_time":0,
		"color_id":0,
		"color":"",
		"behavior_id":0,
		"x":0,"y":0,"w":0,"h":0,
		"parent_idx":-1,
		"world_distance":[],
		"properties":
		{"
		counter_count":
			 [
			0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
			]
		}
	}
],
	"Count":7
}
```
Parameters of Mask:
| class_id	|  Description 	|	 
| ---  		|  -----  	|  
| 80   		| Face 		| 
| 81  		| Half_mask 	| 
| 82  		| Mask   	| 


# HTTP post
HTTP Post can be configured via Aida software for behavior HTTP Post notification.  It can consist of 
Profile: Behavior, [Speed], Color, Object, [Number Plate], [dwell], [Count]

![image](https://github.com/LILINOpenGitHub/LILIN-Edge-Aida-Camera/blob/main/images/edgeai1.jpg)

The tokens in Aida software can be:

| Token | Description |
| ----- | ---------- |
| <%name%> | Name of the object |
| <%confidence%> | Confidence of the object |
| <%left_x%> | Bonding box of X |
| <%top_y%> | Bonding box of Y |
| <%width%> | Bonding box of width |
| <%height%> | Bonding box of height |
| <%center_x%> | Bonding box center X |
| <%center_y%> | Bonding box center Y |
| <%behavior_id%> | Behavior ID |
| <%color%> | Color of the object |
| <%linked_plate%> | Plate in the object |
| <%plate%> | Number plate |
| <%country%> | SA |
| <%logo%> | Car make |
| <%area%> | California, state name |
| <%full_image%> | Picture of the behavior |
| <%crop_image%> | Picture of the bounding box |
| <%pip_image%> | Picture of the behavior with bonding box |
| <%counter_count%> | Counting |

```
Example (Pedestrian flow)
[
  {
    "id": "pedestrianFlown",
    "lat": 22.647610, 
    "lon": 22.647610,
    "save": true,
    "value": ["<%counter_count%>"]
    }
]

Example (HTTP Post with picture attached)
Content-Disposition: form-data; name="meta"
{
  "id": "vehicle",
  "lat": 22.661238,
  "lon": 120.303066,
  "value": [
    "<%color%>"
  ]
}

Content-Disposition: form-data; name="img"
Content-Type: image/jpeg

<%full_image%>
```
# JPEG snapshot
For getting camera snapshot, use 192.168.0.200:8592/snap for getting the camera snapshot.
