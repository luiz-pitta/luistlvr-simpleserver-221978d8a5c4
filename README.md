# README #

A SDDL Server example that communicates with the Mobile Hub Application. The Mobile Hub is a mobile middleware that allows Things (right now only BLE devices e.g. [SensorTag](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide)) communicate with the Internet (e.g. SDDL Middleware), it can pre-process sensor data that arrive from the Things with the use of CEP and thus, send the complex events to the cloud in order to be further processed. The Server example can send/receive messages to/from the Mobile Hubs, where some special kind of messages can deploy/remove CEP rules to/from the Mobile Hubs. **The Mobile Hub apk (requires Android >=4.3 and Bluetooth Low Energy) and gateway jar can be found under resources.**

### What is this repository for? ###

* Quick summary
* How do I get set up? 
* Types of Messages

### How do I get set up? ###

* Start the gateway with the public IP, port and DDS product (e.g. $ gateway 127.0.0.1 5500 OpenSplice)
* Instantiate the processing node (run the CoreServer application)
* Start the Mobile Hub with the server IP
* Deploy some rules and connect some Things

### Types of Messages ###

1. To the Mobile Hub ('|' indicates the possible options for the attribute)

The label is the identifier of the CEP rule or the event. The target can be defined when a rule is add, if it is defined as local then an event type is generated automatically for the output event with the same label as the rule that generates it. The events that the local rule generates are send again to the CEP engine in order to be source for a different rule. When the rule is defined as global, the event is send to the cloud.

~~~~~~~~~~~~~~~~~~~~~
{
 "MEPAQuery": {
  "type":"add|remove|start|stop|clear|get",
  "label":"AVGTemp",
  "object":"rule|event",
  "rule":"SELECT avg(sensorValue[1]) as value FROM 
  	  SensorData(sensorName='Temperature')
  	  .win:time_batch(10 sec)",
  "target":"local|global"
 }
}
~~~~~~~~~~~~~~~~~~~~~

2. From the Mobile Hub ('|' indicates the possible options for the attribute)

~~~~~~~~~~~~~~~~~~~~~
{
  "tag": "LocationData", 
  "uuid": "b06de58d-6a20-44f9-8cd4-83f074c2edd6", 
  "latitude": -22.98137128,
  "longitude": -43.23421961,
  "battery": 50,
  "charging": false,
  "timestamp": 1442169467 
}

{
  "tag": "SensorData", 
  "uuid": "b06de58d-6a20-44f9-8cd4-83f074c2edd6", 
  "source": "00000000-0000-0000-0001-bc6a29aecef5", 
  "action": "found|connected|disconnected|read", 
  "signal": -48,
  "sensor_name": "Temperature",
  "sensor_value": [21.70,23.42],
  "latitude": -22.98137128,
  "longitude": -43.23421961,
  "timestamp": 1442169467 
}

{
  "tag": "EventData", 
  "uuid": "b06de58d-6a20-44f9-8cd4-83f074c2edd6", 
  "label": "AVGTemp",
  "data": {"value": 22.30},
  "latitude": -22.98137128,
  "longitude": -43.23421961,
  "timestamp": 1442169467 
}

{
  "tag": "ErrorData|ReplyData", 
  "uuid": "b06de58d-6a20-44f9-8cd4-83f074c2edd6", 
  "component": "MEPAService|S2PAService",
  "message": "Action not supported.",
  "latitude": -22.98137128,
  "longitude": -43.23421961,
  "timestamp": 1442169467 
}
~~~~~~~~~~~~~~~~~~~~~

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact