# CART_461_Workshop

## workshop 1 :: particle setup

The first workshop was in regards to setting up the particle workbench as well as connecting the particle argon to the cloud. I initially had issues connecting the argon to my wifi network. Due to signal interference, I could only connect to the cloud through my phones hotspot if my router was turned off (I am assuming that the routers signal was causing interference, which was not in regards to the networks frequency, because it was a 2.4ghz). This issue was resolved a few weeks later when I changed my router for other reasons. 

![alt text](https://github.com/apanin/CART_461_Workshop/blob/main/assets/workshop12.JPG| width=100)

Once I could follow the workshop, everything ran as it was during the workshop. They did.
One thing I did notice is that the code would not run when connected to the battery, even if the argon would connect to the cloud. Meaning if the code is flashed locally, it did not seem to executing if the connection was not made with the computer. I have no explanation to this.


### blink equivalent

In order to test all the functionalities of the particle workbench, I tested the compile and flash command to make sure that everything worked as expected.

![alt text](http://url/to/img.png)

## workshop 2 :: publish and subscribe

This workshop was dedicated to posting and receiving data taken in by sensors.
This was done in a manner that can read more easily. Afterwards, we had a look on webhook response and value extraction.


### first try
My initial test was simply using the google maps api for the device locator.

```c
#include <google-maps-device-locator.h>

GoogleMapsDeviceLocator locator;

void setup() {
  Serial.begin(9600);
  // Scan for visible networks and publish to the cloud every 30 seconds
  // Pass the returned location to be handled by the locationCallback() method
  locator.withSubscribe(locationCallback).withLocatePeriodic(30);
}

void locationCallback(float lat, float lon, float accuracy) {
  // Handle the returned location data for the device. This method is passed three arguments:
  // - Latitude
  // - Longitude
  // - Accuracy of estimated location (in meters)
}

void loop() {
  locator.loop();
}
```






## workshop 3 :: 

## external reaserch
