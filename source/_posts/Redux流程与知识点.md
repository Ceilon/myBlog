---
title: Redux流程与知识点以及mockjs的配置
date: 2019-08-12 17:08:17
categories: 代码
thumbnail: http://pvjq3azwj.bkt.clouddn.com/blog/20190807/flkm5jIt2lpu.png
tags:
---

> **前言** ：
>
> ##### Redux是基于数据传递的库，他能将数据放在一个store里，方便任何组件调用。，因为react是一个数据渲染框架，所以配合Redux使用非常合理
>
> ![mark](http://pvjq3azwj.bkt.clouddn.com/blog/20190807/flkm5jIt2lpu.png?imageslim)

### 安装redux

```js
npm i react-redux --save
```

#### 1. 在src根目录下创建store文件夹

#### 2.分别在store下分别创建5个文件夹分别是(actionCreators,actionTypes,index,reducer,sagas)

![mark](http://pvjq3azwj.bkt.clouddn.com/blog/20190812/0Q848OuygvJ0.png?imageslim)



**如何改变redux中的数据？**

某个react - conponents

```jsx
import store from '../store';
export default class TodoList extends React.Component {
     constructor(props) {
        super(props);
        this.state = store.getState();
        store.subscribe(this.changeInput);//subscribe()用于监听store发生变化，当store发生变化，此方法将被执行
    }
    
    
    componentDidMount(){
        const {dispatch}=store;//所有redux中的数据都是通过dispatch来进行改变的
        const action={
            type:'changeName',//store会根据值来觉得如何对数据进行处理
            nameString:'Ceilon'//传入store的参数
        }
        dispatch(action)//向store发起变更
    }
    
    //此方法用于更新conponent中的state，使其刷新组件重新渲染
   upDate=()=>{
       this,setState({store.getState()})//store.getState(用于获取当前store中的数据)
   }
    
    render(){
     
        return(
        <div>
                {this.state.name}
        </div>
        )
    }
}
```

reducer.js

```js
const defaultState = {
    name: '',
};

//reducer可以接受state，但是不能修改state
//reducer必须是一个纯函数，纯函数是指，给定固定的输入，就会产生固定的输出，输出的值不能因为条件不同而发生不同的变化，而且不会有任何副作用，副作用是指，不能直接改变state的值
export default (state = defaultState, action) => {
    if (action.type === 'changeName') {
        const newStore = JSON.parse(JSON.stringify(state));//深度克隆state
        newStore.name = action.nameString;
        return newStore  //这个数据返回给了store,store拿到新数据进行更新数据
    }
    return state
}
```



##### 下面是store下文件的解释:

**index.js：**相当于图书管理员，当action通过store来改变获取reducers中的数据。当store通过reducers查找到数据后，告诉reducers更新数据，reducers更新数据后，将数据返回给store，store传回react组件。

```js
import {createStore, applyMiddleware, compose} from 'redux'
import reducer from './reducer';
import createSagaMiddleware from 'redux-saga';//引入saga
import mySaga from './sagas'
//import thunk from 'redux-thunk'; //引入thunk


//关于redux-saga的配置
const sagaMiddleware = createSagaMiddleware();
const composeEnhancers =
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
        window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;
const enhancer = composeEnhancers(
    applyMiddleware(sagaMiddleware),
    // other store enhancers if any
);
const store = createStore(reducer, enhancer);
sagaMiddleware.run(mySaga);



//redux-thunk的配置
// const composeEnhancers =
//     window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
//         window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;
// const enhancer = composeEnhancers(
//     applyMiddleware(thunk),
//     // other store enhancers if any
// );
// const store = createStore(reducer, enhancer);
export default store;
```

**actionTypes.js:**用来管理action的常量,防止拼写错误，造成找不到错误所在

```js
export const HANDLE_INPUT_CHANGE ='handle_input_change';
export const ADD_NEW_LIST ='add_new_list';
export const EDIT_LIST ='edit_lis';
export const INITIALIZE_LIST ='initialize_list';
export const TEST_SAGA_QUERY_LIST='test_saga_query_list';
```



**actionCreators.js:**用这个文件来管理action

```js
import {HANDLE_INPUT_CHANGE, ADD_NEW_LIST, EDIT_LIST, INITIALIZE_LIST, TEST_SAGA_QUERY_LIST} from '../store/actionTypes'//引用action常量
import axios from 'axios';//axios请求引用


export const getInitializeListData = (data) => ({
    type: INITIALIZE_LIST,
    data
});

export const getInputChangeAction = (value) => ({
    type: HANDLE_INPUT_CHANGE,
    value
});

export const getAddListAction = () => ({
    type: ADD_NEW_LIST
});

export const getDeleteListAction = (index) => ({
    type: EDIT_LIST,
    editKey: index
});

export const testSagaQueryList = () => ({
    type: TEST_SAGA_QUERY_LIST
});

// export const getRequestListData=()=>{
//     //当使用redux-thunk后，dispatch可以传递方法，并且方法中能带返回的方法中自带一个dispatch
//   return (dispatch)=>{
//       axios.get('http://localhost:3000/topicList').then((res) => {
//           if (res.status === 200) {
//               const action = getInitializeListData(res.data);
//               dispatch(action);
//
//           }
//       })
//   }
// };
```

**reducer.js:**用于储存store数据，并且进行处理

```js
import {HANDLE_INPUT_CHANGE, ADD_NEW_LIST, EDIT_LIST, INITIALIZE_LIST} from './actionTypes'


//初始化store值
const defaultState = {
    inputValue: '',
    list: []
};

//reducer可以接受state，但是不能修改state
//reducer必须是一个纯函数，纯函数是指，给定固定的输入，就会产生固定的输出，输出的值不能因为条件不同而发生不同的变化，而且不会有任何副作用，副作用是指，不能直接改变state的值
export default (state = defaultState, action) => {
    if (action.type === HANDLE_INPUT_CHANGE) {
        const newStore = JSON.parse(JSON.stringify(state));//深度克隆state
        newStore.inputValue = action.value;
        return newStore  //这个数据返回给了store,store拿到新数据进行更新数据
    }

    if (action.type === ADD_NEW_LIST && state.inputValue) {
        const newStore = JSON.parse(JSON.stringify(state));
        newStore.list.push(newStore.inputValue);
        newStore.inputValue = '';
        return newStore
    }

    if (action.type === EDIT_LIST) {
        const newStore = JSON.parse(JSON.stringify(state));
        newStore.list.splice(action.editKey, 1);
        return newStore
    }

    if (action.type === INITIALIZE_LIST) {
        let newStore = action.data;
        return newStore
    }
    return state
}
```

**sagas.js:**dispatch发送请求的时候，监听，并进行数据的处理，常常在sagas中发送网络请求

```js
import {put, takeEvery} from 'redux-saga/effects'
import {TEST_SAGA_QUERY_LIST} from './actionTypes'
import {getInitializeListData} from './actionCreators'
import axios from 'axios';

// worker Saga: will be fired on USER_FETCH_REQUESTED actions
function* queryList() {
    try {
        let listData = yield axios.get('web/v1/topicLis');//请求类型及地址
        const action = getInitializeListData(listData.data);//将请求成功后的数据传入到dispatch中用于进行下一步操作，这里的dispatch用put代替了
        console.log(listData);
        yield put(action)//put相当于dispatch
    } catch (e) {
        console.log('eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeor', e)//请求失败后返回值
    }
}

function* mySaga() {
    yield takeEvery(TEST_SAGA_QUERY_LIST, queryList);//监听组件发送的dispatch请求
}


export default mySaga;
```



### mock.js的配置及安装

```js
npm i --save mockjs
```

创建src下的一个文件夹用于模拟数据

![mark](http://pvjq3azwj.bkt.clouddn.com/blog/20190812/HfEDnpR2TaMf.png?imageslim)

```js
const Mock = require('mockjs');//引用mock.js,注意不能用import引用

//创建被请求地址
const url = {
    requestTodoList: 'web/v1/topicList'
};


let Random = Mock.Random;//mockjs自带数据模拟方法，包括创建字符串，等一系列方法
Random.csentence();
module.exports = [
    Mock.mock(url.requestTodoList,'get',{
        'inputValue': '',
        'list|5': ['@csentence(10)']
    })
];
```

```js
// 官方事例：使用 Mock
var Mock = require('mockjs')
var data = Mock.mock({
    // 属性 list 的值是一个数组，其中含有 1 到 10 个元素
    'list|1-10': [{
        // 属性 id 是一个自增数，起始值为 1，每次增 1
        'id|+1': 1
    }]
})
// 输出结果
console.log(JSON.stringify(data, null, 4))
```



参考demo：['gitHub demo'](https://github.com/Ceilon/test-project-react '')