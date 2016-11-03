# 觸控元件

***
### TouchableHighlight
任何回應使用者觸控事件的介面元素都應該被 TouchableHighlight 標籤包裝著, 該元件會與以下事件建立掛鉤, 開發者再將其行為定義即可。

* onPressIn
* onPressOut
* onLongPresswl6

**Github: bonniee/learning-react-native/Touch/PressDemo.js**
```javascript
import React, { Component } from 'react';
import {
  StyleSheet,
  Text,
  View,
  TouchableHighlight
} from 'react-native';

class Button extends Component {
  constructor(props) {
    super(props);
    this.state = { pressing: false };
  }

  _onPressIn = () => {
    this.setState({pressing: true});
  }

  _onPressOut = () => {
   this.setState({pressing: false}); 
  }

  render() {
    return (
      <View style={styles.container}>
        <TouchableHighlight
          onPressIn={this._onPressIn}
          onPressOut={this._onPressOut}
          style={styles.touchable}>

          <View style={styles.button}>
            <Text style={styles.welcome}>
              {this.state.pressing ? 'EEK!' : 'PUSH ME'}
            </Text>
          </View>

        </TouchableHighlight>
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
    color: '#FFFFFF'
  },
  touchable: {
    borderRadius: 100
  },
  button: {
    backgroundColor: '#FF0000',
    borderRadius: 100,
    height: 200,
    width: 200,
    justifyContent: 'center'
  },
});

export default Button;
```

***
### GestureResponder 系統
除了 "觸碰" 以外的行為定義, React Native 也提供的兩種可自訂的觸控處理: GestureResponder(較低階)、PanResponder。

> 預設由最上層的 view 來處理觸控事件; 要能處理觸控事件的 View 應該定義其四種屬性 
> * View.props.onStartShouldSetResponder
> * View.props.onMoveShouldSetResponder
> * View.props.onResponderGrant
> * View.props.onResponderReject