# 第一支程式
在開始寫第一支程式前, 還是需要先來做環境的設定, 這邊以 Mac 的操作環境為主。

## 先安裝 React-Native 使用到的函式庫
``` bash
brew install node
brew install watchman
brew install flow //Facebook 的 型別檢查函式庫
```

## 安裝 React-Native 的 command line tool
``` bash
npm install -g react-native-cli
```

## iOS 相依
根據 Apple 官網文件安裝 XCode (IDE、iOS模擬器、iOS SDK)

## Android 相依
* 安裝 JDK (Java Development Kit)
* 安裝 Android SDK `brew install android-sdk`
* 在 shell 組態檔中 export ANDROID_HOME 變數指到 sdk 的安裝路徑 (ex. export ANDROID_HOME=/usr/local/opt/android-sdk)

透過 command line 執行 `android` 叫起 Android SDK Manager 的視窗介面, 除了預設項目要確保也安裝以下套件
* Android SDK Build-tools version 23.0.1
* Android 6.0 (API 23)
* Android Support Repository

重新啟動 Android SDK Manager 再安裝與模擬器相關的項目
* Intel x86 Atom System Image (for Android 5.1.1-API 22)
* Intel x86 Emulator Accelerator (HAXM installer)

透過執行 `android avd` 創建一個模擬器 (Android Vritual Devices), 確保核選了 Use Host GPU (否則會執行得很慢)