---
title: js的一些技巧
date: 2019-08-01 16:41:08
categories: 代码
thumbnail: http://pvjq3azwj.bkt.clouddn.com/blog/20190801/LNsxAnWF1WUY.png?imageslim
---
**1.通过html5的缓存来储存对象并取出**

```html
<!DOCTYPE html>
<html>
<body>

<div id="result"></div>

<script type="text/javascript">
    let json={a:1,v:2,g:6,as:24}
    sessionStorage.lastname=JSON.stringify(json);
    console.log("缓存内容",JSON.parse(sessionStorage.lastname))
    document.write(sessionStorage.lastname);
</script>

</body>
```

**2.阻止冒泡事件**

```js
 e = e || window.event;
// // 阻止合成事件的冒泡
 e.stopPropagation && e.stopPropagation();
// // 阻止与原生事件的冒泡
e.nativeEvent && e.nativeEvent.stopImmediatePropagation && e.nativeEvent.stopImmediatePropagation();
```

**3.清空readux某个状态**

```js
dispatch({
  type: namespace + '/set',
  payload: {
    getUserList: [],
    searchUserList: []
  }
});
```

**4.ES6数组转集合，集合转数组的方法**

```js
let idArray=[1,2,3,4,5];
let setIds=new Set(idArray);
console.log("这是一个集合object",setIds)


let newArray=Array.from(setIds);
console.log("这是一个数组",newArray)
```

**5.将数组转换成用逗号隔开的字符串方法**

```js
let idArray=[1,2,3,4,5];
let joinToString=idArray.join(',');
console.log('这是Join转换的字符串',joinToString)

let toString=idArray.toString();
console.log('这是ToString转换的字符串',toString)
```

**6.深拷贝，解决指正问题**

```js
function deepClone(obj){
    let _obj = JSON.stringify(obj),
        objClone = JSON.parse(_obj);
    return objClone
}    
let a=[0,1,[2,3],4],
    b=deepClone(a);
a[0]=1;
a[2][0]=1;
console.log(a,b);
```

##### 结果如下:

![mark](http://pvjq3azwj.bkt.clouddn.com/blog/20190801/uWEyhsqeSoIq.png?imageslim)

**7.空对象判断**

```js
if(Object.keys(obj).length===0)){
    console.log("obj是空对象")
}else{
    console.log('obj不为空')
}
```

**8.空数组判断**

```js
Number(false) //0
arr = [];
Number(arr)=[]//0


if(Number(arr)===0){
    console.log('true')
}else{
    console.log('false')
}
//false
```

**9.判断某个数组或者某个字符串中存在某个值**

```js
[1, 2, 3].includes(2);     // true
```

**10.分割对象**

```js
10.分割对象
let newQueryTypeData = {};
let newQueryTypeDataW = {};
let arr = ['highLittle', 'DemarcationLittle', 'DemarcationHigh', 'LowHigh', 'LowLittle', 'AllLowBig', 'AllDemarcationLittle', 'AllDemarcationBig', 'AllHighLittle', 'AllHighBig'];
arr.map(t=>{
  newQueryTypeData[t]=values[t];
  newQueryTypeDataW[t+'W']=values[t+'W'];
});
this.setState({newQueryTypeData,newQueryTypeDataW});
```

**11.判断数组中的所有数据不为nNaN**

```js
!values.every(value => isNaN(value))
```

**12.defineproperty双向数据绑定**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

</head>
<body>
<div id="app">
    <label for="">
        <input type="text" id="userName" value="">
    </label>
    <br>
    <span id="uName"></span>
</div>
<script>
    window.onload=function () {
        let obj = {};
        //defineProperty功能：
        //1:扩展属性，
        Object.defineProperty(obj,'userName',{
            //是、当属性被访问的时候触发
            get(){
                console.log("get init")
            },
            //当属性被修改事访问
            set(val){
                console.log("set init",obj);
                document.getElementById('uName').innerText=val
            }
        });
        console.log("objjjjjjjjj",obj);  //输出：{userName: "ssssssssssssss"}
        document.getElementById('userName').oninput=()=>{
            obj.userName=document.getElementById('userName').value
        };

    }
</script>
</body>
</html>
```

**13.js取整数、取余数的方法**

```js
1.取整

// 丢弃小数部分,保留整数部分
parseInt(5/2)　　// 2
 

2.向上取整

// 向上取整,有小数就整数部分加1
Math.ceil(5/2)　　// 3
 

3.向下取整

// 向下取整,丢弃小数部分
Math.floor(5/2)　　// 2
 

4四舍五入

// 四舍五入
Math.round(5/2)　　// 3
 

取余
// 取余
6%4　　// 2
```

**14、删除对象中的空属性**

```js
let newWork = {a:'2',b:'',c:'',d:'1'}
for(let i in newWork){
      if(newWork[i]===''){
        delete newWork[i]
      }
    }
console.log(newWork)//{a:'2',d:'1'}
```

