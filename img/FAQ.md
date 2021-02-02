# FAQ

In this section, FAQ are presented. For some questions, the answer differs for Android solution.
<br>
<br>

## FAQ – Technical Features

### What does the PenguinIN’s SDK include?

PenguinIN shares

* **Native library file \(.aar for Android or .framework for iOS)**
* **Sample project**
* **SDK documentation**

### Which platforms/mobile phones are supported?

Penguin’s Mobile SDK supports any mobile phones that run

* **[Android v6.0 Marshmallow \(API 23)](https://en.wikipedia.org/wiki/Android_version_history#Android_6.0_Marshmallow_(API_23)) and later**
* **iOS 9.0 and later**

### What happens if a mobile phone does not support BLE?

We need Bluetooth for indoor positioning. Without Bluetooth, **positioning stops and returns a failure initialization status with a related Error Code.**

### What happens if a user does not allow location permission?

We need a location permission for the SDK to calculate the position of the device. Without location permission, **positioning stops and returns a failure initialization status with related Error Code.**

### Does SDK work in the background?

Yes. Penguin’s Mobile SDK works in the background. This useful feature can be configured. Android and iOS show different behavior when running in background. They may also show various OS limitations. For your specific use case, please discuss with PenguinIN support.
<br>
<br>

## FAQ – Data/ Internet Usage

### What happens when a user loses internet connection?

All main components of PenguinIN’s Mobile SDK, such as loading data or positioning, need a connection to the hosting server.

If the server is located on a local domain, the user must be connected to this local network and there is no need for internet connection of their device.

If the server hosts on an online domain, the SDK stops calculating the position and returns a connectivity disconnection status when the user loses internet connectivity.

### Which data is collected?

SDK uploads several types of data to the PenguinIN Cloud. This data includes:

* **Device position \(coordinates and timestamp)**
* **SDK Analytics information**
* **Crash logging \(Advance Crash analytics feature)**

### Which data does the SDK store on the device?

The Mobile SDK uses device storage to improve performance and keep the communication over the network interface to a minimum. The stored data

includes:

* **Venues main data**
* **Floors main data and settings**
* **Routes \(Edges) data**
  <br>
  <br>

## FAQ – Permissions

### Do users need to allow permissions?

Various functionalities like BLE, sensor, and background location only work when the user allows the needed permissions.

### Which permissions are required?

**Android**

* **Location permissions.**
* **Bluetooth permissions.**
* **Internet & Network permissions.**
* **Storage Permissions.**

**iOS**

* **Location permissions.**
* **Bluetooth permissions.**
  <br>
  <br>

## FAQ – Dependencies

### What are the framework dependencies of the SDK?

Penguin’s Mobile SDK needs the following dependencies:

**Android**

* **\(service) com.google.android.gms:play-services-gcm**
* **\(service) com.google.code.gson:gson**
* **\(service) com.android.volley:volley**

**iOS**

* **CoreLocation.framework**
* **CoreBluetooth.framework**
* **CoreMotion.framework**
* **Foundation.framework**
* **SystemConfiguration.framework**
* **UIKit.framework**
  <br>
  <br>

## For any questions and comments, please send us a message to **info@penguinin.com**