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

**ES6**
