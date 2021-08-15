# Embedded Flutter page in native code

Here is the [offical doc](https://flutter.dev/docs/development/add-to-app).

## Adding a Flutter screen to an iOS app
After reading, sort out the four points (I used [Option A](https://flutter.dev/docs/development/add-to-app/debugging))
1. create a flutter module
2. add module to Podfile
3. add some code to initial flutter engine
4. add a test page to launch FlutterViewController

## How to commuicate between Dart and Swift
有兩種方法可提提供了雙向溝通, 一個是從flutter主動呼叫某個method, 另一個是從native傳event回去
### Method Channel (dart -> swift/objc)

- 從flutter主動傳訊息給native, native再回傳資料回去flutter
- https://flutter.dev/docs/development/platform-integration/platform-channels
- https://github.com/flutter/flutter/blob/master/examples/platform_channel_swift/lib/main.dart

1. 在flutter這邊定義methodchannel
```
methodChannel = MethodChannel('samples.flutter.io/battery');
```
2. 呼叫某個method by name
```
result = await methodChannel.invokeMethod('getBatteryLevel');
```
3. 在swift這邊設定handler
```
batteryChannel.setMethodCallHandler
```
### Event Channel (swift/objc -> dart)

- 如果要從native主動傳資料可以用
- 在官網沒有詳述event channel使用方式, 所以google了一下作法如下
1. 在AppDelegate實現FlutterStreamHandler
```
class AppDelegate: FlutterAppDelegate, FlutterStreamHandler {
```
2. 建立FlutterEventChannel
```
let eventChannnel = FlutterEventChannel(name: "samples.flutter.io/charging", binaryMessenger: flutterEngine.binaryMessenger)
eventChannnel.setStreamHandler(self)
```
3. onListen裡面拿到sink
```
func onListen(withArguments arguments: Any?, eventSink events: @escaping FlutterEventSink) {
    self.eventSink = events
}
```
4. 用eventSink打資料回flutter
```
eventSink(xxxxxxxx)
eventSink(FlutterError(xxxxx))
```

## Debugging
- https://flutter.dev/docs/development/add-to-app/debugging
- ios目前測試無法flutter attach
- android還沒測試

## Refference
- https://flutter.dev/docs/development/add-to-app/ios/project-setup
- https://medium.com/flutter-community/add-flutter-to-existing-android-ios-app-ae8c4fb1582e