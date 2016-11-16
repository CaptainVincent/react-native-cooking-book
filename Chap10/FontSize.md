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