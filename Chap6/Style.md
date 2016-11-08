# 樣式設定
一個完整的應用, 除了功能另一個重點就是外觀, 外觀要如何刻畫是本章的學習重點。

如果想要在 React Native 與 網頁應用程式間共用相同的樣式, GitHub 上有專案提供了 React Native 的網頁樣式系統
(https://github.com/js-next/react-style

> 傳統的 CSS 具有 Global 的特性, 當引用函式庫時, 會難以維護同時引入的全域變數 (樣式), React Native 只實作了 CSS 的一個子集。


### 樣式物件
以下介紹的寫法都是樣式物件的衍伸。

**行內樣式**
```html
<Text>
  The quick <Text style={{fontStyle: "italic"}}>brown</Text> fox jumped over the lazy <Text style={{fontWeight: "bold"}}>dog</Text>.
</Text>
```
> {{fontStyle: "italic"}} 其實為一個物件, 將它傳遞給 style 屬性。

**樣式物件**
```html
var italic = {
  fontStyle: "italic"
};

var bold = {
  fontWeight: "bold"
}

<Text>
  The quick <Text style={italic}>brown</Text> fox jumped over the lazy <Text style={bold}>dog</Text>.
</Text>
```

**使用 Stylesheet.Create**

可以參考之前出現過數次的範例程式, 主要的功用是將樣式物件封裝成內部物件, 確保其 不可以變 與 不透明 的特性。