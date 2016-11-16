# Object-C 原生模組解析

在 iOS 系統中原生的開發語言是 Object-C, 所以在 javascript (React Native) 中使用到原生語言開發的模組時, 會需要一層 Bridge (橋接), 這章介紹的就是如何構築使用原生語言撰寫的模組, 並實踐 React Native 提供的 bridge 介面。

```objc
#import "RCTBridgeModule.h"

@interface HelloWorld : NSObject <RCTBridgeModule>
@end
```