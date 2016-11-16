# Navigator
```
Zebreto
./
`-- src
    |-- components //所有的元件
        |-- Zebreto.js
```

### src/components/Zebreto.js
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