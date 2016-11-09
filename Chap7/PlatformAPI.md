# 使用 Device 相關的 API
書中這章節將藉由原先的 WeatherProject 衍伸的 SmartWeather 來做範例, 示範以下存取裝置的 API 如何使用, 這邊會直接解說程式碼一邊介紹。

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

宣告 Button component, 透過屬性的設定來決定 onPress 的行為 以及 渲染在元件上的文字
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

設定按鈕的外觀
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

### LocationButton
**LocationButton/index.js**

在按鈕按壓後, 觸發取得當前位置的 API (使用方式可以參考下面補充, 從 Web API 延伸無須 import 即可使用), 並覆寫 Button 的 style (實際上兩者定義的 style 是相同的)。

> [MDN reference](https://developer.mozilla.org/zh-TW/docs/Web/API/Geolocation/getCurrentPosition)

> navigator.geolocation.getCurrentPosition(success[, error[, options]])
> * **success** 一個回呼函式(callback function) 會被傳入一個Position 的物件。
> * **error** (選擇性) 一個選擇性的錯誤回呼函式(callback function)，會被傳入一個 PositionError 的物件。
> * **options** (選擇性) 一個選擇性的 PositionOptions 的物件。

> [追蹤位置的方式](https://developer.mozilla.org/zh-TW/docs/Web/API/Geolocation/watchPosition)

```javascript
import React, {
  Component,
} from 'react';

import Button from './../Button';
import styles from './styles';

class LocationButton extends Component {
  propTypes: {
    onGetCoords: React.PropTypes.func.isRequired
  }

  _onPress() {
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

### Forecast
**Forecast/index.js**

跟 WeatherProject 版本的相差不多, 唯一的不同就是把 style 抽出去
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

### weather_project.js
接下來的程式碼比較複雜, 將說明直接註解在程式碼內文。
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

const STORAGE_KEY = '@SmarterWeather:zip';
// 使用 AsyncStorage 存取資料下的 key 值, 可以透過 key 存取 value, key 常見的形式為 @AppName:KeyName

const WEATHER_API_KEY = 'bbeb34ebf60ad50f7893e7440a1e2b0b';
const API_STEM = 'http://api.openweathermap.org/data/2.5/weather?';
// OpenWeather API 所需

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

  // 當元件被渲染完成後會調用一次
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

  _getForecastForZip(zip) {
    // Store zip code
    AsyncStorage.setItem(STORAGE_KEY, zip)
      .then(() => console.log('Saved selection to disk: ' + zip))
      .catch((error) => console.log('AsyncStorage error: ' + error.message))
      .done();

    this._getForecast(
      `${API_STEM}q=${zip}&units=imperial&APPID=${WEATHER_API_KEY}`);
  }

  _getForecastForCoords(lat, lon) {
    this._getForecast(
      `${API_STEM}lat=${lat}&lon=${lon}&units=imperial&APPID=${WEATHER_API_KEY}`);
  }

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

  _handleTextChange(event) {
    var zip = event.nativeEvent.text;
    this._getForecastForZip(zip);
  }

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
    paddingTop: 5,
    backgroundColor: '#000000',
    opacity: 0.5,
  },
  row: {
    width: 400,
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