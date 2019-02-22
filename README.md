# 《React设计模式与最佳实践》阅读笔记



[TOC]

## React基础

### 声明式编程

**声明式编程**（英语：Declarative programming）是一种编程范式，与命令式编程相对立。它描述目标的性质，让计算机明白目标，而非流程。声明式编程不用告诉计算机问题领域，从而避免随之而来的副作用。而命令式编程则需要用算法来明确的指出每一步该怎么做。

### React元素

#### 定义

和界面相关，只包含展示界面所必须的信息。描述了屏幕上需要显示的内容，是构成组件的基本要素。

#### 本质

实际上是一个对象，组件和DOM元素可以通过`props/children`可以不断向下嵌套。React会递归的取回整个__DOM节点树__然后渲染，过程被称为一致性比较，React DOM 和 React Native 均是通过这种方式在各自的平台上创建 UI。

* type：如果是字符串，元素就表示DOM节点；如果是函数，那么就是组件

```javascript
// DOM 元素
const element = <div>元素</div>

// element
{
        $$typeof: Symbol(react.element),
        key: null,
        props:{
            children: "元素",
            __proto__: Object
        }
        ref: null,
        type: "div",
        _owner: null,
        _store: {validated: false},
        _self: null,
        _source: null,
        __proto__: Object
    }

// 组件
const component = function (props) {
        return <div>组件</div>   
    }
// component
ƒ component(props) {
    return React.createElement(
        'div',
        null,
        '\u7EC4\u4EF6'
    );
}
```

### 与传统模板的区别

Mustache

```javascript
{{#items}}
    {{#first}}
    	<li><strong>{{name}}</strong></li>
    {{/first}}
    {{#link}}
    	<li><a href="{{url}}">{{name}}</a></li>
    {{/link}}
{{/items}}
```

React

```js
render() {
    return (
        <button style={{ color: 'red' }} onClick={this.handleClick}>
        	Click me!
        </button>
    )
}
```

#### 结论

模板高度依赖从逻辑层接收到的数据模型来显示信息，样式同样也严重依赖模板的结构。所以传统的关注点分离原则更像是技术分离。

React则是将逻辑，数据，样式均放到一个组件内。最终目标是将创建组件所用到的每项技术都封装起来，并根据它们的领域和功能进行关注点分离。



## 整理代码

### JSX

#### JSX是什么

React.createElement()`的语法糖。

#### 特性

- 通过开头字母的大小写来区分创建HTML元素或者React元素。
- 属性的设置通过XML的方式，和html元素的属性设置方式相同（属性名不能为JavaScript的保留字，样式期望传入对象，样式属性为驼峰写法）
- 子元素同样是直接嵌套在标签内
- 通过`{{}}`来调用变量或者表达式
- 只能有一个根元素（因为每个元素是一个createElement和函数，不能同时返回两个函数）
- JSX会过滤空格，需要显示的插入`{' '}`

### Babel是什么

JavasScript的转译器，将代码转译成目标环境支持的代码，React中用来处理JSX语法。

### 最佳实践

* 元素传递属性时通过展开属性来传递，避免传递整个引用对象。

* 书写JSX时换行更加易读，并且换行后需要用`（）`包括。

* 属性同样在换行后更加易读。

* 元素的渲染判断用行内的方式，`if...else`可以用三元条件运算。

* 将判断条件封装到函数内或者设置 `get`属性

* 直接将数组进行map并返回一个数组，可以清晰的创建列表

* `jsx-control-statements` 库可以将条件语句包装正逻辑标签。

### 代码风格管理

#### ESLint

规则分三个等级：`off/0`，`wran/1`，`error/2`

### 函数式编程

#### 高阶函数

接受一个函数作为参数，返回另一个函数，通常会做一些增强操作。

#### 纯粹函数

不产生副作用，不会改变自身作用域以外的任何东西。

#### 柯里化

将多参数函数转换成在不同阶段传入单个参数的函数。

```js
const add = (x, y) => x + y
```

to

```
const add = x => y => x + y

const add1 = add(1)
add1(2) // 3
add1(3) // 4
```

#### 组合

将不同的函数组合成为新函数，这样可以编写更小而简单，易于测试的纯粹函数



#### 函数式编程与UI

这主要是描述一种视角，即将UI看作传入状态的函数，通过运用函数式编程的一些概念来组织这些UI，达到两者相结合的目的。

## 开发真正可复用的组件

### 创建React组件的不同方式和选择

#### 方式

* createClass(),不需要Babel
* extends React.Component()
* function

### 状态的工作原理

对象合并

### prop类型

可复用组件，需要尽可能清晰的定义组件接口。React Docgen 自动生成文档。

### 组件解耦

这个其实不只在React中会有，在软件开发的过程中，代码的解耦规则其实大多通用，均是将相同的部分抽离出来做成有公共接口的组件，差异的部分分别放在不同的场景下去。

### 风格指南/文档

storybook 可以帮助生成风格指南



## 组合一切

### 通信方式

props，children是一种特殊的props

### 容器组件与表现组件

分离__逻辑__和 __表现__的方式，容器组件用来做数据请求以及根据业务对表现组件进行组合，表现组件则指负责接收prop来渲染各自的界面。

### mixin方案

对代码的复用提升很大，但是耦合性偏高，并且不好管理，无法保证修改后每个使用到的地方都不出问题。

### 高阶组件

高阶组件的概念即使函数式编程中的高阶函数套用到组件中来，高阶组件将组件进行能力的增强后返回新的组件。这其实类似于__继承__后修改

### recompose

一个高阶组件库，具体有哪些能力。。看了下，还不错，介绍页面就已经有了 `hook`的部分内容。

[链接](https://github.com/acdlite/recompose)

### 上下文

通过recompose来包装一次解耦context。

### 函数子组件

实例

```js
const Name = ({ children }) => children('World')
Name.propTypes = {
	children: React.PropTypes.func.isRequired,
}

// 使用
<Name>
	{name => <div>Hello, {name}!</div>}
</Name>
```



这种做法与普通组件的区别在于，数据定义在外层`Name`上，而结构和表现则是动态的`function`，子组件的自由度很高。



## 恰当的获取数据

### 数据流

单项的数据流，子组件通过事件来通知上层组件更新。

### 组件间通信

通过共同的父级来保存和设置状态，子组件通过事件来触发上层。状态的更改。

### 数据获取

服务端渲染时要在`componentDidMount`中获取数据，在`componentWillMount`获取会出现预料之外的结果。

封装一个获取数据用的高阶组件，传入参数，返回数据。类似的库` react-refetch`，让组件保持无状态。可以帮助测试，通过修改高阶组件来测试，不用再对展示的组件修改。



## 为浏览器编写代码

### 表单

#### 单一控件

直接监听`onChange`，然后设置状态和其他操作。

#### 多个控件

不可能每个控件都写一个`onChange`回调，所以共用一个处理函数。

```js
handleChange({ target }) {
	this.setState({
		[target.name]: target.value,
	})
}
```

#### 受控组件

完全控制表单组件的内容，通过将`value`设置为`state`，并对`state`进行管理来达到目的

#### 自动创建表单

`react-jsonschema-form`,通过建立schema对象来生成表单。

### 事件

React是用的合成事件，保证不同浏览器中的事件返回值是一样的。并且事件实际上只在根元素上添加了监听，通过实践冒泡机制来做__事件代理__。另外事件会被回收。

### Ref

ref属性需要绑定一个方法，这个方法的参数可能是DOM元素也可能是组件实例，这样就可以访问到指定的组件。这个方法在挂在和卸载都会调用，卸载时会传入`null`来释放内存。

应该尽量少用`ref`因为这样会更不好控制代码的耦合，并且这样的方式更接近命令式编程。

### 动画

`react-addons-css-transition-group`提供了声明式的动画创造方式。

`react-motion`更强大的动画库

### 图形

SVG



## 美化组件

### CSS in JavaScript

为了解决css的问题，例如依赖管理，命名空间，样式隔离等问题

### 行内样式

React 将样式和组件严密的聚合在一起，有利于组件的复用。但是同样也会存在一些问题，例如无法使用伪类，媒体查询，以及服务端渲染的性能问题等等。

#### 解决方案

* Radium

  把样式和伪类等些一块的库

* CSS 模块（webpack）

* Styled Component

  也是一个库，更加方便的封装样式。



## 服务端渲染的乐趣与益处

### 通用应用

__同构应用__，服务端和客户端的应用看起来一摸一样，这种模式再React中叫__通用__

### 使用原因

#### SEO（搜索引擎优化）

#### 通用的代码库

#### 性能更强

他们为 Instagram启用服务端渲染为了 SEO，因为对于 Instagram这类拥有大量动态内容的网站来说，服务端渲染在提升感知速度方面没什么用。

### 示例

。。。代码有点老，没跑起来。后面再实践。

### Next.js

方便地生成通用应用

## 提升应用性能

### 一致性比较与key属性

当显示组件时，React 会调用自己的渲染方法，还会递归调用子组件的渲染方法。组件的渲
染方法会返回 React元素树，然后 React根据它来判断更新 UI需要执行哪些 DOM操作。

当组件的状态发生变化时，React 会再次调用该节点的渲染方法，并将渲染结果与之前的
React 元素树进行比较。库本身非常智能，能够计算出使屏幕上产生预期变化所需的最小操作集
合。这个过程称为一致性比较，并由 React透明地管理。正因为这一点，我们才能简单地声明式
描述某个特定时刻的组件样子，并将其余的工作交给库



* 如果两个元素的类型不同，则它们渲染的树也不同；
* 开发人员可以用 key 属性标记子组件，使它们在不同渲染过程得以保留

### 优化手段

1. 通过`key`来优化，场景如下

在一个list头部插入某数据，然后react会重新调用render，react在做一致性比较时，由于第一个数据和原来的不一样，所以会重新渲染整个list，而不是在第一项的位置插入一个元素。

方法是将元素的`key`设置为数据本身。



2. shouldComponentUpdate/PureComponent

如果`shouldComponentUpdate`返回`false`，那么在父组件的更新过程中，组件及全部子元素的render不会被调用。所以在`shouldComponentUpdate`中可以对本次数据和上一次的数据做对比来决定是否重新渲染。



继承`PureComponent`的组件，会浅比较数据来判断是否更新，只应在必要时使用 PureComponent ，而且只能在测试完性能后，弄清哪个组件
运行耗时太长后才可以使用它。

### 常用解决方案

1. `why-did-you-update`，会检测组件是否需要重新渲染并给出提示。

2. 在渲染函数中使用行内的箭头函数会造成，react对比时以为传入了新的函数，而实际是因为箭头函数会返回一个新的函数，所以尽量不要用行内的函数。

3. 组件的props传值，要避免在传递props时新建引用类型，因为引用类型在做对比时始终不相等，所以会造成重新渲染。可以在render函数的顶部来声明这个值，保证始终传递的同一个prop。



### 工具和库



#### 不可变数据

`PureComponent`只会浅比较数据，所以如果变化的时引用类型的子项，则不会引发渲染。可以通过`Object.assign`或者展开运算符来新建一个对象触发渲染。

#### 浏览器监控性能

chrome的 chrome-react-perf插件

#### Babel插件

* React 常量元素转换器`babel-plugin-transform-react-constant-elements`,会将不随props改变的静态元素抽离，避免多余的调用`createElement`

* React行内元素转换器`babel-plugin-transform-react-inline-elements`，优化JSX声明（或者createElement调用）。

两个插件都只应该在生产环境中启用，因为它们会使开发环境中的调试变得很困难



## 测试与调试

### 测试的好处

测试应用的主要功能可以使代码库更加稳固。

测试有助于避免代码回退，因为它会告诉开发人员新的代码能否通过旧的测试。

TDD开发模式。test driven development，测试驱动开发，先编写测试，再编写能够通过测试的代码。



### JEST

`react-addons-test-utils`，fb开发的测试工具。

### Mocha

灵活的测试框架。

### React JavaScript 测试工具

`enzyme`AirBnb基于TestUtils构建。可以模拟事件

### 测试覆盖率

Istanbul。

### React 开发者工具

chrome react插件



### 错误处理

`react-component-errors`库将组件包裹在`try...catch`中，方便调试。

## 需要避免的反模式

### 用props初始化状态

违背了单一数据原则，数据更新会有问题。可以单独使用另一个语义化的词来初始化状态。

### 修改状态

不通过`setState`更改状态。会造成页面不能及时刷新，以及影响后续`setState`的调用。

### 将数组的索引作为key

会造成列表中的数据混乱，在顺序上可以体现出来，在数组开头新增数据的情况下，react去对比key，得到的结果只会是新数据比原来的多一项，所以只会在末尾加一项，如果数据还对应了其他的内容，就会出现内容和顺序对不上的情况。解决方法是将各项的值作为key，或者使用唯一标识符

### 在DOM元素上展开props对象

无法准确控制props的内容。



## 未来的行动

### 为React做贡献

改bug，good first issue ，社区解答，提供npm包



## 读后感

本书的内容建立在已经入门了React的情况下。在会用的基础上，梳理了React声明式编程的概念。然后比较深入的刨析了__JSX语法__。因为React就是组件式的开发，所以给出了关于组件的一些__设计模式__， 如高阶组件，函数子组件，组件间数据的管理。

结合实际，本书给出了在浏览器中处理表单，事件，动画，图形的一些解决方案，包含一些库。简单的给出了SSR的示例。

对于性能给出了一整套方案，从代码插件到浏览器控制台的插件，在编写时来监控并优化性能。

到了测试环节，React可以组合Jest或者Mocha这样的测试库，提到了测试驱动开发，讲到了诸多使用测试框架的好处。

最后，强调了4个需要避免的反模式，

* 用props初始化状态，在更新这个状态时可能会影响到上层数据，违背了单一数据原则
* 不通过`setState`直接修改状态，页面可能不能及时刷新，并且影响后续`setState`
* 将数组索引用作key，会使得React一致性比较的结果不符合预期
* 直接在元素上展开整个`props`，内容不可控。

React生态已经比较完善，可以参与问答，修复bug，提供库中来。