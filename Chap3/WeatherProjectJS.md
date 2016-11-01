# WeatherProject.js
```javascript
// 引用
import React, {
  Component,
} from 'react';
import {
  StyleSheet,
  Text,
  View,
  TextInput,
} from 'react-native';
import Forecast from './Forecast';

class WeatherProject extends Component {
  constructor(props) {
    // property
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

  // Callback function, 用來處理輸入匡內文送出時的變化
  _handleTextChange(event) {
    console.log(event.nativeEvent.text);
    this.setState({
      zip: event.nativeEvent.text
    });
  }

  // 渲染內容
  // 將 Hardcode 寫死的 fake 資訊傳給 Forecast 元件
  // 宣告一個 Text Input 元件並將送出文字的行為串接 _handleTextChange 函式
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          You input {this.state.zip}.
        </Text>
        <Forecast
          main = {this.state.forecast.main}
          description = {this.state.forecast.description}
          temp = {this.state.forecast.temp}/>
        <TextInput
              style={styles.input}
              returnKeyType='go'
              onSubmitEditing={(event) => this._handleTextChange(event)}/>
      </View>
    );
  }
}

// Text style 設定
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#4D4D4D',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  input: {
    fontSize: 20,
    borderWidth: 2,
    height: 40,
    }
});

export default WeatherProject;
```