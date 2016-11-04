# 組織性元件
相較於之前的單一功能的 Component, 這邊要引入的是關於畫面佈局的元件, 屬於更高階結構的存在角色。

### ListView
```javascript
<ListView
  dataSource={this.state.dataSource}
  renderRow={this._renderRow}
  />
```