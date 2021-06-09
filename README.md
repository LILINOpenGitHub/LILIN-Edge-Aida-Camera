# LILIN-Edge-Aida-Camera

## Overview
This document, HTTPAPI, specifies the HTTP-based application-programming interface (API) for AI/Deep Learning of Merit LILIN Aida cameras.  Application developers can use this document to develop applications for Aida deep learning camera.  



## Default Port Number
The default HTTP communication port number is 8592.

## HTTP API      
### License keys

### Set license key

http://192.168.0.200:8592/setconfig?ch=about_box&<unlocking key>

Example: Get license key

http://192.168.0.200:8592/getconfig?ch=about_box&unlocking key=all

Chapter 3.2.  Query Aida serer status
Get Aida engine status including system ID, version, and unlocking key.

Syntax:

http://<serverIP:8592>/server

Return:
DeviceName=GYNet 
Language=English 
LicenseType= Home Security, LPR (L)-Taiwan, 
LicenseStatus=Licensed(17 days left) 
UnlockingKey=s7cHLj54kGRJlhDALKaR5xyIXfR4ddDYLxBkEPRmfe9Hsswj+LIpiGLqKiJPeBwlsFnGs5pISVjD6aGs041tRnC+dieyVdw0SoyH7DNAVfs= 
--boundary END

Parameter
Parameter	Type	Description	Note
LicenseType		1: No license
2: Traffic detection
GYNet_Traffic_Label.names
3: Poker pattern recognition 
(reserved)
4: Human: Mask detection
GYNet_Human_Label.names
5: LED digits recognition 
(reserved)
6: Sport shoes detection  
(reserved)
7: Pose:  Human detection 
(reserved)
8: ANPR AI Number plate recognition
9: Car logos 
GYNet_LOG_Label.names	
LicenseStatus		1: Invalid license
2: Licensed
3: License mismatch the system ID
4: License not initialized
5: License expired	

### Set and get configurations for Aida engine 
The database of the camera, config.json, can be set and get via HTTP CGI file.  See the CGIs below:
In general, there are number plate recognition and 4 alarm detection zones for AI.

Number plate recognition setting:

Parameters:
Parameter	Value (integer)	Description
min_characters	4	
max_characters	10	
confidence		Number plate recognition
confidence2		Object detection
confidence3		Reserve for future use
confidence4		Reserve for future use
enable_lpr_db		Enable LPR database and snapshots

Alarm detection zones
For alarm detection zone, the view setting is at 889 x 500.  The detection zone is based on the tab_view_size setting 889x500 and number of zones are in active is in  count_zone.

 

### Get configurations for Aida engine 

Syntax: 
http://<serverIP:8592>/getconfig?ch=all

http://<serverIP:8592>/getconfig?ch=<ch_id>&detection_zone=<zone ID>

Example:

http://192.168.0.200:8592/getconfig?ch=1&detection_zone=0&trigger_events=0

Example:

http://192.168.0.200:8592/getconfig?ch=1&detection_zone=0

http://<serverIP:8592>/getconfig?ch=<ch_id>&detection_zone=all

/getconfig?detection_zone=all

The setting of detection_zone
Parameters:
Parameter	Value (integer)	Description
enable_direction1		Enable directional detection
enable_direction2		Reserve for future use
direction1		
direction2		Reserve for future use
enable_tripwire		
enable_traffic_light		Reserve for future use
enable_social_distance		Reserve for future use
set_distance		Reserve for future use
world_distance_unit		Reserve for future use
metadata1		Car, Truck for detection purpose
Metadata2		Reserve for future use
no_parking_time		
queuing_count		Reserve for future use
distance_violation_count		Reserve for future use
detection_time		
link_to_counter		

###  Set configurations for Aida engine 

Syntax: 
http://<serverIP:8592>/setconfig?ch=all

http://<serverIP:8592>/setconfig?ch=<ch_id>&detection_zone=<zone ID>

Example:

http://192.168.0.200:8592/setconfig?ch=1&detection_zone=0&trigger_events=1&checked=1

http://<serverIP:8592>/setconfig?ch=<ch_id>&detection_zone=all

Example:

	http://192.168.0.200:8592/setconfig?ch=1&detection_zone=0&enable_direction1=No

/setconfig?detection_zone&zone=1&x1=290&y1=100&x2=290&y2=250&x3=290&y3=400&x4=581&y4=400&x5=581&y5=250&x6=581&y6=100&zone=2&x1=290&y1=100&x2=290&y2=250&x3=290&y3=400&x4=581&y4=400&x5=581&y5=250&x6=581&y6=100&zone=3&x1=290&y1=100&x2=290&y2=250&x3=290&y3=400&x4=581&y4=400&x5=581&y5=250&x6=581&y6=100&zone=4&x1=290&y1=100&x2=290&y2=250&x3=290&y3=400&x4=581&y4=400&x5=581&y5=250&x6=581&y6=100

###  Get cold object areas 

http://<serverIP:8592>/getconfig?coldobjects=all

Example:

http://192.168.0.200:8592/getconfig?coldobjects=all

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

Parameters:
Parameter	Value (integer)	Description
res_height		
res_width		

###  Set cold object areas 
If you experience false detection of an object, you are able to decrease the recognition rate of the object at the specific area.  We name it as cold object areas.  There are up to 8 objects can be stored in the configuration for suppress the recognition of the object.

http://<serverIP:8592>/setconfig?coldobjects&x=1900&y=4&w=2&h=2&Object=car

Example:

http://192.168.0.200:8592/setconfig?coldobjects&x=3&y=3&w=2&h=2&Object=bicycle

### Tripwire
For setting about the tripwire, please set “enable_direction1” and “direction1”.
 
Parameter:
Parameter	Value (integer)	Description
enable_direction1		Enable directional detection
enable_direction2		Reserve for future use
direction1	1, 2, 3, 4	
direction2		Reserve for future use
enable_tripwire		Reserve for future use

###  Save and reload configurations for Aida engine (GYNet.exe) 
Once the configurations are set, call the reload CGI to make the settings active.

Syntax: 
http://<serverIP:8592>/getconfig?reload=1

### Factory default
Set default settings of a camera including event setting, detection zone setting and HTTP post.

Syntax: 
http://<serverIP:8592>/system?default=1

Example: Manufacture default without license key

http://192.168.0.200:8592/system?default=1

Example: Manufacture default including license key 

http://192.168.0.200:8592/system?default=all

###  Get recognition results
Get run-time recognition results.  Both HTTP based and websocket are supported.

Syntax: 
http://<serverIP:8592>/getalarmmotion

Syntax: 
ws://<serverIP:8592>/getalarmmotion


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
……

Parameters:
Parameter	Value (integer)	Description
CLength	Integer	HTTP content length
TimeStamp	String	Date & time info, eg. 2019-09-27 00:53:48

Example: 
--myboundary
\r\n
Content-Length: 1188
\r\n
Content-Type: text/html; charset=utf-8
\r\n
\r\n
CamTime: 2019-09-27 00:53:48
\r\n
{"AiEngine":[{"id":0,"confidence":99,"engine_type":256,"class_id":0,"obj_type":0,"label_name":"L._Plate_TWN","x":192,"y":209,"w":87,"h":51,"parent_idx":-1,"properties":{"plate":"ARF3250","country":"TWN","area":"","logo":""}},{"id":1,"confidence":99,"engine_type":256,"class_id":10,"obj_type":3,"label_name":"A","x":193,"y":223,"w":10,"h":25,"parent_idx":0},{"id":2,"confidence":99,"engine_type":256,"class_id":27,"obj_type":3,"label_name":"R","x":204,"y":223,"w":11,"h":25,"parent_idx":0},{"id":3,"confidence":99,"engine_type":256,"class_id":15,"obj_type":3,"label_name":"F","x":215,"y":223,"w":11,"h":24,"parent_idx":0},{"id":4,"confidence":99,"engine_type":256,"class_id":3,"obj_type":3,"label_name":"3","x":230,"y":223,"w":11,"h":26,"parent_idx":0},{"id":5,"confidence":99,"engine_type":256,"class_id":2,"obj_type":3,"label_name":"2","x":241,"y":224,"w":11,"h":25,"parent_idx":0},{"id":6,"confidence":99,"engine_type":256,"class_id":5,"obj_type":3,"label_name":"5","x":253,"y":224,"w":11,"h":26,"parent_idx":0},{"id":7,"confidence":99,"engine_type":256,"class_id":0,"obj_type":3,"label_name":"0","x":264,"y":224,"w":11,"h":26,"parent_idx":0}],"Count":8}
\r\n	

(Next  multipart boundary content)

###  Meta data of Aida engine
LILIN defines meta data as JSON profile which contains:

Profile: Object Name, Color, Behavior, [Speed], [Number Plate], [Count]

To describe a behavior of an object, it could explain the following situation:

1.	Three people in red color, blue color, and white color turned left in the paste.  In short, they are:

Person #1, red, turned left, 
Person #2, blue, turned left,
Person #3, white, turned left

2.	A Car in blue color parked there with number plate ABC123 over 3 minutes.  In short, they are:
Car, blue color, parked, plate ABC123, 3 minutes

LILIN meta data is determined by JSON protocol for describing a behavior of an object and explained below:

###  Number plates recognition results by Aida engine

{"AiEngine":
	 [
		{
			"id": 0,
			"confidence": 93,
                                             “engine_type”: 256,
                                             “class_id”: 0,
                                             “obj_type”: 0,
			“label_name": “License Plate”,
			"x": 773,
			"y": 363,
			"w": 349,
			"h": 182,
			“parent_idx”:-1,
                                             "properties":
                                             {
                                                 “plate”: “BMW5957”,
                                                 “country”: “USA”,
                                                 “area”: “California”,
			“area_id”: 43,
                                             }
		},
                              {
			"id": 1,
			"confidence": 78,
                                             “engine_type”: 256,
                                             “class_id”: 45,
                                             “obj_type”: 2,
                                             “label_name": “California”,
			"x": 848,
			"y": 391,
			"w": 60,
			"h": 86,
			“parent_idx”:0
		},
		{
			"id": 2,
			"confidence": 75,
                                             “engine_type”: 256,
                                             “class_id”: 11,
                                             “obj_type”: 3,
                                             “label_name": “B”,
			"x": 793,
			"y": 381,
			"w": 30,
			"h": 82,
			“parent_idx”:0
		},
		{
			"id": 3,
			"confidence": 54,
                                             “engine_type”: 256,
                                             “class_id”: 22,
			“obj_type”: 3,
                                             “label_name": “M”,
			"x": 702,
			"y": 313,
			"w": 28,
			"h": 78,
			“parent_idx”:0
		},
		{
			"id": 4,
			"confidence": 43,
                                             “engine_type”: 256,
                                             “class_id”: 32,
                                             “obj_type”: 3,
                                             “label_name": “W”,
			"x": 813,
			"y": 400,
			"w": 26,
			"h": 80,
			“parent_idx”:0
		},
		{
			"id": 5,
			"confidence": 78,
                                             “engine_type”: 256,
                                             “class_id”: 4,
                                             “obj_type”: 3,
                                            	“label_name": “5”,
			"x": 822,
			"y": 422,
			"w": 31,
			"h": 81,
			“parent_idx”:0
		},
		{
			"id": 6,
			"confidence": 91,
                                             “engine_type”: 256,
                                             “class_id”: 8,
                                             “obj_type”: 3,
                                             “label_name": “9”,
			"x": 831,
			"y": 430,
			"w": 33,
			"h": 76,
			“parent_idx”:0
		},
		{
			"id": 7,
			"confidence": 88,
                                             “engine_type”: 256,
                                             “class_id”: 4,
                                             “obj_type”: 3,
                                             “label_name": “5”,
			"x": 843,
			"y": 439,
			"w": 29,
			"h": 79,
			“parent_idx”:0
		},
		{
			"id": 8,
			"confidence": 77,
                                             “engine_type”: 256,
                                             “class_id”: 6,
                                             “obj_type”: 3,
                                             “label_name": “7”,
			"x": 866,
			"y": 448,
			"w": 29,
			"h": 81,
			“parent_idx”:0
		},		
	],
	“Count”:9
}

Parameters:
Parameter	Value (integer)	Description
engine_type 

0x0100

0x0200

0x0400

0x0800

0x1000

0x2000

0x4000

0x8000

0x10000

0x20000

0x40000

0x80000

0x100000

0x200000	

	

TWN: Taiwan
GYNet_LPD_Label_TWN.names
EUR: Europe
GYNet_OCR_Label_EUR.names
SEA: 
GYNet_OCR_Label_SEA.names
CNA: China
(reserved)
MEA
GYNet_OCR_Label_MEA.names
USA
GYNet_OCR_Label_USA.names
AUS: Australia
GYNet_OCR_Label_AUS.names
GBR: UK
GYNet_OCR_Label_EUR.names
IDN: Indonesia
GYNet_OCR_Label_IDN.names
JPN
GYNet_OCR_Label_JPN.names
IND: India
GYNet_OCR_Label_IND.names
THA: Thailand
Laos
GYNet_LPD_Label_LAO.names
BGD: Bangladesh
GYNet_OCR_Label_BGD.names

Parameters:
Parameter	Value (integer)	Description
obj_type 
	0
1
2
3	0: Plate: A plate is detected.
1: Logo: A logo is detected (reserved).
2: States/Countries/Provinces
3: Number: The digits of the plates

###  Traffic object detection results by Aida engine
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

###  Behavior detection results by Aida engine
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

Parameters:
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

Parameters:
Parameter	Value (integer)	Description
color_id 

1
2
3
4
5
6
7
8
9
10	

1
2
3
4
5
6
7
8
9
10	color_text

1: Red
2: Orange
3: Yellow
4: Green
5: Blue
6: Indigo
7: Purple
8: Black
9: White
10: Silver

Behaviors are defined below:
behavior_id	behavior_name	Description
1
0x00000001	Zone violation for car or pedestrian	 
2
0x00000002	Parking spot detection	 
4
0x00000004	Parking violation	 
8
0x00000008	Traffic flow	 
16
0x00000010	Turn left	 
128
0x00000080	Turn left violation	 
32
0x00000020	Turn right	 
256
0x00000100	Turn right violation	 
512
0x00000200	U-turn violations	 

64
0x00000040	Wrong way	 
1677216
0x10000000	Queuing	 
	Speeding	 
0x00000400	Run the red light (Straight)
	
0x00000800	Run the red light (left)
	
0x00001000	Run the red light (right)
	
0x00010000	No mask detection
	 

	Loitering detection	 




Parameters of Counting:
behavior_id	behavior_name	Description
CountCar		
CountHuman		

### Mask detection results by Aida engine

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

Parameters of Mask:
Parameter	Value (integer)	Description
class_id"
80
81
82
	
	
Face
Half_mask
Mask

### HTTP post
HTTP Post can be configured via Aida software for behavior HTTP Post notification.  It can consist of 
Profile: Behavior, [Speed], Color, Object, [Number Plate], [dwell], [Count]

![] (https://github.com/LILINOpenGitHub/LILIN-Aida-Camera-SDK/blob/main/edgeai1.jpg)

The tokens in Aida software can be:
Token		Description
<%name%>		Name of the object
<%confidence%>		Confidence of the object
<%left_x%>		Bonding box of X
<%top_y%>		Bonding box of Y
<%width%>		Bonding box of width
<%height%>		Bonding box of height
<%center_x%>		Bonding box center X
<%center_y%>		Bonding box center Y
<%behavior_id%>		Behavior ID
<%color%>		Color of the object
<%linked_plate%>		Plate in the object
<%plate%>		Number plate
<%country%>		USA
<%logo%>		Car make
<%area%>		California, state name
<%full_image%>		Picture of the behavior
<%crop_image%>		Picture of the bounding box
<%pip_image%>		Picture of the behavior with bonding box
<%counter_count%>		Counting

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

