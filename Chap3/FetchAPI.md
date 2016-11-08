# Fetch API

### WeatherProject.js (Simple Case)
```javascript
  _handleTextChange(event) {
    console.log(event.nativeEvent.text);
    // 試著在原來的 handle 中增加了網路 fetch 的 API 去抓取網頁回來輸出到 console
    fetch('https://captainvincent.gitbooks.io/index.html')
      .then((response) => response.text())
      .then((responseText) => {
        console.log(responseText);
      });

    this.setState({
      zip: event.nativeEvent.text
    });
  }
```

### Result
鍵入 111 送出後可以在 console 抓回的網頁內容。

![](FetchPage.jpg)

> **React Native fetch() Network Request Failed.**

> iOS 預設僅允許 https 的 request, 若是用 http 的網址則會回傳上面這個 error message, 或是要修改 info.plist (並rebuild project)
```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

### Integration OpenWeatherMap API
主要是將回傳的 JSON 資料導入 JSON  格式的容器中, 再取出來對內部資料設定。

### WeatherProject.js
```javascript
import React, {
  Component,
} from 'react';

import {
  StyleSheet,
  Text,
  View,
  TextInput,
  Image,
} from 'react-native';

import Forecast from './Forecast';

const API_KEY = 'bbeb34ebf60ad50f7893e7440a1e2b0b';

class WeatherProject extends Component {
  constructor(props) {
    super(props);
    this.state = {
      zip: '',
      forecast: {
        main: 'Clouds',
        description: 'few clouds',
        temp: 45.7
      }
    };
  }

  _handleTextChange(event) {
    var zip = event.nativeEvent.text;
    this.setState({zip: zip});
    console.log(event.nativeEvent.text);
    console.log('http://api.openweathermap.org/data/2.5/weather?q='
      + zip + '&units=imperial&APPID=' + API_KEY);
    fetch('http://api.openweathermap.org/data/2.5/weather?q='
      + zip + '&units=imperial&APPID=' + API_KEY)
      .then((response) => response.json())
      .then((responseJSON) => {
        console.log(responseJSON);
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

  render() {
    content = null;
    if (this.state.forecast !== null) {
      content = <Forecast
                  main={this.state.forecast.main}
                  description={this.state.forecast.description}
                  temp={this.state.forecast.temp}/>;
    }

    return (
      <View style={styles.container}>
        <Image source={require('./img/BackGround.png')}
               resizeMode='cover'
               style={styles.backdrop}>
          <View style={styles.overlay}>
            <View style={styles.row}>
              <Text style={styles.mainText}>
                Current weather for
              </Text>
              <View style={styles.zipContainer}>
                <TextInput
                  style={[styles.zipCode, styles.mainText]}
                  returnKeyType='go'
                  onSubmitEditing={(event) => this._handleTextChange(event)}/>
              </View>
            </View>
            {content}
          </View>
        </Image>
      </View>
    );
  }
}

var baseFontSize = 16;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    paddingTop: 30
  },
  backdrop: {
    flex: 1,
    width: null,
    height: null,
    flexDirection: 'column'
  },
  overlay: {
    paddingTop: 5,
    backgroundColor: '#000000',
    opacity: 0.5,
    flexDirection: 'column',
    alignItems: 'center'
  },
  row: {
    flex: 1,
    flexDirection: 'row',
    flexWrap: 'nowrap',
    alignItems: 'flex-start',
    padding: 30
  },
  zipContainer: {
    flex: 1,
    borderBottomColor: '#DDDDDD',
    borderWidth: 1,
    marginLeft: 5,
    marginTop: 3
  },
  zipCode: {
    width: 50,
    height: baseFontSize
  },
  mainText: {
    flex: 1,
    fontSize: baseFontSize,
    color: '#FFFFFF'
  },
  input: {
    fontSize: 20,
    borderWidth: 2,
    height:40
  }
});

export default WeatherProject;
```