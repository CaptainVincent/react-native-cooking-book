# 為應用程式添加背景圖片

## 靜態添加圖檔資源的方式
[官網連結](http://facebook.github.io/react-native/docs/images.html#content), 從 react-native 0.14 版開始, 加入靜態資源的方式 (內嵌入至應用程式中, 而非引用外部資源 ex. 網路上的圖片) 在 iOS 跟 Android 都可以透過相同的方式使用了

> 筆者因為參考書中的做法, 使用 image!filename 遇到了無法使用的問題, 建議還是參考使用官網建議的做法。

```
.
├── button.js
└── img
    ├── check@2x.png
    └── check@3x.png
```
