# 資料模型
```
Zebreto
./
`-- src
    |-- data //資料模型
```

基本的資料模型有：牌組、卡牌

**Deck**
* name //牌組名稱
* id //識別用 id
* totalCards //牌組內含的卡牌數
* dueCards //過期的卡數量

**Card**
* front //與 back 為成對關係
* back
* deckID //隸屬於牌組的 id
* strength //根據錯誤正確率評估的強度
* dueDate //何時過期
* id //識別用 id

### src/Deck.js
```javascript
import md5 from 'md5';

class Deck {
  constructor(name) {
    this.name = name;
    this.totalCards = 0;
    this.dueCards = 0;
    this.id = md5(name);
  }

  setFromObject(ob) {
    this.name = ob.name;
    this.totalCards = ob.totalCards;
    this.dueCards = ob.dueCards;
    this.id = ob.id;
  }

  resetCounts() {
    this.totalCards = 0;
    this.dueCards = 0;
  }

  static fromObject(ob) {
    let d = new Deck(ob.name);
    d.setFromObject(ob);
    return d;
  }
}

module.exports = Deck;

```