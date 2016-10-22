# Weather Project

接下來將實作一個 Demo 的程式來介紹進一步的細節, 首先 `react-native init WeatherProject` 創建一個新的 project, 再接著新增兩個 file 來做練習 (WeatherProject.js、Forecast)

```
./
|-- index.ios.js //React code
|-- WeatherProject.js //New file
|-- Forecast.js //New file
```

### index.ios.js
之所以將原先 init 建立的 Component 搬移出去到 WeatherProject.js 是因為共 code (iOS 與 Android 可以一同使用的部分希望可以整合到單一檔案)

```javascript
// 引用
import {
  AppRegistry,
} from 'react-native';
import WeatherProject from './WeatherProject';

// 註冊頂層元件給 AppDelegate.m
AppRegistry.registerComponent('WeatherProject', () => WeatherProject);
```

### Forecast.js
預報的元件, 用來呈現預報內容

```javascript
```