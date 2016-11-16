# Navigator
```
Zebreto
./
`-- src
    |-- components //所有的元件
        |-- Zebreto.js
```

### src/components/Zebreto.js

從 Root Component 可以看到將畫面切成上方標題, 與下方的 Navigator 元件。
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