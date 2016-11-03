# Forecast.js

預報的元件, 用來呈現預報內容

```javascript
// 引用
import React, {
  Component,
} from 'react';

import {
  StyleSheet,
  Text,
  View,
} from 'react-native';

class Forecast extends Component {
  // 渲染內容, 預報的三項資訊
  render() {
    return( 
      <View>
        <Text style={styles.bigText}>
          {this.props.main}
        </Text>
        <Text style={styles.mainText}>
          Current condiction: {this.props.description}
        </Text>
        <Text style={styles.bigText}>
          {this.props.temp} F
        </Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  bigText: {
    flex: 2,
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
    color: '#FFFFFF'
  },
  mainText: {
    flex: 1,
    fontSize: 8,
    textAlign: 'center',
    margin: 10,
    color: '#FFFFFF'
  },
});

export default Forecast;
```