### About browserify-node
Transformer for browserify to generate clientside code.  
[![Build Status](https://travis-ci.org/lexich/browserify-node.svg)](https://travis-ci.org/lexich/browserify-node)
[![NPM version](https://badge.fury.io/js/browserify-node.svg)](http://badge.fury.io/js/browserify-node)
[![Coverage Status](https://coveralls.io/repos/lexich/browserify-node/badge.png?branch=master)](https://coveralls.io/r/lexich/browserify-node?branch=master)

###Information
* Include file with `node.js` extention
* In this file define exports funtion which return string of client js code
```js
//test.node.js
module.exports = function() {
  return "{hello: \"world\"}";
}
```
* require `test.node.js` file using browserify infrastructure
```js
var test = require("test.node");
test.hello === "world"; // true
```
### Instalation
```
npm install browserify-node --save-dev
```

### Example
For example we want to load async text file

```js
//text.node.js
var fs = require("fs");
var md5 = require("md5");
module.exports = function() {
  var data = fs.readFileSync("./text.txt");
  var hash = md5(data);
  var json = "{content:\"" + data + "\"}";
  var filename = "test-" + hash + ".json";
  fs.writeFileSync(filename, json);
  return "function(){ return $.getJSON('" + filename + "'); }"
}
```

```js
//app.js
var text = require("text.node");
text().done(function(json) {
  console.log(json.content);
});
```
