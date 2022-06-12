### React 学习

1. React中内联样式如何设置伪类状态？  -- 采用npm包 radium 注意版本，不能太高,0.25.2

2. React中内联样式如何设置媒体查询状态？  -- 采用npm包 从radium解构出来 | StyleRoot标签去包裹根组件，不兼容最新版18版本的react的StrictMode，但不会影响使用。

3. React中创建Ref:
* ref = "name"  -- 官方不推荐       --调用 this.refs.name 
* 函数 this.name = null   ref = { ref => this.name = ref }  --调用 this.refs.name
* createRef  this.name = createRef() ref={ this.name }  --调用 this.refs.name

4. React中想要把css写在js文件中，如果文件较小，则可以利用第三方包：styled-components 来写：
* import styled from 'styled-components'
* const newTag = styled.div.attrs({ className: 'v1 v2 v3' })
* < newTag />

5. 全局css引入，直接引入后，直接使用类名； 局部css，import style from 'path' -- style.类名

6. React 设置默认值：
* 函数组件 函数名.defaultProps={ ... }
* 类组件 static defaultProps = { ... }

7. React 传入参数类型校验：依赖第三方包--  prop-types
* import PropTypes from 'prop-types'
* 函数组件数据类型校验：App.propTypes = { name: PropTypes.string.isRequired , age: PropTypes.number }
* 类组件数据类型校验：static propTypes = { name: PropTypes.string, age: PropTypes.number }

8. React组件间传递JSX：
* < a >此处书写传递的内容< /a >
* < b >  { this.props.children } < /b > --类组件
* < b > { props.children } < /b > --函数组件
* 注意1： JSX传递时不要有空格，空格会被作为传递的数据
* 注意2： {  } 内部不能书写对象，这是React禁止的，可以写数组等(数组会被直接解构出来哟!)

9. React中state不允许被直接修改，需要通过setState方法进行异步修改，[Bug： state中的数组可以直接赋值为[ ]，仅此可以修改，应该是一个Bug!]
* 因为是异步修改，所以不能及时获取其修改后的值，要想及时获取值：方法有两种 -async 与 await -setState方法第二个参数传入回调函数 ； 可以及时拿到修改后的值。
* 数组更新，必须有唯一的标识Key，否则不Warning  ||  如何修改数组对象的值？？？？？
* 第一个参数传入的是对象的话，会将对同一数据的处理结果合并，只执行最后一个；若传入的是函数，则都会执行。

10. 受控表单中，实现Vue的v-model：
* < input name="a" value={state.a} onChange={this.fn}>< /input >
* < input name="b" value={state.b} onChange={this.fn}>< /input >
* fn = (ev) => { const name = ev.target.name  this.setState({ ** [ name ] ** : ev.target.value }) } --动态属性写为 [] 形式。

* 如果使用受控表单，说明要使用React中的state，要交给React处理数据，倘若你只想用state中的数据，不想让其被React识别为双向绑定的数据，添加以下属性之一：(1)-defaultValue (2)-readOnly

11. React中复选框作为受控表单进行绑定的注意事项：
* HTML是通过map遍历得到的结构 -- map是重点哦！！！ --forEach不得行！！！
* value绑定值 | checked是否被选中【同radio】 | onChange改变执行的函数
* onChange函数执行时应该注意: checkBox对应的数据不能直接被修改，先浅拷贝一下，然后修改对应索引的数据，再通过setState进行修改

12. 类组件中，state、函数等声明在render()前，获取的数据声明在render()内的return前。

13. React中的请求转发--同源策略仅限本地服务端的跨域，同属于localhost，异服务端还是得服务端允许跨域才行：
* package.json: proxy: 'http://localhost:3000'
* http-proxy-middleware  | 新建js文件解构createProxyMiddleware | module.export= app => { app.use('/api', createProxyMiddleware({ target: '', changeOrigin: true })) }

14. Redux的基本使用：     Action -- Store ←-----→ Reduce
*                                    ↓             
*                             ↖  组件             
* createStore | configureStore |  Provider   |   combineReducers    | connect    | bindActionCreators
*   Redux     |@reduxjs/toolkit| react-redux |Redux/@reduxjs/toolkit| react-redux|Redux/@reduxjs/toolkit


* redux包解构出creteStore方法，createStore()中传入一个reduce函数，之后赋值给变量Store进行统一管理，然后在对应的组件传递数据< App store={Store} />   ||   @reduxjs/tollkit解构出 configureStore, configureStore({ reducer: 对应的reduce函数 })
* reduce函数的内容： 导出一个函数 | 函数内部接受两个值：state、action | 对于state不直接操作，先赋值后拷贝 | 判断action.type 进行相应的操作 | 返回的结果是一个对象，对对应的值进行操作！

* 组件进行使用传递的数据：通过react-redux包解构出connect方法，connect((store)=>({ 组件名: store.对应的数据 }))(组件名) | 前面几个参数是决定传入哪些数据，最后一个参数是决定传给谁 | 使用的参数都在props中
* connect方法的使用：connect((state/dispatch|接受参数)=>({传递属性名到props | 可以属性名+属性值 | 可以函数}))

* 组件传递指令：组件中调用props.dispatch()方法，内部传入一个对象----属性名为type时属性值为自定义指令 | 属性名为payLoad时传递额外附加参数！

* Actions触发器：从redux导出 bindActionCreators, 在mapDispatchToProps中: dispatch=> ({ ...bindActionCreators(AppActions模块, dispatch) })

* 组件拆分与合并：当有不同的数据存储在redux中时，应该进行组件拆分，再通过combineReducers进行结合数据。

15. Redux中间件的使用：
*                                     MiddleWare                              
*                                 ↗       ↓                                  
*                              Action    Store ←---双向箭头--→ Reduce                 
*                                          ↓                                  
*                                     ↖  组件                                

* 中间件产生原因：原来的模式适合同步任务的执行，对于异步任务可能会出现问题，有了中间件以后，我们就可以直接使用中间件去将异步任务先处理完成，然后将同步任务+异步任务发送给Store进行进一步的处理。
* 中间件实际上就是一个函数，通过redux导出applyMiddleware中间件实现。
* 中间件用来处理异步任务, applyMiddleware拦截action进行异步操作后将指挥权再次交给store！！！！  

* 使用：const store = createStore(reducer, applyMiddleware(middle))
* function middle({getState, dispatch}) {
*  语法要求middle内部再次定义一个函数，appliMiddleware会自动执行该函数 
*  return function(next) {
*    // 此处可以书写异步函数
*    return function(action) {
*       console.log('action指令触发后，此处代码会被执行!')
*       此处异步操作执行完毕后，还需要将action 交给store进行操作
*       return next(action)
*    }
*  }
* }

* 使用thunk异步解决方案： 从redux-thunk中导出thunk,然后传递给中间件applyMiddleware，这样的话可以直接在对应的action指令中这样写：
* 同步--export const increment = (payload) => ({ type:'increment',payload })  ==> 
* 异步--export const increment = () => async (dispatch) => { const data = await axios.get('url').then(res=> res.data);  dispatch({ type:'increment', payload: data }) }
* 思考： bindActionCreators会不会受到影响，使用中间件后？ 回答：不会，中间件只是劫持了指令，之后还会通过dispatch将指令返回给store!!!
* 问题/缺陷：指令和异步函数混合，不太好！！！


* 使用saga异步解决方案：解决了thunk异步解决方案的缺点！
* 从redux-saga中导出中间件：createSagaMiddleware函数 const sagaMiddleware = createSagaMiddleware()
* 从redux中导出中间件：applyMiddleware函数
* const store = createStore(reducer, applyMiddleware(sagaMiddleware))
* action中只需要传递type指令，payload不需要在这里边传递！

* 1.const sagaMiddleware = createSagaMiddleware()
* 2.const store = createStore(reducer, applyMiddleware(sagaMiddleware))
* 3.sagaMiddleware.run(.saga文件)
* --------------------------------------------------------------------
* 4.const sagaMiddleware = createSagaMiddleware()
* 5. const store = configureStore({ reducer: '', middleware: [] })
* 6.sagaMiddleware.run(.saga文件)
* 注意：！！！ .saga文件依赖于ES6+的Generator语法糖---- { takeEvery, put, all } -> redux-saga

* 16.todoList中代码优化：使用Facebook推出的immutable, 防止数据被修改。 {fromJS,getIn,setIn,updateIn,mergeDeep}

* 17. React-hooks:

* react-router-dom：index.js 导入 HashRouter 去包裹需要开启路由的组件
* 需要进行导航的页面 导入 Link、Route+Switch 去进行路由操作
* 注意：react-router-dom采用5.2.0版本，6~版本语法不同； 且不可采用React.StrictMode模式，否则页面不能跳转！
* <Route></Route> 在根组件写， <Link></Link> 在任意组件写
* --------------------------------------------------------------------------
* < Switch >                                                               |
*   < Link to="/home" > 首页 < /Link >                                     |
*   < Link to="/list" > 列表页 < /Link >                                   |
*   < Route path="/" component={Home} exact > 首页 < /Route >              |
*   < Route path="/home" component={Home} > 首页 < /Route >                |
*   < Route path="/list" component={List} > 列表页 < /Route >              |
*   < Route component={NotFound} > 列表页 < /Route >                       |
* < /Switch >                                                              |
* --------------------------------------------------------------------------

* 动态路由：在Route中的path写 /:id 去占位； 接受：this.props.match.params
* params传参： 在Link中的to写 /?cb='传递的内容'; 接受：this.props.location.search 【需要qs转换】
* 二级路由： 为了防止因为一级路由的变化而导致二级路由还得重新修改，我们采用 Link的to=this.props.match.url 拼接 /二级路由  && Route的path=this.props.match.path 拼接 /二级路由
* 路由重定向：导入Redirect标签，添加to属性
* 路由跳转：history中导入createHashHistory, createHashHistory().push('/path')
* 路由懒加载：第三方工具包@loadable/component 解构出 loadComponent, 提高单页面应用网站首屏加载速度！
* × import Home from './components/home'
* const Home = loadComponent(() => import('./components/home'))
* 非路由组件传递路由信息，拿到路由信息的方法：解构出 withRouter, 去包裹非路由组件[非路由组件导出时！]
* export default withRouter(Test)

* 路由守卫： -------------------------------------------------------------------------------------
* 原本==>< Route path="/list" component={List} > 列表页 < /Route >                               |
* 现在==>< Route path="/list" render="{(props)=> { if(conditon) { return < List {...props} /> }  |
* else { return < Redirect to="/path" /> } } }" > 列表页 < /Route >                              |
* 进阶： guard.js写成组件                                                                        |
*                                                                                                |
* import React, { Component } from 'react'                                                       |
* import { Route,Redirect } from 'react-router-dom'                                              |
* export default class guard extends Component {                                                 |
*   render() {                                                                                   |
*     console.log(this.props)                                                                    |
*     const { component: Component, ...rest } = this.props                                       |
*     【此处...rest是为了将外边传递的path和其他路由信息传递给 <Route>】                           |
*     return (                                                                                   |
*       <Route {...rest} render={(props) => {                                                    |
*         if (true) {                                                                            |
*           return <Component {...props} />                                                      |
*      【此处...props是为了将接受的数据或者发送的数据传递给目标Component】                        |
*         } else {                                                                               |
*           return <Redirect to="/home" />                                                       |
*         }                                                                                      |
*       }} />                                                                                    |
*     )                                                                                          |
*   }                                                                                            |
* }                                                                                              |
*                                                                                                |
* < Guard path="/listGuard" component={List} > 列表页 < /Guard > 给自定义的guard组件传参          |
* ------------------------------------------------------------------------------------------------

* hooks:

* -------------------------------------
* useState-hook:
* const [personState, personSetState] = useState({ "name": "zs", "age": 40 })
* {personState} - 直接展示
* onClick={()=> { personSetState({...personState, "name": "ls"}) } }
* -------------------------------------


* -------------------------------------
* useEffect-hook 与 useLayoutEffect-hook
* useEffect 用于组件加载完毕后需要执行的生命周期函数 | useLayoutEffect 用于组件加载前执行的函数 | 二者用法类似
* useEffect(()=> { //此处书写组件加载完成、组件更新的函数  return () => { //此处写组件卸载的函数 } })
* -------------------------------------


* -------------------------------------
* useContext-hook 与 createContext-hook
* const personContext = createContext({})
* 提供方 <personContext.Provider store={store}></personContext.Provider>
* 接收方 <personContext.Consumer> { store => { return (  {store}  { store2 => { return ( {store2} ) } }  ) } } </personContext.Consumer>
* 很明显，personContext.Comsumer很复杂，这是提供了useContext钩子，直接 接收方： const storeInfo = useContext(personContext)
* -------------------------------------


* -------------------------------------
* useReducer-hook:
* useReducer可以看作是useState的替代品,解决了代码复用性的问题，秩序设置初始值，相同的逻辑操作通过reducer函数进行指令操作
* function reducer(state, action) {
*   switch(action.type) { case 'add': return {...state, num: state.num + 1} default: return {...state} }
* }
* const [state,dispatch] = useReducer(reducer, {初始值})
* onClick={() => { dispatch({type: 'add'}) } }
* -------------------------------------


* -------------------------------------
* useCallback 与 useMemo 与 memo --解决性能消耗问题！
* 解决加载问题，防止组件被重复加载，让其没有发生变化时不加载，根组件传递方法给子组件时使用，防止根组件发生变化，接受数据的子组件发生变化
* memo去包裹被复用的子组件，防止数据没有发生变化也被重新渲染
* const increment = useCallback(()=> { setAgeState(ageState - 1) }, [ageState]) --只要ageState不变化，即使根组件的其他数据发生了变化，increment也不会变化，也不会让接受increment的子组件被重新渲染。
* const person = useMemo(() => { return getData() }, []) --对于异步函数获取的数据，如果其值没有变化，使用useMemo可以防止其在根组件数据发生变化后被重新渲染，影响性能！
* -------------------------------------


* -------------------------------------
* useRef-hook 与 createRef-hook
* useRef 可以保存状态[除非手动修改，否则状态不会改变]，适用于获取、修改数据 | createRef 适用于获取数据,但是createRef无法获取函数组件实例，只能获取类组件实例。
* -------------------------------------


* -------------------------------------
* useInperativeHandle-hook
* 相当于子组件的一个中间件，接受两个参数，第一个参数是父组件创建的ref实例，届时父组件获取到的就是当前的中间件实例； 而当前的中间件可以提供一些方法给父组件调用以修改子组件的值！
* -------------------------------------


* -------------------------------------
* 自定义hook:
* 产生原因：需要复用的生命周期钩子，不能放在函数里边，然后调用该函数，这是不可行的！
* 解决办法：*在该函数名字前面加上use关键字，这样其就从一个普通函数变为了自定义hook!
* -------------------------------------
