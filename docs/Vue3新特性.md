# Vue3新特性

- 向后兼容
- 新特性https://vue3js.cn/docs/zh/guide/migration/introduction.html
- 非兼容的变更
- 性能提升
  - 打包体积减少41%
  - 初次渲染提速55%
  - 热更新提升133%
  - 内存缩减44%
- 良好的TS支持

## Composition API

```JS

```



## 响应式对象

- 追踪变化

```js
const person = {
  name: 'lipeng'
}
console.log(Reflect.get(person, 'name'));
const handler = {
  // get(target, prop, value, receiver) {
  get() {
    console.log('trigger get');
    // return target[prop]
    return Reflect.get(...arguments);
  },
  // set(target, prop, value, receiver) {
  set() {
    console.log('trigger set');
    // target[prop] = value
    // return true
    return Reflect.set(...arguments);
  }
}
const proxy = new Proxy(person, handler);
proxy.name = 'peng'
console.log('proxy.name :>> ', proxy.name);
console.log('person.name :>> ', person.name);
```

- 存储和触发effect

```js
// let product = { price: 5, count: 2 }
// let total = product.price * product.count
let total = 0
let dep = new Set()
function track() {
  dep.add(effect)
}
function trigger() {
  dep.forEach(effect => effect())
}
const reactive = (obj) => {
  const handler = {
    get() {
      let result = Reflect.get(...arguments);
      track()
      return result
    },
    set() {
      let result = Reflect.set(...arguments);
      trigger()
      return result
    }
  }
  return new Proxy(obj, handler)
}

const product = reactive({price: 5, count: 2})

let effect = () => {
  total = product.price * product.count
}
effect()
console.log(total);
// track()
product.price = 10
// effect()
// trigger()
console.log(`total is ${total}`);

```

## 纯函数 pure function

- 相同的输入，永远会得到相同的输出
- 没有副作用

```javascript
const double = x => x * 2 // 符合

Math.random()  // 不符合
```



## 副作用-函数外部环境发生的交互

- 网络请求
- DOM操作
- 订阅数据来源
- 写入文件系统
- 获取用户输入

## Vue3 watchEffect

- 自动收集依赖并且触发

```js
const count = ref(0)

watchEffect(() => console.log(count.value))
// -> logs 0

setTimeout(() => {
  count.value++
  // -> logs 1
}, 100)
```

- 自动销毁effect

  - 组件的生命周期结束时，effect也会自己销毁

  - 手动销毁：

    ```js
    const stop = watchEffect(() => {
      /* ... */
    })
    
    // later
    stop()
    ```

- 使副作用失效

  ```js
  setup(props) {
    const count = ref( 1 )
    watchEffect(( onInvalidate) => {
      console. log(' props effect ' ,props.msg )
      console.log( ' inner effect' , count.value )
      const source = axios.CancelToken.source()
      axios.get(`https://jsonplaceholder.typicode.com/todos/${count.value}`,{
        cancelToken: source.token
      }).catch(err => {
        console.log(err.message)
      })
      onInvalidate(() => {
        source.cancel('trigger')
      })
    })
    return {
      count
    }
  }
  ```

  

- 副作用执行顺序

  - 组件的 `update` 函数也是一个被侦听的副作用。当一个用户定义的副作用函数进入队列时，默认情况下，会在所有的组件 `update` **前**执行。如果需要在组件更新**后**重新运行侦听器副作用，我们可以传递带有 `flush` 选项的附加 `options` 对象 (默认为 `'pre'`)：`post`

  - ```js
    <div ref="node">{{}}</div>
    setup(props) {
      const count = ref(1)
      const node = ref<null | HTMLElement>(null)
      watchEffect(( onInvalidate) => {
        console. log(' props effect ' ,props.msg )
        console.log( ' inner effect' , count.value)
        const currentText = node.value ? node.value.innerText : ''
        console.log('currentText: ', currentText)
        const source = axios.CancelToken.source()
        axios.get(`https://jsonplaceholder.typicode.com/todos/${count.value}`,{
          cancelToken: source.token
        }).catch(err => {
          console.log(err.message)
        })
        onInvalidate(() => {
          source.cancel('trigger')
        })
      }, {
        flush: 'post' // update之后再执行effect
      })
      return {
        count
      }
    }
    ```

## watch精确控制effect

- watch的基本用法

- watch reactive的单个值

  - 使用toRefs

  - ```js
    // 直接侦听ref
    const count = ref(0)
    watch(count, (count, prevCount) => {
      /* ... */
    })
    ```

  - 使用getter函数

  - ```js
    // 侦听一个 getter
    const state = reactive({ count: 0 })
    watch(
      () => state.count,
      (count, prevCount) => {
        /* ... */
      }
    )
    ```

- watch多个值

  - ```js
    watch([fooRef, barRef], ([foo, bar], [prevFoo, prevBar]) => {
      /* ... */
    })
    ```

- react的做法

  - ```js
    const [ count, setCount] = useState(0) ;
    useEffect(() => {
    	console.log(props.msg) ;
    	console.log(count) ;
    }, [props.msg, count]) ;
    ```

- 和watchEffect对比
  - 懒执行副作用
  - 什么状态应该触发watcher重新运行
  - 访问数据变化前后的值



## 自定义函数 - hooks

- 将相关的feature组合在一起
- 非常易于重用



### 界面的需求 - 转化为数据的描述

```js
const useURLLoader = (url) => {
  const data = reactive({
    result: null as any,
    loading: true,
    loaded: false,
    error: null
  })
  axios.get(url). then(resp => {
    data.result = resp. data
    data.loaded = true 
  }).catch((e) => {
    data.error = e
  }).finally(() => {
    data.loading = false
  })
  return data
}
```

### 自定义函数的优点

- 以函数的形式调用，清楚地了解参数和返回类型
- 避免命名冲突
- 代码逻辑脱离组件的存在

### 自定义函数接入TS

```tsx
import { reactive } from 'vue'
import axios from 'axios'

interface DataProps<T> {
  result: T | null;
  loading: boolean;
  loaded: boolean;
  error: any;
}
const useURLLoader = <T = any>(url: string) => {
  const data = reactive<DataProps<T>>({
    result: null,
    loading: true,
    loaded: false,
    error: null
  })
  axios.get(url).then(resp => {
    data.result = resp.data
    data.loaded = true 
  }).catch((e) => {
    data.error = e
  }).finally(() => {
    data.loading = false
  })
  return data
}

interface PostProps {
  userId: number;
  id: number;
  title: string;
  body: string;
}
const post = useURLLoader<PostProps>('https://jsonplaceholder.typicode.com/posts/1')
post.result
```

### react和vue的自定义hooks

- 更新数据的方式

- 触发的时机
  - 为什么要包裹在useEffect中
  - 删除了会有什么问题
  - 为什么vue3不需要这么做也可以



## 小结

## Vue3小结
###新特性总结
* https://v3.vuejs.orglguide/migration/introduction.html
###为什么要有新版本?
- vue2的困境-抽象逻辑代码的缺失

* Typescript支持很差
### Composition API
* setup
* ref
* reactive **注意丧失响应性**
* toRefs
* 生命周期
###深入响应式对象原理
- 保存effect,未来想重新执行的代码-压入特定的数据结构
- 探测对象值的改变- **Proxy对象**
- 执行(trigger) 之前的effect -触发已经保存在数据结构中的effect函数

###副作用side-effect
- 纯函数
  - 相同的输入，永远会得到相同的输出
  - 没有副作用

- 副作用-函数外部环境发生的交互

- React 和Vue的函数式写法

- watchEffect -响应式对象改变的时候自动触发
  - 自动收集依赖
  - 手动销毁副作用
  - 使副作用失效
  - 副作用执行顺序
- watch -精确控制effect

###自定义Hooks
- 将相关的feature组合在一-起
- 非常易于重用

###自定义函数的优点
- 以函数的形式调用，清楚的了解参数和返回的类型
- 避免命名冲突
- 代码逻辑脱离组件存在
- 泛型在函数中的使用
- 和React的实现对比

### 其他

- Teleport
- Fragment
- Emits Component Options
- Global API 修改
- 语法糖
  - ` <script setup>`
  - `<style vars>`