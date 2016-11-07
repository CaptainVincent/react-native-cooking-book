# Emulator (模擬器)
在將程式部署到手機前, 我們可以先透過模擬器先在電腦上驗證功能, 畢竟也不是每個人都有那麼多隻手機可以當作開發機, 再者部署到裝置的流程也較為複雜。

這邊一樣使用 `react-native init FirstProject` 創建的資料夾來示範

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
* Shell 進入 project 的目錄底下執行 `npm start` (把 server 端跑起來)
* 然後再用用 Xcode 開啟 ios 的目錄, 在左上角選擇完模擬的 Device 後按下 ▷