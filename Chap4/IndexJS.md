# index.ios.js / index.android.js

同 WeatherProject, 這層僅是一個轉介層。
```javascript
import React from 'react';
import {
  AppRegistry,
} from 'react-native';

import PanDemo from './PanDemo';

AppRegistry.registerComponent('PanDemo', () => PanDemo);
```