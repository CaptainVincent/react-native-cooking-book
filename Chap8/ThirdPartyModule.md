# 引用第三方模組

使用過 npm 管理套件的人可能就會對 package.json 有點概念, 因為不管是開發網頁應用 或是 用 React Native 開發的 App, 都會時常引用到不同版本的 package, 且因為這些模組之間可能存在的相依關係, 並非每個版本都能保證彼此兼容, 所以為了解決這個問題, 可以建立 package.json, 將開發該應用程式所使用到的這些模組版本皆描述在內, 進而達到開發環境的一致性。

使用方式則是透過指令 `npm install <module name> --save` (save 會自動將該模組版本記錄到 package.json)

> 可以透過以上指令的方式安裝大部分模組, 但是對於 DOM 操作相關的方法一般來說都會失敗, 因為這部分操作關係到執行環境。

***
 ### install react-native-video (Demo for iOS)

`npm install react-native-video --save`

另外 [Link Library with iOS](https://facebook.github.io/react-native/docs/linking-libraries-ios.html) or 直接執行 `react-native link` 會自動幫你 iOS 與 Android 的 Library 都連結好, 之後就可以在程式碼中插入 Video tag。

**index.ios.js**

Source Code:
```javascript
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

import Video from 'react-native-video';

export default class Depends extends Component {
  render() {
    return (
      <View style={styles.container}>
        <Video source={require('./videoplayback.mp4')}
               rate={1.0}                     // 0 is paused, 1 is normal.
               volume={1.0}                   // 0 is muted, 1 is normal.
               muted={false}                  // Mutes the audio entirely.
               paused={false}                 // Pauses playback entirely.
               playInBackground={false}       // Audio continues to play when app entering background.
               playWhenInactive={false}       // [iOS] Video continues to play when control or notification center are shown.
               progressUpdateInterval={250.0} // [iOS] Interval to fire onProgress (default to ~250ms)
               onLoadStart={this.loadStart}   // Callback when video starts to load
               onLoad={this.setDuration}      // Callback when video loads
               onProgress={this.setTime}      // Callback every ~250ms with currentTime
               onEnd={this.onEnd}             // Callback when playback finishes
               onError={this.videoError}      // Callback when video cannot be loaded
               style={styles.backgroundVideo} />

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
  backgroundVideo: {
    position: 'absolute',
    top: 0,
    left: 0,
    bottom: 0,
    right: 0,
  },
});

AppRegistry.registerComponent('Depends', () => Depends);

```

***
 ### install react-native-linear-gradient (Demo for Android)
 
`npm install react-native-linear-gradient --save`

**此模組 官網提供的 link Library 方式**

1. in android/settings.gradle
```
...
include ':react-native-linear-gradient'
project(':react-native-linear-gradient').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-linear-gradient/android')
```

2. in android/app/build.gradle add:
```
dependencies {
   ...
   compile project(':react-native-linear-gradient')
}
```

3. and finally, in android/src/main/java/com/{YOUR_APP_NAME}/MainActivity.java for react-native < 0.29, or android/src/main/java/com/{YOUR_APP_NAME}/MainApplication.java for react-native >= 0.29 add:
```javascript
//...
import com.BV.LinearGradient.LinearGradientPackage; // <--- This!
//...
@Override
protected List<ReactPackage> getPackages() {
 return Arrays.<ReactPackage>asList(
   new MainReactPackage(),
   new LinearGradientPackage() // <---- and This!
 );
}
```

or 直接執行 `react-native link` 會自動幫你 iOS 與 Android 的 Library 都連結好。 

**index.android.js**
```javascript
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

import LinearGradient from 'react-native-linear-gradient';

export default class Depends extends Component {
  render() {
    return (
      <LinearGradient colors={['#FFFFFF', '#00A8A8']} style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
        <Text style={styles.instructions}>
          To get started, edit index.android.js
        </Text>
        <Text style={styles.instructions}>
          Double tap R on your keyboard to reload,{'\n'}
          Shake or press menu button for dev menu
        </Text>
      </LinearGradient>
    );
  }
}

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

AppRegistry.registerComponent('Depends', () => Depends);
```