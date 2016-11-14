# ES6 語法

### Destructing
ES5
```javascript
var myObj = {a:1, b:2};
var a = myObj.a;
var b = myObj.b;
```
ES6
```
var {a, b} = {a:1, b:2};
```

常見的 require 語法使用 Destructing
ES5
```javascript
var React = require('react-native');
var View = React.React;
```
ES6
```
var {View} = require('react-native')
```