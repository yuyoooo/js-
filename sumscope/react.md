# 安装

1. 脚手架

   ```
   npm i -g create-react-app
   create-react-app Object-name
   ```

# 写法

## 函数式组件

1. 多数情况下是为了渲染dom
2. 没有生命周期
3. 没有this
4. 生命周期可以使用 useEffect 来实现
5. state 可以使用 useState 实现
6. 无状态组件（可以通过hook修改）

## 类组件

1. `class App extends React.Component` 创建组件
2. 渲染使用 `render () {return}`
3. 包含生命周期

# 组件

## 生命周期

1. constructor
2. componentDidMount - 挂载
   - 在DOM被渲染之后运行
3. componentWillUnmount - 卸载
   - 在DOM卸载之前运行
4. static getDerivedStateFromError() - class组件抛出错误，他可以用于渲染备用UI
5. componentDidCatch - calss组件抛出错误，可以用于打印错误信息

## state

### 设置

```js
this.state = {}
const [name, setName] = useState(0)
```

### 修改

```js
this.setState({
	name: newVal
})
// 防止异步问题
this.setState((state, props) => ({
  name: state.name + props.increment
}));
```

### 读取

```js
this.state.name
```

### 注意

1. 构造函数是唯一可以给 `this.state` 赋值的地方
2. 不要直接修改state
3. state更新可能是异步的
4. state更新会被合并

## 数据的流动（单向）

# 事件处理

## 基本

1. 绑定：`onClick={fn}`
2. 阻止默认事件：`e.preventDefault();`
3. this绑定2种方法：
   - fn是箭头函数
   - 箭头函数调用`onClick={() => this.handleClick()}`
   - bind
4. 传参的2种方式
   - `onClick={(e) => this.deleteRow(id, e)}`
   - `onClick={this.deleteRow.bind(this, id)}`

# this

1. class的方法默认**不绑定**this

# 条件渲染（if）

> 核心还是数据驱动状态控制组件return的内容

## 切换赋值

```js
let Tag
if(this.state.type === 'Success') {
   Tag = <Success />
}else{
   Tag = <Error />
}
```

## &&

```js
{this.state.type === 'Success' && <Success />}
```

## 三目运算符

```js
{this.state.type === 'Success' ? <Success /> : <Error />}
```

## 阻止渲染

```js
// return null 即可
if(!this.props.show) {
  return null
}
return <div className="Success">Success</div>
```

# 列表渲染（for）

1. map,filter...
2. key

# 表单

# 状态提升

## 基本

1. 使用父组件来为子组件提供公共数据 state
2. 子组件修改数据使用 `this.props.onEmit()`

# 组合&&继承

## 包含关系（类似于slot）

使用 **props.children**

```react
// 组件
function Self(props) {
  return (
    <div className='Self'>
      {props.children}
    </div>
  );
}
// 父组件中使用
<Self>
   <input type="text" />
</Self>
```

## 自定义插槽

类似于传参

```react
function Self(props) {
  return (
    <div className='Self'>
      {props.left}
      {props.right}
    </div>
  );
}
<Self left={<input type="text" />} right={<b>right</b>} />
```

# 高级

ref

## 路由懒加载 - lazy()

```react 
const Home = lazy(() => import('../pages/Home/Home'))
```

## Context

> 数据注入

### 创建

```react
const {Provider, Consumer} = React.createContext('ddd');
```

### 使用

```react
// 传递数据
<Provider value={this.state.name}>
   <Son></Son>
</Provider>

// Son 组件使用数据
<Consumer>
   {value => <b>{value}</b> }
</Consumer>
```

### 更新

```react
// 传递Provider的更新函数下去即可
constructor(props) {
    super(props)
    this.state = {
      name: 'About111',
      handle: this.handle
    }
  }
  handle = () => {
    this.setState(state => ({
      name: '8888888888'
  }))
}
<Provider value={this.state}>
   <Son></Son>
</Provider>

// son 
<Consumer>
   {({name,handle}) => <b onClick={handle}>{name}</b> }
</Consumer>
```

### 注意

1. 防止重新渲染的问题

## Refs 转发

### 用于获取子组件对象

```react
this.sonRefs = React.createRef()

<SonRefs ref={this.sonRefs} />
```

### 用于获取DOM对象

```react
this.inputRefs = React.createRef()

<input type="text" ref={this.inputRefs} />
```

## Fragments / <></>

> 允许存在多个节点

```react
render() {
  return (
    <React.Fragment>
      <td>Hello</td>
      <td>World</td>
    </React.Fragment>
  );
}

render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
```

## 高阶组件

## Portals

> Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

```react
class Portal extends React.Component {
  body = document.getElementById('root')
  render() {
    return ReactDOM.createPortal(<div>Portal</div>, this.body)
  }
}
```

## Render prop

> **render prop 是一个用于告知组件需要渲染什么内容的函数 prop**

# API

# Hooks

## useState

## useEffect

### 执行一次

```react
// 作为componentDidMount使用
useEffect(() => {}, [])
```

### 监听数据

```react
useEffect(() => {}, [list])
```

### 卸载回调

```react
// componentWillUnMount
useEffect(() => {
	return () => {
		// 函数会在卸载时调用	
	}
})
```

### 每次render都会执行

```react
// 没有第二个参数
useEffect(() => {})
```



## useCallback

# redux

## 安装

```xshell
npm install --save react-redux
```

## 开发思想

> 容器组件与展示组件分开

1. 容器组件：数据获取 状态更新
2. 展示组件：基于props展示页面

# prop-types

> 参数props校验
>















