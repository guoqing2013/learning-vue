## 单向数据流

**不要在子组件内部改变prop**

prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。这是为了防止子组件无意修改了父组件的状态——这会让应用的数据流难以理解。

另外，每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你**不应该**在子组件内部改变 prop 。如果你这么做了，Vue 会在控制台给出警告。

为什么我们会有修改prop中数据的冲动呢？通常是这两种原因：

1. prop 作为初始值传入后，子组件想把它当作局部数据来用；

2. prop 作为初始值传入，由子组件处理成其它数据输出。

对这两种原因，正确的应对方式是：

1. **定义一个局部变量，并用 prop 的值初始化它**：

   ```javascript
   props: ['initialCounter'],
   data: function () {
     return { counter: this.initialCounter }
   }
   ```

2. **定义一个计算属性，处理 prop 的值并返回。**

   ```javascript
   props: ['size'],
   computed: {
     normalizedSize: function () {
       return this.size.trim().toLowerCase()
     }
   }
   ```
   
   
## 子组件使用$emit改变props
  
1. 父组件可以使用 props 把数据传给子组件；  
2. 父组件中通过v-on:myevent监听自定义事件；  
3. 子组件可以使用 $emit 触发父组件的自定义事件；  
4. 父组件的数据改变，通过prop传递给子组件的值都会更新。  
  
  
子组件：
```html
<template>
  <div class="train-city">
    <h3>父组件传给子组件的toCity:{{sendData}}</h3> 
    <br/><button @click='select(`大连`)'>点击此处将‘大连’发射给父组件</button>
  </div>
</template>
<script>
  export default {
    name:'trainCity',
    props:['sendData'], // 用来接收父组件传给子组件的数据
    methods:{
      select(val) {
        let data = {
          cityname: val
        };
        this.$emit('showCityName',data);//select事件触发后，自动触发showCityName事件
      }
    }
  }
  </script>
  ```
  
  父组件：
```html
<template>
    <div>
        <div>父组件的toCity{{toCity}}</div>
        <!-- <train-city v-on:showCityName="updateCity" :sendData="toCity"></train-city> -->
        <train-city @:showCityName="updateCity" :sendData="toCity"></train-city>
    </div>
<template>
<script>
  import TrainCity from "./train-city";
  export default {
    name:'index',
    components: {TrainCity},
    data () {
      return {
        toCity:"北京"
      }
    },
    methods:{
      updateCity(data){//触发子组件城市选择-选择城市的事件
        this.toCity = data.cityname;//改变了父组件的值
        console.log('toCity:'+this.toCity)
      }
    }
  }
</script>
```
