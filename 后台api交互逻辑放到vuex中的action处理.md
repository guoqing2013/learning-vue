旧的写法：
```javascript
import api from '@src/api/counter';
const store = {
  state: {
    count: 0
  },
  mutations: {
    setCount (state, count) {
      state.count = count;
    }
  },
  actions: {
    loadCount ({commit}) {
      api.loadCount().then((count) = {
        commit('setCount', count);
      })
    }
  }
};
```

`api`中通过`axios`向服务端发起请求获取数据：

// @src/api/counter.js
import axios from 'axios';
export default {
  loadCount: () => axios.get('/api/counter')
};

对于`api/serive`s，太薄了，仅仅只是对`axios`进行了一个函数的包裹，而中间我们没有加入任何的逻辑，也不应该加入逻辑。这样做一方面不便于管理，也没办法发挥api的意义。所以，我们将`api`整合进了`store`，对于仅仅用于展示的组件来说就更加便利了，不需要定义`state`、`mutation`，直接通过`action`返回`promise`，交由组件去使用。

把与后台 api 交互的逻辑放到 vuex 中的action 去处理，后台返回的状态放到 store 管理。这样处理后，组件就只负责对数据进行渲染，逻辑非常清晰。
