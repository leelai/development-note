# Embedded Flutter page in native code

Here is the [offical doc](https://flutter.dev/docs/development/add-to-app).
這邊我只focus在ios/flutter, android尚未研究(因為是google的應該沒啥問題😆）
## Adding a Flutter screen to an iOS app
After reading, sort out the four points (I used [Option A](https://flutter.dev/docs/development/add-to-app/debugging))
1. create a flutter module
```
flutter create --template module my_flutter
```
3. add module to Podfile
4. add some code to initial flutter engine
5. add a test page to launch FlutterViewController

## Development (important)

There is a hidden ios folder which contains xcode worksapce in my_flutter project, so you can run it by xcode for development.

## How to commuicate between Dart and Swift
有兩種方法可提提供了雙向溝通, 一個是從flutter主動呼叫某個method, 另一個是從native傳event回去
- [offical doc](https://flutter.dev/docs/development/platform-integration/platform-channels)
- [sample code](https://github.com/flutter/flutter/tree/master/examples/platform_channel_swift)

### Method Channel (dart -> swift/objc)

從flutter主動傳訊息給native, native再回傳資料回去flutter

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

## [Platform channel data types](https://flutter.dev/docs/development/platform-integration/platform-channels#codec)
- https://flutter.dev/docs/development/platform-integration/platform-channels#codec
- 目前專案需求要從flutter把ByteData 轉成 Uint8List 往native送, 所以在kotlin會收到 ByteArray, swift收到FlutterStandardTypedData(bytes: Data)
- 可考慮base64, native <--> flutter都用string傳送; 轉base64後資料會增加,但目前應用不會透過網路傳輸,而且目前專案資料量不大.

## Debugging(目前還沒測試過)
- https://flutter.dev/docs/development/add-to-app/debugging
- [flutter attach要先啟用network permission](https://flutter.dev/docs/development/add-to-app/ios/project-setup#local-network-privacy-permissions)
- [vs-code debug設定](https://flutter.dev/docs/development/add-to-app/debugging#vs-code)
## Troubleshooting
- [kernel binary format problem](https://stackoverflow.com/questions/56051502/cant-load-kernel-binary-invalid-kernel-binary-format-version-no-active-packag)

## Refference
- https://flutter.dev/docs/development/add-to-app/ios/project-setup
- https://medium.com/flutter-community/add-flutter-to-existing-android-ios-app-ae8c4fb1582e
