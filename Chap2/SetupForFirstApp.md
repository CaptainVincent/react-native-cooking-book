# 第一支程式
在開始寫第一支程式前, 還是需要先來做環境的設定, 這邊以 Mac 的操作環境為主。

### 先安裝 React-Native 使用到的函式庫
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
根據 Apple 官網文件安裝 XCode

## Android 相依
* 安裝 JDK (Java Development Kit)
* 安裝 Android SDK `brew install android-sdk`
* 在 shell 組態檔中 export ANDROID_HOME 變數指到 sdk 的安裝路徑 (ex. export ANDROID_HOME=/usr/local/opt/android-sdk)

透過 Android SDK Manager 的視窗介面再安裝以下套件
*