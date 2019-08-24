---
routerRedux路由跳转
---

1、不带参数跳转：
```
dispatch(routerRedux.push({
  pathname : '/couponDetail'
}))
```
2、带参数跳转
```
dispatch(routerRedux.push({
  pathname : '/couponDetail',
  query:要携带的参数object 
}))
```
注意：通过location.query.参数字段来获取参数值

3、在effect里面跳转
```
yield put (routerRedux.replace(pathname: '/domains/buy/pay', query: {payload}));
```
作者：Tina任
链接：https://www.jianshu.com/p/b58a7f059327
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
