# router.beforeEach做登陆验证

## 1.vue router

路由判断首先我们想到的是router.beforeEach 前置导航守卫 ，这个方法接受三个参数 to from next 。

to参数为即将跳转的路由路径，from为当前导航正要离开的路由，next方法用来resolve这个钩子。

下面以工作中写的一个判断为为例子：

```javascript
router.beforeEach(async (to, from, next) => {
 const { name, meta } = to;
 const { requireLogin } = meta;
 if (name === 'login') { // 如果是登录页则用next方法resolve掉这个钩子，如果不是，进行到下一个判断
  return next();  
 }
 const needLogin = requireLogin && !store.getters.user.isLogin; // 从store中读取是否获取了已登录的信息
 if (needLogin) {
  return next({  // 如果没有则跳转到登录页，将当前路由路径放到参数中
   name: 'login',
   params: { back: to },
  });
 }
 return next(); 
});
```


## this.$router 与 this.$route   this.$router.push 与 this.$router.replace

在登录页完成登录请求后进行下面的操作

获取路径中存放前一个路径的参数 ,然后跳转到该页面

```javascript
loginSuccess() {
  const { params: { back } } = this.$route;
  const route = back || { name: 'home' };
  const { name, params, query } = route;
  this.$router.replace({ name, params, query });
 },
```

