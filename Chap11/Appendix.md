# ES6 語法

### Destructing
**ES5**
```javascript
var myObj = {a:1, b:2};
var a = myObj.a;
var b = myObj.b;
```

**ES6**
```
var {a, b} = {a:1, b:2};
```

#### 常見的 require 語法使用 Destructing

**ES5**
```javascript
var React = require('react-native');
var View = React.React;
```

**ES6**
```
var {View} = require('react-native')
```

***
### 匯入模組


**CommonJS 的模組語**

Example: other_component
```javascript
var MyComponent = React.createClass({
  :
});
module.exports = MyComponent;
```

```javascript
var OtherComponent = require('./other_component');
```
**ES6**

Example: other_component
```javascript
var MyComponent = React.createClass({
  :
});
export default MyComponent;
```

```javascript
import OtherComponent from './other_component';
```

***
### 函式撰寫
**ES5**
```javascript
render: function(){
  return <Text>Hi</Text>;
}
```

**ES6**
```javascript
render(){
  return <Text>Hi</Text>;
}
```

***
### 箭頭函式

**ES5** 

符合 ES5 的 javascript 中通常需要 bind 我們的函式以確保它的內容 (ex. this)
```javascript
var callbackFunc = function(val){
  console.log('do something');
}.bind(this);
```

**ES6**

箭頭 (fat arrow) 函式是自動綁定的
```javascript
var callbackFunc = (val) => {
  console.log('do something');
};
```

***
### 字串插值
**ES5**
```javascript
var API_KEY = 'abcdefg';
var url = 'http://someapi.com/request&key=' + API_KEY;
```

**ES6**
```javascript
var API_KEY = 'abcdefg';
var url = `http://someapi.com/request&key=${API_KEY}`;
```