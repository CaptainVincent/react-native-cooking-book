# Debug
介紹一些常用的除錯方式。

### Console.log
最簡單的方式是使用 console.log 輸出 debug message, 可以在 Xcode 與 Android Studio 中看見 log, 更方便的做法是 Shell 在 project 底下執行 (可以直接輸出 log 資訊在 terminal 中)

```bash
$ react-native log-ios
$ react-native log-android
```

### Device develop tool
* iOS use **Command ⌘ + D**
* Android use **Command ⌘ + M** 

Launch **Show Inspector**

![](Inspect1.jpg)

Select Instance 可以看到渲染的資訊

![](Inspect2.jpg)

### Remote debug
根據 Facebook 官方的資料, 以前是能透過 Google 的 Develop Tool 開啟 plugin **React Develop Tool**, 不過目前功能已經被閹掉了 (默哀)。

>It is currently **not possible** to use the "React" tab in the Chrome Developer Tools to inspect app widgets. You can use Nuclide's "React Native Inspector" as a workaround.

至於用 Safari 連結 iphone 的部分, 使用模擬器測試看不到 UI 元件的 view, 僅 console log 正常, 所以這部分就先暫擱, 稍後當功課再補完。

[Weinre Reference](http://people.apache.org/~pmuellr/weinre/docs/latest/)

### Red Screen of Death
一般常見語法錯誤都會造成紅色畫面, 畫面上會呈現的 Debug Message, 通常都是滿有用的資訊。