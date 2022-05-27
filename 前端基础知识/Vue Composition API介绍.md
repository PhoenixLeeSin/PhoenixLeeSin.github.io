Composition API是一系列接口的总称，下文将逐一介绍Composition API的各个接口。[学习代码](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FPerryHuan9%2Fvue3-test)

### `setup`

setup是vue新增的一个选项，它是组件内使用Composition API的入口。setup中是没有`this`上下文的（js 函数都有this，但是setup的this跟vue2的this不是一个东西，已经完全没用了，故为没有）。

- 执行时机
   setup是在创建vue组件实例并完成props的初始化之后执行，也是在`beforeCreate`钩子之前执行,这意味着setup中无法使用其它option（如data）中的变量，而其它option可以使用setup中返回的变量。
- 与模板结合使用
   如果setup返回一个对象，这个对象的所有属性会合并到template的渲染上下文中，也就是说可以在template中使用setup返回的对象的属性。

```js
<template>
  <div>{{ count }} {{ object.foo }}</div>
</template>

<script>
import { ref, reactive } from 'vue'

export default {
  setup() {
    const count = ref(0)
    const object = reactive({ foo: 'bar' })

    // 暴露至模板中
    return {
      count,
      object
    }
  }
}
</script>
复制代码
```

- 与Render Function/jsx结合使用
   setup也可以返回render function，这个时候则与vue2中的render方法l类似，此时不需要template。

```js
import { h, ref, reactive } from 'vue'

export default {
  setup() {
    const count = ref(0)
    const object = reactive({ foo: 'bar' })

    return () => h('div', [
      count.value,
      object.foo
    ])
  }
}
复制代码
```

- `setup`的参数
   setup有两个参数，第一个参数为props，是组件的所有参数，与vue2中一样，参数是响应式的，改变会触发更新；第二个参数是setup context，包含emit等属性。下面是其函数签名：

```ts
interface Data {
  [key: string]: unknown
}

interface SetupContext {
  attrs: Data
  slots: Slots
  parent: ComponentInstance | null
  root: ComponentInstance
  emit: ((event: string, ...args: unknown[]) => void)
}

function setup(
  props: Data,
  context: SetupContext
): Data
复制代码
```

### `reactive`

`reactive`函数接收一个对象，并返回一个对这个对象的响应式代理。它与vue2中的`Vue.obserable()`是等价的，为避免与RxJs中的`observable`重名，故改名为`reactive`。

```js
<template>
  <div>
    <input type="text" v-model="state.input">
    <p>input: {{state.input}}</p> 
    <p>computedInput: {{state.computedInput}}</p> 
  </div>
</template>
<script>
  import {reactive, computed, watch} from 'vue'

  export default {
    setup(props) {
      const state = reactive({
        input: '', 
        computedInput: computed(() => '?  '+state.input+'  ?' )
      })
      return {state, singleComputed}
    }
  }
</script>
复制代码
```

必须注意的是，reactive使用者必须始终保持对返回对象的引用，以保持反应性。该对象不能被破坏或散布,所有setup和组合函数中不能返回reactive的解构。

```js
<template>
  <div>
    {{input}}
    <button @click="onClick">change</button>
  </div>
</template>
<script>
  import {reactive, computed, watch, toRefs} from 'vue'

  export default {
    setup(props) {
      const state = reactive({input: ''})
      
      const onClick = () => {
        state.input = '我已经改变了'
      }
      // return state 模板上下文丢失引用
      // return {...state, onClick} 模板上下文丢失引用
      return {...toRefs(state), onClick} // 正确的
    }
  }
</script>
复制代码
```

如上组件如果不使用`toRefs`,在点击change按钮时，组件并不会重新渲染，也就是说模板中的input还是之前的值。出现这种现象的根本原因是js中是值传递，并不是引用传递。解决方案是使用`toRefs`，至于ref是什么，请看下文。

### `ref`

ref函数接收一个用于初始化的值并返回一个响应式的和可修改的ref对象。该ref对象存在一个value属性，value保存着ref对象的值。

```js
<template>
  <div class="tf">
    <div>
      <!--在模板中使用时不需要使用count.value, 会自动解包-->
      <p>{{count}}</p>
      <button @click="onClick(true)">+</button>
      <button @click="onClick(false)">-</button>
    </div>
  </div>
</template>
<script>
  import {ref, reactive} from 'vue'

  export default {
    setup() {
      const count = ref(0)
      <!--在reactive中使用也不需要count.value, 也会解包-->
      const state = reactive({count})
      const onClick = (isAdd) => {
        isAdd ? count.value++ : count.value -- 
      }
      return {count, onClick}
    },
  }
</script>
复制代码
```

*注意*：在reactive和tempalte中使用的时候，不需要使用`.value`，它们会自动解包。

### `isRef`

`isRef`用于判断变量是否为ref对象。

```js
const unwrapped = isRef(foo) ? foo.value : foo
复制代码
```

### `toRefs`

`toRefs`用于将一个reactive对象转化为属性全部为ref对象的普通对象。

```js
const state = reactive({
  foo: 1,
  bar: 2
})
const stateAsRefs = toRefs(state)
/*
Type of stateAsRefs:
{
  foo: Ref<number>,
  bar: Ref<number>
}
*/
复制代码
```

`toRefs`在setup或者Composition Function的返回值特别有用：

```js
import {reactive, toRefs} from 'vue'
function useFeatureX() {
  const state = reactive({
    foo: 1,
    bar: 2
  })
  return state
}
function useFeature2() {
  const state = reactive({
    a: 1,
    b: 2
  })
  return toRefs(state)
}

export default {
  setup() {
    // 使用解构之后foo和bar都丧失响应式
    const { foo, bar } = useFeatureX()
    // 即便使用了解构也不会丧失响应式
    const {a, b}= useFeature2()
    return {
      foo,
      bar
    }
  }
}
复制代码
```

### `computed`

computed函数与vue2中computed功能一致，它接收一个函数并返回一个value为getter返回值的不可改变的响应式ref对象。

```js
const count = ref(1)
const plusOne = computed(() => count.value + 1)
console.log(plusOne.value) // 2
plusOne.value++ // 错误，computed不可改变

// 同样支持set和get属性
onst count = ref(1)
const plusOne = computed({
  get: () => count.value + 1,
  set: val => { count.value = val - 1 }
})
plusOne.value = 1
console.log(count.value) // 0
复制代码
```

### `readonly`

`readonly`函数接收一个对象（普通对象或者reactive对象）或者ref并返回一个*只读的*参数对象代理，在参数对象改变时，返回的代理对象也会相应改变。如果传入的是reactive或ref响应对象，那么返回的对象也是响应的。

```js
<template>
  <!--vue3中允许template下有多个根元素-->
  <input type="text" v-model="state.count">
  <button @click="onClick">change</button>
</template>
<script>
  import {reactive, readonly, watch} from 'vue'

  export default {
    setup() {
      const state = reactive({count: 12})
      const rnState = readonly(state)
      watch(() => {
        // state改变也会触发rnState的watch
        console.log(rnState.count)
      })

      const planObj = {count: 12}
      const rnPlanObj = readonly(planObj)
      const onClick = () => {
        planObj.count = 888  
        console.log(rnPlanObj.count) // 888
      }
      return {state, onClick}
    }
  }
</script>
复制代码
```

### `watch`

相比于vue2的watch，Composition API的watch接口不仅仅是将其逻辑抽取出来，其功能也得到了极大的丰富。下面看其类型定义：

```js
type StopHandle = () => void

type WatcherSource<T> = Ref<T> | (() => T)

type MapSources<T> = {
  [K in keyof T]: T[K] extends WatcherSource<infer V> ? V : never
}

type InvalidationRegister = (invalidate: () => void) => void

interface DebuggerEvent {
  effect: ReactiveEffect
  target: any
  type: OperationTypes
  key: string | symbol | undefined
}

interface WatchOptions {
  lazy?: boolean
  flush?: 'pre' | 'post' | 'sync'
  deep?: boolean
  onTrack?: (event: DebuggerEvent) => void
  onTrigger?: (event: DebuggerEvent) => void
}

// basic usage
function watch(
  effect: (onInvalidate: InvalidationRegister) => void,
  options?: WatchOptions
): StopHandle

// wacthing single source
function watch<T>(
  source: WatcherSource<T>,
  effect: (
    value: T,
    oldValue: T,
    onInvalidate: InvalidationRegister
  ) => void,
  options?: WatchOptions
): StopHandle

// watching multiple sources
function watch<T extends WatcherSource<unknown>[]>(
  sources: T
  effect: (
    values: MapSources<T>,
    oldValues: MapSources<T>,
    onInvalidate: InvalidationRegister
  ) => void,
  options? : WatchOptions
): StopHandle
复制代码
```

- 基础用法

```js
template>
  <div>
    <input type="text" v-model="state.count">{{state.count}}
    <input type="text" v-model="inputRef">{{inputRef}}
  </div>
</template>
<script>
  import {watch, reactive, ref} from 'vue'

  export default {
    setup() {
      const state = reactive({count: 0})
      const inputRef = ref('')
      // state.count与inputRef中任意一个源改变都会触发watch
      watch(() => {
        console.log('state', state.count)
        console.log('ref', inputRef.value)
      })
      return {state, inputRef}
    }
  }
</script>
复制代码
```

- 指定依赖源

  在基础用法中:

  - 当依赖了多个reactive或ref是无法直接看到的，需要在回调中寻找.
  - 在回调中我虽然使用了多个reactive，但我希望只有在某个reactive改变时才触发watch。

  解决上面两个问题可以为watch指定依赖源：

```js
<template>
  <div>
    state2.count: <input type="text" v-model="state2.count">
    {{state2.count}}<br/><br/>
    ref2: <input type="text" v-model="ref2">{{ref2}}<br/><br/>
  </div>
</template>
<script>
  import {watch, reactive, ref} from 'vue'

  export default {
    setup() {
      const state2 = reactive({count: ''})
      const ref2 = ref('')
      // 通过函数参数指定reative依赖源
      // 只有在state2.count改变时才会触发watch
      watch(
        () => state2.count,
        () => {
          console.log('state2.count',state2.count)
          console.log('ref2.value',ref2.value)
      })
      // 直接指定ref依赖源
      watch(ref2,() => {
        console.log('state2.count',state2.count)
        console.log('ref2.value',ref2.value)
      })

      return {state, inputRef, state2, ref2}
    }
  }
</script>

复制代码
```

- watch多个数据源

```js
<template>
  <div>
    <p>
      <input type="text" v-model="state.a"><br/><br/>
      <input type="text" v-model="state.b"><br/><br/>
    </p>
    <p>
      <input type="text" v-model="ref1"><br/><br/>
      <input type="text" v-model="ref2"><br/><br/>
    </p>
  </div>
</template>

<script>
  import {reactive, ref, watch} from 'vue'

  export default {
    setup() {
      const state = reactive({a: 'a', b: 'b'})
      // state.a和state.b任意一个改变都会触发watch的回调
      watch(() => [state.a, state.b],
       // 回调的第二个参数是对应上一个状态的值
       ([a, b], [preA, preB]) => {
        console.log('callback params:', a, b, preA, preB)
        console.log('state.a', state.a)
        console.log('state.b', state.b)
        console.log('****************')
      })

      const ref1 = ref(1)
      const ref2 = ref(2)
      watch([ref1, ref2],([val1, val2], [preVal1, preVal2]) => {
        console.log('callback params:', val1, val2, preVal1, preVal2)
        console.log('ref1.value:',ref1.value)
        console.log('ref2.value:',ref2.value)
         console.log('##############')
      })
      return {state, ref1, ref2}
    }
  }
</script>
复制代码
```

- 取消watch 
   watch接口会返回一个函数，该函数用以取消watch。

```js
<template>
  <div>
    <input type="text" v-model="inputRef">
    <button @click="onClick">stop</button>
  </div>
</template>

<script>
  import {watch, ref} from 'vue'

  export default {
    setup() {
      const inputRef = ref('')
      const stop = watch(() => {
        console.log('watch', inputRef.value)
      })
      const onClick = () => {
        // 取消watch，取消之后对应的watch不会再执行
        stop()
      }
      return {inputRef, onClick}
    }
  }
</script>
复制代码
```

- 清除副作用
   为什么需要清除副作用？有这样一种场景，在watch中执行异步操作时，在异步操作还没有执行完成，此时第二次watch被触发，这个时候需要清除掉上一次异步操作。

```js
<template>
  <div>
    <h3>Cleanup</h3>
    <input type="text" v-model="inputRef">
    <input type="text" v-model="inputRef2">
  </div>
</template>

<script>
  import {ref, watch} from 'vue'

  export default {
    setup() {
      const inputRef = ref('')
      const getData = (value) => {
        const handler = setTimeout(() => {
            console.log('已获得数据', value)
        }, 5000)
        return handler
      }
      watch((onCleanup) => {
        const handler = getData(inputRef.value)
        // 清除副作用
        onCleanup(() => {
          clearTimeout(handler)
        })
      })

      // 另外一种获取onCleanup的方式
      const inputRef2 = ref('')
      watch(inputRef2, (val, oldVal, onCleanup) => {
        const handler = getData(val)
        // 清除副作用
        onCleanup(() => {
          clearTimeout(handler)
        })
      })

      return {inputRef, inputRef2}
    }
  }
</script>
复制代码
```

watch提供了一个onCleanup的副作用清除函数，该函数接收一个函数，在该函数中进行副作用清除。那么onCleanup什么时候执行？

- watch的callback即将被第二次执行时先执行onCleanup。
- watch被停止时，即组件被卸载之后。
- watch选项

watch接口还支持配置一些选项以改变默认行为，配置选项可通过watch的最后一个参数传入。

- lazy
   vue2中的watch默认在组件挂载之后是不会执行的，但如果希望立刻执行，可设置immediate为true。而在vue3中，watch默认会在组件挂载之后执行，如果希望取得与vue2 watch同样的行为，可配置lazy为true：

  ```js
  <template>
    <div>
       <h3>Watch Option</h3>
      <input type="text" v-model="inputRef">
    </div>
  </template>
  <script>
    import {watch, ref} from 'vue'
  
    export default {
      setup() {
        const inputRef = ref('')
        // 配置lazy为true之后，组件挂载不会执行，知道inputRef改变才执行
        watch(() => {
          console.log(inputRef.value)
        }, {lazy: true})
        return {inputRef}
      }
    }
  </script>
  复制代码
  ```

- deep
   如vue2中的deep参数一般，在设置deep为true之后，深层对象的任意一个属性改变都会触发回调的执行。

  ```js
  <template>
    <div>
      <button @click="onClick">CHANGE</button>
    </div>
  </template>
  <script>
    import {watch, ref} from 'vue'
  
    export default {
      setup() {
        const objRef = ref({a: {b: 123}, c: 123})
        watch(objRef,() => {
          console.log('objRef.value.a.b',objRef.value.a.b)
        }, {deep: true})
        const onClick = () => {
          // 设置了deep之后，深层对象任意属性的改变都会触发watch回调的执行
          objRef.value.a.b = 780
        }
        return {onClick}
      }
    }
  </script>
  复制代码
  ```

- flush
   当同一个tick中发生许多状态突变时，Vue的反应性系统会缓冲观察者回调并异步刷新它们，以避免不必要的重复调用。默认的行为是：当调用观察者回调时，组件状态和DOM状态已经同步。
   这种行为可以通过flush来进行配置，flush有三个值，分别是`post`(默认)、`pre`与`sync`。`sync`表示在状态更新时同步调用，`pre`则表示在组件更新之前调用。

- onTrack与onTrigger
   onTrack与onTrigger用于调试:

  - onTrack：在reactive属性或ref被追踪为依赖时调用。
  - onTrigger： 在watcher的回调因依赖改变而触发时调用。

  ```js
  <template>
    <div>
      <h4>test onTrigger & onTrack</h4>
      <input type="text" v-model="debugRef">
    </div>
  </template>
  <script>
    import {watch, ref, reactive} from 'vue'
  
    export default {
      setup() {
        const debugRef = ref(0)
        // 在组件挂载之后只有onTrack被调用
        // 在debugRef改变之后先调用onTrigger，再调用onTrack
        watch(() => {
          console.log('debugRef.value:',debugRef.value)
        }, {
          onTrack() {
            debugger;
          },
          onTrigger() {
            debugger;
          }
        })
        return {inputRef, onClick, debugRef}
      }
    }
  </script>
  复制代码
  ```

### Lifecycle Hooks

Composition API当然也提供了组件生命周期钩子的回调。

```js
import { onMounted, onUpdated, onUnmounted } from 'vue'

const MyComponent = {
  setup() {
    onMounted(() => {
      console.log('mounted!')
    })
    onUpdated(() => {
      console.log('updated!')
    })
    onUnmounted(() => {
      console.log('unmounted!')
    })
  }
}
复制代码
```

对比vue2的生命周期钩子：

- `beforeCreate` -> 使用 `setup()`
- `created` -> 使用 `setup()`
- `beforeMount` -> `onBeforeMount`
- `mounted` -> `onMounted`
- `beforeUpdate` -> `onBeforeUpdate`
- `updated` -> `onUpdated`
- `beforeDestroy` -> `onBeforeUnmount`
- `destroyed` -> `onUnmounted`
- `errorCaptured` -> `onErrorCaptured`

### `provide`&`inject`

类似于vue2中`provide`与`inject`, vue3提供了对应的`provide`与`inject` API。

```js
import { provide, inject } from 'vue'

const ThemeSymbol = Symbol()

const Ancestor = {
  setup() {
    provide(ThemeSymbol, 'dark')
  }
}

const Descendent = {
  setup() {
    const theme = inject(ThemeSymbol, 'light' /* optional default value */)
    return {
      theme
    }
  }
}
复制代码
```

### Template Refs

当使用前文中的ref时会感到迷惑，模板中也有一个ref用于获取组件实例或dom对象，这样会不会冲突。而实际上在vue3中，ref会用来保存templates的ref。

```js
<template>
  <div ref="root">
    Test Template Refs
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  setup() {
    const root = ref(null)

    onMounted(() => {
      // 在render初始化之后，DOM元素将被赋值为ref
      console.log(root.value) // div dom对象
    })

    return {
      root
    }
  }
}
</script>
复制代码
```

### `defineComponent`

该接口是为了支持ts类型引用：

```js
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    foo: String
  },
  setup(props) {
    props.foo // <- type: string
  }
})
复制代码
```

当只有`setup`选项时：

```
import { defineComponent } from 'vue'

// provide props typing via argument annotation.
export default defineComponent((props: { foo: string }) => {
  props.foo
})
复制代码
```

## 总结

  总的来说，vue3的Composition API为开发者提供了更大的灵活，但更大的灵活需要更大的规则，对编程者的代码素质有一定的要求，一定程度上增加了vue的入门难度，另外为了持有原始类型的引用引入了ref，引入了一些复杂度，但相比于Composition API  所产生的效益，这些微不足道。至此本文已到尾声，本文讲解比较浅显，并没有深入讲解各个接口的实现原理，但对于使用Vue3的Composition API已经足够，有不足之处请指出及谅解。

## 参考学习文档

[Composition API RFC](https://link.juejin.cn?target=https%3A%2F%2Fvue-composition-api-rfc.netlify.com%2F%23summary)

