---
title: ES6的基本用法
date: 2019/08/01
categories: 代码
thumbnail: https://www.runoob.com/wp-content/uploads/2017/08/20130807-es6-e1420205362328.png
---
> #### 深度克隆和对象的拼接Object.assign(target,source1,source2)

```js
let obj={};
let obj2={a:1,b:2,c:3}
Object.assign(obj,obj2)
obj.a=9;
console.log("更改前后对比",obj,obj2)//{a:9,b:2,c:3},{a:1,b:2,c:3}
```

> #### 方法的默认参数设置

```js
test=(a="1")=>{
    console.log(a)
}

test()//1
test("输出传入的值")//输出传入的值
```

>### 字符串模板

```js
let name='Tom';
console.log(`他的名字叫%{name}`)//他的名字叫Tom
```

> #### const能修改引用类型

```js
const k={a:1}
k.b=3
console.log(k)//{a:1,b:3}
```

> #### const 能修改引用类型

```js
const k={a:1}
k.b=3
console.log(k)//{a:1,b:3}
```

> #### 数组的解构赋值

```js
let a,b,c;
[a,b,...c]=[1,2,3,4,5,6,7]
console.log(a,b,c)//1,2,[3,4,5,6,7]//注意c变成了数组

//变量交换
let a=1;
let b=2;
[a,b]=[b,a]
console.log('a=',a,'b=',b)//a=2,b=1

//取出某个位置的某些值
function f(){
  return [1,2,3,4,5]
}
let a,b,c;
[a,,,b]=f();
console.log(a,b);
```

> #### 对象的解构赋值

```js
let {a=10,b=5}={c=3}
console.log(a,b)//3,5


//相对复杂的场景
let metaData={
  title:'abc',
  test:[{
    title:'test',
    desc:'description'
  }]
}
let {title:esTitle,test:[{title:cnTitle}]}=metaData;
console.log(esTitle,cnTitle);
```

> #### map方法

```
//判断数字是否有穷
console.log('15',Number.isFinite(15));
//判断数字是否为NaN
console.log('0',Number.isNaN(0));

//判断是否是整数
console.log('25',Number.isInteger(25));
console.log('25.0',Number.isInteger(25.0));
console.log('25.1',Number.isInteger(25.1));
console.log('25.1',Number.isInteger('25'));

//判断正负零
console.log('-5',Math.sign(-5));//-1
console.log('0',Math.sign(0));//0
console.log('5',Math.sign(5));//1
console.log('50',Math.sign('50'));//1
console.log('foo',Math.sign('foo'));//NaN

//小数转整数
console.log(4.1,Math.trunc(4.1));//4
```

> #### 数组

```js
//生成数组
let arr=Array.og(1,2,3,4,5)
let a=[2,3,4,5]
console.log(arr)//[1,2,3,4,5]

//**获取所有的某个标签节点并生成集合
let p=document.querySelectorAll('p')//p会生成一个p标签集合


//第一种用法将集合转成数组
let pArr=Array.from(p)//pArr被转成数组

//第二种用法,对处理的数组或集合遍历
console.log(Array.from(a,(t)=>{return t*2}));//[4,6,8,10]

//fill替换数组的元素
console.log(a.fill(7))//[7,7,7,7]
//fill从指定的起始位置到结束位置开始替换
console.log(a.fill(7,1,2))//[2,7,7,5]

//Keys
console.log([6,5,3].keys())//[0,1,2]
cosole.log({a:5,b:7}.keys())//[a,b]

//values
cosole.log({a:5,b:'jc'}.values())//[5,'jc']

//entries
for(let [index,value] of ['a','bb','re'].entries()){
    console.log('==>',index,values)
}
//==>0 a
//==>1 bb
//==>2 re

//find,找到第一个满足条件的值并返回
console.log([1,2,3,5,6].find(item=>{
return item>3
}))//4

//findIndex，找到第一个满足条件的值返回下标
 console.log([1, 2, 3, 7, 8, 9].findIndex(function (item) {
        return item > 3
     })) //4
     
//includes,查找数组中是否存在某个值包括NaN也能判断是否存在
onsole.log('number',[1,2,NaN].includes(NaN))//true
```

> #### 函数

```
//作用域
let test='test';
let fc=(test,a=test)=>{
    console.log(test,a)    
}
fc('kill')
//kill kill (注意test的作用域，fc的参数中互相引用)，如果是:
    let fc=(c,a=test){
        console.log(test,a)
    }
    输出结果变为  kill test
    
//rest参数 把多个参数转换成数组
let test=(...rest)=>{
    for(let v of rest){
        console.log('rest',v)
    }
}
test(1,2,3,'5')
//rest 1
//rest 2
//rest 3
//rest '5'
```

> #### 对象

```js
/简洁写法
let a=1;
let b=2;
let es6={a,b}
console.log(es6)//{a:1,b:2}

let es6_method={
    hello(){
        console.log('hello')
    }
}
console.log(es6_method.hello())//hello

//判断对象
console.log('数组',Object.is([],[]))//false
//浅拷贝对象
console.log('拷贝',Object.assign({a:'a'},{b:'b'}))//{a:'a,b:'b'}

//扩展运算符
let {a,b,...c}={a:'test',b:'kill',c:'ddd',d:'ccc'};
c={
  c:'ddd',
  d:'ccc'
}
```

> #### symbol生成独一无二的值

```js
let a1=Symbol.for('abc');
let obj={
  [a1]:'123',
  'abc':345,
  'c':456
};
console.log('obj',obj);
//无法取出symbol为对象的箭与值
for(let [key,value] of Object.entries(obj)){
  console.log('let of',key,value);
}

//Object.getOwnPropertySymbols(obj)只能取出symbol作为键的属性
Object.getOwnPropertySymbols(obj).forEach(function(item){
  console.log(obj[item]);
})

//Reflect.ownKeys(obj)获取obj的所有键，包括symbol
Reflect.ownKeys(obj).forEach(function(item){
  console.log('ownkeys',item,obj[item]);
})
```

> #### 模版字符串判断是否应用某个样式

```jsx
let isRest=3;
<div 
className={`${styles.studentLevelFather} 
${isRest===3?styles.standbySpanStyle:''}`}>
    hello world
</div>//当isRest===3那么standbySpanStyle样式被使用
```

> #### generator生成器

**说明：**普通函数一次性执行完，generator函数能中途停止



```js
//创建
function* showString(){
  
   alert('执行第一次');
    yield;
    alert('执行第二次');
}

//执行
let genObj=showString();
genObj.next();//弹出执行第一次
genObj.next();//弹出执行第二次

```



**什么是yield？**

```js
//1.yield可以传参

function* showString(){
  
   console.log('执行第一次');
    let a= yield;
    console.log('执行第二次',a);
}

let genObj=showString();
genObj.next('11111');//执行第一次
genObj.next('22222');//执行第二次22222
//结论，generator是分块执行的，当执行到第二步的时候，yield获得了22222的参数

//2.yield返回


function* showString(){
   	console.log('执行第一次');
     yield 12;
    console.log('执行第二次');
    return 55
}

let genObj=showString();
let res1= genObj.next();//执行第一次
console.log(res1)//{value:12,done:false}
let res2= genObj.next();//执行第二次
console.log(res2)//{value:55,done:true}
//结论:执行的yield有返回值，返回值为对象，分别是value和done，value为yield返回的值，done为是否是最后一步
```

伪代码：

![mark](http://pvjq3azwj.bkt.clouddn.com/blog/20190809/Jwn41CAHFIPs.png?imageslim)







> #### mockjs的使用小技巧

```js
let Random = Mock.Random;
Random.date();
var data = Mock.mock({
    // 属性 list 的值是一个数组，其中含有 1 到 10 个元素
    'list|1-10': [{
        // 属性 id 是一个自增数，起始值为 1，每次增 1
        'id|+1': 1,
        'date':()=>Random.date('yyyy-MM-dd')
    }]
});
```



> #### Promise创建回调函数

```js

    func=()=>{
       return new Promise(((resolve, reject) => {
            setTimeout(()=>{
                //成功回调
                resolve()
            },2000)
        }))
    };


    func().then(()=>{
        console.log(20);
        return func()
    }).then(()=>{
        console.log(30);
        return func()
    }).then(()=>{
        console.log(40);
        return func()
    })
```

