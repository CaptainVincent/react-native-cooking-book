# Fetch API

### Simple Case
![](FetchPage.jpg)
```javascript
  _handleTextChange(event) {
    console.log(event.nativeEvent.text);
    // 試著在原來的 handle 中增加了網路 fetch 的 API 去抓取網頁回來輸出到 console
    fetch('https://captainvincent.gitbooks.io/index.html')
      .then((response) => response.text())
      .then((responseText) => {
        console.log(responseText);
      });

    this.setState({
      zip: event.nativeEvent.text
    });
  }
```

> React Native fetch() Network Request Failed.

> iOS 預設僅允許 https 的 request, 若是用 http 的網址則會回傳上面這個 error message, 或是要修改 info.plist
```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```