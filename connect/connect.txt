# @connect 装饰器

来自： https://www.cnblogs.com/bjlhx/p/9009056.html

组件写法中调用了 dva 所封装的 react-redux 的 @connect 装饰器，用来接收绑定的 list 所对应的  redux store 中的 model。
1.list的作用？
2.list的内容？

注意到这里的装饰器实际除了 app.state.list 以外还实际接收 app.state.loading 作为参数，这个 loading 的来源是 src/index.js 中调用的 dva-loading这个插件。
1.loading的作用？
/*
* src/index.js
*/
import createLoading from 'dva-loading';
app.use(createLoading());

它返回的信息包含了 global、model 和 effect 的异步加载完成情况。

数据map一：
{
  "global": true,
  "models": {
    "list": false,
    "user": true,
    "rule": false
  },
  "effects": {
    "list/fetch": false,
    "user/fetchCurrent": true,
    "rule/fetch": false
  }
}

注意：到在这里带上 {count: 5} 这个 payload 向 store 进行了一个类型为 list/fetch 的 dispatch，在 src/models/list.js 中就可以找到具体的对应操作。 

import { queryFakeList } from '../services/api';

export default {
  namespace: 'list',

  state: {
    list: [],
  },

  effects: {
    *fetch({ payload }, { call, put }) {
      const response = yield call(queryFakeList, payload);
      yield put({
        type: 'queryList',
        payload: Array.isArray(response) ? response : [],
      });
    },
    /* ... */
  },

  reducers: {
    queryList(state, action) {
      return {
        ...state,
        list: action.payload,
      };
    },
    /* ... */
  },
};

1、connect使用

@connect(({ list, loading }) => ({
  list,//①
  loading: loading.models.list,//②
}))

说明：

　　　　1、connect 有两个参数,mapStateToProps以及mapDispatchToProps,mapStateToProps将状态绑定到组件的props，mapDispatchToProps将方法绑定到组件的props

　　　　2、代码①：将实体list中的state数据绑定到props，注意绑定的是实体list整体，使用时需要list.[state中的具体变量]

　　　　3、代码②：通过loading将上文“数据map一”中的models的list的key对应的value读取出来。赋值给loading，以方便使用，如表格是否有加载图标

　　　　　　当然代码②也可以通过key value编写：loading.effects["list/fetch"]
      
2、变量获取

因，在src/models/list.js

export default {
  namespace: 'list',

  state: {
    list: [],
  },
}

故在view中使用
render() {
    const { list: { list }, loading } = this.props;
    
说明：

　　定义使用时：list: { list }  ，含义实体list下的state类型的list变量
