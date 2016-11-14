# 大型專案示範

以 [bonniee/learning-react-native/Zebreto](https://github.com/bonniee/learning-react-native/tree/master/Zebreto) 為例

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
![](ZebretoDecks.png)
