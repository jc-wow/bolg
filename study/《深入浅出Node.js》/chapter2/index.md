## 模块机制
### CommanJS规范
Node借鉴CommanJS的Modules规范实现了一套非常易用的模块系统。  
1. 模块引用  
`var math = require('math')`
2. 模块定义
* 上下文提供了`export`对象用于导出当前模块的方法或者变量
* 在模块中还存在一个`module`对象，它代表模块自身，而`exports`是`module`的属性。
* Node中一个文件就是一个模块，方法挂载在`exports`对象上作为属性即可定义导出的方式：
```javascript
// math.js
exports.add = function() {}
```
在另一个文件中的引用方式：
```javascript
// program.js
var math = require('math');
exports.increment = function(val) {
	return math.add(val, 1);
}
```
3. 模块标识
* 模块标识就是传递给`require()`方法的函数，它必须是符合小驼峰命名的字符串，或者以.、..开头的相对路径，或者绝对路径。它可以没有文件名后缀.js。
### Node的模块实现
node中模块分为两类，一类是node提供的模块，称为核心模块；另一类是用户编写的模块，称为文件模块。
* 核心模块部分在node源码编译过程中，编译进了二进制执行文件。
* 文件模块在运行时动态加载，需要完整的路径分析、文件定位、编译执行过程，速度比核心模块慢。  
1. 模块编译
* js文件编译。编译过程中，node对获取的js文件内容进行了头尾包装，一个正常的js文件会被包装成如下的样子：

```javascript
(function(exports, require, module, __filename, __dirname) {
	var math = require('math');
	export.area = function(radius) {
		return Math.PI * radius * radius;
	}
})
```
如果要达到require引入一个类的效果，直接赋值给`module.exports`对象。  
2. 核心模块
c/c++编写的文件存放在node项目的src目录下，js文件存放在lib目录下。  
3. 发布一个npm包
* 编写模块
```javascript
exports.sayHello = function () { return 'Hello, world.';};
```  
* 初始化包描述文件
`npm init`生成package.json文件。
* 注册包仓库账号
`npm adduser`
* 上传包
`npm public <folder>`
* 安装包
`npm install`
* 管理包权限
`npm owner`


