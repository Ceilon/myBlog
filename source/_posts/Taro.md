# Taro

### 1.微信小程序父子组件之间传递函数

---



```js
function Father(){
    let test =()=>{
        conosole.log('父级被执行了')
    }
    return(
      <View>
      	<Child onTest={this.test}></Child>  
      </View>
    )
}

function Child(props){
    return(
       <View>
      	<button onClick={props.onTest}>点击</button>
      </View>
    )
}
```

### 2.跳转

---



```js
improt Taro from '@tarojs/taro'

taro.navigateTo({url:'pages/index/index'})//可返回
toro.redirectTo(url:'pages/index/index')//无返回

```

### 3.传参

---



```js
improt Taro from '@tarojs/taro'
taro.navigeteTo({url:'pages/index/index?name=张三'})


useEffect(()=>{
    setUserName(this.$router.params.name)
  },[]);
 // console.log('sssssss',useRouter())
```

### 4. taro所有的标签都必须引用

---



* taro的所有标签都必须引用进来，不然可能h5支持，但是微信小程序没任何反应

  ```js
  //这个Button被引入
  import { View,Button } from "@tarojs/components";
  export default function Child(props) {
    return (
      <View>
        <Button onClick={props.onTest}>点击</Button>
      </View>
    );
  }
  ```

  

### 5.图片的引用

---



```js
import img from '../../static/img/c.jpg'
<Image src={img}/>
//<Image src={request(../../static/img/c.jpg)}/>
```



### 6. css-modules开启

---



```js
  h5: {
    publicPath: '/',
    staticDirectory: 'static',
    postcss: {
      autoprefixer: {
        enable: true,
        config: {
          browsers: [
            'last 3 versions',
            'Android >= 4.1',
            'ios >= 8'
          ]
        }
      },
      cssModules: {
        enable: true, // 默认为 false，如需使用 css modules 功能，则设为 true
        config: {
          namingPattern: 'global', // 转换模式，取值为 global/module
          generateScopedName: '[name]__[local]___[hash:base64:5]'
        }
      }
    }
  }

```

### 7. taro不允许函数渲染

```js
   let a = ()=>{
    return(<Button>sssss</Button>)
  }
  return (
    <View >
      {a()}//react是允许这样写的，taro不允许，因为要兼容微信小程序
      <Image className={styles.img}  src={img}/>
      <Button onClick={props.onTest}>点击</Button>
    </View>
  );
}
```

### 8.阻止事件冒泡

```js
export default function Event() {
  let onTest=()=>{
    console.log('ggggg')
  }
  return (
    <View onClick={(event)=>event.stopPropagation()}>
      <Button onClick={onTest}>点击</Button>
    </View>
  );
}
```

### 9.环境变量，判断当前运行环境(weapp,h5.....)

```js
 const env = process.env.TARO_ENV//h5,weapp
```

### 10.hooks给微信添加title

```js
CheckProfessional.config = {
  navigationBarTitleText: "数据查询"
};
```

