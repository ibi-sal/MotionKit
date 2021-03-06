#MotionKit

[![Platform](http://img.shields.io/badge/platform-ios-blue.svg?style=flat
)](https://developer.apple.com/iphone/index.action)
[![Language](http://img.shields.io/badge/language-swift-brightgreen.svg?style=flat
)](https://developer.apple.com/swift)
[![License](http://img.shields.io/badge/license-MIT-lightgrey.svg?style=flat
)](http://mit-license.org)
[![Issues](https://img.shields.io/github/issues/MHaroonBaig/MotionKit.svg?style=flat
)](https://github.com/MHaroonBaig/MotionKit/issues?state=open)

A nice and clean wrapper around the **CoreMotion Framework**. The Core Motion framework lets your application receive motion data from device hardware and process that data.
The data can be retrieved from **Accelerometer**, **Gyroscope** and **Magnetometer**.
You can also get the *__refined and processed gyroscope and accelerometer data__* from the **deviceMotion** datatype itself instead of getting raw Gyroscope or Accelerometer values.

#How does it work
You can retrieve all the values either by a trailing closure or by a delegate method. Both the approaches are fully supported.

##Getting Accelerometer Values
You can get the accelerometer values using just a few lines of code.

```swift
    var motionKit = MotionKit()

    motionKit.getAccelerometerValues(interval: 1.0){
        (x, y, z) in
        //Do whatever you want with the x, y and z values
        println("X: \(x) Y: \(y) Z \(z)")
        ....
      }

```
##Getting Gyroscope Values
Gyroscope values could be retrieved by the following few lines of code.

```swift
    var motionKit = MotionKit()

    motionKit.getGyroValues(interval: 1.0){
        (x, y, z) in
        //Do whatever you want with the x, y and z values
        println("X: \(x) Y: \(y) Z \(z)")
        ....
      }

```
##Getting Magnetometer Values
Getting Magnetometer values is as easy as grabing a cookie.

```swift
    var motionKit = MotionKit()

    motionKit.getMagnetometerValues(interval: 1.0){
        (x, y, z) in
        //Do whatever you want with the x, y and z values
        println("X: \(x) Y: \(y) Z \(z)")
        ....
      }

```
##Installation
Just copy **MotionKit.swift** file into your Xcode project, and you're ready to go.
MotionKit would soon be available through CocoaPods and Carthage.

#CMDeviceMotion - as easy as pie
In case if you want to get the processed values Accelerometer or Gyroscope, you can access the deviceMotion object directly to get those values, or, you can access the individual values from the standalone methods which work seamlessly with Trailing Closures and Delegates.

The deviceMotion object includes:
- Acceleration Data
  - userAcceleration
  - gravity
- Calibrated Magnetic Field
  - magneticField
- Attitude and Rotation Rate
  - attitude
  - rotationRate

All of the values can be retrieved either by individual methods or by getting the deviceMotion object itself.

###Getting the whole CMDeviceMotion Object
```swift

    motionKit.getDeviceMotionObject(interval: 1.0){
        (deviceMotion) -> () in
          var accelerationX = deviceMotion.userAcceleration.x
          var gravityX = deviceMotion.gravity.x
          var rotationX = deviceMotion.rotationRate.x
          var magneticFieldX = deviceMotion.magneticField.x
          var attitideYaw = deviceMotion.attitude.yaw
          ....
        }

```
###Getting refined values of Acceleration
You can get the refined and processed userAccelaration through the Device Motion service by just a few lines of code, either by a Trailing Closure or through Delegation method.
```swift

    motionKit.getAccelerationFromDeviceMotion(interval: 1.0){
        (x, y, z) -> () in
          // Grab the x, y and z values
          ....
        }

```
###Getting Gravitational Acceleration
Again, you can access it through the Device Motion service as well.
```swift
      motionKit.getGravityAccelerationFromDeviceMotion(interval: 1.0) {
          (x, y, z) -> () in
          // x, y and z values are here
          ....

        }
```
###Getting Magnetic Field around your device
Interesting, Get it in a magical way.
```swift
      motionKit.getMagneticFieldFromDeviceMotion(interval: 1.0) {
        (x, y, z, accuracy) -> () in
        // Get the values with accuracy
        ....
        }

```
###Getting the Attitude metrics
```swift
      motionKit.getAttitudeFromDeviceMotion(interval: 1.0) {
        (attitude) -> () in
          var roll = attitude.roll
          var pitch = attitude.pitch
          var yaw = attitude.yaw
          var rotationMatrix = attitude.rotationMatrix
          var quaternion = attitude.quaternion
          ....
        }

```
###Getting Rotation Rate of your device
```swift
      motionKit.getRotationRateFromDeviceMotion(interval: 1.0) {
        (x, y, z) -> () in
        // There you go, grab the x, y and z values
        ....
        }

```



##Precautions
For performance issues, it is suggested that you should use only one instance of CMMotionManager throughout the app. Make sure to stop receiving updates from the sensors as soon as you get your work done.
You can do this in MotionKit like this.
```swift

    //Make sure to call the required function when you're done
    motionKit.stopAccelerometerUpdates()
    motionKit.stopGyroUpdates()
    motionKit.stopDeviceMotionUpdates()
    motionKit.stopmagnetometerUpdates()

```

##Delegates
In case if you dont want to use the trailing closures, we've got you covered. MotionKit supports the following Delegate methods to retrieve the sensor values.
```swift
    optional func retrieveAccelerometerValues (x: Double, y:Double, z:Double, absoluteValue: Double)
    optional func retrieveGyroscopeValues     (x: Double, y:Double, z:Double, absoluteValue: Double)
    optional func retrieveDeviceMotionObject  (deviceMotion: CMDeviceMotion)
    optional func retrieveMagnetometerValues  (x: Double, y:Double, z:Double, absoluteValue: Double)


    optional func getAccelerationValFromDeviceMotion        (x: Double, y:Double, z:Double)
    optional func getGravityAccelerationValFromDeviceMotion (x: Double, y:Double, z:Double)
    optional func getRotationRateFromDeviceMotion           (x: Double, y:Double, z:Double)
    optional func getMagneticFieldFromDeviceMotion          (x: Double, y:Double, z:Double)
    optional func getAttitudeFromDeviceMotion               (attitude: CMAttitude)

```
To use the above delegate methods, you have to add the MotionKit delegate to your ViewController.
```swift
    class ViewController: UIViewController, MotionKitDelegate {
      ...
    }
```
And in the ViewDidLoad method, you simply have to add this.
```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        motionKit.delegate = self
        ......
      }

```
Having that done, you'd probably want to implement a delegate method like this.
```swift
    func retrieveAccelerometerValues (x: Double, y:Double, z:Double, absoluteValue: Double){
      //Do whatever you want with the x, y and z values. The absolute value is calculated through vector mathematics
      ......
    }

   func retrieveGyroscopeValues (x: Double, y:Double, z:Double, absoluteValue: Double){
    //Do whatever you want with the x, y and z values. The absolute value is calculated through vector mathematics
    ......
   }

```


#Discussion
- You can join our [Reddit] (https://www.reddit.com/r/MotionKit/) channel to discuss anything.

- You can also open an issue here for any kind of feature set that you want. We would love to hear from you.

- Don't forget to subscribe our Reddit channel, which is [/r/MotionKit] (https://www.reddit.com/r/MotionKit/)

- Our StackOverflow tag is 'MotionKit'


#Requirements
* iOS 7.0+
* Xcode 6.1

#TODO
- [ ] Add More Methods
- [ ] Adding Background Functionality

#License
<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">Creative Commons Attribution-NonCommercial 4.0 International License</a>.
