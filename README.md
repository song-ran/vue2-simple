# 说明

>  非常简单的一个vue2 + vuex的项目，整个流程一目了然，麻雀虽小，五脏俱全，适合作为入门练习。

>  如果对您有帮助，您可以点右上角 "Star" 支持一下 谢谢！ ^_^

>  或者您可以 "follow" 一下，我会不断开源更多的有趣的项目

>  如有问题请直接在 Issues 中提，或者您发现问题并有非常好的解决方案，欢迎 PR 👍

>  开发环境 windows7  Chrome 56 nodejs 6.10.0 以上

>  这个项目主要用于 vue2 + vuex 的入门练习


## 项目运行（nodejs 6.0+）
``` bash
# 克隆到本地
git clone https://github.com/song-ran/vue2-simple.git

git@github.com:song-ran/vue2-simple.git

# 安装依赖
npm install

# 若提示node-sass（fsevents）安装失败
  请在浏览器手动输入下载地址
  （https://github.com/sass/node-sass/releases/download/v4.5.3/win32-x64-57_binding.node）下载
  复制下载完成的文件所在路径 ==> 到项目根目录下按住Shift右键打开命令行窗口输入：
  set SASS_BINARY_PATH=D:/code/win32-x64-57_binding.node
  然后再输入：
  npm i node-sass -D --verbose
  等待安装完成即可

# 开启本地服务器(localhost:8080)
npm run dev

# 发布环境(生成编译后放入服务器的文件)
npm run build
```



# 效果演示


[demo地址](http://cangdu.org/happyfri/)（请用chrome手机模式预览）

### 移动端扫描下方二维码
<img src='https://github.com/songran/vue2-simple/blob/master/src/images/ewm.png' width="300" height="300" />



## 路由配置
```js
import App from '../App'

export default [{
    path: '/',
    component: App,
    children: [{
        path: '',
        component: r => require.ensure([], () => r(require('../page/home')), 'home')
    }, {
        path: '/item',
        component: r => require.ensure([], () => r(require('../page/item')), 'item')
    }, {
        path: '/score',
        component: r => require.ensure([], () => r(require('../page/score')), 'score')
    }]
}]

```



## 配置actions
```js
import ajax from '../config/ajax'

export default {
	addNum({ commit, state }, id) {
		//点击下一题，记录答案id，判断是否是最后一题，如果不是则跳转下一题
		commit('REMBER_ANSWER', id);
		if (state.itemNum < state.itemDetail.length) {
			commit('ADD_ITEMNUM', 1);
		}
	},
	//初始化信息
	initializeData({ commit }) {
		commit('INITIALIZE_DATA');
	}
}

```


## mutations
```js
const ADD_ITEMNUM = 'ADD_ITEMNUM'
const REMBER_ANSWER = 'REMBER_ANSWER'
const REMBER_TIME = 'REMBER_TIME'
const INITIALIZE_DATA = 'INITIALIZE_DATA'
export default {
	//点击进入下一题
	[ADD_ITEMNUM](state, payload) {
		state.itemNum += payload.num;
	},
	//记录答案
	[REMBER_ANSWER](state, payload) {
		state.answerid[state.itemNum] = payload.id;
	},
	/*
	记录做题时间
	 */
	[REMBER_TIME](state) {
		state.timer = setInterval(() => {
			state.allTime++;
		}, 1000)
	},
	/*
	初始化信息，
	 */
	[INITIALIZE_DATA](state) {
		state.itemNum = 1;
		state.allTime = 0;
	},
}
```

## 创建store
```js
import Vue from 'vue'
import Vuex from 'vuex'
import mutations from './mutations'
import actions from './action'


Vue.use(Vuex)

const state = {
	level: '第一周',
	itemNum: 1,
	allTime: 0,
	timer: '',
	itemDetail: [],
	answerid: {}
}

export default new Vuex.Store({
	state,
	actions,
	mutations
})
```


## 创建vue实例
```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import routes from './router/router'
import store from './store/'

Vue.use(VueRouter)
const router = new VueRouter({
	routes
})

new Vue({
	router,
	store,
}).$mount('#app')
```

