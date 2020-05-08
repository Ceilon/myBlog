---
title: vue学习记录
date: 2019-10-10 11:18:49
categories: 代码
tags:
thumbnail: https://cn.vuejs.org/images/logo.png
---

## 一、vue上手

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
<title>Examples</title>
<meta name="description" content="">
<meta name="keywords" content="">
<link href="" rel="stylesheet">
<!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
<script src="./lib/vue.js"></script>
    //会创建一个vue的全局对象
</head>
<body>
//mvvm的v层，vue所控制的元素区域
	<div id="app">
		<p id="content">
			{{ msg }}
		</p>
	</div>
    <script>
        //mvvm的vm层
    	var vm = new Vue({
    		el:'#app',//指定作用区域
    		data:{//参数的存放，mvvm的么层
    			msg:'Hello vue'//参数具体值
    		}
    	})
    </script>
</body>
</html>
```

## 二、v-vloak、v-text、v-htm、v-bind、v-on

``` html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
	<title>Examples</title>
	<meta name="description" content="">
	<meta name="keywords" content="">
	<link href="" rel="stylesheet">
	<!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->

	<style>
		[v-cloak]{
			display: none;
		}
	</style>
</head>
<body>

<div id="app">
	<p v-cloak id="content">
		{{ msg }}
	</p>
	<!--v-text会覆盖掉标签里的值-->
	<h4 v-text="msg">这里的值会被覆盖掉</h4>
	<!--//可以解析一个标签-->
	<div v-html="msg2"></div>
	<!--v-bind是vue中用于提供绑定属性的指令，title里面的内容是一个表达式,可以用冒号简写掉v-bind-->
	<!--<div v-bind:title="mytitle + '我们是一个表达式'">移动到我</div>-->
	<div :title="mytitle + '我们是一个表达式'">移动到我</div>
    
	<!--v-on用于绑定事件-->
	<input value="按钮" type="button" v-on:click="show" />
</div>
<script src="./lib/vue.js"></script>
<script>
	var vm = new Vue({
		el:'#app',
		data:{
			msg:'Hello vue',
			msg2:'<h1>这是一个大的H1</h1>',
			mytitle:'定义了一个title',
		},
		methods:{
			//这里定义一个方法的集合，方便v-on来调用
			show:function () {
				alert("你好")
			}
		}
	})
</script>
</body>
</html>
```

## 三、跑马灯实例演示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <input type="button" value="开始" @click="start">
    <input type="button" value="结束" @click="end">
    <div>
        {{ msg }}
    </div>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            msg: '这是一个跑马灯效果，不信你让我跑一个?',
            myVar: null
        },
        methods: {
            start() {
                //判断定时器是否为空，若为空则启动，防止多次启动定时器
                if (!this.myVar) {
                    this.myVar = setInterval(() => {
                        let startString = this.msg.substring(1);
                        let endString = this.msg.substring(0, 1);
                        this.msg = startString + endString
                    }, 400);
                }
            },
            end() {
                clearTimeout(this.myVar);
                this.myVar=null;//重置定时器
            }
        }
    })
</script>
</body>
</html>
```

## 四、事件修饰符

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
    <style>
        .inner{
            width:200px;
            height: 200px;
            background: aqua;
        }
    </style>
</head>
<body>
<!--阻止冒泡事件-->
<!--<div id="app">-->
    <!--<div class="inner" @click="click2">-->
        <!--<input type="button" value="点击" @click.stop="click1">-->
    <!--</div>-->
<!--</div>-->

    
<!--.capture从外到里执行方法-->
<!--<div id="app">-->
    <!--<div class="inner" @click.capture="click2">-->
        <!--<input type="button" value="点击" @click="click1">-->
    <!--</div>-->
<!--</div>-->

<!--.self点击自生触发函数-->
<!--<div id="app">-->
    <!--<div class="inner" @click.self="click2">-->
        <!--<input type="button" value="点击" @click="click1">-->
    <!--</div>-->
<!--</div>-->


<!--阻止默认事件-->
<a href="http://www.baidu.com" @click.prevent="linkclick">有问题，先去百度</a>

<!--只触发一次事件处理函数-->
<!--<a href="http://www.baidu.com" @click.prevent.once="linkclick">有问题，先去百度</a>-->
<script>
    let vm = new Vue({
        el:'#app',
        data:{},
        methods:{
            click1(){
                console.log("点击第一层")
            },
            click2(){
                console.log("点击第二层")
            },
            linkclick(){
                console.log("默认事件")
            }
        }
    })
</script>
</body>
</html>
```

## 五、双向事件绑定v-model

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="lib/vue.js"></script>
</head>
<body>
<!--v-model实现双向数据绑定，但注意的是只能在表单元素中使用（input ,text,address,email,select,checkbox,textarea）-->
<div id="app">
    <h4>{{msg}}</h4>
    <input type="text" v-model="msg"/>
</div>

<script>
    let vm = new Vue({
        el:'#app',
        data:{
            msg:'这是一个vue的学习demo，内容不多但是用处很大'
        },
        methods:{

        }
    })
</script>
</body>
</html>
```

## 六、css样式类名的样式绑定

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
    <style>
        .red {
            color: red;
        }

        .background {
            background: aqua;
        }

        .height {
            height: 50px;
        }
    </style>
</head>
<body>
<div id="app">
    <!--第一种方式可以传递一个数组，但是class需要做数据绑定-->
    <div :class="['red','background','height']">(1)这里有一堆文字，有不同的样式</div>


    <!--在数组中使用三元表达式，可以开启或关闭类名-->
    <div :class="['red',flag?'':'background','height']">(2)这里有一堆文字，有不同的样式</div>


    <!--也可以包在对象里面,flag的bool决定类名的开启状态，此时的flag为true那么background的状态是开启的-->
    <div :class="['red',{'background':flag},'height']">(3)这里有一堆文字，有不同的样式</div>

    <!--classObj是一个类名的对象，对应data里的classObj,后面的值决定了某个类名的开启和关闭状态-->
    <div :class="classObj">(4)这里有一堆文字，有不同的样式</div>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            flag: true,
            classObj:{red:true,background:true,height:false}
        }
    })
</script>

</body>
</html>
```

## 七、通过内联样式设置标签样式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <!--styleObj1可以以对象的形式传入内联样式-->
    <h1 :style="styleObj1">（1）这是一个h1</h1>
    <!--运用两个内联样式可以传入字符串，里面的对象名写成数组-->
    <h1 :style='[styleObj1,styleObj2]'>（2）这是一个h1</h1>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            //键名当有“-”符号时，必须用''符号包裹
            styleObj1: {color: 'red', 'font-weight': 100, background: 'yellow'},
            styleObj2: {'font-style': 'italic'}
        },
        method: {}
    })
</script>
</body>
</html>
```

## 八、v-for循环数组，对象数组，对象，数字

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="lib/vue.js"></script>
</head>
<body>
<div id="app">
    <!--循环数组(item,i) in list相当于list.forEach((item.index)=》{})-->
    <p v-for="(item,i) in list">索引值：{{i}},数组中的每一项{{item}}</p>


    <!--循环对象数组,和循环数组类似，当要用到对象中的某一个值的话，将其.出来就可以了-->
    <p v-for="(item,i) in objArray">>索引值：{{i}},数组中的每一项的值id:{{item.id}}-----name:{{item.name}}}</p>


    <!--循环对象,遍历对象时，除了value，key，还有i的索引值-->
    <p v-for="(value,key) in obj">循环对象键值{{key}}}-------------键名{{value}}</p>

    <!-- in后面可以放普通数组，对象数组，对象，还可以放数字，功能是数字是几，就循环几次，count是从1开始-->
    <p v-for="count in 10">循环次数，目前是第{{ count }}次</p>
</div>

<script>
    let vm = new Vue({
        el: '#app',
        data: {
            list: ['1', '2', '3', '4', '5'],
            objArray: [
                {id: 1, name: 'zs1'},
                {id: 2, name: 'zs2'},
                {id: 3, name: 'zs3'},
                {id: 4, name: 'zs4'},
            ],
            obj: {name: '张三', sex: '男', hobby: '电影，写代码'}
        },
        methods: {}
    })
</script>
</body>
</html>
```

- **值得注意的是，在2.2.0+版本中，当组件使用v-for时，必须指定一个key值来保证组件或者标签的唯一性**

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <div>
        <label>id:
            <input type="text" v-model="id">
        </label>
        <label>
            name:
            <input type="text" v-model="name">
        </label>
        <label>
            <input type="button" value="添加" @click="add">
        </label>
    </div>

    <!--注意：v-for循环的时候，key属性只能使用number获取string-->
    <!--注意：key在使用的时候，必须使用v-bind属性的绑定形式，指定key的值-->
    <!--在组件中，使用v-for循环的时候，或者在一些特殊的情况中，如果v-for有问题，必须在使用v-for的同时，指定唯一的字符串/数字类型:key值-->
    
    <p v-for="(item,index) in users" :key="item.id">
        <input type="checkbox">
        id:{{item.id}}-------------------name:{{item.name}}
    </p>
</div>


<script>
    let vm = new Vue({
        el: '#app',
        data: {
            id: '',
            name: '',
            users: [
                {id: 1, name: '赵高'},
                {id: 2, name: '嬴政'},
                {id: 3, name: '项羽'},
                {id: 4, name: '李斯'}
            ]
        },
        methods: {
            add() {
                this.users.unshift({name: this.name, id: this.id});
                this.id = '';
                this.name = '';
            }
        }
    })
</script>
</body>
</html>
```

## 九、v-if和v-show的使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <input type="button" value="toggle" @click="flag=!flag"/>

    <!--v-if和v-show两个都是控制元素的显示和隐藏，
    不同点在于，v-if是移除了元素，而v-show是通过给元素添加一个display:none从而达到隐藏效果
    前者的性能消耗高，最好不要使用，
    但是v-show有可能在创建后从来没有被使用过，有很高的初始渲染消耗。
    -->
    <h3 v-if="flag">这是用v-if控制的元素</h3>
    <h3 v-show="flag">这是用v-show控制的元素</h3>
</div>
<script>
    let vm = new Vue({
        el:'#app',
        data:{
            flag:true
        },
        methods:{

        }
    })
</script>
</body>
</html>
```

## 总结1

+ ###### 1.mvc和mvvm的区别

  -  MVVM即Model-View-ViewModel的简写。即模型-视图-视图模型。模型（Model）指的是后端传递的数据。视图(View)指的是所看到的页面。视图模型(ViewModel)是mvvm模式的核心，它是连接view和model的桥梁。它有两个方向：一是将模型（Model）转化成视图(View)，即将后端传递的数据转化成所看到的页面。实现的方式是：数据绑定。二是将视图(View)转化成模型(Model)，即将所看到的页面转化成后端的数据。实现的方式是：DOM 事件监听。这两个方向都实现的，我们称之为数据的双向绑定。

    ​      MVC是Model-View- Controller的简写。即模型-视图-控制器。M和V指的意思和MVVM中的M和V意思一样。C即Controller指的是页面业务逻辑。使用MVC的目的就是将M和V的代码分离。MVC是单向通信。也就是View跟Model，必须通过Controller来承上启下。MVC和MVVM的区别并不是VM完全取代了C，只是在MVC的基础上增加了一层VM，只不过是弱化了C的概念，ViewModel存在目的在于抽离Controller中展示的业务逻辑，而不是替代Controller，其它视图操作业务等还是应该放在Controller中实现。也就是说MVVM实现的是业务逻辑组件的重用，使开发更高效，结构更清晰，增加代码的复用性。

+ ###### 2.vue的代码结构

  - ```js
    let mv = new Vue({
        el:'#app',//指定控制区域
        data:{},//数据存储
        methods:{//方法的存储
    }
    })
    ```

    

+ ###### 3.插值表达式

  -  v-cloak
  - v-text
  - v-on(简写@)
  - v-html
  - v-bind(简写:)
  - v-model
  - v-for
  - v-if
  - v-show

## 十、品牌列表案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
    <link href="./lib/bootstrap-3.3.7-dist/css/bootstrap.css" rel="stylesheet">
</head>
<body>
<div id="app">
    <div class="panel panel-primary">
        <div class="panel-heading">
            <h3 class="panel-title">添加品牌</h3>
        </div>
        <div class="panel-body form-inline">
            <label>Id:<input class="form-control" type="text" v-model="obj.id"></label>
            <label>Name:<input class="form-control" type="text" v-model="obj.name"></label>
            <label><input class="btn" type="button" value="添加" @click="addBrand"></label>
            <label>搜索关键字:
                <input type="text" class="form-control" placeholder="Search for..." v-model="keywords">
            </label>
        </div>


        <table class="table table-bordered table-hover table-striped">
            <thead>
            <tr>
                <th>Id</th>
                <th>Name</th>
                <th>Ctime</th>
                <th>Operation</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="item in search(keywords)">
                <td>{{ item.id }}</td>
                <td>{{ item.name }}</td>
                <td>{{ item.date | formatDate }}</td>
                <td><a href="" @click.prevent="deleteBrand(item.id)">删除</a></td>
            </tr>
            </tbody>
        </table>
    </div>
</div>
<script>

    //过滤器
    Vue.filter('formatDate', (date) => {
        let dt = new Date(date);
        let y = dt.getFullYear();
        let m = dt.getMonth() + 1;
        let d = dt.getDate();
        return `${y}-${m}-${d}`
    });
    let vm = new Vue({
        el: '#app',
        data: {
            keywords: '',
            obj: {},
            deleteId: '',
            brands: [
                {id: 1, name: '奔驰', date: new Date()},
                {id: 2, name: '宝马', date: new Date()},
                {id: 3, name: '奥迪', date: new Date()}
            ]
        },
        methods: {
            addBrand() {
                this.obj.date = new Date();
                this.brands.push(this.obj);
                this.obj = {}
            },
            deleteBrand(id) {
                // 两种方式来删除
                this.brands = this.brands.filter((t) => ![id].includes(t.id));
                // this.brands.map((item,i)=>{
                //     if(item.id===id){
                //         this.brands.splice(i,1)
                //     }
                // })
            },
            search(keywords) {
                // let searchBrands = this.brands.map(item=>{
                //      if(!(item.name.indexOf(keywords)===-1)){
                //          return item
                //      }
                //  })
                //  console.log("22222",searchBrands)
                //  return  searchBrands


                //筛选出结果
                return this.brands.filter(item => item.name.indexOf(keywords) !== -1)
            }
        }
    })
</script>
</body>
</html>
```

## 十一、过滤器的使用



### 1、定义全局过滤器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <!--msg为参数字段，| 后面的为过滤器，过滤器可以调用n多个，一个|对应调用了一个过滤器
    过滤器中后的()内部可以传递参数如msgFilter('这是一个参数')
    -->
    <p>{{ msg | msgFilter('替换','++++++++++++这是额外添加的字段') | msgChange}}</p>
</div>
<script>


    //定义一个过滤器如下所示，值得注意的是空号内的参数第一个固定是msg的值，从第二个起，就是从调用的过滤器中传递进来的
    Vue.filter('msgFilter',(data,arg,arg2)=>{
        return data.replace(/过滤/g,arg) + arg2
    });

    Vue.filter('msgChange',(data)=>{
        return '这是另一个过滤器添加的东西-----------------------' + data
    });

    let vm = new Vue({
        el:'#app',
        data:{
            msg:'这是一个过滤器，过滤一些要被过滤的东西，比如说这个“过滤”两个字已经不是原来的了'
        }
    })
</script>
</body>
</html>
```

### 2、定义私有过滤器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <!--msg为参数字段，| 后面的为过滤器，过滤器可以调用n多个，一个|对应调用了一个过滤器
    过滤器中后的()内部可以传递参数如msgFilter('这是一个参数')
    -->
    <p>{{ msg | msgFilter('替换')}</p>
</div>
<script>




    let vm = new Vue({
        el:'#app',
        data:{
            msg:'这是一个过滤器，过滤一些要被过滤的东西，比如说这个“过滤”两个字已经不是原来的了'
        },
        filters:{
            //在这里定义一个私有的过滤器，当全局和私有同时存在相同名称的过滤，优先调用私有过滤器
            msgFilter:(data,arg)=>{
                   return data.replace(/过滤/g,arg)
            }
        }
    })
</script>
</body>
</html>
```

## 十二、键盘修饰符

```html
//使用一个键盘修饰符
<input @keyUp.f2='console.log('输入了f2')'/>
//自定义一个键盘修饰符
Vue.config.keyCodes.f2=113;
```

## 十三、自定义指令

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <input type="text" placeholder="Search for..." v-focus="attribute">
    <h1 v-getcolors="'red'" v-updatesize="80">略略略略略略</h1>
</div>
<script>

    //全局自定义指令
    // Vue.directive('focus', {
    //     bind: (el, attribute) => {
    //         console.log('现在我插入到了dom中去了，还未进行渲染', attribute)
    //     },
    //     inserted: (el, attribute) => {
    //         console.log("现在我渲染了，可以获取焦点了")
    //         el.focus()
    //     }
    // });
    let vm = new Vue({
        el: '#app',
        data: {
            attribute: {
                name: 'aaaaaaa',
                color: 'red'
            }
        },
        directives:{
            //局部自定义指令，注意directives中的s,属性分别是
            // bind(元素绑定的时候调用，调用一次),
            // componentUpdated(元素更新的时候调用，多用N次),
            // inserted(被绑定的元素被渲染的时候调用，调用一次),
            // unbind(指令与元素解绑时调用，只调用一次)
            //el就是dom元素
            getcolors:{
                bind:(el,binding)=>{
                    el.style.color=binding.value
                }
            },
            focus:{
                bind: (el, attribute) => {
                    console.log('现在我插入到了dom中去了，还未进行渲染', attribute)
                },
                inserted: (el, attribute) => {
                    console.log("现在我渲染了，可以获取焦点了")
                    el.focus()
                }
            },
            updatesize:(el,binding)=>{//定义一个钩子函数，相当于写入了update和bind相当于自定义指令的简写
                el.style.fontSize=binding.value+'px'
            }
        }
    })
</script>
</body>
</html>
```

## 十四、vue的生命周期

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <div>{{msg}}</div>
    <input type="button" value="点击" @click="update">
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {msg: 'OK'},
        methods: {
            show() {
                console.log('执行了show方法')
            },
            update() {
                this.msg = 'ssss';
            }
        },
        beforeCreate() {//组件没有被渲染，初始化的时候调用此函数
            // console.log(this.msg)
            // this.show()
            //    此时，以上两个方法都会保存，原因是此时，data,methods中的数据都没有被初始化
        },
        created() {
            console.log(this.msg);
            this.show()
            //在created中，data,methods都已经被初始化好了，已经可以被处理了，所以如果要调用他们的方法或数据，最早只能在created中操作
        },
        beforeMount() {
            //这个生命周期函数执行在模版已经编译完成，还没有进行渲染（已经编译好虚拟dom，真实dom还没有渲染）
            console.log('beforeMount输出结果：', document.getElementById('app').innerText)//输出结果：{{ msg }}，然而页面显示的是OK，说明此时拿到的并不是真实DOM
        },
        mounted() {
            //mounted就是beforeMount后面一个阶段，DOM渲染完成后，所以，通过document打印出来的内容就是经过处理后的
            //只要执行完mounted，说明整个vue实例已经初始化完成了，此时组件已经脱离创建阶段，来到运行阶段
            console.log('mounted输出结果:', document.getElementById('app').innerText)
        },
        //接下来是数据运行的时候触发的两个事件
        beforeUpdate() {
            console.log("打印出即将改变的DOM查看是否变化", 				       					document.getElementById('app').innerText);
            console.log("打印出已经改变的数据看是否变化", this.msg);
            //结论是，数据发生了变化，可是页面还是旧的页面，说明虚拟dom已经改变，真实dom还没有渲染，此为更新前的生命周期
        },
        updated(){
            console.log("update打印出即将改变的DOM查看是否变化", 		        					document.getElementById('app').innerText);
            console.log("update打印出已经改变的数据看是否变化", this.msg);
            //此时页面和数据都已经变化了，说明此时的真实dom和虚拟dom都已经渲染完毕
        },
        beforeDestroy(){
            console.log('在组件销毁前调用');
            //当被执行时，实例从运行阶段进入销毁阶段，实例身上的data,methods过滤器，指令都处于可用状态，还没有真正的销毁。
        },
        destroyed(){
            console.log("此时组件完全被销毁，所有实例都不能使用")
        }

    })
</script>
</body>
</html>
```

## 十五、vue动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
      <link rel="stylesheet" href="lib/animate.css">
        <!--引用了animate，一些常用动画库-->
    <style>
        .fade-enter-active {
            transition: all .3s ease;
        }
        .fade-leave-active {
            transition: all .8s;
        }
        
        /*定义从开始到结束状态*/
        .fade-enter, .fade-leave-to
            /* .fade-leave-active for below version 2.1.8 */ {
            transform: translateX(100px);
            opacity: 0;
        }
    </style>
</head>
<body>
<div id="app">
    <input type="button" value="toggle" @click="flag=!flag">
    //vue定义的一个组件，用于动画，fade能够替代v-实现指定动画效果，v-未定义name默认指定方式
    <transition name="fade">
        <h3 v-if="flag">这是一个h3</h3>
    </transition>
    <!-- 使用animate,enter-action和leave-active定义进入前后的动画名，注意主体要引入animate的class-->
     <transition enter-active-class="bounceIn" leave-active-class="bounceOut">
        <h3 class="animated" v-if="flag2">这是一个h3</h3>
    </transition>
</div>
<script>
    let vm = new Vue({
        el:'#app',
        data:{
            flag:false//设定初始状态不显示
        }
    })
</script>
</body>
</html>
```

![示例图](https://cn.vuejs.org/images/transition.png"")

```html
<!--实例-->

<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="./lib/vue.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        #app {
            width: 500px;
            margin: 200px auto;
        }

        .list > ul {
            position: relative;
            margin-top: 16px;
            list-style: none;
            width: 100%;
        }

        .addLi {
            margin-top: 5px;
            border: 1px black dashed;
            transition: background 0.5s;
            width: 100%;
            padding-left: 10px;
            box-sizing: border-box;
        }

        .addLi:hover {
            background: #80d4de;
        }

        .add-enter, .add-leave-to {
            opacity: 0;
            transform: translateY(80px)
        }

        .add-enter-active, .add-leave-active {
            transition: all 1s;
        }


        /*在进行删除的时候做一个过渡处理*/
        .add-move {
            transition: all 1s;
        }

        .add-leave-active {
            position: absolute;
        }
    </style>
</head>
<body>
<div id="app">
    <div>
        <label>名字：<input type="text" v-model="name"></label>
        <label><input type="button" value="添加" @click="add"></label>
    </div>
    <div class="list">
        <!--过渡元素需要 用v-if渲染出来就不能使用transition则要使用transition-Group来给元素指定动画-->
        <!--appear让动画块渲染的时候有一个出现的动画效果-->
        <!--tag属性指定transition渲染出一个自定义标签,如果不加，则默认渲染出一个span-->
        <transition-Group appear name="add" tag="ul">
            <li class="addLi" v-for="(item,i) in list" :key="item.id" @click="deleteIt(item.id)">
                {{item.id}}--------------------{{item.name}}
            </li>
        </transition-Group>
    </div>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            name: '',
            list: [{id: 1, name: '王思聪'}, {id: 2, name: '刘恒'}, {id: 3, name: '陈贵'}, {id: 4, name: '王飞翔'}]
        },
        methods: {
            add() {
                this.list.push({id: this.list.length + 1, name: this.name});
                this.name = ''
            },
            deleteIt(t) {
                this.list = this.list.filter(item => item.id !== t)
            }
        }
    })
</script>
</body>
</html>
```

## 十六、组件化

* 组件化和模块化有什么区别？
  * 模块化：模块是从代码逻辑进行拆分的，方面分层开发，保证每个功能模块的职能	
  
  * 组件化： 组件化是从ui界面进行划分的，方便ui的重用
  
    #### 创建方法1
  
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="./lib/vue.js"></script>
  </head>
  <body>
  <div id="app">
      <my-com1></my-com1>
  </div>
  
  <script>
      // import Vue from './lib/vue'
      //定义一个组件，通过template来指定组件要展示的html结构
      let com1 = Vue.extend({
          template:'<h1>这是通过Vue.extend来创建的组件</h1>'
      });
      //将组件放入vue中方便渲染，Vue.component('组件名称',创建出来的组件模板对象)
      Vue.component('myCom1',com1);
      let vm = new Vue({
          el:'#app'
      })
      
      //可以简写成以下形式，省去定义一个变量的步骤
      // Vue.component('myCom1',Vue.extend({
      //     template:'<h1>这是通过Vue.extend来创建的组件</h1>'
      // }));
      // let vm = new Vue({
      //     el:'#app'
      // })
  </script>
  </body>
  </html>
  ```
  
  
  
  #### 创建方法2

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <my-com2></my-com2>
</div>

<script>
    
    Vue.component('myCom2',{
       template:'<div>使用component直接创建的标签</div>'
    });
    let vm = new Vue({
        el:'#app'
    })
</script>
</body>
</html>
```

#### 定义一个私有的组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<template id="jc">
    <div>
        <h1>这是用指定ID来创建的template组件</h1>
        <div>很好用，有智能提示</div>
        <h2>{{msg}}</h2>
    </div>
</template>
<div id="app">
    <!--<my-com1></my-com1>-->
    <mycom3></mycom3>
</div>

<script>
    //私有组件
    //组件中也可以定义一个data属性值，但是区别是data的类型是function，返回一个对象
    //为什么组件的data会是一个function，原因是如果组件被多次调用的话，如果不是function返回的独立参数，那么每当改变一个组件出参数，其他相同组件出参数都会被改变
    let vm = new Vue({
        el:'#app',
        components:{
            mycom3:{
                template:'#jc',
                  data:()=>{
                    return {
                        msg:'这是组件中的data值'
                    }
                }
            }
        }
    })

</script>
</body>
</html>
```

## 十七、创建一个tab切换功能

* #### 1、用v-if&v-else来实现两个tab之间的切换

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<template id="login">
    <div>
        <h1>登录</h1>
    </div>
</template>
<template id="register">
    <div>
        <h1>注册</h1>
    </div>
</template>
<div id="app">
    <div><a href="" @click.prevent="flag=true"> 登录</a><a href="" @click.prevent="flag=false">注册</a>
        
        <!--flag为true，登录会显示，注册会关闭，反正亦然-->
        <login v-if="flag"></login>
        <register v-else="flag"></register>
    </div>
</div>
<script>
    let vm =new Vue({
        el: '#app',
        data: {
            flag: true
        },
        components: {
            login: {
                template: '#login'
            },
            register: {
                template: '#register'
            }
        }
    })
</script>
</body>
</html>
```

* 2、利用vue提供的componnet占位符中的:is='组件名称'来实现组件间的切换,以及组件切换过渡效果

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="./lib/vue.js"></script>
      <style>
          .v-enter,.v-leave-to{
              opacity: 0;
              transform: translateX(150px);
          }
          .v-enter-active,.v-leave-active{
              transition: all 0.5s;
          }
      </style>
  </head>
  <body>
  <template id="login">
      <div>
          <h1>登录</h1>
      </div>
  </template>
  <template id="register">
      <div>
          <h1>注册</h1>
      </div>
  </template>
  <template id="about">
      <div>
          <h1>关于</h1>
      </div>
  </template>
  <div id="app">
      <div><a href="" @click.prevent="comName='login'"> 登录</a><a href="" @click.prevent="comName='register'">注册</a><a href="" @click.prevent="comName='about'">关于</a>
          <!--component为占位符，利用:is='组件名称'来切换不同的组件-->
          <!--在component外层包一个transition来实现组件切换的过渡效果，通过mode来改变组件切换模式-->
          <transition mode="out-in">
              <component :is="comName"></component>
          </transition>
      </div>
  </div>
  <script>
      let vm =new Vue({
          el: '#app',
          data: {
              comName:'login'
          },
          components: {
              login: {
                  template: '#login'
              },
              register: {
                  template: '#register'
              },
              about: {
                  template: '#about'
              }
          }
      })
  </script>
  </body>
  </html>
  ```

  

## 十八、父组件子组件间的值和方法的传递

* #### 1、父组件通过给子组件绑定属性来实现，父组件把值传递给子组件

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="./lib/vue.js"></script>
  </head>
  <body>
  <template id="child">
      <div><h1>{{parentmsg}}</h1></div>
  </template>
  <div id="app">
      <!--通过父组件绑定值的形式传递给子组件，然后在子组件的props的数组中定义一个从子组件中传递的值-->
      <!--props中的值是只读的，不能改变-->
      <child :parentmsg="msg"/>
  </div>
  <script>
      let vm = new Vue({
          el:'#app',
          data:{
              msg:'这是父组件里面的值'
          },
          components:{
              child:{
                  template:'#child',
                  data:()=>{
                      return {
                          msgC:'这是子组件上的值'
                      }
                  },
                  props:['parentmsg']
              }
          }
      })
  </script>
  </body>
  </html>
  ```

  

* 2、父组件通过绑定事件来将方法传递给子组件、同时，子组件通过调用方法来将值传递给子组件

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="./lib/vue.js"></script>
  </head>
  <body>
  <template id="children">
      <div>
          <input type="button" value="子组件按钮" @click="showTime">
      </div>
  </template>
  <div id="app">
      <!--@show="show('aaaaa')"这样传递的话，aaaaa的优先级比子组件中传递值的优先级高,通过绑定事件把方法传递给子组件-->
      <child @show="show"></child>
  </div>
  <script>
      let vm = new Vue({
          el:'#app',
          data:{
          childrenMsg:''
          },
          methods:{
              show(s){
                  this.childrenMsg=s;
                  console.log("父组件的方法被调用了"+s)
              }
          },
          components:{
              child:{
                  template:'#children',
                  methods:{
                      showTime(){
                          //show后面接的是传递给父组件方法的值
                          this.$emit('show','-------------子组件同时传递了一个值给父组件')
                      }
                  }
              }
          }
      })
  </script>
  </body>
  </html>
  ```

  

#### 实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="./node_modules/mockjs/dist/mock.js"></script>
    <script type="text/javascript" src="./mockjs/mock.js"></script>
    <link href="./lib/bootstrap-3.3.7-dist/css/bootstrap.css" rel="stylesheet">
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="./lib/vue.js"></script>

</head>
<body>
<div id="app" class="container">
    <div class="panel panel-default">
        <div class="panel-body">
            <!--这个指令不能用驼峰不然无法调到，比如@pushList这样的写法是错误的-->
            <comment @pushlist="pushList"></comment>
        </div>
        <div class="panel-footer">
            <ul class="list-group">
                <li v-for="(item,i) in list" :key="item.id" class="list-group-item"><span
                        class="col-10">{{item.content}}</span><span class="badge col-2">评论人：{{item.user}}</span></li>
            </ul>
        </div>
    </div>
</div>
<template id="comment">
    <div>
        <div class="form-group">
            <label>评论</label>
            <input type="text" class="form-control" v-model="user">
        </div>
        <div class="form-group">
            <label>评论内容</label>
            <textarea class="form-control " v-model="textContent">
           </textarea>
        </div>
        <button class="btn btn-primary" @click="publish">发表</button>
    </div>
</template>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            list: []
        },
        methods: {
            pushList(obj,callback){
                this.list = obj;
                callback(this.list);
            }
        },
        components: {
            comment: {
                template: '#comment',
                data: () => {
                    return {
                        user: '',
                        textContent: ''
                    }
                },
                methods: {
                    publish() {
                        axios.post('v2/publishText', {user: this.user, content: this.textContent}).then((response) => {
                            console.log("response");
                            this.$emit('pushlist',response.data.list,(s)=>{
                                if(s===response.data.list){
                                    this.user='';
                                    this.textContent='';
                                }
                            });
                        }).catch((error) => {
                            console.log(error)
                        })
                    }
                }
            }
        },
        created() {
            axios.get('p2/initList').then((response) => {
                this.list = response.data.list;
            }).catch((error) => {
                console.log("错误代码" + error)
            })
        }
    })
</script>
</body>
</html>
```

#### mockjs层

```js
let Random = Mock.Random;
Random.csentence();
Random.cparagraph(1);
Random.guid();


let listData = Mock.mock({
    'list|1-10': [{
        // 属性 id 是一个自增数，起始值为 1，每次增 1
        'id':'@guid()',
        'user': '@csentence(2,4)',
        'content': '@cparagraph(1)',
    }]
});



//------------------------------------组件评论案例--------------------------------
Mock.mock('p2/initList','get',()=>{
   return listData;
});

Mock.mock('v2/publishText','post',(option)=>{
    let acceptData = {id:Mock.mock('@guid()'),... JSON.parse(option.body)};
    listData.list.push(acceptData);
    console.log('=====================>',listData)
    return listData
});
```

## 十九、用ref获取标签或组件dom

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
    <link href="./lib/bootstrap-3.3.7-dist/css/bootstrap.css" rel="stylesheet">
</head>
<body>
<div id="app" class="container">
    <h3 ref="h3Dom">这是一个h3标签</h3>
    <login ref="login"></login>
    <button class="btn btn-primary" @click="changeColor">变色</button>
</div>
<template id="login">
    <h4>登录</h4>
</template>
<script>
    let vm = new Vue({
        el:'#app',
        data:{

        },
        methods:{
            changeColor(){
                //通过$refs来获取某个标签的dom从而对标签操作dom
                this.$refs.h3Dom.style.color='red';
                //获取组件dom
                console.log('msg=====>',this.$refs.login.msg,'show============',this.$refs.login.show())
            }
        },
        components:{
            login:{
                template:'#login',
                data:()=>{
                    return{
                        msg:'this is son msg'
                    }
                },
                methods:{
                    show(){
                        console.log("组件方法被调用了")
                    }
                }
            }
        }
    })
</script>
</body>
</html>
```

## 二十、路由



#### 路由的基本用法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    <style>
        .myactive{
            text-decoration: none;
            color: #75a77c;
            background: #ffc8cd;
            font-size:30px;
        }
        .v-enter,.v-leave-to{
            transform:translateX(150px);
            opacity: 0;

        }
        .v-enter-active,.v-leave-active{
            transition:0.5s all;
        }
    </style>
</head>
<body>
<div id="app">
 <!--router-link是vue-router提供的一个跳转标签，tag可以指定router-link在页面上渲染出来的标签是什么-->
   <router-link to="/login" tag="a">登录</router-link>
    <!--?后面可以带query参数-->
    <router-link to="/register?id=10">注册</router-link>
    <!--这是vue-router提供的元素，专门用来当做占位符，将来，路由规则匹配到的组件，就会展示到这个router-view中-->
    <transition mode="out-in">
        <router-view></router-view>
    </transition>
</div>



<template id="register">
    <div>
        <h2>注册{{$route.query.id}}</h2>
        <router-link to="/register/child">child</router-link>
        <router-view></router-view>
    </div>
</template>
<script>

    let login = {
        template: '<h2>登录</h2>',
        created(){
            //用当前route可以取出query,path等参数
            console.log('login,query',this.$route)
        }
    };
    let register = {
        //利用模板也能将route里的对象参数取出
        template: '#register',
        created(){
            //用当前route可以取出query,path等参数
            console.log('query',this.$route)
        }
    };
    let child = {
        //利用模板也能将route里的对象参数取出
        template: '<h2>路由下面的子组件</h2>',
    };

    //创建一个路由对象，当导入vue-router包之后,在window全局对象中就有了一个路由的构造函数叫VueRouter，
    //在new路由对象的时候，可以为构造函数传递一个配置对象
    let routerObj = new VueRouter({
        //route这个配置对象表示【路由的匹配规则】
        routes: [
            //redirect重定向
            // {path:'/',redirect:'/login'},
            //每一个路由规则都是一个对象，这个规则对象身上有两个必须属性,path和component。
            //path所监听的地址栏位置，component表示如果路由是前面path所匹配的，则展示当前组件
            {path: '/login', component: login},
            {
                path: '/register',
                component: register,
                children:[
                    {path:'child',component:child}
                ]

            }
        ],
        linkActiveClass:'myactive'//可配置当前被点击元素的默认class，如果没有配置，则默认为router
    });

    let vm = new Vue({
        el: '#app',
        data: {},
        router: routerObj,//将注册对象注册到vm实例上,用来监听url地址的变化，然后展示对应的组件
        component: {}
    })
</script>
</body>
</html>
```

#### 用路由实现经典布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    <link href="./lib/bootstrap-3.3.7-dist/css/bootstrap.css" rel="stylesheet">
</head>
<style>
    *{
        padding: 0;
        margin: 0;
    }
    .header{
        height: 200px;
        background: #c3ffd7;
    }
    .contentBox{
        display: flex;
    }
    .leftBox{
        background: #acd5de;
        height: 500px;
        flex: 2;
    }
    .mainBox{
        background: #9da2c1;
        height: 500px;
        flex:8;
    }
</style>
<body>
<div id="app" class="container">
    <router-view name="header"></router-view>
    <div class="contentBox">
        <router-view name="left"></router-view>
        <router-view name="main"></router-view>
    </div>
</div>


<script>

    let header = {
        template: '<div class="header"><h1>我来组成头部</h1></div>'
    };
    let leftBox = {
        template: '<div class="leftBox"><h1>我来组成躯干</h1></div>'
    };
    let mainBox = {
        template: '<div class="mainBox"><h1>我来组成身体</h1></div>'
    };

    let routerObj = new VueRouter({
        routes: [
            {
                path: '/', components:{
                    header: header,
                    left: leftBox,
                    main: mainBox
                }
            }
        ]
    });
    let vm = new Vue({
        el: '#app',
        router:routerObj
    })
</script>
</body>
</html>
```

## 二十一、watch监听器，computed计算属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

</head>
<body>
<div id="app">
    <label><input type="text" v-model="firstName"></label> +
    <label><input type="text" v-model="lastName"></label> =
    <label><input type="text" v-model="fullName"></label>=
    <label><input type="text" v-model="fullName2"></label>
    <router-link to="/login">登录</router-link>
    <router-link to="/register">注册</router-link>
    <router-view></router-view>
    <br/>
    <input v-model="changeFirstNameFn">
</div>
<script>

    let login = {
        template: '<div><h1>login</h1></div>'
    };


    let register = {
        template: '<div><h1>register</h1></div>'
    };

    let router = new VueRouter({
        routes: [
            {path: '/login', component: login},
            {path: '/register', component: register}
        ]
    });


    let vm = new Vue(
        {
            el: '#app',
            data: {
                firstName: '',
                lastName: '',
                fullName: ''
            },
            watch: {
                //通过watch监听属性值的变化
                firstName(newData, oldData) {
                    this.fullName = newData + '+' + this.lastName
                },
                lastName(newData, oldData) {
                    this.fullName = this.firstName + '+' + newData
                },
                //监听地址栏的变化
                '$route.path': function (newVal, oldVal) {
                    console.log("newVal", newVal, '================>oldVal', oldVal)
                }

            },
            //定义一个计算参数，当方法中所用到的值改变，则计算参数的方法就会被调用
            computed: {
                fullName2(a) {
                    console.log("computed参数", a);
                    return this.lastName + '======='

                },
                changeFirstNameFn: {
                    //当被引用就已经被执行了
                    get: function () {
                        console.log("======>")// get 函数，返回绑定的“姓” 的输入框的value 值
                        return this.firstName + "aaaaaa"
                    },
                    //当变化就被执行
                    set: function (newVal) { //set函数是当数据发生变化时调用.监听到 “姓”的变化，执行set 函数
                        console.log("222222222222222222>")
                        this.firstName = newVal
                        this.fullName = newVal + " " + this.lastName
                    }
                }
            },

            router
        }
    )
</script>
</body>
</html>
```

* ####  watch,computed,methods区别

  * computed:属性的结果会被缓存、除非依赖的响应式属性变化会被重新计算，主要当属性来使用
  * methods:表示一个具体的操作，主要用来写业务逻辑
  * watch:用来监听某个特定的属性的改变
  
  

## 二十二、Vuex全局数据存储处理

```js
import Vue from 'vue'
import Vuex from 'vuex'
import Mock from 'mockjs'

Vue.use(Vuex);

export default new Vuex.Store({
    //vuex数据出状态初始化
    state: {
        count: 1,
        list: []
    },
    mutations: {
        increment(state, payload) {
            state.count += payload
        },
        initList(state,payload){
            console.log(payload);
            state.list = payload;
        }
    },
    getters: {
        filterList(state) {
            console.log('!!!!!!!!!!!!!!!!!!!!!!!!',state.list);
            return state.list&&state.list.list&&state.list.list.map(item => {
                item.sexName = item.sex === 1 ? '男' : '女';
                return item
            })
        }
    },
    actions: {
        getList({commit},payload){
            return new Promise((resolve)=>{
                let list =  Mock.mock({
                    [`list|${payload?payload:4}`]: [
                        {
                            'id|+1': 1,
                            'sex|1': [0, 1],
                            'name': '@cname'
                        }
                    ]
                });
                commit('initList',list);
                resolve("传值成功")
            });
        }
    },
    modules: {}
})

```



```vue
<template>
    <div>
        <h1>vuex状态管理</h1>
        <p>{{counts}}</p>
        <p>{{count1}}</p>
        <p>{{count}}</p>
        <button @click="add(1)">加1</button>
        <button @click="add2(10)">加10</button>
        <button @click="increment(100)">加100</button>
        <br/>
        <ul>
            <li v-for="item in filterList" :key="item.id">
                {{item.name}}========================{{item.sexName}}
            </li>
        </ul>
        <br/>
        <ul>
            <li v-for="item in filterList1" :key="item.id">
                {{item.name}}========================{{item.sexName}}
            </li>
        </ul>
        <br/>
        <ul>
            <li v-for="item in filterList2" :key="item.id">
                {{item.name}}========================{{item.sexName}}
            </li>
        </ul>
        <button @click="getList1(5)">获取5条</button>
    </div>
</template>

<script>
    import {mapState,mapMutations,mapGetters,mapActions} from 'vuex'
    export default {
        name: "VuexDemo",
        created:function () {
            this.getList().then((s)=>{
                console.log(s)
            })
        },
        computed: {
            //获取vuex公共数据的三种写法：
            //1：在计算属性里面监听count数据的变化调用counts函数获取vuex里面count的值
            counts() {
                return this.$store.state.count
            },
            //2：通过定义一个方法返回state里面的值，此时可以对数据进行相应的处理
            ...mapState({
                count1:state=>state.count
            }),
            //3：数组的值来直接过去vuex里的值，此时不能对数据做处理
            ...mapState(['count']),

            //定义一个通过计算vuex后返回值的方法
            filterList1(){
               return this.$store.getters.filterList
            },
            ...mapGetters(['filterList']),
            ...mapGetters({filterList2:'filterList'}),

        },
        methods:{
            //
            add(val){
                //通过vuex添加的store对象里的commit来调用vuex里的计算方法
                this.$store.commit('increment',val)
            },
            //更改vuex里的mutations方法名并将其调用
            ...mapMutations({
                add2:'increment'
            }),
            //直接调用vuex里的increment方法
            ...mapMutations(['increment']),
            getList1(n){
                this.$store.dispatch('getList',n)
            },
            ...mapActions(['getList'])
        }
    }
</script>

<style scoped>

</style>
```



## 二十三、render函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">

</div>
<script>
    
    let login={
      template:'<div>注册{{name}}</div>',
        data:function(){
          return {
              name:"流畅"
          }
        }
    };

    let vm  = new  Vue({
    el:'#app',
        data:{

        },
        //注意，render会替换掉app标签中的所有元素,渲染出子组件
        render:function(creatElements){
            return creatElements(login)
        }
    })
</script>
</body>
</html>
```

