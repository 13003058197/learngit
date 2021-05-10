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

// 事件
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

## 高阶函数 和 柯里化

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



## Hooks

### useEffect 

```jsx
// 在函数式组件中，执行模拟生命周期钩子 
import React from "react"
import ReactDOM from "react-dom"

function comp{
  const [count, setCount] = React.useState(0)
  
  React.useEffect(() => {
    // componentDidMount
    // compoentDidUpdate
    return () => {
      // componentWillUnmount
    }
  }, []) // 监听变化的属性
  
  React.useEffect(() => {}) // 监听所有变化 
}
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
  // 只能捕获后代生命周期函数中的错误
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



