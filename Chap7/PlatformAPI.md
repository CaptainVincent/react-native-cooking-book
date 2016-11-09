# Device 相關的 API 使用
書中這章節將藉由原先的 WeatherProject 衍伸的 SmartWeather 來做範例, 示範以下存取裝置的 API 使用, 這邊會一面解說程式碼一邊介紹。

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

宣告 Button component
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