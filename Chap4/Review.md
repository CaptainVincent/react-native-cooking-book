# TEXT/IMG 元件介紹
在開始介紹新的 Component 前, 先複習一下之前常用到的兩種。

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

### 建立樣式元件 React Native
透過上面 React Native version 的寫法, 很繁瑣而且程式碼會顯得很醜。

```javascript
var styles = StyleSheet.create(
);
```