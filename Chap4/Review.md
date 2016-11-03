# TEXT/IMG 元件介紹
在開始介紹新的 Component 前, 先複習一下之前常用到的兩種。
***

## Text
在 React Native 中只有 <Text> 是純文字的節點, 要以 <Text> 標籤包圍你的文字內容。

**Web version**
```html
<p> The quick <em>brown</em> fox jumped over the lazy <strong>dog</strong>.</p>
```
透過其他標籤說明文字樣式。

**React Native version**
```javascript
<Text>
  The quick <Text style={{fontStyle: "italic"}}>brown</Text> fox jumped over the lazy <Text style={{fontWeight: "bold"}}>dog</Text>.
</Text>
```
透過 style 去額外設定樣式屬性。

**Result**

<p> The quick <em>brown</em> fox jumped over the lazy <strong>dog</strong>.</p>

#### 建立樣式元件 (React Native)
透過上面 React Native version 的寫法, 很繁瑣而且程式碼會顯得很醜。

```javascript
var styles = StyleSheet.create({
  bold: {
    fontWeight: "bold"
  },
  italic: {
    fontStyle: "italic"
  }
});

var Strong = React.createClass({
  render: function(){
    return(
      <Text style={styles.bold}>
        {this.props.children}
      </Text>);
  }
});

var Em = React.createClass({
  render: function(){
    return(
      <Text style={styles.italic}>
        {this.props.children}
      </Text>);
  }
});
```
宣告完樣式元件後, 就可以使 React Native 的版本相當接近 Web 的寫法。

```javascript
<Text>
  The quick <Em>brown</Em> fox jumped over the lazy <Strong>dog</Strong>.
</Text>
```

> React Native 中偏好使用重複的 **樣式元件**, 而非重複的使用 **樣式**。

***
## Image
靜態引入的方式, 可以參考前面章節的使用方式。

**引用網路資源的方式**
```javascript
<Image source={{uri: 'http://placekitten.com/g/320/240'}}>
```

> **resizeMode** PropTypes.oneOf(['cover', 'contain', 'stretch', 'repeat', 'center']), 可以用來調整圖片 size 沒有 fit View 的 size 時如何縮放裁切。