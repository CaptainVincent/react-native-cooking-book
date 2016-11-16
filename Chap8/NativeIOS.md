# Object-C 原生模組解析

在 iOS 系統中原生的開發語言是 Object-C, 所以在 javascript (React Native) 中使用到原生語言開發的模組時, 會需要一層 Bridge (橋接), 這章介紹的就是如何構築使用原生語言撰寫的模組, 並實踐 React Native 提供的 bridge 介面。

### HelloWorld.h
表明是 NSObject 子類別, 並實作 RCTBridgeModule 介面
```objc
#import "RCTBridgeModule.h"

@interface HelloWorld : NSObject <RCTBridgeModule>
@end
```

### HelloWorld.m
RCT_EXPORT_MODULE 是定義好的巨集負責將方法匯出給 javascript 使用
```objc
#import "HelloWorld.h"
//輸出 console 用
#import "RCTLog.h" 

@implementation HelloWorld

RCT_EXPORT_MODULE();

RCT_EXPORT_METHOD(greeting:(NSString *)name)
{
  RCTLogInfo(@"Saluton, %@", name);
}

@end
```
> Object-C 內以 greeting:name 方法命名包含到**參數名**, React Native 預設以到冒號前的作為 JavaScript 的方法名, 也可以另外使用 RCT_REMAP_METHOD 巨集來重新對應兩邊的方法名稱。

### JavaScript 使用方式
```javascript
var HelloWorld = require('react-native').NativeModules.HelloWorld;
```

或是包裝一層 HelloWorld.js
```javascript
import { NativeModules } from 'react-native';
export default NativeModules.HelloWorld;
```

之後就可以直接使用
```javascript
var HelloWorld = require('./HelloWorld');
```

### [更複雜的例子](http://reactnative.cn/docs/0.36/native-component-ios.html)
