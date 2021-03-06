# 使用 Device 相關的 API

書中這章節將藉由原先的 WeatherProject 衍伸的 SmartWeather 來做範例, 示範以下存取裝置的 API 如何使用, 這邊會直接解說程式碼一邊介紹。

* AsyncStorage //存取簡單資料的示範
* CameraRoll //讀取 iOS 內建相簿圖片的示範
* navigator.geolocation //取得座標位置的示範

**在開始之前, 如同之前的 WeatherProject**
> iOS 預設僅允許 https 的 request, 若是用 http 的網址則會回傳上面這個 error message, 或是要修改 info.plist (並rebuild project)

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

**程式碼的結構如下**
```
./
|-- Button
|   |-- index.js
|   `-- styles.js
|-- LocationButton
|   |-- index.js
|   `-- styles.js
|-- Forecast
|   `-- index.js
|-- PhotoBackdrop
|   |-- camera_roll_example.js
|   |-- flowers.png
|   |-- index.js
|   `-- local_image.js
|-- index.android.js
|-- index.ios.js
`-- weather_project.js
```

### Button
**Button/index.js**

宣告 Button component, 透過屬性的設定來決定 onPress 的行為 以及 渲染在元件上的文字。

Source Code:
```javascript
import React, {
  Component,
} from 'react';

import {
  Text,
  View,
  TouchableHighlight
} from 'react-native';

import styles from './styles';

class Button extends Component {
  propTypes: {
    onPress: React.PropTypes.func,
    label: React.PropTypes.string
  }

  render() {
    return (
      <TouchableHighlight onPress={this.props.onPress}>
        <View style={[styles.button, this.props.style]}>
          <Text>
            {this.props.label}
          </Text>
        </View>
      </TouchableHighlight>
      );
  }
}

export default Button;
```

**Button/styles.js**

設定按鈕的外觀。

Source Code:
```javascript
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  button: {
    backgroundColor: '#FFDDFF',
    width: 200,
    padding: 25,
    borderRadius: 5
  },
});

export default styles;
```

***
### LocationButton
**LocationButton/index.js**

在按鈕按壓後, 觸發取得當前位置的 API (使用方式可以參考下面補充, 從 Web API 延伸無須 import 即可使用), 並覆寫 Button 的 style (實際上兩者定義的 styles.js 內容是相同的)。

> Android 的位置權限需透過修改 AndroidManifest.xml 增加下面這行才能值行。

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```
***
> [MDN reference](https://developer.mozilla.org/zh-TW/docs/Web/API/Geolocation/getCurrentPosition)

> navigator.geolocation.getCurrentPosition(success[, error[, options]])
> * **success** 一個回呼函式(callback function) 會被傳入一個Position 的物件。
> * **error** (選擇性) 一個選擇性的錯誤回呼函式(callback function)，會被傳入一個 PositionError 的物件。
> * **options** (選擇性) 一個選擇性的 PositionOptions 的物件。

> [追蹤位置的方式](https://developer.mozilla.org/zh-TW/docs/Web/API/Geolocation/watchPosition)

Source Code:
```javascript
import React, {
  Component,
} from 'react';

import Button from './../Button';
import styles from './styles';

class LocationButton extends Component {
  propTypes: {
    // 由外部傳入必要的屬性
    onGetCoords: React.PropTypes.func.isRequired
  }

  _onPress() {
    // 呼叫實作 react 實作 MDN 標準介面的 API, 並將成功回傳的 initialPosition 資訊送給查詢天氣的 function
    navigator.geolocation.getCurrentPosition(
      (initialPosition) => {
        this.props.onGetCoords(initialPosition.coords.latitude,
          initialPosition.coords.longitude);
      },
      (error) => {alert(error.message)},
      {enableHighAccuracy: true, timeout: 20000, maximumAge: 1000}
    );
  }

  render() {
    return (
      <Button label="Use CurrentLocation"
        style={styles.locationButton}
        onPress={this._onPress.bind(this)}/>
      );
  }
}

export default LocationButton;
```

***
### Forecast
**Forecast/index.js**

跟 WeatherProject 版本的相差不多, 唯一的不同就是把 style 抽出去。

Source Code:
```javascript
import React, {
  Component,
} from 'react';

import {
  Text,
  View,
  StyleSheet
} from 'react-native';

import styles from '../styles/typography';

class Forecast extends Component {
  render() {
    return (
      <View style={forecastStyles.forecast}>
        <Text style={styles.bigText}>
          {this.props.main}
        </Text>
        <Text style={styles.mainText}>
          Current conditions: {this.props.description}
        </Text>
        <Text style={styles.bigText}>
          {this.props.temp}°F
        </Text>
      </View>
    );
  }
}

const forecastStyles = StyleSheet.create({
  forecast: {
    alignItems: 'center'
  }
});

export default Forecast;
```

***
### weather_project.js
接下來的程式碼比較複雜, 將說明直接註解在程式碼內文。

Source Code:
```javascript
import React, {Component} from 'react';
import {
  StyleSheet,
  Text,
  View,
  TextInput,
  AsyncStorage,
  Image
} from 'react-native';

import Forecast from './Forecast';
import LocationButton from './LocationButton';

// 使用 AsyncStorage 存取資料下的 key 值, 可以透過 key 存取 value, key 常見的形式為 @AppName:KeyName
const STORAGE_KEY = '@SmarterWeather:zip';

// OpenWeather API 所需
const WEATHER_API_KEY = 'bbeb34ebf60ad50f7893e7440a1e2b0b';
const API_STEM = 'http://api.openweathermap.org/data/2.5/weather?';

// 底下已註解的方式決定引入的 PhotoBackdrop 元件從哪一個檔案來

// This version uses flowers.png from local assets
// import PhotoBackdrop from './PhotoBackdrop/local_image';

// This version has you to pick a photo
import PhotoBackdrop from './PhotoBackdrop';

// This version pulls a specified photo from the camera roll
// import PhotoBackdrop from './PhotoBackdrop/camera_roll_example';

class WeatherProject extends Component {
  constructor(props) {
    super(props);
    this.state = { forecast: null };

    // Bind event handlers, since we're using a ES2015 class.
    this._getForecastForZip = this._getForecastForZip.bind(this);
    this._getForecastForCoords = this._getForecastForCoords.bind(this);
    this._handleTextChange = this._handleTextChange.bind(this);
    this._getForecast = this._getForecast.bind(this);
  }

  // 當元件被渲染完成後會調用一次, 此時用來從 AsyncStorage 確認有無之前已存取的郵遞區號, 若有則透呼叫 _getForecastForZip
  componentDidMount() {
    AsyncStorage.getItem(STORAGE_KEY)
      .then((value) => {
        if (value !== null) {
          this._getForecastForZip(value);
        }
      })
      .catch((error) => console.log('AsyncStorage error: ' + error.message))
      .done();
  }

  // 被呼叫的同時透過 AsyncStorage 先存放這次查找的 ZIP
  _getForecastForZip(zip) {
    // Store zip code
    AsyncStorage.setItem(STORAGE_KEY, zip)
      .then(() => console.log('Saved selection to disk: ' + zip))
      .catch((error) => console.log('AsyncStorage error: ' + error.message))
      .done();
    
    // 呼叫查找天氣的 API, 並送出透過郵遞區號查找的網址
    this._getForecast(
      `${API_STEM}q=${zip}&units=imperial&APPID=${WEATHER_API_KEY}`);
  }

  _getForecastForCoords(lat, lon) {
    // 呼叫查找天氣的 API, 並送出透過座標查找的網址
    this._getForecast(
      `${API_STEM}lat=${lat}&lon=${lon}&units=imperial&APPID=${WEATHER_API_KEY}`);
  }

  // 查找的 API, 從傳入的網址取得 JSON 格式的資料存入 forecast 物件中
  _getForecast(url, cb) {
    fetch(url)
      .then((response) => response.json())
      .then((responseJSON) => {
        this.setState({
          forecast: {
            main: responseJSON.weather[0].main,
            description: responseJSON.weather[0].description,
            temp: responseJSON.main.temp
          }
        });
      })
      .catch((error) => {
        console.warn(error);
      });
  }

  // 用來處理 TextInput 手動輸入郵遞區號的天氣詢問
  _handleTextChange(event) {
    var zip = event.nativeEvent.text;
    this._getForecastForZip(zip);
  }

  // 以下的部分跟原先 WeatherProject 雷同, 除了新增透過 PhotoBackdrop 元件來決定背景圖片
  render() {
    var content = null;
    if (this.state.forecast !== null) {
      content = (
        <View style={styles.row}>
          <Forecast
            main={this.state.forecast.main}
            description={this.state.forecast.description}
            temp={this.state.forecast.temp}/>
        </View>);
    }

    return (
        <PhotoBackdrop>
          <View style={styles.overlay}>
           <View style={styles.row}>
             <Text style={textStyles.mainText}>
               Current weather for
             </Text>
             <View style={styles.zipContainer}>
               <TextInput
                 style={[textStyles.mainText, styles.zipCode]}
                 returnKeyType='go'
                 onSubmitEditing={this._handleTextChange}/>
             </View>
           </View>
           <View style={styles.row}>
             <LocationButton onGetCoords={this._getForecastForCoords}/>
           </View>
           {content}
         </View>
        </PhotoBackdrop>
    );
  }
}

import textStyles from './styles/typography.js';
const styles = StyleSheet.create({
  overlay: {
    marginTop: 20,
    height: 400,
    width: 400,
    flexDirection: 'column',
    alignItems: 'center',
    paddingTop: 5,
    paddingBottom: 5,
    backgroundColor: '#000000',
    opacity: 0.5,
  },
  row: {
    flex: 1,
    flexDirection: 'row',
    flexWrap: 'nowrap',
    alignItems: 'center',
    justifyContent: 'center',
    padding: 30
  },
  zipContainer: {
    flex: 1,
    borderBottomColor: '#DDDDDD',
    borderBottomWidth: 1,
    marginLeft: 5,
    marginTop: 3,
    width: 10
  },
  zipCode: {
    width: 50,
    height: textStyles.baseFontSize,
  }
});

export default WeatherProject;
```

***
### PhotoBackdrop
這邊因為作者在 Github 提供的做法跟書本有差, 所以這裡僅以 Github 上的範例程式做說明。
```
./
|-- camera_roll_example.js
|-- flowers.png //圖片檔
|-- index.js
|-- local_image.js
`-- styles.js
```

**PhotoBackdrop/local_image.js**

指定 local 的圖片 PhotoBackdrop/flowers.png 為背景。

Source Code:
```javascript
import React, {
  Component,
} from 'react';

import {
  Image
} from 'react-native';

import styles from './styles.js';

class PhotoBackdrop extends Component {
  render() {
    return (
        <Image
          style={styles.backdrop}
          source={require('./flowers.png')}
          resizeMode='cover'>
          {this.props.children}
        </Image>
      );
  }
}

export default PhotoBackdrop;
```

**PhotoBackdrop/camera_roll_example.js**

主要是指定 this.state.photoSource 作為背景圖片, 而 photoSource 的來源是透過 CameraRoll 的 getPhotos 取得, 使用方式可以參考程式碼中筆者添加的註解 並對應 CameraRoll.js 原始碼。(目前似乎仍只支援 iOS)

> 在使用 CameraRoll 的 API 前需要先 link CameraRoll 的 library, 可參考 [React Native link Library](https://facebook.github.io/react-native/docs/linking-libraries-ios.html) 教學, 按照上面指示就可以順利連結。
> 
> Library Path: SmartWeather/node_modules/react-native/Libraries/CameraRoll
>
> 另外還要加入為何使用手機相簿說明的 Description 到 info.plist (並rebuild project) 才能使用

```xml
	<key>NSPhotoLibraryUsageDescription</key>
	<string>This app requires access to the photo library.</string>
```

Source Code:
```javascript
import React, {
  Component,
} from 'react';

import {
  Image,
  CameraRoll
} from 'react-native';

import styles from './styles';

var PhotoBackdrop = React.createClass({
  getInitialState() {
    return {
      photoSource: null
    }
  },

  componentDidMount() {
    // 參數物件是給 getPhotosParamChecker 用的參數, 會根據這個設定產生對應需求的回傳 data
    CameraRoll.getPhotos({first: 5})
      .then((data) => {
        // 將回傳的 data 取出第三章的 uri 指定給背景 
        this.setState({
          photoSource: {uri: data.edges[1].node.image.uri}
        });
        console.log(data.edges[0].node.image.uri);
      }
      , (error) => {
        console.warn(error);
      });
  },

  render() {
    return (
      <Image
        style={styles.backdrop}
        source={ this.state.photoSource }
        resizeMode='cover'>
        {this.props.children}
      </Image>
      );
  }
});

export default PhotoBackdrop;
```

**PhotoBackdrop/index.js**

此 Demo Code 在 Github 上的版本已經與書中的差異滿多, 這邊就以 Github 的為主介紹。

**install module**

這邊使用到 [react-native-image-picker](https://github.com/marcshilling/react-native-image-picker) 這個模組, 可以參考 Github 上的模組安裝方式 (連結 library 有提供指令 `react-native link` 自動連結的方式可以使用)。 

> Android 的話另外參考網頁上開權限的方式, 修改 AndroidManifest.xml

```xml
<!-- file: android/app/src/main/AndroidManifest.xml -->
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.myApp">

    <uses-permission android:name="android.permission.INTERNET" />

    <!-- add following permissions -->
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-feature android:name="android.hardware.camera" android:required="false"/>
    <uses-feature android:name="android.hardware.camera.autofocus" android:required="false"/>
    <!-- -->
    ...
```

Source Code:
```javascript
import React, {
  Component,
} from 'react';

import {
  Image,
  Platform
} from 'react-native';

import ImagePicker from 'react-native-image-picker';
import styles from './styles.js';
import Button from './../Button';

class PhotoBackdrop extends Component {
  constructor(props) {
    super(props);
    this.state = {
      photoSource: require('./flowers.png')
    };
  }

  _pickImage() {
    // See https://github.com/marcshilling/react-native-image-picker#usage
    
    // 彈出的選單內容, 底下字面上的說明還滿清楚的
    var options = {
      title: 'Select Image',
      cancelButtonTitle: 'Cancel',
      chooseFromLibraryButtonTitle: 'Choose From Library...',
      takePhotoButtonTitle: 'Take Photo...',
      cameraType: 'back', // 'front' or 'back'
      mediaType: 'photo' // 'photo' or 'video'
    };

    // 兩個參數, 第一個是供客製化的 option 選項 (null 的話則會以 default 的選單產生), 另一個是 callback 收到 response 要如何處理
    ImagePicker.showImagePicker(
      options,
      (response) => {
        console.log('response = ', response);

        if (response.didCancel) {
          console.log('Canceled ImagePicker');
        }
        else if (response.error) {
          console.log('ImagePicker error: ', response.error);
        }
        else {
          // 替換選取到圖片的 uri
          var source;
          if (Platform.OS === 'ios') {
            source = {uri: response.uri.replace('file://', ''), isStatic: true};
          }
          else {
            source = {uri: response.uri, isStatic: true};
          }
          this.setState({ photoSource: source });
        }
      }
    );
  }

  render() {
    return (
      <Image
        style={styles.backdrop}
        source={ this.state.photoSource }
        resizeMode='cover'>
        {this.props.children}
        <Button
          style={styles.button}
          label="Load Image"
          onPress={this._pickImage.bind(this)}/>
      </Image>
      );
  }
}

export default PhotoBackdrop;
```

### Result
![](SmartWeather.jpg)
