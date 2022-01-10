学习笔记
=========

2022年1月7日
--------------

### [echarts大屏使用合集](./nuxt-echarts)
2021年12月8日
---------------
### 关于package.json
```json
{
  "name": "项目名称",
  "version": "项目版本",
  "private": true, // 私有项目
  // 脚本命令
  "scripts": { 
    "dev": "cross-env process.env.ENV=dev nuxt",
    "prod": "cross-env process.env.ENV=production nuxt",
    "build": "nuxt build",
    "start": "nuxt start",
    "generate": "nuxt generate"
  },
  "config": {
    "nuxt": {
      "host": "0.0.0.0",
      "port": "50913"
    }
  },
  // 生产环境依赖
  "dependencies": {
    "@nuxtjs/axios": "^5.13.1",
    "@nuxtjs/proxy": "^2.1.0",
    "@popperjs/core": "^2.9.2",
    "bootstrap": "^3.3.7",
    "bootstrap-vue": "^2.21.2",
    "core-js": "^3.9.1",
    "cross-env": "^7.0.3",
    "jquery": "^2.1.1",
    "js-cookie": "^2.2.1",
    "js-md5": "^0.7.3",
    "nuxt": "^2.15.3",
    "popper.js": "^1.16.1",
    "vuex": "^3.6.2",
  },
  // 开发环境依赖
  "devDependencies": {
    "eslint-config-prettier": "^8.1.0",
    "eslint-plugin-prettier": "^3.3.1",
    "node-sass": "^6.0.0",
    "prettier": "^2.2.1", // 用于美化代码
    "nuxt-precompress": "^0.5.9", // nuxt配置代码压缩
    "sass-loader": "^10.1.1",
  }
}

```
### nuxt.config.js一般用到的配置
```javascript
const webpack = require('webpack')

const CompressionPlugin = require('compression-webpack-plugin') 

const NUXT_ENV = {
  NODE_ENV: process.env.NODE_ENV,
  VUE_APP_BASE_URL: process.env.VUE_APP_BASE_URL,
  VUE_APP_BASE_PORT: process.env.VUE_APP_BASE_PORT,
  VUE_APP_SERVER_API: process.env.VUE_APP_SERVER_API,
  VUE_APP_WS_URL: process.env.VUE_APP_WS_URL,
  VUE_APP_WS_PORT: process.env.VUE_APP_WS_PORT,
  VUE_APP_WEBSOCKET_API: process.env.VUE_APP_WEBSOCKET_API,
  VUE_APP_IMG_URL: process.env.VUE_APP_IMG_URL || ''
}

const APP_BASE_URL = `${NUXT_ENV.VUE_APP_BASE_URL}${NUXT_ENV.VUE_APP_BASE_PORT ? ':' + NUXT_ENV.VUE_APP_BASE_PORT : ''}`
const APP_WS_URL = `${NUXT_ENV.VUE_APP_WS_URL}${NUXT_ENV.VUE_APP_WS_PORT ? ':' + NUXT_ENV.VUE_APP_WS_PORT : ''}`

// import $ from 'jquery'
export default {
  // Global page headers: https://go.nuxtjs.dev/config-head
  head: {
    title: '项目标题', // 全局配置，支持在page钩子里单独配置
    htmlAttrs: {
      lang: 'zh-CN'
    },
    meta: [
      { charset: 'utf-8' },
      {
        name: 'viewport',
        content: 'width=device-width, initial-scale=1, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no, shrink-to-fit=no'
      },
      // SDK配置，page中可自定义，用于SEO
      { hid: 'description', name: 'description', content: '' }
    ],
    link: [
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
      { rel: 'stylesheet', type: 'text/css', href: '/bootstrap/bootstrap.min.css' },
      { rel: 'stylesheet', type: 'text/css', href: '/bootstrapvalidator0.5.3/css/bootstrapValidator.css' }
    ],
    script: [
      { src: '/jquery2.1.1/jquery.min.js' },
      { src: '/bootstrap/bootstrap.min.js' },
      { src: '/bootstrapvalidator0.5.3/js/bootstrapValidator.min.js' }
    ]
  },

  // Global CSS: https://go.nuxtjs.dev/config-css
  css: [
    'assets/scss/all.scss'
  ],

  // 自定义loading
  // loading: {
  //   color: rgb(112, 79, 160)
  // },
  loading: '~/components/common/loading.vue',

  // Plugins to run before rendering page: https://go.nuxtjs.dev/config-plugins
  plugins: [
    { src: '~/plugins/common.js', ssr: true }, // 全局配置入口，该文件中的配置在服务端和客户端生效
    { src: '~/plugins/commonWithoutSSR.js', ssr: false }, // 全局配置入口，该文件中的配置只在客户端生效
    { src: '~/plugins/initial.css', ssr: true }, // 样式文件
    { src: '~/plugins/lib/util', ssr: true }, // 用于注册公用方法
    { src: '~/plugins/lib/protoFun', ssr: true } // 用于增加原型方法
  ],

  // Auto import components: https://go.nuxtjs.dev/config-components
  components: true,

  // Modules for dev and build (recommended): https://go.nuxtjs.dev/config-modules
  buildModules: [],

  // Modules: https://go.nuxtjs.dev/config-modules
  modules: [
    // 配置压缩代码
    'nuxt-precompress',
    // https://go.nuxtjs.dev/bootstrap
    'bootstrap-vue/nuxt',
    '@nuxtjs/proxy',
    // https://go.nuxtjs.dev/axios
    '@nuxtjs/axios',
    '@nuxtjs/style-resources'
  ],
  // 配置代码压缩 https://www.npmjs.com/package/nuxt-precompress
  nuxtPrecompress: {
    enabled: true, // Enable in production
    report: false, // set true to turn one console messages during module init
    test: /\.(js|css|html|txt|xml|svg)$/, // files to compress on build
    // Serving options
    middleware: {
      // You can disable middleware if you serve static files using nginx...
      enabled: true,
      // Enable if you have .gz or .br files in /static/ folder
      enabledStatic: true,
      // Priority of content-encodings, first matched with request Accept-Encoding will me served
      encodingsPriority: ['br', 'gzip']
    },
    // build time compression settings
    gzip: {
      // should compress to gzip?
      enabled: true,
      // compression config
      // https://www.npmjs.com/package/compression-webpack-plugin
      filename: '[path].gz[query]', // middleware will look for this filename
      threshold: 10240,
      minRatio: 0.8,
      compressionOptions: { level: 9 }
    },
    brotli: {
      // should compress to brotli?
      enabled: true,
      // compression config
      // https://www.npmjs.com/package/compression-webpack-plugin
      filename: '[path].br[query]', // middleware will look for this filename
      compressionOptions: { level: 11 },
      threshold: 10240,
      minRatio: 0.8
    }
  },

  styleResources: {
    scss: [
      '~/assets/scss/all.scss' // 全局 scss 变量
    ]
  },

  // 配置代理
  axios: {
    proxy: true
  },
  // 从上到下顺序匹配，先匹配到哪个就走对应的代理
  proxy: {
    // 一般请求的转发
    '/service': {
      target: APP_BASE_URL
    },
    // 用于websocket转发
    '/wservice': {
      target: APP_WS_URL,
      ws: true
    },
    // 用于图片、excel等文件的预览
    '/file': {
      target: APP_BASE_URL,
      pathRewrite: {
        '^/file': '/file'
      }
    }
  },
  // 配置服务启动的端口号和IP访问形式
  server: {
    port: '50913',
    host: '0.0.0.0'
  },
  // mode: "universal",
  // 配置env,通过此配置，可在page、js文件中通过process.env.XXX 获取环境变量
  env: NUXT_ENV,
  // 开启devtools 调试模式
  vue: {
    config: {
      productionTip: true,
      devtools: true
    }
  },
  // build构建参数
  build: {
    analyse: true,
    plugins: [
      new CompressionPlugin({
        test: /\.js$|\.html$|\.css/, // 匹配文件名
        threshold: 10240, // 对超过10kb的数据进行压缩
        deleteOriginalAssets: false // 是否删除原文件
      })
    ],
    optimization: {
      splitChunks: {
        minSize: 10000,
        maxSize: 250000
      }
    },
    vendor: ['axios'],
    // md文件编辑器： 去掉这个的话，会报无法引入外部资源的错
    transpile: ['vue-meditor'],
    cache: true
  }
}

```

2021年9月3日
----------------
### 关于登录鉴权
#### HTTP 无状态协议，即每次请求都是独立的，无法获取上一次请求的状态数据。
1. HTTP Auth Authentication: 请求时携带验证信息，该方法安全性能不高
2. 服务端存储session，客户端存cookie
3. JWT: header中传token
4. OAuth: 用于颁发令牌（token)，有4种授权方式
  * 授权码 authorization code（通过code授权码、redirect_uri回调网址，例子可看下企业微信提供的授权API）
  * 隐藏式 implicit
  * 密码式
  * 客户端凭证
### 关于nuxt内存溢出问题
#### nuxt初次使用，用到现在，记录下问题
1. nuxt生命周期跟vue的有所不同，nuxt
2. nuxt生命周期执行了两次, 目前发现使用v-if会有这种情况，换用v-show无
3. nuxt内存溢出问题，
4. 打包方式与vue有所不同
4. nuxt asyncData header新增钩子的使用
5. 引入外部插件方法有所不同，因为nuxt是在服务端渲染之后返回给客户端，可能存在以下问题：   
  * 服务端无法获取到windows对象，如果插件包中存在关于客户端操作方法的，则会报错不支持，应该分开两种方式，一种只作用于客户端，一种作用域服务端和客户端
  * 特殊情况特殊处理，如vue-meditor md编辑器的引入方法，如下
  ```javascript
    export default {
      build: {
        vendor: ['axios'],
        // md文件编辑器： 去掉这个的话，会报无法引入外部资源的错
        transpile: ['vue-meditor']
      }
    }
  ```
  * 全局引用封装组件，注意下，该组件的运行环境是否包括服务端，针对
6. nuxt内置就已经引入了store状态存储，无须再自己另外引入，跟vue同，都可支持模块化，getters等。
 ![img](./img/store-nuxt.png)
  * store目录下除了index文件不是具名文件，其余都是，在调用的时候需要带上文件名，如下：
  ```javascript
    this.$store.commit('upload'/'SET_WEBKIT_DIRECTORY', true);
    const getters = {
      device: state => state.device,
      userInfo: state => state.user.userInfo
    }
  ```
  * store文件写法：
  ```javascript
    export const state = () => ({
      dict: Cookies.get('dict') || {}
    })
    export const mutations = {
      SET_DICTINFO: (state, data) => {
              state.dict = data;
              Cookies.set('dict', data)
      }
    }
    export const actions = {
      // 获取数据字典
      getSysDict({ commit, state }) {
        return new Promise((resolve, reject) => {
          // getUserInfo(state.token)
          getDict().then(response => {
            console.debug(response, 'response')
            commit("SET_DICTINFO", response)
            resolve(response)
          }).catch(error => {
            reject(error)
          })
        })
      }
    }
  ```
7. nuxt需要在nuxt.config.js中配置env配置，才能保证在page，js中通过process.env.XXX来获取环境变量
  ```javascript
  const NUXT_ENV = {
    NODE_ENV: process.env.NODE_ENV,
    VUE_APP_BASE_URL: process.env.VUE_APP_BASE_URL,
    VUE_APP_BASE_PORT: process.env.VUE_APP_BASE_PORT,
    VUE_APP_SERVER_API: process.env.VUE_APP_SERVER_API,
    VUE_APP_WS_URL: process.env.VUE_APP_WS_URL,
    VUE_APP_WS_PORT: process.env.VUE_APP_WS_PORT,
    VUE_APP_WEBSOCKET_API: process.env.VUE_APP_WEBSOCKET_API,
  }
  export default {
      env: NUXT_ENV,
  }
  ```
8. nuxt项目部署方法有所不同，服务端需要装pm2（当内存不够用时会自动重启，释放内存~）
9. nuxt assets文件夹中的引用，引用方法如下
、、、html
  <template>
    <img src="~/assets/img.png" />
    <img :src="require(`~/assets/img/${image}.jpg`)" />
  </template>
```
```css
  background: url('~assets/banner.svg')
```
2021年7月23日
----------------
## 隔好久没写了欸，总结下最近的情况吧
### [bookmarkHub](https://github.com/dudor/BookmarkHub/blob/main/README_cn.md#%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85) 用于不同浏览器同步书签的插件

### 最近在开发企业微信移动端项目，发现一个调试方法
1. 打开企业微信[内置](http://debugx5.qq.com/)
2. 勾选vConsole调试功能，如果打开后提示内核版本不符，可以降版本或者手动打开TBS settings，勾选vConsole调试功能，如下图：
 ![img](./img/vConsole.jpg)
3. 但这个只能

2021年5月21日
----------------

### gitconfig配置
C盘下有个gitconfig配置，可以用来配置git显示的名称哦，直接改文件就能生效啦~

### node_modules快速删除
全局安装rimraf  命令：rimraf node_modules 就酱，3秒内就删掉了，再也不用管什么管理员权限啥的，删个文件删老久……


2021年5月11日
----------------

### http-server简单应用
1. 安装命令（需有node环境），[github地址](https://github.com/http-party/http-server)
  ```bash
  # 全局安装
  $ npm i http-server -g
  # 快速使用
  $ http-request
  $ hs
  ```
2. 应用： 本地项目打包成静态文件后，用http服务起下即可看下效果，具体应用在[nuxt-seo项目中](./nuxt-seo) ，生成的路径可在局域网里访问


2021年4月19日
----------------

### 监听页面刷新以及强制关闭方法
1. 产品提出需求，在进编辑页面的时候需要给页面加上锁，不让其他人进入编辑。一开始，我只是在入口处调用接口，进入页面后，通过钩子beforeDestroy释放锁。后面发现，进到页面后，在当前页面强制关闭页签，是不会走beforeDestroy这个钩子的，故引入第2点；
2. 页面created钩子里增加beforeunload监听，destroyed钩子里取消改监听，代码如下：
```javascript 
  created() {
    window.addEventListener('beforeunload', e => this.beforeunloadFn(e))
  },
  beforeDestroy () {
    this.releaseLock();
  },
  destroyed () {
    window.removeEventListener('beforeunload', e => this.beforeunloadFn(e))
  },
  methods: {
    beforeunloadFn() {
      this.releaseLock();
    },
    releaseLock() {
      // 调用接口释放锁
    }
  }
```
以上方法实现了路由跳转时、浏览器页签关闭以及重新刷新之后的释放锁，但是~重新刷新的时候用户的页面还是会在编辑页面的，这个时候没有锁了，别的用户就也能进来编辑了，不允许，于是引入第3点.
3. 在页面挂载**请求页面数据完**之后，**页面重新渲染完成之后**再请求一次加锁的接口。
  ```javascript 
    if(!localStorage.getItem('actProLockHash')) {
      this.$nextTick(()=>{
        that.handleViewProjectDetail(that.projectInfo.actInfoHash, that.projectInfo.id)
      })
    }
  ```
  以上，就可以啦~~

4. **2021年4月20日补充：** 但是但是，beforeunload,unload事件数据浏览器事件，同时也没有相应的规范，不同浏览器会有不同的写法，好坑。!!!现在测试的结果是：谷歌浏览器beforeunload事件不是任意时候都能触发的，需要进到页面后，有页面的交互之后关闭浏览器页签或者刷新，才能触发。

5. **补充：** 原本beforeunload的出现，是为了弹框提示关闭浏览器，这里用来加异步请求，百度了下看到其他人踩过的坑，ajax请求是不支持的，axios是可以的，具体原理还不清楚。虽然axios支持，但是axios方法之后的then方法还没有进去，页签就己经关闭了。

6. **5月25日补充：** 监听浏览器页签关闭之后触发的事件中，可以加一个延时，等执行完再关闭浏览器，这样子能解决上面的问题，只设置了100ms，使用过程中用户不会感受到明显的关闭页签缓慢，又能有时间去调用接口释放锁。代码如下：
```javascript
  beforeunloadFn(e,flag) {
    if(flag = 'onUnload') {
      let now = new Date()
      while (new Date() - now < 100) { }
    }
    this.releaseLock();
  }
```
另外，之前将释放锁的哈希值存在本地缓存当中，存的是字符串类型；后面需求改动成进入编辑页是另开一个页签来显示，也就意味着，同个用户可以打开多个编辑页，有多个锁，哈希值的存储也应该相应的改成对象存储。

7. **7月23日补充：** 基于以上，后端将锁的哈希值拼上用户ID，通过判断ID，使用户可以多次打开同个页面，页面中再通过websocket监听页面关闭，一旦用户关闭其中一个页面，则全部页面都关闭。

### pxCook 用于量设计图尺寸的神器

### iframe初体验
一直以来只知道iframe可以嵌套别的网页进自己的项目页面中，这次公司的项目刚好有个需求可以用iframe实现，实现过程中才发现有些问题和用法，总结如下：
1. iframe有load事件，通过load可以调用postMessage()将cookie/userInfo等传递过去，如下：
![img](./img/iframe.png)
2. 每次切换页签回来，iframe中的内容不会被加进缓存，每次都需要重新请求页面，会有空白页面的效果，此时iframe里没有内容，宽高不够，需要写死一个最小高度和100%宽度来撑开。
  * 因为iframe中的内容不属于节点的信息，keep-alive对iframe中的内容无效。
  * 在网上看到的一个解决方法： 在主界面下router-view以外渲染iframe，通过v-show控制显隐，后面想想可行性，觉得不太可行：
    在app.vue中，router-view 以外渲染iframe, iframe页面中没有定义在page文件夹中的公用页面，侧边栏、顶部、底部和登录页锁屏页404页等等，需要自己在iframe中配，感觉工作量增大不小且肯定会有其他问题，因此不采取。

### wangeditor扩展功能


2021年4月1日
----------------
### 搭建nuxt框以实现SEO
代码在[nuxt-seo](./nuxt-seo) 

2021年3月31日
------------
### 小程序实现纵横向可滑动，横向滑动时奇数行固定，纵向滑动时顶部底部固定，效果如下:
![img](./img/sticky-show.gif)

* 采用sticky布局，写一个scroll-view，支持横纵向滚动
![img](./img/sticky.png)
代码在[mini-app-sticky](./mini-app-sticky)                                                                                                        
### 小程序实现左列固定，右列可横向滚动，总体可纵向滚动，纵向滚动时顶部底部固定，效果如下：

### websocket实时监听消息

### 大屏，效果如下
<!-- ![Image text](./image/print.gif) -->
* 通过一个平台做成，平台上引入了：
  1. echart图表
  2. [echarts-liquidfill水球图（案例）]](https://www.jianshu.com/p/2ebb1591cedc)，[echarts-liquidfill水球图（github）]](https://github.com/ecomfe/echarts-liquidfill)
  3. [轮播插件（案例）]](https://www.dowebok.com/demo/188/index6.html)，[轮播插件（github）]](https://github.com/omcg33/jquery.limarquee)
* 这个大屏中，需要写随机数展示，根据节假日工作日、时间点判断展示不同用户数，代码如下（只加了2021年的）
  ```Javascript
  function getWeekDate() {
      var now = new Date();
      var day = now.getDay();
      var weeks = new Array("星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六");
      var week = weeks[day];
      return (week == '星期日' || '星期六') ? 'holiday' : 'weekday';
  }
  function get_time(a) {
      return new Date(a).getTime() / 1000; 
  }

  function getWorkingDay() {
      var now = new Date().getTime()/1000; //获取当前时间戳,精确到秒,示例:1570463999
      
      var nowDate = new Date();
      var year = nowDate.getFullYear();
      var month = nowDate.getMonth() + 1 < 10 ? "0" + (nowDate.getMonth() + 1) : nowDate.getMonth() + 1;
      var day = nowDate.getDate() < 10 ? "0" + nowDate.getDate() : nowDate.getDate();
      var dateStr = year + "-" + month + "-" + day;

      var time_10_01 = get_time("2021/10/01 00:00:00");  //节假日时间
      var time_10_07 = get_time("2021/10/07 23:59:59");
      var time_09_19 = get_time("2021/09/19 00:00:00");
      var time_09_20 = get_time("2021/09/20 23:59:59");
      var time_06_12 = get_time("2021/06/12 00:00:00");
      var time_06_13 = get_time("2021/06/13 23:59:59");
      var time_05_01 = get_time("2021/05/01 00:00:00");
      var time_05_04 = get_time("2021/05/04 23:59:59");
      var time_04_03 = get_time("2021/04/03 00:00:00");
      var time_04_04 = get_time("2021/04/04 23:59:59");
      var time_02_11 = get_time("2021/02/11 00:00:00");
      var time_02_16 = get_time("2021/02/16 23:59:59");
      
      //获取时间戳
      //判断
      if(now > time_10_01 && now < time_10_07 ||
          now > time_09_19 && now < time_09_20 || 
          now > time_06_12 && now < time_06_13 || 
          now > time_05_01 && now < time_05_04 || 
          now > time_04_03 && now < time_04_04 ||
          now > time_02_11 && now < time_02_16) {
          return 'holiday'
      } else if(dateStr == '2021-02-07' ||
          dateStr == '2021-02-20' ||
          dateStr == '2021-04-25' ||
          dateStr == '2021-05-08' ||
          dateStr == '2021-09-18' ||
          dateStr == '2021-09-26' ||
          dateStr == '2021-10-09') {
          return 'weekday'
      } else if(new Date().getDay() == '6' || new Date().getDay() == '0'){
          return 'holiday'
      } else{
          return 'weekday'
      }
  }
  ```
* 通过一些插件包实现了很多动态效果，其中也加了很多css动态样式：
  1. 每个分块区域上的动态效果，上面的亮点图，左右来回移动
  ```css
  .normal_box1:before {
    content: url(/img-light-2.png);
    position: absolute;
    animation: rotation 4.5s infinite alternate;
    top: -25px;
    left: -100px;
    opacity: 0.8;
  }
  @-webkit-keyframes rotation {
    0% {
        transform: translateX(0);
   }
    100% {
        transform: translateX(350px);
   }
  }
  ```
  2. 背景色渐变的样式，在div::before 设置60度渐变背景色
  ```css
  .blue-border::before {
    background-image: linear-gradient(60deg, #00c2ff, #0575e6, #007bff, #021b79);
    background-size: 300%;
    content: '';
    position: absolute;
    width: calc(100% + 1%);
    height: calc(100% + 4%);
    border-radius: 0.5em;
    z-index: -1;
    animation: animate_bg 5s infinite;
  }
  @-webkit-keyframes animate_bg {
    0%, 100% {
      background-position: 0%, 50%;
    }
    50% {
      background-position: 100%, 50%;
    }
  }
  ```
  3. 设置文字渐变且动态改变
  ```css
  .blue-border {
    animation: animate_text_blue 2s linear infinite alternate;
  }
  @-webkit-keyframes animate_text_blue {
    0% {
      color: #00c2ff;
    }
    100% {
      color: #021b79;
    }
  }
  ```

2021年3月3日
------------
### px rpx rem em的区别
* px 相对长度单位： 相对显示器屏幕分辨率而言
* em 相对长度单位： 相对于当前对象内文本的字体尺寸
    1. 任意浏览器的默认字体为16px，即: 1em=16px => 0.625em=10px,在body中声明Font-size=62.5%，这样 16px*62.5%=10px,即12px=1.2em,10px=1em
    2. em的值不是固定的，它会继承父级元素的字体大小
* rem 相对长度单位： 相对于根元素（root em)
    1. IE8及更早版本不支持
* rpx 小程序解决自适应屏幕尺寸的尺寸单位，小程序规定屏幕宽度为750rpx
    1. 小程序同时支持rem尺寸单位，小程序规定屏幕宽度为20rem, 1rem=(750/20)rpx=37.5rpx
* upx uni-app使用upx为尺寸单位，uni-app规定屏幕基准宽度750upx

### css样式的应用
 ```css
  <!-- 逐渐消失的样式 -->
    .truncate-text-multiline {
        overflow: hidden;
        display: block;
        height: 109.2px;
        margin: 0 auto;
        font-size: 26px;
        line-height: 1.4;
        width: 400px;
        position: relative;
    }
    .truncate-text-multiline:after {
        content: '';
        position: absolute;
        bottom: 0;
        right: 0;
        width: 150px;
        height: 36.4px;
        background: linear-gradient(to right, rgba(0, 0, 0, 0), #f5f6f9 50%);
    }
  <!-- 列表计数器 -->
    ul {
    counter-reset: counter;
    }
    li::before {
    counter-increment: counter;
    content: counters(counter, '.') ' ';
    }
   <!-- 自定义文本选择样式 -->
    ::selection {
        background: aquamarine;
        color: black;
    }
    .custom-text-selection::selection {
        background: deeppink;
        color: white;
    }
    <!-- 创建动态阴影 -->
    .dynamic-shadow {
        position: relative;
        width: 10rem;
        height: 10rem;
        background: linear-gradient(75deg, #6d78ff, #00ffb8);
        z-index: 1;
    }
    .dynamic-shadow::after {
        content: '';
        width: 100%;
        height: 100%;
        position: absolute;
        background: inherit;
        top: 0.5rem;
        filter: blur(0.4rem);
        opacity: 0.7;
        z-index: -1;
    }
    <!-- 更改子项聚焦时，父类的外观 -->
    form {
        border: 3px solid #2d98da;
        color: #000000;
        padding: 4px;
    }
    form:focus-within {
        background: #f7b731;
        color: #000000;
    } 
    <!-- 给p标签添加渐变颜色 -->
    .gradient-text {
        background: -webkit-linear-gradient(pink, red);
        -webkit-text-fill-color: transparent;
        -webkit-background-clip: text;
    }

  ```

### vue 如何实现数据的实时渲染
#### 通过发布者订阅者这个设计模式
* 发布者：数据  
* 订阅者：Watch(3种：render Wathcer    computed Watcher    user Watcher）
##### render Watch

### vue 依赖包
前阵子发现项目越跑越慢，编译的很慢，有时候切换个分支就会报错，想着可能是依赖包太大了，于是整理了下并删掉一些没有用的：
* vue-json-editor：旧的编辑器  quill，vue-quill-editor：目前在用的编辑器
* el-bigdata-table：大数据表格渲染（未用到）
* id-validator：中国大陆身份证校验器（未用到）
* pdfobject：PDF预览插件
* vue-draggable-resizable-gorkys：缩放插件
* vue-bus: 用于组件间的通信
* vue-json-editor: json编辑器
* vue-json-tree-view：有配置但是木有用到
* jsencrypt：对数据进行加密（wk有用到）
* codemirror：代码高亮（系统参数）
* lockr：本地存储插件，配合i18n，用于语言切换等
* numeral：数据格式化
* nzh：数字跟中文转换插件（wk有用到）
* pinyin-match：拼音match(wk有用到)
* qrcodejs2：生成二维码插件
* vue-cropper：截取图片（wk用于头像截取）
* vue-draggable-resizable：拖拽插件
* vuedraggable: 拖拽插件
* vue-radial-progress：进度条(wk有用)
* Lockr: vue插件 用于操作localStorage，本地存储对象、数组、数字、字符串，提供多种方法：
  1. Lockr.prefix 给键值添加前缀
  2. Lockr.set 设置值（类型可以为String,Number,Array,Object)
  3. Lockr.get 通过键值返回值，可以指定默认返回值
  4. Lockr.rm 完全删除键值对
  5. Lockr.sadd 在原来数值的基础上追加值
  6. Lockr.sismember 判断指定键是否存在某个值（验证下是否只能是String,Number)
  7. Lockr.srem 移除指定键的某个值
  8. Lockr.getAll 获取localStorage所有键值对

### 埋点功能
前阵子公司项目里产品提需求：要统计每个功能模块的使用次数，于是引入：埋点，做的过程中总结以下问题（以公司CRM项目为基础）
* 埋点根据功能编码，初步想通过登录后返回的权限功能数组，发现有一部分页面是公共模块，没有配权限编码，因此通过在路由中meta属性中增加字段，如下（vue-element PC端）：
```javascript
var funcBm = '';
var findFuncBm = function(target) {
  let menus = JSON.parse(localStorage.getItem('menus'));
  if(!menus || menus.length == 0) return;
  funcBm = '';
  console.debug('menus',menus)
  menus.forEach((item)=>{
    item.children.forEach((cItem)=>{
      if(cItem.path == target.path) {
        funcBm = cItem.funcBm;
        return;
      }
    })
  })
}

// 功能使用记录
export function logFuncUseLog(target){
  console.debug('target', target)
  // 获取功能编码
  if(target.meta.funcBm) funcBm = target.meta.funcBm;
  else findFuncBm(target);

  return new Promise((resolve, reject)=>{
    try{
      resolve()
      if(funcBm) {
        handleSetUseLog(funcBm).then((res) =>{
        }).catch((err) => {
          console.log('统计使用次数报错了', err)
        })
      }
    } catch (e) {
      reject(e)
    }
  })
}
```
以上方法置于路由跳转之前调用
```javascript
Router.beforeEach((to, from, next) => {
  // console.debug(to, from)
  const pageErrorList = ['/login', '/500', '/502', '/11', '/403', '/404']
  if (checkNull(Store.getters.userInfo) && pageErrorList.indexOf(to.path) < 0) {
    getUserInfo().then(res => {
      const {data, code} = res.data
      if (res.status === 200 && res.data.code === 0) {
        Store.commit("SET_USERIFNO", data);
        localStorage.setItem('userInfo', JSON.stringify(data))
        logFuncUseLog(to).then(() => {
          next()
        });
      } else {
        if (localStorage.getItem('isLoginFlag')) {
          Notification({
            message: '获取信息失败,请重新登录',
            type: 'error'
          })
          localStorage.removeItem("isLoginFlag");
        }
        next('/login')
      }
    }).catch(res => {
      console.error(res)
    });
  } else {
    logFuncUseLog(to).then(() => {
      next({replace: true})
    })

  }
})
```
* 在小程序端，小程序端是个体力活动了，需要到每个页面去，在onShow中手动调用方法从而实现埋点
  除了有一点，接口需要通过header传参，把功能编码传递给后端的，小程序的请求用了flyio，flyio中已设置了拦截器，在不改变原来配置的基础上，只能另起一个文件（不能跟其它接口放置同个文件中），重新配置需要的headers参数，如下：
  ```javascript
  export function handleSetUseLog(data) {
    request.interceptors.request.use((request) => {
        // request.headers['analyseCode'] = data
        request.headers = {
            "content-type": "application/json",
            'Admin-Token': wx.getStorageSync('token'),
            "Client-Type": "APP",
            "token": wx.getStorageSync('token'),
            "Span-Session": getApp() ? getApp().globalData.spanSession : null,
            'analyseCode': data,
            "version": config.version,
        }
    });
    return request.request({
        url: `user/func/use/l_log`,
        method: 'GET'
    })
  }
  ```

2020年7月24日
------------
### 终于有空了，来学习下，等下就来总结总结
##### 进程和线程
* 进程是CPU资源分配的最小单位（进程是能拥有资源和独立运行的最小单位）
* 线程是CPU调度的最小单位（线程是建立在进程基础上的一次程序运行单位，一个进程可以有多个线程）
  1. 浏览器是多进程的，每打开一个tab页就相当于创建了一个独立的浏览器进程
  2. 浏览器进程的分类：
    - 2.1 Browser进程： 浏览器主进程，只有一个，负责协调、主控
    - 2.2 第三方插件进程（每种类型的插件对应一个进程，仅当插件使用时才创建）
    - 2.3 GPU进程： 最多一个，用于3D绘制
    - 2.4 Render进程（浏览器内核）：浏览器渲染进程，默认每个tab页一个进程，内部是多线程的，主要作用是页面渲染、脚本执行、事件处理
        - 2.4.1 GUI渲染线程
        - 2.4.2 JS引擎线程（JS内核）
        - 2.4.3 事件触发线程：用于控制事件循环
        - 2.4.4 定时触发器线程
        - 2.4.5 异步HTTP请求线程
  3. Browser进程和Render进程的通信：
    - 3.1 JS引擎会阻塞页面加载 -> 引入webWorker,JS的多线程： 
      - 3.1.1 一个worker是使用构造函数创建的一个对象（Worker())运行一个命名的js文件；
      - 3.1.2 这个文件包含将在工作线程中运行的代码，workers运行在另一个全局上下文中，不同于当前的window，创建Worker时，JS引擎向浏览器申请一个子线程（子线程受主线程控制，不能操作DOM）
      - 3.1.3 worker线程跟JS引擎线程的通信，通过postMessage API,通过序列化对象与线程交互特定的数据
      - 3.1.4 webWorker 只属于某个页面，不会和其它页面的Render进程共享，是属于render进程下的一个线程
    - 3.2 SharedWorker
      - 3.2.1 浏览器所有页面共享，可以为多个Render进程共享使用
      - 3.2.2 chrome 为sharedWorker 单独创建一个进程来运行JS程序，由独立的进程管理。
      
2020年4月30日
---------------
### element滚动条宽高的设置
```css
    ::webkit-scrollbar {
        width: 12px;
        height: 12px;
    }
```
      
### vue-element 实现一个带选择框，table表格，单选或多选框
1. 在开发过程中，有踩到很多坑，写在前面吧，下次看的时候能一下子看到
    * props 传回来的数据是不允许修改的，只能监听。
    * 修改this中对象数组的值时，最好先复制一份temp出来，对temp进行push，pop等操作完之后，再把整个temp赋到data中。在开发过程中，我就是因为直接对data进行push等操作，最后发现页面显示的一直是改变之前的数据，在chrome中用vue的devtool工具查看，数据是已经修改成功的了，但页面就是监听不到没有渲染出来~只有对整个data进行重新布局才可以。
    * element dialog有个问题，我原本是用dialog，里面嵌套table啥的，当table列数太多，width太大的时候，选中数据的时候整个dialog都会跟着抖动了下，查了方法，改了很久，最后也……还是会。很心累，所以，我换成了element的抽屉组件。
    * 在写其他模块的时候，有过一个问题，原本我想的是：列表页->编辑页，使用弹框跳转，跳转的时候直接把data对象传过去，在编辑页通过computed属性重新赋值到data中，之后我在编辑页有加下面写的搜索选择组件，选择完之后通过validate校验，可是校验不通过，其他的form-item都是可以的，唯独这个通过弹框选择组件选择的没办法。最后发现原因是，form表单的自动校验要一开始就把data定义好，给个空值就好。试了很多种方法之后，决定采用，进入编辑页的时候传个id过来，通过调接口获取到form表单数据。
    * 有一个问题是这样子的，关于滚动的问题，问题出现的场景是，弹框页面form提交表单，点击提交，校验失败之后回滚到失败的位置去。一开始一直没成功的原因是，我点击弹框，他的滚动条是加在el-scrollbar上面的，我一直获取不到容器，后面我把滚动条加在了form上，再设置scrollTop就能滚动了。  
    * 关于计算属性的问题，computed属性里默认只有getter，没有setter。在该组件的list.vue中，我一开始在data里面定义selectedList和tempList,把父组件传过来的赋给他们，但是这有一个问题，当我选择框选择数据之后，再去搜索table表格数据，这个时候selectedList数据会有丢失的情况。于是我改成了用computed来定义selectedList和tempData,但这又会报错，原因是computed默认是只有get方法让你获取到值，没有set方法来支持你改变他的值，所以需要自己手动定义一下set方法，就不会报错了。
        get方法 是取，即给变量值赋值
        set方法 当变量值发生改变后会出发set方法。
    * 记一个困扰了我1个多小时的bug~~~~~~我一个页面，调用了多个组件，其中有2个组件的name是一样的，导致我整个页面白屏，一直在重复请求一个接口。一开始以为是我哪里写了死循环，后面同事发现了是这个问题，以后一定要注意每个组件的name值都设成唯一的值，不要重复。
    * 另外，还有一个问题，关于select框的回显，我从接口已经拿到select的id了，select选择列表也已经有了，但是渲染出来的就始终是id，没有显示name值，排除了：
        1. 设置setTimeout,确保列表数据已经拿到再来给选择赋值
        2. 用v-if，让数据全部拿到之后重新渲染
        3. 用this.$nextTick(()=>{}),同1原理相似
        用了以上3个方法，仍然不行，最后用typeof 发现，列表里的id类型是number,接口获取到的选择值是stirng,~~~~晕死。
2. 实现逻辑
    * 封装el-table组件，命名为list-table：
        * 组件中传递api(function)，查询参数(object)，是否为单选||多选(boolean),已选择的唯一ID数组(array),后续的再根据需求增加
        * 通过$emit定义选择框的方法,并将选择的数据传回给父组件（含有点击行选择，点击checkbox多选，点击radio单选，全选，手动选择）
        * 需要实现跨页选择，即点击页面跳转之后勾选数据，选择的列表要往上叠加，不能丢失。需要el-table-column中添加```:reserve-selection="true"```,定义type ```type="selection"```,如下： 

    * 封装el-table组件，命名为list-table：
        ** 组件中传递api(function)，查询参数(object)，是否为单选||多选(boolean),已选择的唯一ID数组(array),后续的再根据需求增加
        ** 通过$emit定义选择框的方法,并将选择的数据传回给父组件（含有点击行选择，点击checkbox多选，点击radio单选，全选，手动选择）
        ** 需要实现跨页选择，即点击页面跳转之后勾选数据，选择的列表要往上叠加，不能丢失。需要el-table-column中添加```:reserve-selection="true"```,定义type ```type="selection"```,如下： 

        ```vue
          <el-table-column :reserve-selection="true" :row-key="item.id" fixed="left" type="selection" class="selection" :prop="item.id" width="50" align="center"/>
        ```
        在el-table中添加```:row-key="getRowKeys```,
        getRowKeys方法：```getRowKeys(row) { return row[this.id] },```

        * 根据传过来的已选择的唯一ID数组，循环列表勾选上

        ```javascript
          // 判断是否为选中状态
            ifChooseData() {
              if(this.multi) {
                this.$refs['listTableObject'].clearSelection();
                var tempList = this.tempList || this.tempData;
                for(let j=0; j<tempList.length; j++) {
                  this.$nextTick(() => {
                    this.list.forEach(item => {
                      if(item[this.id] == tempList[j]) {
                        this.$refs['listTableObject'].toggleRowSelection(item,true)
                      } else {
                        // this.$refs['listTableObject'].toggleRowSelection(item,false)
                      }
                    })
                  })
                }
              }
            }
        ```

        * 通过slot插槽，在父组件中自定义table列数据。
        * 添加分页组件。
        * 具体代码如下： 

        ```vue
           <template>
             <div class="zsl-table">
               <el-table
                 v-loading="queryLoading"
                 :data="list"
                 v-bind="$attrs"
                 :default-sort="sort"
                 highlight-current-row
                 :row-key="getRowKeys"
                 stripe
                 fit
                 border
                 use-virtual
                 :span-method="objectSpanMethod"
                 ref="listTableObject"
                 @current-change="getCurrentChange"
                 @row-click="getRowSelect"
                 @select="getChangeSelect"
                 @select-all="getAllSelect"
                 @selection-change="handleSelectionChange">
                 <slot/>
               </el-table>
               <div class="zsl-table-pagination">
                 <pagination :page="page" @paging="pages" />
               </div>
             </div>
           </template>
        ```
        methods方法：
        ```javascript
           getRowKeys(row) {
             return row[this.id]
           },
           async getList() {
             try {
               this.queryLoading = true
               const params = { ...this.params, ...this.page }
        
               if (typeof this.api !== 'function') {
                 // throw new Error('api应该传入一个方法')
                 this.queryLoading = false;
                 this.list = [{id: 1, name: '我'},{id: 2, name: '我的下属'}];
                 this.page= {pageNum: 1,pageSize: 10,total: 2}
                 this.$emit('getTableData', this.list)
               } else {
                 const res = await this.api(params);
                 let data = res.data.data
                 if (this.listable) {
                   this.list = data.datas;
                   this.page = {
                     pageNum: data.pageNum,
                     pageSize: data.pageSize,
                     total: data.total,
                     pages: data.pages
                   }
                   this.$emit('getTableData', data.datas)
                 } else {
                   this.list = data
                 }
               }
               this.ifChooseData();
             } catch (e) {
               // throw new Error('处理异常')
               throw e
             }
             this.queryLoading = false;
           },
           // 判断是否为选中状态
           ifChooseData(){
             if(this.multi) {
               this.$refs['listTableObject'].clearSelection();
               var tempList = this.tempList || this.tempData;
               for(let j=0; j<tempList.length; j++) {
                 this.$nextTick(() => {
                   this.list.forEach(item => {
                     if(item[this.id] == tempList[j]) {
                       this.$refs['listTableObject'].toggleRowSelection(item,true)
                     } else {
                       // this.$refs['listTableObject'].toggleRowSelection(item,false)
                     }
                   })
                 })
               }
             }
           },
           pages(newPage) {
             this.page = newPage;
             this.getList()
           },
           _resetPage() {
             this.page = defaultPage(this.page)
           },
           reload() {
             this.$nextTick(() => {
               this._resetPage()
               this.getList()
             })
           },
           // 暴露获取选中的方法
           get_listTableObject() {
             console.debug(this.$refs.listTableObject)
           },
           getCurrentChange(val) {
             this.currentRow = val;
             this.$emit('getCurrentChange',val)
           },
           getRowSelect(row,column,event) {
             this.$refs['listTableObject'].toggleRowSelection(row)
             this.$emit('getRowSelect',row,column,event)
           },
           getChangeSelect(selectItem,changItem){
             this.$emit('getChangeSelect',selectItem,changItem)
           },
           handleSelectionChange(val) {
             // this.$refs['listTableObject'].toggleRowSelection(val)
             this.$emit('handleSelectionChange', val)
           },
           getAllSelect(val) {
             this.$emit('getAllSelect', val)
           }
        ```
    
    * 封装多选组件：

        * 调用list-table组件
        * 布局方面，用flex布局，左边70%的table，右边30%的选择列表，将选择的列表存到selectedList对象中，唯一ID存在tempList数组中，tempList用于遍历渲染选择列表框。点击选定退出或取消的时候，把selectedList传回给父组件。
        * methods方法里，将list-table选择返回来的对象，进行判断，如果不存在则push进数组，存在则用splice方法去除项
        * 关键代码如下：

        methods方法： 
       ```javascript
          getTableData(data) {
            this.tableList = data;
          },
          reloadTable() {
            this.$refs.tableData.reload();
          },
          resetQuery() {
            this.param = this.query;
            this.reloadTable();
          },
          handleSelectionChange(row) {
            var temp = [];
            var selectedList = {};
            for(let i=0;i<row.length;i++) {
              let item = row[i]
              let index = item[this.id]
              temp.push(index);
              selectedList[index] = item;
            }
            this.tempList = deepClone(temp)
            this.selectedList = deepClone(selectedList)
          },
          getRowSelect(row, column, event) {
            var temp = this.tempList;
            let index = temp.findIndex(item => item == row[this.id]);
            let i_index = row[this.id]
            if(index>=0) {
              temp.splice(index, 1);
              delete this.selectedList[i_index]
            } else {
              temp.push(i_index);
              this.selectedList[i_index] = row;
            }
            this.tempList = deepClone(temp)
          },
          getChangeSelect(selectItem,changItem){
            var temp = this.tempList;
            let i_index = changItem[this.id]
            if(selectItem.indexOfObj(changItem,this.id) >= 0) {
              let index = temp.findIndex(k => k == i_index)
              if(index>=0) {
    
              }else {
                temp.push(i_index)
                this.selectedList[i_index] = changItem;
              }
            } else {
              let index = temp.findIndex(k => k == i_index)
              if(index >= 0) {
                  temp.splice(index, 1)
                  delete this.selectedList[i_index]
              } else {}
            }
            this.tempList = deepClone(temp)
          },
          getAllSelect(val) {
            var temp = this.tempList;
            if (val.length > 0) {
              for (let i = 0; i < val.length; i++) {
                let item = val[i];
                let i_index = item[this.id];
                if (temp.findIndex(i_item => i_item == i_index) < 0) {
                  temp.push(i_index);
                  this.selectedList[i_index] = item;
                } else {}
              }
            }else {
              for (let i = 0; i < this.tableList.length; i++) {
                let item = this.tableList[i]
                let i_index = item[this.id]
                let index = temp.findIndex(i_item => i_item == i_index);
                if (index >= 0) {
                  temp.splice(index, 1);
                  delete this.selectedList[i_index];
                } else {}
              }
            }
            this.tempList = deepClone(temp)
          },
          handleChoose() {
            this.$emit('handleChoose', this.selectedList)
          },
          cancelChoose() {
            this.tempList = []
            for(let k in this.selectedList) {
              delete this.selectedList[k]
            }
            this.selectedList = {};
            this.$emit('handleChoose', this.selectedList)
          }
          ```    
    * 封装单选组件：
        * 就类似与多选组件，只是不需要定义那么多方法，只需要行点击方法，以及radio点击方法即可，并且不需要多加一个选择列的框框，在template中，把type='checkbox'的el-table-column改成用template自定义成radio，代码跟上一个类似就不贴了。
    * 封装tab切换页，其实谈不上封装了，这个以后有时间再来改改吧。
3. 贴下代码，把代码直接抽出来放文件夹里上传的，不能运行的。
    [components](./components)
    
### vue深度监听的方法
1. 具体例子，代码如下
    ```vue.js
     watch:{
        deep: true,
        'searchMapParam.custMapKeyword':{
          handler(n){
            if(!n) return
            this.getAddrSeletList(n)
          }  
        }
     }
    ```
2. 当监听的数据是对象或者数组时，就无所谓newVal和oldVal,因为两者指向的都是同一个数据。
    
    
### vue 中keep-alive 组件和activated、deactivated 生命周期

1. keep-alive 是个抽象组件，跟template一样，自身不会渲染成DOM元素，它用于将整个组件缓存起来。
    该组件的写法可以直接在template中包在组件的外层，也可以直接在路由中定义meta:{keepAlive: true/false}。
    它的属性有max,include,exclude(这两个可以传入一个字符串也可以是数组，并且是组件定义的name值，定义是否会被缓存的组件)
2. activated和deactivated 生命周期是在设置了keep-alive缓存组件当中，每当切换组件的时候会执行一次：
    第一次进入缓存组件：beforeRouterEnter -> created -> ... -> activated -> deactivated
    后续再进入： beforeRouterEnter -> activated -> deactivated
3. 使用场景，在上面的实现一个带选择框，table表格，单选或多选框的组件中，用于切换tab页的时候缓存数据，具体代码如下：
    ```html
        <el-tabs v-model="active" style="margin-top:15px;" type="card">
            <el-tab-pane v-for="item in tabOptions" :label="item.label" :key="item.key" :name="item.key">
              <keep-alive :exclude="exclude">
                <list v-if="multi && active==item.key && refresh" :if-load="ifLoad" :tab-list="tabList" :query="query" :type="item.key" :selected-data="selectedList" :temp-data="tempList" @handleChoose="submit" />
              </keep-alive>
              <keep-alive :exclude="exclude">
                <single-list v-if="!multi && active==item.key && refresh" :if-load="ifLoad" :tab-list="tabList" :query="query" :type="item.key" :selected-data="selectedList" @handleChoose="submit" />
              </keep-alive>
            </el-tab-pane>
         </el-tabs>    
    ```
    因为带有查询功能，当点击查询的时候，通过v-if和exclude让组件重新渲染，同样也可用include来代替exclude实现。
    ```javascript
     // 查询功能
      onSubmit() {
        this.refresh = false;
        this.exclude = 'ZydSearch,ZydSingleSearch'
        this.$nextTick(() => {
          this.exclude = null;
          this.refresh = true;
        })
      }
    ```
    在 ZydSearch 和 ZydSingleSearch 中，通过activated生命周期函数定义方法，获取数据
    ```javascript
     activated() {
       this.selectedList = {};
       this.tempList = [];
     }
    ```

2020年3月16日
-------------------------
### 前两天看了个实现瀑布流的视频，今天想着实现下，总结如下：（三个方法）
html中，我是这么写的： 
```html
<div class="warp">
    <div class="item"><img src="../../assets/images/0.png" /></div>
    <div class="item"><img src="../../assets/images/1.png" /></div>
    <div class="item"><img src="../../assets/images/2.png" /></div>
    <div class="item"><img src="../../assets/images/3.jpg" /></div>
    <div class="item"><img src="../../assets/images/4.png" /></div>
</div>
```
* 第一个方法：用jquery实现动态的瀑布流布局，在页面获取到图片资源后渲染出效果;
具体思路是：通过获取图片资源，循环数组，数组第一行无需再加定位操作；从第二行开始，图片相对父元素绝对定位，找到目前为止高度最小的一列，将图片插入那一列中，那一列的高度加上图片的高度，再进入下一个循环。
```javascript
  mounted() {
    this.waterFall();
  },
  methods: {
    waterFall() {
      var box = $(".item");
      var boxWidth = $(box).outerWidth() + 5; //outerWidth() jquery方法 返回的width包含padding和border，如果要加上margin，则传'true'参数
      var windowWidth = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth; // 保证兼容性
      // var windowWidth = $(window).width();
      var cols = Math.floor(windowWidth / boxWidth);
      var heightArr = [];
      $.each(box,function(index,item){
        let boxHeight = $(item).outerHeight() + 5;
        if(index < cols) {
          heightArr[index] = boxHeight;
        } else {
          let minboxHeight = Math.min(...heightArr);
          let i = heightArr.indexOf(minboxHeight);
          item.style.position = 'absolute';
          item.style.top = minboxHeight + "px";
          item.style.left = i*boxWidth + 'px';
          // item.style = ({
          //   position: "absolute",
          //   top: minboxHeight + "px",
          //   left: i*boxWidth + 'px'
          // });
          heightArr[i] += boxHeight;
        }
      })
    }
  }
```
* 第二个方法： 用column来布局，在父元素中设置column-count列数
```scss
.warp {
    column-count: 4;
    .item {
      border: solid 1px #f2f2f2;
      margin: 5px;
      img {
        width: 100%;
      }
    }
  }
```
* 第三个方法： 用flex弹性布局，定义flex-flow属性，flex-flow即flex-direction和flex-wrap的复合属性
```scss
.warp {
    display: flex;
    flex-flow: column wrap;
    height: 100vh;
    .item {
      width: calc(100%/4  - 10px);
      margin: 5px;
      img {
        width: 100%;
      }
    }
}
```
* 总结上面三个方法，个人觉得用column布局和flex布局都很方便，用jquery布局的，图片从第二行开始是绝对布局，会脱离文档流，当加载的图片很多的时候，很难控制父元素的height值，无法滚动下拉。

### 封装了下vue 穿梭框组件
具体看github,因为之前公司有一个需求，需要一个穿梭狂，原本想用vue-element-admin里封装好的，发现它这个组件写的实在有限，所以就自己粗略写了一个。

2020年1月6日
------------
### 小程序文档下载预览功能
这阵子主要时间都用在开发小程序上面了,总结下新学到的东西
* 第一个就是看文档的重要性,真的要平时没事就多看看文档,一旦开发上有遇到什么新的需求,特殊的功能,才能更快速的找到解决方法.当然看,是要理解着看的,不是随意看一遍就完事的那种.
* 第二个是在写PDF文档下载功能的时候.后端返回给我的的是二进制流,以前没有做过二进制流文件的下载查看,搞了会时间,总结下怎么完成的
  以前的做法: 
  * 调用wx.downloadFile({})下载文件,在success接口中调用wx.saveFile({})把文件保存在本地,在再success回调中通过返回的文件临时地址,用wx.openDocument({})打开预览.
  ```Javascript 
  wx.downloadFile({
      url: filepath,
      // url:"https://www. .../uploads/ufa/fit/DecOrderVO/3468830325178368.pdf",
      success: function (res) {
  
        //保存文件到本地
        wx.saveFile({
          tempFilePath: res.tempFilePath,
          success(result) {
            var Path = result.savedFilePath
            //返回的文件临时地址，用于后面打开本地预览所用
            wx.openDocument({
              filePath: Path,
              success: function (res) {
                console.log('打开文档成功')
              }
            })
          },
          fail(res) {
            wx.showToast({
              icon: 'none',
              title: '下载失败'
            })
          }
        })
      }
    })
    ```
  * 对于二进制流文件,用以下方法:
  用小程序原生的请求api,wx.request({}),在header中传入token,定义响应内容为二进制流形式responseType:'arraybuffer',通过wx.getFileSystemManager().writeFile({}),将下载回来的data写在一个新建的本地文件中,写完后调用wx.openDocument({})实现预览功能,代码如下:
  ```Javascript
   wx.request({
      url: app.globalData.apiUrl + '/swh/prepareOrder/download/' + id,
      header: {
        wmpToken: wx.getStorageSync("wmpToken")
      },
      responseType:'arraybuffer',
      success: function(res) {
        var data = res.data 
        console.log(res,'111res')
        wx.getFileSystemManager().writeFile({
          filePath: `${wx.env.USER_DATA_PATH}/${purName + id}.pdf`,
          data:data,  
          success: function (rest){  
            console.log(rest, '111rest')
            wx.openDocument({
              filePath: `${wx.env.USER_DATA_PATH}/${purName + id}.pdf`,
              success: function (res) {
                console.log('打开文档成功')
              }
            })  
          },
          complete: function (rest) {
            console.log(rest, 'complete')
          },
        })
      }
    })
  ```
    1. 代码中,```Javasctipt ${wx.env.USER_DATA_PATH} ```是小程序自带的文件系统用户目录路径, 若在小程序开发助手上,**点击详情->文件系统->usr目录**中,若在手机端上,则在tencent\MicroMsg\wxanewfiles\.....; 
    2. 另外,开发中还遇到一个小知识点,是文件下载下来之后打印出来,在开发助手上看是http://..........,用手机调试发现是wxFile://.......,不用太过纠结,这只是两种不同的协议.
* 第三个点是要写一个数据字典选择的组件
* 第四个点是写一个类似于购物车功能的实现,其实没用到什么,它的大致需求是这样的,A页面(主页面),进入B页面(选择物料页面),填写物料信息后,点击保存,这个时候返回A页面,在A页面中最后调用接口保存时也将B页面的物料信息提交上去.在数据未提交给后端的时候,需要把已填写的数据给缓存起来,之后提交成功或者没有提交返回到上一个页面需要把缓存清除,否则会报错.
