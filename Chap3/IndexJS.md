# index.ios.js / index.android.js
之所以將原先 init 建立的 Component (參考 FirstProject 建立的 index.ios.js 內容) 搬移出去到 WeatherProject.js 是因為想要共 code (iOS 與 Android 可以一同使用的部分希望可以整合到單一檔案)

```javascript
// 引用
import {
  AppRegistry,
} from 'react-native';
import WeatherProject from './WeatherProject';

// 註冊頂層元件給 AppDelegate.m
AppRegistry.registerComponent('WeatherProject', () => WeatherProject);
```