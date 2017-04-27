# learn_react
## 1 es6常用语法及等效的普通js语法

### 1.1 变量声明
js里面只有var，没有变量和常量的概念，而es6里面let是定义变量，const是定义常量
	
	//变量，会随时改变
	var items = [];
	let items = [];
	//常量，不允许改变
	var api_host = "http://api.baidu.com";
	const api_host = "http://api.baidu.com";

es6新增加了模板字符串的定义，支撑字符串中加入变量和换行

	// 多行
	const firstName=allen;
	const qty = "1";
	const event = "Singing"
	const content = `
	  Hello ${firstName},
	  Thanks for ordering ${qty} tickets to ${event}.
	`;

	console.log(content)

	=>
      Hello allen,
      Thanks for ordering 1 tickets to Singing.

### 1.2 箭头函数
	[1, 2, 3].map(x => x + 1); 
	等同于
	[1, 2, 3].map((function(x) {
	  return x + 1;
	}).bind(this));
### 1.3 析构赋值
例如
	
	state={UserModel,DataSourceModel}
而我们在route中只需要DataSourceModel，则可以写成

	const {DataSourceModel} = state;
而不必写成 
	
	const DataSourceModel = state.DataSourceModel

赋值别名，有时候对象的key无法直接用.的方式获取，比如state = {'bi/olap/model/DataSourceModel':{}} 这个时候就需要别名

	const { 'bi/olap/model/DataSourceModel'：DataSourceModel} = state;

更多参考：[https://github.com/dvajs/dva-knowledgemap](https://github.com/dvajs/dva-knowledgemap)

## 2 React、AntDesign、dva的一些疑惑

### 2.1 三者的关系
React是facebook推出的，多端展示的前端框架，React只是前端MVC模式中的View层，而且没有具体组件，甚至没有具体的实现，比如针对浏览器的具体实现，依赖于React-Dom组件，而不包含在React本身，针对安卓、iOS都有分别的实现，但是React层是对UI的抽象，所以能够很大程度的复用

而AntDesign是基于React和React-Dom的html版本实现，antd mobile则是针对html、安卓、iOS多端的实现

dva，前端不仅仅只有UI，还有业务逻辑，远程数据交互，本地数据存储等复杂功能，如果每个组件随心所欲的写代码，工程将无法多人协作，难以维护，所以类似后端的Controller、Service、Mapper的分层结构，dva根据React提出的数据单向流向，指定了route、component、model、service四层，其中route和component可以理解为一层，同时model中又提出了三个概念，分别是reducer（通知react重新渲染UI）、effect（异步进行数据交互）、subscribtion（监听用户url输入，键盘输入等）
![](https://camo.githubusercontent.com/c826ff066ed438e2689154e81ff5961ab0b9befe/68747470733a2f2f7a6f732e616c697061796f626a656374732e636f6d2f726d73706f7274616c2f505072657245414b62496f445a59722e706e67)
dva的更多概念： [https://github.com/dvajs/dva/blob/master/docs/Concepts_zh-CN.md](https://github.com/dvajs/dva/blob/master/docs/Concepts_zh-CN.md)
### 2.2 state model props 
state是整个单页应用的数据树，由所有的model组成
而props是单个组件需要用到的所有属性及事件，一般只是一个model的一部分
具体关系： 
	
	state > model > props

### 2.3 数据单向绑定，用户输入需要监听事件
在Angular中，数据是双向绑定的，即$scope中的数据改变，会影响界面上的数据改变
同时，界面上的input，textarea的数据改变，也会影响$scope中的数据，$scope与dva中的model类似

而React中，数据是单向绑定的，需要主动添加onChange事件,而在表单中，我们一般还需要验证表单的输入合法性，而antd中表单是通过 getFieldDecorator 方法帮我们添加了onChange等事件

### 2.4 无状态函数 VS 有状态的类 Stateless Function V.S. ES6 Classes
我们可以看到antd官网中的demo有经常用到this关键字，而dva中则没有使用this关键字，是因为
antd的官网上demo是有状态的类，所以有this，以及this.state和this.setState等属性和方法
而现在facebook官方，dva框架等都推荐无状态函数的方式，没有this，所以antd的demo和我们自己写的代码会有差异，这样做的好处是组件中不维护数据，进一步提高组件的可复用性，但是同时model的复杂度会提高，详细介绍可以参考
[http://blog.csdn.net/sinat_24070543/article/details/52621369](http://blog.csdn.net/sinat_24070543/article/details/52621369)
