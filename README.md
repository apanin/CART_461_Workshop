# CART_461_Workshop

## workshop 1 :: particle setup

## workshop 2 :: publish and subscribe

This workshop was dedicated to posting and receiving data taken in by sensors.
This was done in a manner that can read more easily.

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
