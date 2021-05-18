# git

```bash
git branch -vv
git remote -vv
git branch -a 
git checkout -b dev-branch origin/dev-branch
git restore ../../.
git fetch -all

git remote add origin xxxxxx.git
git remote 本地与远程简历链接，add：是给远程仓库起个名字 

git stash save 'xxx'
git stash list
git stash apply  stash{0}
```



# Reat

```jsx
<script src="/17.0.1/react.development.js"></script>
<script src="/17.0.1/react-dom.development.js"></script>
<script src="/17.0.1/babel.min.js"></script>
<script type="text/babel">
  class A extends React.Components{
    render(){
      return (<div>hell world</div>)
    }
  }
  ReactDOM.render(<A/>, document.getElementById('root'))
</script>
```



## 入门

```jsx
// bootcdn.cn
模块化：向外提供特定功能的js程序，一般就是一个js文件
组件: 用来实现局部效果的代码和资源的集合(html,js,css,image)等
// 开发工具
React Developer Tools
// 类精简写法
class A extends React.Components{
  constructor(props){
    super(props)
    this.say = this.say.bind(this)
    this.state={}
  }
  // 实例属性
  state={}
	a = 1 
  say = () => {}
  // 原型方法
  say(){}
  render(){
    return <div></div>
  }
}
// 自定义方法中的this为undefined如果解决
this.say = this.say.bind(this)
say = () => {}
// <a href="#!" >

```

## 基础

```jsx
// 定义虚拟dom时不要加引号
const li = <li>{name}</li>
// css 模块化
1.index.css 改为 index.module.css
2.import style from "index.module.css"
	className={style.title}

// react vscode插件 es7 react...
rcc 类组件快捷键
rcf 函数组件快捷键

//事件
handler = (event) => {
  event.target.value
  event.keyCode === 13 // 回车
}
hendler2 = (name)=>{
  return event => {
    
  }
}

<input onChange = {this.handler}></input>
<input onChnage = {() => {this.handler('zs')}}></input>
<input onChange = {this.handler2("zs")} type="checkbox" checked={this.state.cheked}></input>

// uuid 和 nanoid 生成随机id的库

// 类型校验
import PropTypes from "prop-types"
class A{
  static propTypes = {
    name: PropTypes.func.isRequired,
    list: PropTypes.array.isRequired
  }
}
```

## 配置代理

```js
// 解决跨域问题
// 方法一
package.json文件中
"proxy": "http://xxx.com"

// 方法二
在src下添加setupProxy.js,内容如下(commonj语法)
cont proxy =require("http-proxy-middleware")
module.exports = function(app){
  app.use(
    // 遇到/api前缀的请求，会触发代理配置
    proxy("/api", { 
      // 请求转发给谁
      target: "https://xxx.com", 
      // 控制服务器收到的请求头中的host值
      changeOrigin: true,
      // 重写请求路径
      pathRewrite: {"^/api": ""}
    }),
    // 配置多个
    proxy("/api2", { 
      target: "https://xxx.com",
      changeOrigin: true,
      pathRewrite: {"^/api": ""}
    })
  )
}

```



## 生命周期

```jsx
// 旧的
class A {
  // 初始化
  constructor(){}
  componentWillMount(){} # unsafe
  render(){}
  componentDidMount(){}
  // 更新
  componentWillRecevieProps(){} #unsafe
  shouldComponentUpdate(){}
  componentWillUpdate(){} # unsafe  // forceUpdate() 
  render(){}
  componentDidUpdate(){}
  // 卸载
  componentWillUnmount(){} //ReactDOM.unmountComponentAtNode(ele)
}
                             
// 新的
class A {
   // 初始化
  constructor
  getDerivedStateFromProps
  render
  componentDidMount
  // 更新
  #派生状态，如果state的值取决于props，使用该方法。很少用   
  static getDerivedStateFromProps(props, state ){
    // return props
    return null 
  } 
  shouldComponentUpdate
 	render
  getSnapshotBeforeUpdate(){
    return null
  } #
  componentDidUpdate(prevProps, prevState,snapshot){}
  // 卸载
  componentWillUnmount
 }                          
                          
 // 应用
 getSnapshotBeforeUpdate(){
   return this.refs.list.scrollHeight
 }
componentsDidUpdate(prevProps, prevState, prevHeight){
  this.refs.scrollTop += this.refs.list.scrollHeight - prevHeight
}
```

## 脚手架

```jsx
> npm install -g create-react-app
> create-react-app my-react

<link rel="app-tounch-icon" /> //添加周手机主屏幕的图表
  
```



## ref

```jsx
// 字符串形式
<input ref="input"></input>
this.refs.input 
// 回调形式的ref, 会调用两次，但无关紧要
<input ref={c=>this.myInput = c} />
// createRef
myRef = React.createRef();
<input ref={this.myRef} />
```

## 事件（高阶函数 和 柯里化）

```jsx
//高阶函数：一个函数，接受一个函数作为参数或者返回一个函数
//柯里化： 通过函数连续调用继续返回函数，实现多次接受参数，最后统一处理 
changeHandle = (name) => {
  return (event) => {
    this.state({ [name]:event.target.value })
  }
}
<input onChange={this.changeHandle("name")}/>
<input onChange={this.changeHandle("password")}/>

//普通写法
<input onChange={event => {this.changeHandle("password", event.target.value)}/>
```

## 消息订阅与发布

```js
import PubSub from "pubsub-js"

var token = PubSub.subscribe("hello", function(eventName, data){})
PubSub.publish("hello", {name: "zs"})
```

## react-router

```jsx
npm i --save react-router-dom
// history.js 库 简化原生history操作
const history = History.createBrowserHistory();
const history = History.createHashHistory()

ReactDOM.render(
	<BrowserRouter>
  	<Link to="/home">Home</Link>
    <Link to="/about">About</Link>
    <Switch exact>
     	<Route path="/about" components={About}></Route>
      // 嵌套路由，不建议开启严格模式，先匹配外层，再继续匹配嵌套路由
      <Route path="/home" components={Home}>
      	<Route path="/home/news"></Route>
        <Route path="/home/message"></Route>
      </Route>
      <Redirect to="/about"></Redirect> // 兜底，重定向
    </Switch>
  </BrowserRouter>,
  document.getElementById("app")
)

// 路由组件会接受 history，location，match参数
// 用withRouter(App): 在一般组件中加上路由api
history
	go(),goBack(),goFroward(),push(),replace()
location
	pathname, search, state
match
 params, path, url;

// replace 模式
<Link replace>about</Link>

//<NavLink> 模式会添加.active类做高亮显示，也可以自定义选中的类型activeClassName="myActive"
<NavLink to="/about" activeClassName="myActive">About</NavLink>
封装一个自己的NavLink
function MyLink(){
  return <NavLink activeClassName="myActive" {...this.props}></NavLink>
}

// 多级路径问题
% PUBLIC_URL%
```

### 参数传递

 ```jsx
// params
<Route path="/home/:id" />
this.props.match.params

//search  xxx?a=1
this.props.location.search;
import qs from "querystring"
qs.stringify(obj)
qs.parse(this.props.location.search.slice(1))

// state
<Link to={{path: "/home", state: {a: 1}}} />
this.props.location.state

// 编程式导航
this.props.history.push(url, state)
this.props.history.push("/home", {a: 1})
 ```

## ant-design

```js
codesandbox，codeopen 在线编辑器
css按需引入
自定义主题颜色，通过修改less文件，来达到修改css文件的目的
```

## Redux

```jsx
// store
import {createStore} from "redux"
export default createStore(myReducer)

// reducer
cosnt initState = 0;
export default function myReducer(preState = initState, action){
    const {type, data} = action;
    switch(type){
        case "increment":
            return preState + data;
        case "decrement":
            return preStte - data;
        default:
            return preState
    }
}
// store Api
store.getState();
store.dispatch({type: "increment", data})
store.subscribe(() => {
    this.setState({}) // 监听store变化，来更新state
    ReactDOM.render(<App></App>, document.getElementById("root"))
})

```

### Action

```js
// 同步
export incrementAction(data){
    return {type: "increment", data}
}

```

### 异步Action redux-thunk

```js
// 中间件
import {createStore, applyMiddleware} from "redux"
import thunk from "redux-thunk"
createStore(myReducer, applyMiddleware(thunk))
// action
export function AsyncAction(id){
    return dispatch => {
        const data = await getInfo(id);
        dispatch({type: "increment"}, data) #dispatch
    }
}
// 调用
store.dispatch(AsyncAction(id)) #dispatch
```

### react-redux

```jsx
// 简化redux
import {connect, Provider} from "react-redux"
// 容器组件，负责和redux通信，将结果交给ui组件
// 下面两个函数返回的对象，注入到UI组件中props中 this.props.count / this.props.add(1)
function mapStateToProps(state){
    return {count: state}
}
function mapDispatchToProps(dispatch){
    return {
        add: (data)=>{ dispatch({type: "add", data}) }
    }
}
export default connect(mapStateToProps, mapDispatchToProps)(UIComponents)

// 调用容器组件
<Container store = {store} /> //或
<Provider store={store}>
    <Container store = {store} />
</Provider>
    
// mapDispatchToProps 也可以是对象
    mapDispatchToProps ={
        add: data=>（{type: "add", data},
        add: AddAction        
    }
```

### 多个reducer

```js
import {combineReducer} from "redux"
cosnt allReducer = combineReducers({
    count: countReducer,
    person: personReducer
})
export createStore(allReducer, applyMiddleware(thunk))

// 读取
this.props.person.xxx
this.props.count.xxx
```

### 纯函数、高阶函数

```js
// 纯函数 reducer必须是一个纯函数
1.同样的输入，必须得到同样的输出
2.不得改写参数数据
3.不产生任何副作用，例如：网络请求，输入输出设备

//高阶函数
参数是函数，或者返回是函数
```

### devTools

```js
chrome插件 redux devTools
js库： npm install redux-devtools-extension

import {composeWithDevTools} from "redux-devtools-extension"
createStore(allReducer, composeWithDevTools(applyMiddleware(thunk)))
```





## Hooks (16.8)

### useState

```jsx
function fn(){
    const [count, setCount] = React.useState(0)
    function add(){
        setCount(count+1)
    }
    return <button onClick={add}>count</button>
}
```



### useEffect 

```jsx
// 在函数式组件中，执行模拟生命周期钩子 

React.useEffect(() =>{}) // didMounted + 监听所有属性
React.useEffect(() => {}, []) // didMounted
React.useEffect(() =>{}, [count]) // didMounted + 监听count的变化
React.useEffect(()=> {
    return ()=>{
        // componentWillUnmount
    }
})


```

### refHook

```jsx
// 类组件中的写法
myRef = React.createRef();
<input ref={this.myRef}></input>
this.myRef.current.value

// 函数式组件ref
const myRef =  React.useRef()
<input ref={myRef}></input>
```

## 打包运行

```js
npm i serve -g
在项目根目录下运行 serve build
```

## setState

```js
// 状态更新是异步的
this.setState({}, callback)
this.setState((state, props)=>({
    count: state.count++
}), callback)
```

## 路由懒加载lazyload

```jsx
import {lazy, Suspense} from "react"
const Home = lazy(() => import("./Home"))

const dom = 
      <Suspense fallbck={<div>loading...</div>}>
          <Route path="/home" component={home}></Route>
      </Suspense>
```



## Fragment

```jsx
<framgement></framgement> 可以有key属性
<></>
```

## Contenxt

```jsx
// 祖组件和后代组件之间的通信，一般应用开发不使用，多用于插件开发中
<A>
  <B>
    <C></C>
  </B>
</A>

const MyContext = React.createContext() #
class A extends React.Component{
  render(){
    return (
      #
      <MyContext.Provider value={this.state.username}> 
        <B></B>
      </MyContext.Provider>
    )
  }
}
class C {
  static contextType = MyContext # 
  console.log(this.context) #类组件读取方式 1
}

#函数组件和类组件读取方式 2
<Mycontext.Consumer>
  {
    value => (
  		// 要显示的内容
  	)
  }
</Mycontext.Consumer>

```

## 组件优化

```jsx
//组件的问题
1.只要执行setState，即使不改变状态也会重新render
2.如果当前组件render，就会自动render子组件，效率低
// 应该怎么样
只有当state和props发生变化才render
// 方法 1
shouldComponentUpdate(nextProps, nextState){
  if(this.state.name == nextState.name){return false}
  else return false
}
// 方法2 
1.PureComponent帮我们重写了shouldComponentUpdate，浅比较 this.state == nextState
class A extends React.PureComponent{
  
}
```

## render Props 插槽

```jsx
<A> hello </A>
<A> 
  <B/>
</A>
this.props.children ;

<A render={(name) => <B name={name} />}/>

function A(){
  const name = "jay"
  return <div>
    {this.props.children} // 插槽
  	{this.props.render(name)} //预留位置，作用域插槽
  </div>
}
```

## 错误边界

```jsx
#只适用于生产环境
class Parent extends React.Components{
  state = { hasError: ""}
  // 只能捕获后代 -生命周期函数- 中的错误
  static getDerivedStateFromError(error){
    return { hasError: error  } // 返回新的state
  }

  componentDidCatch(){
    //统计错误，发送给后台
  }	

  render(){
    return （<div>
      {this.state.hasError?<h2>网络异常</h2>:<Child/>}
    </div>）
  }
}
```

## 组件通信方式总结

```jsx
1. props
2. 消息订阅与发布
3. 集中式管理 redux,dva等等
4. context
```



