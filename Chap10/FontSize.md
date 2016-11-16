# 字型大小的設計

針對不同螢幕大小, flexbox 通常已經可以解決排版的問題, 再來要討論字體大小的設計, 如何適應這些不同尺寸的螢幕。

```
Zebreto
./
`-- src
    `-- styles //各處重複的樣式定義於此
```

### src/styles/fonts.js
包含了字體的樣式, 與縮放的比例。

```javascript
import { StyleSheet } from 'react-native';

var fonts = StyleSheet.create({
  normal: {
    fontSize: 24,
    fontFamily: 'Avenir Medium'
  },

  alternate: {
    fontSize: 50,
    fontFamily: 'Avenir Heavy',
    color: '#FFFFFF'
  },

  big: {
    fontSize: 32,
    alignSelf: 'center',
    fontFamily: 'Avenir Medium'
  }
});

var scalingFactors = {
  normal: 15,
  big: 7
};

module.exports = {fonts, scalingFactors};
```

### src/components/NormalText.js
以 NormalText.js 為例, 示範如何調整字體大小。

```javascript
import React, { Component } from 'react';
import {
  StyleSheet,
  Text,
  View
} from 'react-native';

import {fonts, scalingFactors} from './../styles/fonts';
import Dimensions from 'Dimensions';
let {width} = Dimensions.get('window'); //透過此方式取得視窗大小

class NormalText extends Component {
  static displayName = 'NormalText';

  render() {
    return (
      <Text style={[this.props.style, fonts.normal, scaled.normal]}>
        {this.props.children}
      </Text>
      );
  }
}

NormalText.propTypes = {
  style: Text.propTypes.style
};

const scaled = StyleSheet.create({ //透過寬度除以比例關係得到想要的字體大小
  normal: {
    fontSize: width / scalingFactors.normal
  }
});

export default NormalText;
```