# Emulator (模擬器)
在將程式部署到手機前, 我們可以先透過模擬器先在電腦上驗證功能, 畢竟也不是每個人都有那麼多隻手機可以當作開發機, 再者部署到裝置的流程也較為複雜。

這邊引用後面章節的範例程式, 透過 react-native init 建立、新增完檔案 (crossplatform.js, switch.android.js, switch.ios.js) 後檔案結構如下, 這邊一樣也可以使用 FirstProject 的資料夾來進行

```
./
|-- android
|-- index.android.js
|-- index.ios.js
|-- ios
|-- node_modules
`-- package.json
```

### iOS
首先先用 Xcode 開啟