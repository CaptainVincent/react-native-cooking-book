# 大型專案示範

以 [bonniee/learning-react-native/Zebreto](https://github.com/bonniee/learning-react-native/tree/master/Zebreto) 為例, 這是一個 **S**paced **R**epetition **S**ystem 的系統 - 提高記憶學習的策略, 藉由一系列呈現 pair 關係的牌卡, 先亂數產生許多找出 pair 關係的選擇題來出題, 再藉由 **正確與錯誤的機率** 以及 **上次出現時間的間距**, 降低或提高調整該牌卡的問題出現頻率回饋此系統, 最終達成訓練的目標。

```
Zebreto
./
|-- android
|-- iOS
|-- icon.png
|-- index.android.js
|-- index.ios.js
|-- node_module
|-- package.json
|-- screenshots
|   |-- create.png
|   |-- decks.png
|   |-- feature_image.png
|   |-- review.png
|   `-- start.png
`-- src
    |-- actions.js //Reflux 的操作
    |-- components //所有的元件
    |-- data //資料模型
    |-- stores //Reflux 儲存體
    `-- styles //各處重複的樣式定義於此
```

> 使用 Github 要下載 Repository 的子目錄時可以這麼做
> 1. git init {repo}
* cd {repo}
* git remote add origin {http://github....}
* git config core.sparsecheckout true
* echo {sub folder} >> .git/info/sparse-checkout
* git pull origin master

### Component Tree
介紹 UI 元件分部結構

**Decks**

提供新增牌組, 或是針對已存在的牌組, 切到 NewCard 的場景。
![](ZebretoDecks.png)

**NewCard**

新增牌組(建立成對的關係[front, back])、結束回到起始頁面 或是 開始進行 Review。
![](ZebretoNewCard.png)

**Review**

選出正確的配對關係 或是 結束 Review。
![](ZebretoReview.png)