# 平台專屬的元件
雖說 React Native 的目的在於打造跨平台一次性的寫作, 但我們仍舊能夠在這些程式中使用到平台專屬的元件, 混合這兩者一起使用。

```
./
|-- index.ios.js //React code
|-- index.android.js //React code
|-- crossplatform.js //New file
|-- switch.ios.js //New file
|-- switch.android.js //New file
```

### index.ios.js / index.android.js
轉介層
```javascript
import React from 'react';
import {
  AppRegistry,
} from 'react-native';

import CrossPlatform from './crossplatform';

AppRegistry.registerComponent('CrossPlatform', () => CrossPlatform);
```

