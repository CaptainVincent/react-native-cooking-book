# Navigator
```
Zebreto
./
`-- src
    |-- components //所有的元件
        |-- Zebreto.js
```

### src/components/Zebreto.js
從 Root Component 可以看到將畫面切成上方標題, 與下方的 Navigator 元件, 第一個部分可以看出 Navigator 需要的結構

* ref='navigator' //該元件透過 refs.navigator 取用此 Navigator
* initialRoute //定義起始的 route object 內容
* _renderScene //提供 Navigator callback 來執行渲染的函式。

```javascript
  render() {
    return (
      <View style={styles.container}>
        <Heading/>
        <Navigator
          ref='navigator'
          initialRoute={{name: 'decks'}}
          renderScene={this._renderScene}/>
      </View>
    );
  }
```

根據當前 route.name 來決定要渲染的 View 為何, 並透過 props 來傳遞 callback 給該元件 以及 執行 navigator 合適的更新動作。
> 筆者認為這邊的範例寫得不好, 根據 Flux 的資料流架構, 不應該把 callback function 傳遞給下層, 下層應該直接拋出 Action 來處理。

```javascript
  _renderScene(route) {
    switch (route.name) {
    case 'decks':
      return <Decks review={this.review}
        createdDeck={this.createdDeck}/>;
    case 'createCards':
      return <NewCard
        review={this.review}
        quit={this.goHome}
        nextCard={this.createdDeck}
        {...route.data}/>;
    case 'review':
      return <Review quit={this.goHome} {...route.data} />;
    default:
      console.error('Encountered unexpected route: ' + route.name);
    }
    return <Decks/>;
  },
```

這邊定義的 function 主要是用來處理 navigator 的交替動作。
```javascript
  review(deckID) {
    DeckActions.reviewDeck(deckID);
    this.refs.navigator.push({
      name: 'review',
      data: {
        deckID: deckID
      }
    });
  },

  createdDeck(deck) {
    this.refs.navigator.push({
      name: 'createCards',
      data: {
        deck: deck
      }
    });
  },

  goHome() {
    this.refs.navigator.popToTop();
  },
```