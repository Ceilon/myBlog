---
title: TypeScript
date: 2020-03-11 11:02:56
tags:
---

### 1.typeScript类型

```ts
let b:number = 123;//定义一个ts的数字类型
function tsDamo(data:{x:number,y:number}){
    return Math.sqrt(data.x**2+data.y**2)
}
tsDamo({x:2,y:3})//ts有更好的类型提示a
```

### 2.安装环境

``npm i typescript -g``**//安装ts环境,用tsc来操作**

``npm i ts-node -g``  **//安装ts-node就能直接运行ts文件代码**

``set-ExecutionPolicy RemoteSigned``  **//获取命令行运行权限**

### 3.interface：静态类型

```ts
interface point:{
    x:nmber,
    y:nmber
}
const person:point = {x:12,b:3}
//定义了person，那person就必须满足point的数据类型
```

### 4.类型

* 基本类型：null,undefind,symbol,boolean,boid,void

  ```ts
  const count:number = 123;
  const studentName:string = 'dell'
  ```

  

* 对象类型

  ```ts
  const teacher:{name:string,age:number}={name:'dell',age:18}
  const numbers:number[]=[1,2,3]//数组对象类型,表明numbers是一个对象数组
  class Person{}
  const dell:Person = new Person();
  ```

  

* 函数类型

  ```ts
  const getTotal:()=>nmber=()=>{
      return 12
  }
  //表示getTotal说个方法，这个方法必须返回一个数字
  ```


### 5.类型注解和类型推断

* 类型注解 ``type annotation`` 和类型推断``type inference``

  * 我们来告诉ts变量是什么类型
  * 如果ts能自动分析变量类型，我们就不用注解
  * 如果ts不能分析，我们需要使用类型注解

  ```ts
  // 不需要类型注解
  // const firstNmber = 1;
  // const secondNumber = 2;
  // const total = firstNmber + secondNumber;
  
  
  
  //需要类型注解
  function getTotal(firstNmber: number, secondNumber: number) {
    return firstNmber + secondNumber;
  }
  //不需要注解
  const total = getTotal(1, 2);
  ```
  
  

### 6.函数相关类型

* ts的函数定义方式

  ```ts
  //由于return返回的是字符串，所以add会报错,
  //由于不知道函数返回的类型是什么类型，所有最好给函数一层约束
  function add(first:nmber,second:number):number{
      return first + second+''
  }
  const total = add(1,2)
  
  //如果没有返回值
  function sayHello():void{
      console.log('hello')
  }
  
  //永远不可能执行完用never
  function errorEmitter():never{
    throw new Error("2222222");
    console.log("ssss");
  }
  
  //对象解构赋值
  function add({ first, second }: { first: number; second: number }): number {
    return first + second;
  }
  const a = add({ first: 1, second: 2 });
  ```

  

### 7.数组和元组

* 数组

```ts
//如果数组中存在字符串或者其他类型可以用这种方式注解
const arr:(number|string)[]=[1,2,'3'];
//全字符串注解
const stringArr:string[]=['1','2','3']


//类型别名
type User={name:string,age:number};
const objArr:User[]=[{name:'张三',age:12}]

class Teacher {name:string;age:number;}
const ObjArr:Teacher[]=[{name:'王',age:10}]
```

* 元组 tuple

  ```ts
  //元组是，定义一个数组，但是这个数组已经有了固定的长度和固定每一项的类型
  const teacherInfo:[string,string,number] = ['Dell','male',18]
  
  //应用场景
  const user:[string,string,number][]=[['dell','male',19],['sun','female',26],['jeny','femal',29]]
  ```

  

### 8.自定义验证类型interface

* 常规的应用

```ts
//age?:number表示Person这个静态类型中的age是一个数字，但可有可无
//readonly表示这个值只能被读取，不能被改写所以setPersonName会报错
interface Person {
  // readonly name:string
  name: string;
  age?: number;
  //如果改成这样，如此调用getPersonName({name:'dell',sex:'男'})不会报错
  [propName: string]: any; //表示可以多出其他键名是string类型，值为任何值的属性
  say(): string; //表示Person里面必须还要有个方法，而且返回值是string
}
type Person1 = {
  name: string;
};
type Person2 = string;
const getPersonName = (person: Person): void => {
  console.log(person.name);
};
const setPersonName = (person: Person, name: Person2): void => {
  person.name = name;
};

const person = {
  name: "dell",
  sex: "男",
  say() {
    return "hello";
  }
};

//值得注意的是如果改成getPersonName({name:'dell',sex:'男'})
//校验就会提示错误，而以下面这种形式去写则不会报错
getPersonName(person);
setPersonName(person, "Tom");

//类的应用,通过interface来约束class里的内容
class User implements Person {
  name: "Dell";
  say() {
    return "hello";
  }
}

```

* interface继承

```ts
//继承
interface Person {
  name: string;
  say(): string; 
}
interface Teacher extends Person {
  teach(): string;
}

const setPersonTeacher = (person: Teacher, name: Person2): void => {
  person.name = name;
};

//由于这里继承了person，在属性里除了Person以外，还得新增teach()这个方法
const teacherPerson= {
  name: "Wang",
  say() {
    return "hellow";
  },
  teach() {
    return "teach";
  }
}

setPersonTeacher(
 teacherPerson,
  "Cheng"
);
```

* interface定义函数类型

```ts
//定义函数类型,表示这个函数接收一个word且为string类型的参数，返回值为string
interface SayHi {
  (word:string):string
}
const say:SayHi=(word:string)=>{
return 'hi'
}
```

* 编译问题

  当ts代码编译过后，全部转换成js，ts里面新的东西会被剔除掉，其实这些东西只是为了更好的编写代码作校验

### 9.类Clss的定义与继承

```ts
interface User {
  name: string;
  getName(): string;
}
class Person implements User {
  name = "dell";
  getName() {
    return this.name;
  }
}
class Teacher extends Person {
  getTeacherName() {
    return "Tom";
  }
  //重写getName()，会覆盖掉父类方法,则下面一段代码会输出Lee而不是Dell
  //   getName() {
  //     return "Lee";
  //   }

  //super相当于父类，super.getName()调用父级的方法输出的说dell
  getName() {
    return super.getName() + "Lee";
  }
}

const teacher = new Teacher();
console.log(teacher.getName());
console.log(teacher.getTeacherName());
```

### 10.类的访问类型和构造器(constructor) 

* 访问类型说明
  * private允许在类内被使用
  * protected允许在类内以及继承的子类中使用
  * public允许在类的内外被使用

```ts
class UserName {
  name: string;
  private sayHi() {
    console.log("hi");
  }
  protected sayName() {
    console.log("my name is", this.name);
  }
}

class Teacher extends UserName {
  sayBay() {
    console.log(this.name + " bay");
  }
  teacherName() {
    //由于是protected,子类里可以调用
    this.sayName();
  }
}
const person = new UserName();
const teacher = new Teacher();
teacher.name = "Lee";
person.name = "dell";
// person.sayHi()//因为私有所以不能被调用
teacher.sayBay();
teacher.teacherName();
```

* 构造器constructor

  * 当被new 创建出来时，就会执行构造器

  ```ts
  class Person {
      //name:string;
      //constructor(name:string){
          //this.name=name;
      //}
      //简写
      constructor(public name:string){}
  }
  const person = new Person('dell')//此时就会执行构造器
  console.log(person.name)
  ```


* 子类构造器

  ```ts
  class Person{
      constructor(public name:string){}
  }
  
  //父类如果有构造器，子类继承了父类，则必须调用super()并且传递父类构造器所需要的属性值
  class Teacher extends Person {
    constructor(public age: number) {
      super("dell");
    }
  }
  const teacher = new Teacher(12);
  console.log("name", teacher.name, "age", teacher.age);//name dell age 12
  ```

  

### 11.静态属性，Setter和Getter

```ts
class Person {
  constructor(private _name: string) {}
    //当被读取的时候调用
  get name() {
    return this._name + " Lee";
  }
    //当被修改的时候调用
  set name(name: string) {
    const realName = name.split(" ")[0];
    this._name = realName;
  }
}
const person = new Person("dell");
person.name = "Dell lee";
console.log("name=>", person.name);//Dell Lee
```

* static单例模式

  ```ts
  class Demo {
    constructor(public name: string) {}
    private static instance: Demo;
    static getInstance() {
      if (!this.instance) {
        this.instance = new Demo("dell");
      }
      return this.instance;
    }
  }
  //getInstance通过static挂载到了Demo类上，可以不用new就能直接调用
  const demo1 = Demo.getInstance();
  const demo2 = Demo.getInstance();
  console.log(demo1.name);
  console.log(demo2.name);
  这两次创建的demo1和demo2是完全相同的
  
  ```

  

### 12.抽象类

* readonly只读 

  ```ts
  class Person{
      readonly name :string
      constructor(name:string){
          this.name=name
      }
  }
  const person = new Person('dell')
  console.log(person.name)
  person.name='Lee'//这里会报错，因为name属性只支持读取
  ```

  

* abstract抽象类

  ```ts
  abstract class Graph{
      //这里必须表面方法中可以传入任何参数
      abstract area(...params: any[]):number
  }
  
  class Square extends Graph{
     area(w:number,h:number){
      return w * h;
    }
    // static aa(name:string){
  
  
  }
  let square = new Square()
  square.area(1,2)
  ```

  

### 13.in运算

```js
const name = {a:'Tome',b:'Bob'}
'a' in name //ture
'c' in name //false
```

