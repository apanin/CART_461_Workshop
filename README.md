# CART_461_Workshop

## workshop 1 :: particle setup

The first workshop was in regards to setting up the particle workbench as well as connecting the particle argon to the cloud. I initially had issues connecting the argon to my wifi network. Due to signal interference, I could only connect to the cloud through my phones hotspot if my router was turned off (I am assuming that the routers signal was causing interference, which was not in regards to the networks frequency, because it was a 2.4ghz). This issue was resolved a few weeks later when I changed my router for other reasons. 

Once I could follow the workshop, everything ran as it was during the workshop. They did.

<img src="https://github.com/apanin/CART_461_Workshop/blob/main/assets/workshop12.gif" width="80%" height="80%"> <br/>

One thing I did notice is that the code would not run when connected to the battery, even if the argon would connect to the cloud. Meaning if the code is flashed locally, it did not seem to executing if the connection was not made with the computer. I have no explanation to this.

<img src="https://github.com/apanin/CART_461_Workshop/blob/main/assets/workshop11.gif" width="80%" height="80%"> <br/>

### blink equivalent

In order to test all the functionalities of the particle workbench, I tested the compile and flash command to make sure that everything worked as expected.

<img src="https://github.com/apanin/CART_461_Workshop/blob/main/assets/workshop12.JPG" width="80%" height="80%"> <br/>


## workshop 2 :: publish and subscribe

This workshop was dedicated to posting and receiving data taken in by sensors as well a geolocation parameters from the google maps api.
The focus of the workshop was formatting data in a manner that can be read and received more easily. Afterwards, we had a look on webhook response and value extraction on a computational level.

From experience, creating particle webhooks can be a finicky process for syntax reasons (curly bracket hell), its not always clear how things work exactly in regards to the webhook integrations because the data formatting is specific. 

### first try
My initial test was simply using the google maps api for the device locator code with a push button since I did not feel the need to make a webhook without a specific sensor need. Considering our project was to be using the internal code to count steps and evaluate sensor reads, we actually only needed an internal counter variable, and data was not to be processed in any particular manner (we would simply be sending a time stamp when the tile is triggered by pressure sensor reads exceeding the threshold).

<img src="https://github.com/apanin/CART_461_Workshop/blob/main/assets/workshop2.gif" width="80%" height="80%"> <br/>

### json formatting
I tend to agree that the json formatting is tedious, particularly in a context where we are using arduino c. The json writer is a good tool to format data if many parameters are sent to the data base, it makes things more clear and easy to read, I think this could be a good reference for future projects.

I have had headaches formatting such things for computer science assignments
```c++
/* ADDITIONALLY, WE WANT TO PUBLISH DATA TO A SERVER NOT WITHIN THE PARTICLE CLOUD PLATFORM */
/* HERE WE CAN PREPARE THE DATA TO TRANSMIT VIA A WEBHOOK (HTTPS SERVER) USING A STRING FORMATTED AS JSON OBJECT */
snprintf(buffer, sizeof(buffer),"{\"device-name\":\"%s\",\"lat\":%.07f,\"lon\":%.07f,\"accu\":%.07f,\"accel-x\":%.02f,\"accel-y\":%.02f,\
\"accel-z\":%.02f,\"magnetom-x\":%.02f,\"magnetom-y\":%.02f,\"magnetom-z\":%.02f,\"gyro-x\":%.02f,\"gyro-y\":%.02f,\
\"gyro-z\":%.02f,\"yaw\":%.02f,\"pitch\":%.02f,\"roll\":%.02f,\"magnitude\":%.02f}",\
"cslab-proline",lat,lon,accuracy,accel[0],accel[1],accel[2],magnetom[0],magnetom[1],magnetom[2],gyro[0],gyro[1],gyro[2],\
yaw,pitch,roll,magnitude );
/* ^^^ NASTY AND ERROR PRONE ^^^ */
```

This is a nice tool:

```c++
JsonWriterStatic<512> jw;
{
JsonWriterAutoObject obj(&jw);
jw.insertKeyValue("device-name", "cslab-proline");
jw.insertKeyValue("lat", lat);
jw.insertKeyValue("lon", lon);
jw.insertKeyValue("accu", accu);
jw.insertKeyValue("accel-x", accel[0]);
jw.insertKeyValue("accel-y", accel[1]);
jw.insertKeyValue("accel-z", accel[2]);
jw.insertKeyValue("magnetom-x", magnetom[0]);
jw.insertKeyValue("magnetom-y", magnetom[1]);
jw.insertKeyValue("magnetom-z", magnetom[2]);
jw.insertKeyValue("gyro-x", gyro[0]);
jw.insertKeyValue("gyro-y", gyro[1]);
jw.insertKeyValue("gyro-z", gyro[3]);
jw.insertKeyValue("yaw", yaw);
jw.insertKeyValue("pitch", pitch);
jw.insertKeyValue("roll", roll);
jw.insertKeyValue("magnitude", magnitude);
}

Serial.println(jw.getBuffer());
```


In regards to what was done in this workshop, I created a webhook that would post our data to a firebase db using a push button instead of piezo sensors (I was trying to test the data sending functionalities rather than the sensors)

<img src="https://github.com/apanin/CART_461_Workshop/blob/main/assets/workshop2.JPG" width="80%" height="80%"> <br/>

```json 
{
"event": "DoresDreamPostPosition",
"url": "https://dores-dream-default-rtdb.firebaseio.com/position.json",
"requestType": "PUT",
"query": {
"auth":"yXLGOyAvAHqHYpJ6JIMUt2K3V1epe9uYAeGWarF5"
},
{
"ts": "{{PARTICLE_PUBLISHED_AT}}",
"coreid": "{{{PARTICLE_DEVICE_ID}}}",
"stepCount": "{{stepCount}}",
"hour": "{{hour}}",
"minutes": "{{minutes}}",
"day": "{{day}}",
"month": "{{month}}",
"year": "{{year}}"
}
,
"mydevices": true,
"noDefaults": true
}
```

and used this function to send the data

```c++
void publishData() {
char buf[256];
snprintf(buf, sizeof(buf), "{\"stepCount\":%d,\"hour\":%d,\"minutes\":%d,\"day\":%d,\"month\":%d,\"year\":\%d}", stepCount, Time.hour(), Time.minute(), Time.day(), Time.month(), Time.year());
Serial.printlnf("publishing %s", buf);
Particle.publish(PUBLISH_EVENT_NAME, buf, PRIVATE);
}
```

data was successfully sent to the database

<img src="https://github.com/apanin/CART_461_Workshop/blob/main/assets/firebase.png" width="80%" height="80%"> <br/>


## workshop 3 :: 

The third workshop discussed core functionalities embedded within particles which permit device optimization in regards to battery life as well as time management.
In this workshop I mainly listened because I had not considered most of the topics addressed.

## STOP vs ULTRA_LOW_POWER vs hibernate

<img src="https://github.com/apanin/CART_461_Workshop/blob/main/assets/sleepmode.png" width="80%" height="80%"> <br/>

It was interesting to consider some of these options for our project because the particle does not need to be running on the network in between data sends. Therefore I was looking into the differences between these modes to consider which would work best in our case. Considering tiles would be spread throughout the park, it could be that some tiles are left untouched for hours (possibly). With the use of gpio wake mode configuration for pins, we could make it so that tiles hibernate between reads and only send data through packets of information received within a fifteen minute interval of the initial contact.

Furthermore this would require a timer object because you would want to keep track of the fifteen minute timeframe.


## TCP vs UDP

Neither of these protocols seem of interest for data capture we are not using web protocol, but rather firebase webhooks as seen in the previous workshop.

TCP is connection based / peer to peer
needs : ip address
socket number
3 way handshake
safer (the packets will arrive effectively in the order they are transmitted)
higher latency

```c++
unsigned int outPort = 8000;
unsigned int inPort = 8001;
```

UDP is connectionless
best effort protocol
does not verify that information will reach its destination
faster

These protocols are useful in the case:
-where you would be sending data to another software in realtime (such as max msp)
-peer to peer communication (between argons)

this can also permit communication between particles without necessarily being connected on the same local network.



