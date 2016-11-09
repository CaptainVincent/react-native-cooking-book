# 使用 Device 相關的 API
書中這章節將藉由原先的 WeatherProject 衍伸的 SmartWeather 來做範例, 示範以下存取裝置的 API 如何使用, 這邊會直接解說程式碼一邊介紹。

**程式碼的結構如下**
```
./
|-- Buttion
|   |-- index.js
|   `-- styles.js
|-- Forecast
|   `-- index.js
|-- LocationButton
|   |-- index.js
|   `-- styles.js
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