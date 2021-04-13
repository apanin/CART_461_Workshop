# CART_461_Workshop

## workshop 1 :: particle setup

The first workshop was in regards to setting up the particle workbench as well as connecting the particle argon to the cloud. I initially had issues connecting the argon to my wifi network. Due to signal interference, I could only connect to the cloud through my phones hotspot if my router was turned off (I am assuming that the routers signal was causing interference, which was not in regards to the networks frequency, because it was a 2.4ghz). This issue was resolved a few weeks later when I changed my router for other reasons. 

Once I could follow the workshop, everything ran as it was during the workshop.


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
