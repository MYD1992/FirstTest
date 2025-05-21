### Overview
DragyBLESDK 0.1 is the Bluetooth SDK designed to connect with Dragy GPS performance devices, supporting both DRG70 and DRGPR models. It handles device connection management, GPS data analysis, and other services.
### Compatibility
**Deployment Target**
Requires iOS 15.0 or later

**Supported Language**
* Objective-C or
* Swift 5 or later
### Installation
1. Drag the DragyBLESDK folder into your project
2. The SDK requires the use of Bluetooth functionality, so need to ensure your project's Info.plist includes the proper use description of the sensitive information.
### Getting Started
#### 1. Import SDK
Objective-C:
    
```
    #import <DragyBLESDK/DeviceManager.h>
```
Swift:

```
    import DragyBLESDK
```
#### 2. Search, Connect, or Disconnect Dragy Device
##### 2.1 Initialize device management object DeviceManager and assign delegate
```
    let manager = DeviceManager()
    manager.deviceDelegate = self
```
DeviceManager will perform all the functions of the device.

##### 2.2 Search device
```
    manager.startDeviceScan()
```
The name of the nearest device will be returned by DeviceManager via delegate:
```
    func device(_ device: DeviceManager, searchResultObtained deviceName:String?, error: Error?) 
```

##### 2.3 Connect device
```
    manager.startDeviceConnect()
```
Must be called after startDeviceScan(). DeviceManager will connect the device returned in the previous step.

##### 2.4 Stop searching or disconnect device
```
    manager.stopDeviceScan()
```
Connection status changes will be returned by DeviceManager via callback method:
```
    func device(_ device: DeviceManager, deviceConnectStatusChanged state: DeviceConnectState, error: (any Error)?)
```

#### 3. GPS Data Streaming
Once connected, Dragy GPS data will stream automatically by DeviceManager and can be accessed via delegate:
```
    func device(_ device: DeviceManager, gpsDataObtained item: DeviceGPSItem)
```
    
#### 4. Weather Data
Once device is connected by DeviceManger and satellites positioning type is fixed, weather information wii be returned via DragyWeatherDelegate:
```
    @func didGetCurrentWeather(_ info: DragyWeatherInfo)
```
DragyWeatherInfo contains information including weather code, temperature, pressure, humidity, DA, etc. Possible values for weatherCode:

| weatherCode | icon | Description |
| --- | --- | --- |
| 01d | ![(http://openweathermap.org/img/wn/01d@2x.png)](http://openweathermap.org/img/wn/01d@2x.png) | clear sky |
| 02d | ![http://openweathermap.org/img/wn/02d@2x.png](http://openweathermap.org/img/wn/02d@2x.png) | few clouds |
| 03d | ![http://openweathermap.org/img/wn/03d@2x.png](http://openweathermap.org/img/wn/03d@2x.png) | scattered clouds |
| 04d | ![http://openweathermap.org/img/wn/04d@2x.png](http://openweathermap.org/img/wn/04d@2x.png) | broken clouds |
| 09d | ![http://openweathermap.org/img/wn/09d@2x.png](http://openweathermap.org/img/wn/09d@2x.png) | shower rain |
| 10d | ![http://openweathermap.org/img/wn/10d@2x.png](http://openweathermap.org/img/wn/10d@2x.png) | rain |
| 11d | ![http://openweathermap.org/img/wn/11d@2x.png](http://openweathermap.org/img/wn/11d@2x.png) | thunderstorm |
| 13d | ![http://openweathermap.org/img/wn/13d@2x.png](http://openweathermap.org/img/wn/13d@2x.png)| snow |
| 50d | ![http://openweathermap.org/img/wn/50d@2x.png](http://openweathermap.org/img/wn/50d@2x.png) | mist |
| 01n | ![http://openweathermap.org/img/wn/01n@2x.png](http://openweathermap.org/img/wn/01n@2x.png) | clear sky |
| 02n | ![http://openweathermap.org/img/wn/02n@2x.png](http://openweathermap.org/img/wn/02n@2x.png) | few clouds |
| 03n | ![http://openweathermap.org/img/wn/03d@2x.png](http://openweathermap.org/img/wn/03d@2x.png) | scattered clouds |
| 04n | ![http://openweathermap.org/img/wn/04d@2x.png](http://openweathermap.org/img/wn/04d@2x.png) | broken clouds |
| 09n | ![http://openweathermap.org/img/wn/09d@2x.png](http://openweathermap.org/img/wn/09d@2x.png) | shower rain |
| 10n | ![http://openweathermap.org/img/wn/10d@2x.png](http://openweathermap.org/img/wn/10d@2x.png) | rain |
| 11n | ![http://openweathermap.org/img/wn/11d@2x.png](http://openweathermap.org/img/wn/11d@2x.png) | thunderstorm |
| 13n | ![http://openweathermap.org/img/wn/13d@2x.png](http://openweathermap.org/img/wn/13d@2x.png)| snow |
| 50n | ![http://openweathermap.org/img/wn/50d@2x.png](http://openweathermap.org/img/wn/50d@2x.png) | mist |


### Developer Notes

* DeviceManager requires a small amount of network access to download AGPS data and retrieve additional information such as weather conditions and density altitude.

* Some DRG70 units may require ~45 seconds for initial performance mode activation (causes temporary disconnect and reconnect automatically after the process).

* AGPS data (helps Dragy lock onto satellites faster) transmission begins automatically after connection and takes ~8 seconds. GPS data may pause during this time.

#### Control AGPS behavior
Stop sending:
```
manager.stopSendAgps()
```
Resume or trigger manually:
```
manager.sendAgps()
```

#### ⚠️ Important Configuration Warning
By default, Dragy devices operate in two GNSS mode with 10Hz update rate. 

**Please do not use u-blox tools to permanently modify device configuration**, as this may interfere with Dragy App functionality.

If any configuration changes are needed, please use the commands provided through our SDK whenever possible.

### Error Codes
| Code | Description |
| --- | --- |
| 101 | Bluetooth not authorised |
| 102 | Bluetooth not enabled |
| 103 | Device not found |
| 104 | Connection failed (system error) |
| 105 | Disconnected (system error) |
| 201 | AGPS file download failed |
| 202 | Device disconnected |
| 301 | Failed to reconnect to device |
| 302 | Failed to set Performance Mode|

### Revision History
| Version | Date | Description |
| --- | --- | --- |
| 0.1 | 2025-05-20 | Initial release of DragyBLESDK with support for DRG70 and DRGPR. Includes GPS data streaming, AGPS handling, and weather integration. Warning added regarding u-blox configuration tools. |
|  |  |  |

### Need Support?
If you encounter any issues during integration or have questions about using the Dragy BLE SDK, feel free to contact our development support team at sales@godragy.com.

We're here to help ensure a smooth and successful integration experience.

Happy coding!
— **Dragy Developer Team**




