# 組織性元件
相較於之前單一功能的 Component, 這邊要引入的是關於畫面佈局的元件, 屬於更高階的結構角色。

### ListView

ListView 需要指名兩個屬性, 分別是 dataSource 說明呈現的內容來源, renderRow 如何根據內容去渲染的方式。
```javascript
<ListView
  dataSource={this.state.dataSource}
  renderRow={this._renderRow}
  />
```

而 ListView.DataSource 又需要時做 rowHasChanged 的判斷方法, 底下是個 Simple Case
```javascript
ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
```