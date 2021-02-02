# Mobile SDK

## iOS Documentation - v4.0.0

## Table of Contents
[1 Background](#background)

[2 SDK Methods](#sdk-methods)

[2.1 initialize](#initialize)

[2.2 initializeBlueTooth](#initializebluetooth)

[2.3 isReady](#isready)

[2.4 checkError](#checkerror)

[2.5 checkWarning](#checkwarning)

[2.6 getErrorMessage](#geterrormessage)

[2.7 getWarningMessage](#getwarningmessage)

[2.8 setMode](#setmode)

[2.9 getMyPosition](#getmyposition)

[2.10 projectedPoint](#projectedpoint)

[2.11 setFloor](#setfloor)

[2.12 startNavigation](#startnavigation)

[2.13 stopNavigation](#stopnavigation)

[2.14 setTestMode](#settestmode)

[2.15 setTestNavPath](#settestnavpath)

[2.16 getVersion](#getversion)

[2.17 setDataService](#setdataservice)

[2.18 setPositionService](#setpositionservice)

[2.19 setStepDetectionEnabled](#setstepdetectionenabled)

[2.20 setFileSimulationModeEnabled](#setfilesimulationmodeenabled)

[2.21 setVenueID](#setvenueid)

[2.22 getInitializationProgress](#getinitializationprogress)

[2.23 getCurrentSubpath](#getcurrentsubpath)

[2.24 isNavigationErrorPath](#isnavigationerrorpath)

[2.25 isNavigationFloorError](#isnavigationfloorerror)

[2.26 setUpdateInBAckGroundMode](#setupdateinbackgroundmode)

[2.27 CheckTrustedFloor](#checktrustedfloor)

[2.28 GetPositionRequestErrorMessage](#getpositionrequesterrormessage)

[2.29 CheckResetSDKRequest](#checkresetsdkrequest)


# SDK and Sample Application Delivery 
PenguinIN shares the Mobile SDK as a **.framework** file. Then, this file is dragged and dropped into an iOS project's Frameworks, Libraries, and Embedded Content section. After that, the Embed config field, check the "Embed and Sign" option.

For the sample application, project is already pre-configured and ready for build. 

<br>
<br>

# Sample Application  

The sample application has two modes of operation: (1) simulation mode and (2) live mode. In simulation mode, the application runs a pre-configured scenario with predetermined list of positions. In live mode, the position is fetched from the position engine services and of course requires complete BLE beacon roll-out and site calibration data to have been completed. In both modes, connectivity to the position engine service and the presence of at least one BLE beacon is required.

The steps below explain how to run the sample application.



# SDK: Methods and Integration 

In this section we present the methods availed to the application developer through our SDK followed by best practices to integrate it within a mobile application. 

<br>

## initialize
Returns initialization success or failure as Boolean value.
<br>

### Operation
These steps describe the process of initialization:

 * Validate OS support and server connectivity.
 * Initialize input sources for positioning platform.
 * Detect current venue and get venue data \(will use data API):
             * Venue specific positioning information
             * Floors details
             * Edges details
 * Perform device specific calibration
 * Establish connectivity requirements.
 * Save the results and Log for analytics \(if supported)
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| userID | Front End UserID | String |
| ipAddressDetails | Penguin Data API IP address \(mandatory)<br>Penguin Position API IP address \(mandatory)
Third party API IP address \(optional) | List(String) |
| noneIMU | Enable Just JAR None IMU calculation | Boolean |
| regionUUIDs | Mounted region/beacons UUID for scanning BLE | List(String) |
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Result | Success: 1 or Fail: 0
Error Code.
Warning Code.
separated by “_”
ex: “1*0_0” or “0_1*2” | String |
<br>
<br>

## initializeBluetooth
Initialize Bluetooth for library, must be called upon app start.
<br>

### Parameters
**N/A**
<br>

### Response
**N/A**
<br>
<br>

## isReady
Returns a Boolean value indication, that all needed requirements are ready and you can start calling other methods.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Ready | Ready for calling other methods | Boolean |
<br>
<br>

## checkError
Returns current error code if it exists.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| ErrorCode | Current Error Code \(0: No Errors) |Int|
<br>
<br>

## checkWarning
Returns current warning code if it exists.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| WarningCode | Current Warning Code \(0: No Warnings) |Int|
<br>
<br>

## getErrorMessage
Returns error message from Error Code Look Up table.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| ID | Error Code |Int|
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| ErrorMessage | Error Message | String |
<br>

### Error Code Look Up:
| Value | Description |
|---|---|
| 1 | OS not supported |
| 2 | Signal Lost |
| 3 | Venues details Import Error |
| 4 | Floors details Import Error |
| 5 | Settings Error |
| 6 | Map North angle calculation Error |
| 7 | Edges details Import Error |
| 8 | Wi-Fi Permission OFF |
| 9 | BLE Permission OFF |
| 10 | Device RF not available |
| 11 | Venue RF not available |
| 12 | Bluetooth not enabled |
| 13 | Sensors Initial Error |
| 14 | GPS Disabled |
<br>
<br>

## getWarningMessage
Returns warning message from Warning Code Look Up table.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| ID | Warning Code |Int|
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| WarningMessage | Warning Message | String |

| Value | Description |
|---|---|
| 1 | Accelerometer sensor not available |
| 2 | Magnetometer sensor not available |
| 3 | Magnetometer accuracy low |
| 4 | Gyro sensor not available |
| 5 | Orientation sensor not available |
| 6 | Barometer sensor not available |
<br>
<br>

## setMode
Set the position mode and scanning time frequency:

 * Background:
             * Low frequency position scanning for logging and analytics.
 * Freestyle:
             * High frequency position scanning for trusted position on the venue. This mode assumes that the user is not wayfinding between a known source and a destination POI.
 * Navigation:
             * High frequency position scanning for trusted position while navigating to a known destination with a know route already identified.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| ModeID | BackGround,FreeStyle,Navigation |Int|
| TimePeriod | Time Period in ms |Int|
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Success | Updated the mode successfully | Boolean |
<br>

### Modes
| ID | Description | Scan Time / milliseconds |
|:---|:---|:---:|
| 1 | Background \(Default) | 10000 |
| 2 | Free Style | 2000 |
| 3 | Navigation | 2000 |
<br>
<br>

## getMyPosition
Returns last detected position concatenated together with "_" separator:

 * Current Floor ID.
 * Position point \(X, Y).
 * Accuracy circle radius in pixels.
 * Current Edge ID.

**Recommendation:**

This method provides an update on the position of the mobile application user. It is not recommended to call it with high frequency. The recommended value is 500msec-2000msec and should be adjusted according to the desired UI behavior.
<br>

### Operation
 * Concatenate FloorID, X, Y, Accuracy Circle Radius , Current Edge ID together with "_" separator:  
    * Ex:  
        * Current Floor ID: 2.  
        * My Position \(X, Y): \(3000, 5100).  
        * Accuracy radius in pixels: 120.  
        * Current Edge ID: 150.  
        * Result:  
            * 2_3000_5100_120_150  
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Position | FloorID, X, Y, Accuracy Circle Radius , Current Edge ID <br> Concatenated together with "_" separator | String |
<br>
<br>

## projectedPoint
Projects any point \(X, Y) \(i.e. chosen from the map or any other source) onto the defined edges. Use of such point depends on the enabled mode: background, freestyle, navigation.
<br>

### Operation
 * Load requested floor edges list then project the requested MapPoint.
 * Returns the projected point \(X,Y)
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| MapPoint | Requested Point \(X,Y) | Point |
| FloorID | Requested Floor ID |Int|
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| ProjectedPoint | The Projected MapPoint on Requested Floor Edges | Point |
<br>
<br>

## setFloor
Set current floor ID.
<br>

### Operation
 * Set the current Floor ID.
 * Request new position within the floor id to start new positioning area.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| FloorID | Requested Floor ID |Int|
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Success | New Floor ID saved successfully | Boolean |
<br>
<br>

## startNavigation
Start positioning on the given navigation path.
<br>

### Operation
 * Enable Navigation Mode.
 * Set Navigation path edges as projection edges list.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| PathEdges | Navigation path edges list | List(Edges) |
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Success | Navigation mode enabled successfully | Boolean |
<br>

### Edge data structure: 
| Attribute | Type |
|---|---|
| id | Int |
| venueId | Int |
| floorId | Int |
| type | Int |
| p1VenueID | Int |
| p1FloorID | Int |
| p1 | Point |
| p2VenueID | Int |
| p2FloorID | Int |
| p2 | Point |
<br>
<br>

## stopNavigation
Stop navigation positioning and go back to freestyle mode positioning.
<br>

### Operation
 * Disable navigation mode and return to the previous mode.
 * Set all edges as projection edges list.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Success | Navigation mode disabled successfully | Boolean |
<br>
<br>

## setTestMode
Set the test position mode and scanning time frequency.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| FileName | Point’s Source text file name at Root destination | String |
| TimePeriod | Time Period in ms |Int|
| NoneIMU | Enable Just Framework None IMU calculation | Boolean |
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Success | Updated the mode successfully | Boolean |
<br>
<br>

## setTestNavPath
Set the test **navigation** position mode and scanning time frequency.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| FileName | Navigation Path ordered edges Source text file name at Root destination | String |
| PointsFileName | Point’s Source text file name at Root destination | String |
| TimePeriod | Time Period in ms |Int|
| NoneIMU | Enable Just JAR None IMU calculation | Boolean |
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Navigation Edges List | File’s Edges list | List(Edge) |
<br>

### Edge data structure: 
| Attribute | Type |
|---|---|
| id | Int |
| venueId | Int |
| floorId | Int |
| type | Int |
| p1VenueID | Int |
| p1FloorID | Int |
| p1 | Point |
| p2VenueID | Int |
| p2FloorID | Int |
| p2 | Point |
<br>
<br>

## getVersion
Returns SDK’s current version number.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Version | Returns version’s number | String |
<br>
<br>

## setDataService
Sets data API service name.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| ServiceName | - | String |
<br>

### Response
**N/A**
<br>
<br>

## setPositionService
Sets position service name.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| ServiceName | - | String |
<br>

### Response
**N/A**
<br>
<br>

## setStepDetectionEnabled
Enables positioning dependency on steps.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| isEnabled | - | Boolean |
<br>

### Response
**N/A**
<br>
<br>

## setFileSimulationModeEnabled
Enables positioning simulation mode.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| isEnabled | - | Boolean |
<br>

### Response
**N/A**
<br>
<br>

## setVenueID
Sets positioning venue ID.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| ID | In case used value was “-1”; it will detect the venue where user is located. <br>Otherwise, if sent specific venue ID, it will get data for the sent venue ID. |Int|
<br>

### Response
**N/A**
<br>
<br>

## getInitializationProgress
Returns SDK initialization progress status.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Status | - |Int|
<br>

### Initialization Progress Code Look Up: 
| Value | Description |
|---|---|
| \-1 | No Beacons found till now |
| 0 | Bluetooth & Sensors initialization |
| 1 | Venue Detection |
| 2 | Loading Venue Data \(Floors & Edges) |
<br>
<br>

## getCurrentSubpath
Returns SDK current navigation path (Venue, Floor, Edges, List, etc.).
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Navigation Sub Path | SDK Current navigation Sub Path | NavigationSubPath |
<br>

### NavigationSubPath data types: 
| Value | Description | Value Type |
|:---|:---|:---:|
| getSubPathsSize | Int | all navigation sub paths count |
| pathIndex | Int | Current index of all navigation sub paths list |
| venueID | Int | Current Path Venue ID |
| floorID | Int | Current Path Floor ID |
| firstEdge | Edge | Current Path First Edge |
| nextEdge | Edge | Next Path Edge |
| pathEdges | List(Edge) | Current Sub path List of Edges |
<br>

### Edge data structure: 
| Attribute | Type |
|---|---|
| id | Int |
| venueId | Int |
| floorId | Int |
| type | Int |
| p1VenueID | Int |
| p1FloorID | Int |
| p1 | Point |
| p2VenueID | Int |
| p2FloorID | Int |
| p2 | Point |
<br>
<br>

## isNavigationErrorPath
Returns navigation error path flag.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Error Path | - | Boolean |
<br>
<br>


## isNavigationFloorError
Returns flag that your position floor is out of all navigation path.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| Navigation Floor Error Flag | Current Floor is out of all navigation path | Boolean |
<br>
<br>


## setUpdateInBAckGroundMode
Enables/Disables scanning when the app in background state.
<br>

### Parameters
| Parameters | Description | Value Type |
|:---|:---|:---:|
| isEnabled | Set the value to true or false to enable/disable scanning in the background | Boolean |
<br>

### Response
**N/A**
<br>
<br>

## CheckTrustedFloor
Returns Position engine’s Floor Trusted Flag.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| TrustedStatus | Ask User to select his current floor if not trusted | Boolean |
<br>
<br>

## GetPositionRequestErrorMessage
Returns error message that received from position engine.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| PositionEngineMessageError | Same error message that received from position engine | String |
<br>
<br>

## CheckResetSDKRequest
Returns flag that you must reset SDK to continue positioning.
<br>

### Parameters
**N/A**
<br>

### Response
| Value | Description | Value Type |
|:---|:---|:---:|
| resetSDKFlag | Positioning will stop if reset SDK flag is true. | Boolean |
<br>
<br>
<br>

# Sample App and SDK Configuration

To change the **SDK configuration** from Sample app folder open the Sample App PenguinIN/Services/MethodUrl, and you can start modifying the following paramters:

- rootIP.
- dataServerIPAddress
- dataServerIPAddressHTTPS
- positionIPAddress
- positionIPAddressHTTPS
- dataServiceFileName
- pathServiceFileName
- dataServiceName
- positionServiceName

To change the **Sample App Settings** from Sample app folder open the o	Sample App PenguinIN / Settings, and you can start modifying the following paramters:

- nonIMU 
  - withIMU 
  - nonIMU

- simulationModeEnabled
  - withSimulator : Simulation scenarios run
  - nonSimulator : Live BLE scan run

- username : User test cases:
  - Enum UserTest : Choose user and set it to username variable in setting.

- password
- venueID  : Fix Venue Value
  <br>
  ![](C:/Users/MSmadi/Penguinin/M. Zghoul - 4.0.0/iOS_Settings.png) 
    <br>


To change the **PoI Data** from Sample app folder open the Sample App PenguinIN/VC/MapHomeVC, and you can start modifying the following paramters:

- ID.
- Floor ID.
- Name.
- Description
- Zone Points.
  <br>
  ![](C:/Users/MSmadi/Penguinin/M. Zghoul - 4.0.0/iOS_POI.png) 
    <br>

<br>
<br>

# SDK Integration Flow Chart

The SDK Integration flow charts provided below help developers with the SDK integration process. They show each step of the integration. The three flow charts show the initialization, the free style and the navigation mode.
<br>
<br>

## Initialization Flow Chart

<br>

![](C:/Users/MSmadi/Penguinin/M. Zghoul - 4.0.0/image2.png)
<br>

## Free Style/ Get Position Flow Chart

<br>

![](C:/Users/MSmadi/Penguinin/M. Zghoul - 4.0.0/image3.png)
<br>
<br>

## Navigation Mode Flow Chart

<br>

![](C:/Users/MSmadi/Penguinin/M. Zghoul - 4.0.0/image4.png)
<br>
<br>

## For any questions and comments, please send us a message to

**info@penguinin.com**
