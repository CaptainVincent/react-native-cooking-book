# 建構第一個 App

透過 React-Native command line tool 創建一個 project 名稱為 **FirstProject**
```bash
react-native init FirstProject
```

產生的目錄結構, 目錄後方有 //註解 簡略註解資料夾用途。
```
./
|-- __tests__
|   |-- index.android.js
|   `-- index.ios.js
|-- android // About Android platform template
|   |-- app
|   |-- build.gradle
|   |-- gradle
|   |-- gradle.properties
|   |-- gradlew
|   |-- gradlew.bat
|   |-- keystores
|   `-- settings.gradle
|-- index.android.js //React code
|-- index.ios.js //React code
|-- ios // About iOS platform template
|   |-- FirstProject
|   |-- FirstProject.xcodeproj
|   `-- FirstProjectTests
|-- node_modules
|   |-- babel-jest
|   |-- babel-preset-react-native
|   |-- jest
|   |-- jest-react-native
|   |-- react
|   |-- react-native
|   |-- react-test-renderer
|   `-- whatwg-fetch
`-- package.json
```

### index.ios.js / index.android.js
有提供簡單的 //註解, 解釋程式碼的運作
```javascript
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

// 引用
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

// 宣告一個 FirstProject 的元件
export default class FirstProject extends Component {
  // 實作渲染的內容, 這邊預設產生歡迎畫面的文字
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
        <Text style={styles.instructions}>
          To get started, edit index.ios.js
        </Text>
        <Text style={styles.instructions}>
          Press Cmd+R to reload,{'\n'}
          Cmd+D or shake for dev menu
        </Text>
      </View>
    );
  }
}

// Text 的 style 設定
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});

// 註冊頂層元件給 AppDelegate.m
AppRegistry.registerComponent('FirstProject', () => FirstProject);
```