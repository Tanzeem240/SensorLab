# SensorLab

COMP3074 - Mobile App Development I | Lab: Reading Android Device Sensors
Kotlin + Jetpack Compose

## Description
An Android app that reads two device sensors and shows the live values in a Jetpack Compose screen:
- **Ambient temperature** (`Sensor.TYPE_AMBIENT_TEMPERATURE`), shown in °C
- **Gyroscope** (`Sensor.TYPE_GYROSCOPE`), shown as X, Y and Z angular velocity in rad/s

The app checks whether each sensor exists with `getDefaultSensor()`. If a sensor is missing, it shows an "unavailable" message and does not crash.

## How it works
- `MainActivity` implements `SensorEventListener`.
- Sensor listeners are registered in `onResume()` and unregistered in `onPause()`.
- Sensor values are stored in `mutableStateOf`, so Compose updates the UI whenever a new `SensorEvent` arrives.
- The UI is split into `SensorScreen`, `TemperatureCard` and `GyroscopeCard`.

## Test device results
- **Test device:** Samsung s24 
- **Gyroscope:** available. X, Y and Z values stayed near 0 while the phone was still and changed when the phone was rotated.
- **Ambient temperature sensor:** not available on this device. The app displayed "Temperature sensor is not available." and did not crash. Battery temperature was not used as a substitute.

## Screenshot
![SensorLab running on a physical device](screenshots/sensorlab.png)

## Independent practice
[Describe the extension you added, e.g. "Added a Reset button that sets the gyroscope values back to 0 until the next sensor event."]

## Reflection questions
1. **Why can `getDefaultSensor()` return null?** Android devices are not required to include every sensor, so it returns null when the hardware is missing.
2. **Why unregister listeners in `onPause()`?** It stops unnecessary sensor callbacks, saves battery, and avoids listening while the app is not in the foreground.
3. **Angular velocity vs. orientation:** the gyroscope measures how fast the device is rotating, while orientation is the angle or position the device is currently in.
4. **Units:** radians per second (rad/s).
5. **Why not battery temperature?** It measures the heat of the battery, which is affected by charging and CPU load, not the temperature of the surrounding air.
6. **How does `mutableStateOf` update the UI?** Compose tracks which composables read that state. When `onSensorChanged` assigns a new value, Compose recomposes those composables with the new data.