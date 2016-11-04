# 組織性元件
相較於之前單一功能的 Component, 這邊要引入的是關於畫面佈局的元件, 屬於更高階的結構角色。

### ListView

List
```javascript
<ListView
  dataSource={this.state.dataSource}
  renderRow={this._renderRow}
  />
```