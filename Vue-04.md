## 十一、vue-router路由

 Vue Router是Vue.js官方的**路由管理器**（路径跳转）。它和Vue.js的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有:

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于Vue.js过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的CSS class的链接
- HTML5历史模式或hash模式，在IE9中自动降级
- 自定义的滚动条行为

### 1. 安装

 基于第一个vue-cli进行测试学习;先查看node_modules中是否存在 vue-router
 vue-router 是一个插件包，所以我们还是需要用 npm/cnpm 来进行安装的。打开命令行工具，进入你的项目目录，输入下面命令。

```txt
npm install vue-router --save-dev
```

 安装完之后去`node_modules`路径看看是否有vue-router信息 有的话则表明安装成功。

### 2. vue-router demo实例

1. 将之前案例由vue-cli生成的案例用idea打开

2. 清理不用的东西 assert下的logo图片 component定义的helloworld组件 我们用自己定义的组件

3. 清理代码 以下为清理之后的代码 src下的App.vue 和main.js以及根目录的index.html
   这三个文件的关系是 `index.html` 调用`main.js` 调用`App.vue`

   **index.html:**

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <meta charset="utf-8">
       <meta name="viewport" content="width=device-width,initial-scale=1.0">
       <title>myvue</title>
     </head>
     <body>
       <div id="app"></div>
       <!-- built files will be auto injected -->
     </body>
   </html>
   ```

   **main.js:**

   ```js
   import Vue from 'vue'
   import App from './App'
   import router from './router' //自动扫描里面的路由配置
   
   Vue.config.productionTip = false
   
   new Vue({
     el: '#app',
     //配置路由
     router,
     components: { App },
     template: '<App/>'
   })
   ```

   **App.vue:**

   ```vue
   <template>
     <div id="app">
       <img src="./assets/logo.png">
       <h1>迪师傅</h1>
   
       <router-link to="/main">首页</router-link>
       <router-link to="/content">内容页</router-link>
       <router-link to="/kuang">Kuang</router-link>
       <router-view></router-view>
   
     </div>
   </template>
   
   <script>
   
   export default {
     name: 'App',
     components: {
     }
   }
   </script>
   
   <style>
   #app {
     font-family: 'Avenir', Helvetica, Arial, sans-serif;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     text-align: center;
     color: #2c3e50;
     margin-top: 60px;
   }
   </style>
   ```

   1. 在components目录下创建一个自己的组件Content,Test,Main(这两个和Content内容一样的就不放示例代码了

   **Content.vue:**

   ```vue
   <template>
     <h1>内容</h1>
   </template>
   
   <script>
       export default {
           name: "Content"
       }
   </script>
   
   <style scoped>
   
   </style>
   ```

   安装路由,在src目录下,新建一个文件夹 : router,专门存放路由 index.js(默认配置文件都是这个名字)

   ```js
   import Vue from "vue";
   import VueRouter from "vue-router";
   import Content from "../components/Content";
   import Main from "../components/Main";
   import Kuang from "../components/Kuang";
   
   //安装路由
   Vue.use(VueRouter);
   
   //配置导出路由
   export default new VueRouter({
     routes: [
       {
         //路由路径
         path: '/content',
         name: 'content',
         //跳转的组件
         component: Content
       },
       {
         //路由路径
         path: '/main',
         name: 'main',
         //跳转的组件
         component: Main
       },
       {
         //路由路径
         path: '/kuang',
         name: 'kuang',
         //跳转的组件
         component: Kuang
       }
     ]
   })
   ```

   在main.js中配置路由

   **main.js:**

   ```js
   import Vue from 'vue'
   import App from './App'
   import router from './router' //自动扫描里面的路由配置
   
   Vue.config.productionTip = false
   
   new Vue({
     el: '#app',
     //配置路由
     router,
     components: { App },
     template: '<App/>'
   })
   ```

   在App.vue中使用路由

   **App.vue:**

   ```vue
   <template>
     <div id="app">
       <img src="./assets/logo.png">
       <h1>迪师傅</h1>
   
       <router-link to="/main">首页</router-link>
       <router-link to="/content">内容页</router-link>
       <router-link to="/kuang">Kuang</router-link>
       <router-view></router-view>
   
     </div>
   </template>
   
   <script>
   
   export default {
     name: 'App',
     components: {
     }
   }
   </script>
   
   <style>
   #app {
     font-family: 'Avenir', Helvetica, Arial, sans-serif;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     text-align: center;
     color: #2c3e50;
     margin-top: 60px;
   }
   </style>
   ```

   1. 启动测试一下 ： npm run dev



## 十二、vue + ElementUI

根据之前创建vue-cli项目一样再来创建一个新项目

1. 创建一个名为 hello-vue 的工程

```txt
vue init webpack hello-vue
```

安装依赖，我们需要安装 `vue-router`、`element-ui`、`sass-loader` 和`node-sass` 四个插件

```txt
# 进入工程目录
cd hello-vue
# 安装 vue-router
npm install vue-router --save-dev
# 安装 element-ui
npm i element-ui -S
# 安装依赖
npm install
# 安装 SASS 加载器
cnpm install sass-loader node-sass --save-dev
# 启动测试
npm run dev	
```

1. **Npm命令解释**
   - `npm install moduleName`：安装模块到项目目录下
   - `npm install -g moduleName`：-g 的意思是将模块安装到全局，具体安装到磁盘的哪个位置，要看 npm config prefix的位置
   - `npm install moduleName -save`：–save的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖，-S为该命令的缩写
   - `npm install moduleName -save-dev`：–save-dev的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖，-D为该命令的缩写
2. 创建成功后用idea打开，并删除净东西 创建views和router文件夹用来存放视图和路由

在views创建Main.vue

**Main.vue：**

```vue
<template>
  <h1>首页</h1>
</template>
<script>
    export default {
        name: "Main"
    }
</script>
<style scoped>
</style>
```

1. 在views中创建Login.vue视图组件

**Login.vue:**（用的ElementUI中的代码）

```vue
<template>
  <div>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎登录</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onSubmit('loginForm')">登录</el-button>
      </el-form-item>
    </el-form>

    <el-dialog
      title="温馨提示"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">
      <span>请输入账号和密码</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: "Login",
    data() {
      return {
        form: {
          username: '',
          password: ''
        },

        // 表单验证，需要在 el-form-item 元素中增加 prop 属性
        rules: {
          username: [
            {required: true, message: '账号不可为空', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '密码不可为空', trigger: 'blur'}
          ]
        },

        // 对话框显示和隐藏
        dialogVisible: false
      }
    },
    methods: {
      onSubmit(formName) {
        // 为表单绑定验证功能
        this.$refs[formName].validate((valid) => {
          if (valid) {
            // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
            this.$router.push("/main");
          } else {
            this.dialogVisible = true;
            return false;
          }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  .login-box {
    border: 1px solid #DCDFE6;
    width: 350px;
    margin: 180px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }

  .login-title {
    text-align: center;
    margin: 0 auto 40px auto;
    color: #303133;
  }
</style>
```

创建路由

在 router 目录下创建一个名为 index.js 的 vue-router 路由配置文件

**index.js：**

```js
import Vue from "vue";
import Router from "vue-router";
import Main from "../views/Main";
import Login from "../views/Login";

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/main',
      component: Main
    },
    {
      path: '/login',
      component: Login
    }
  ]
});
```

在main.js中配置相关

main.js是index.html调用的 所以前面注册的组件要在这里导入

**一定不要忘记扫描路由配置并将其用到new Vue中**

**main.js:**

```js
import Vue from 'vue'
import App from './App'
//扫描路由配置
import router from './router'
//导入elementUI
import ElementUI from "element-ui"
//导入element css
import 'element-ui/lib/theme-chalk/index.css'

Vue.use(router);
Vue.use(ElementUI)

new Vue({
  el: '#app',
  router,
  render: h => h(App),//ElementUI规定这样使用
})
```

在App.vue中配置显示视图

**App.vue :**

```vue
<template>
  <div id="app">
    <router-link to="/login">login</router-link>
    <router-view></router-view>
  </div>
</template>
<script>
export default {
  name: 'App',
}
</script>
```

测试运行

## 十三、路由嵌套

 嵌套路由又称子路由，在实际应用中，通常由多层嵌套的组件组合而成。

### Demo

1. 创建用户信息组件，在 views/user 目录下创建一个名为 Profile.vue 的视图组件；

   **Profile.vue**

   ```vue
   <template>
     <h1>个人信息</h1>
   </template>
   <script>
     export default {
       name: "UserProfile"
     }
   </script>
   <style scoped>
   </style>
   ```

   在用户列表组件在 views/user 目录下创建一个名为 List.vue 的视图组件；

   **List.vue**

   ```vue
   <template>
     <h1>用户列表</h1>
   </template>
   <script>
     export default {
       name: "UserList"
     }
   </script>
   <style scoped>
   </style>
   ```

   1. 修改首页视图，我们修改 Main.vue 视图组件，此处使用了 ElementUI 布局容器组件，代码如下：

   **Main.vue**

   ```vue
   <template>
     <div>
       <el-container>
         <el-aside width="200px">
           <el-menu :default-openeds="['1']">
             <el-submenu index="1">
               <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
               <el-menu-item-group>
                 <el-menu-item index="1-1">
                   <!--插入的地方-->
                   <router-link to="/user/profile">个人信息</router-link>
                 </el-menu-item>
                 <el-menu-item index="1-2">
                   <!--插入的地方-->
                   <router-link to="/user/list">用户列表</router-link>
                 </el-menu-item>
               </el-menu-item-group>
             </el-submenu>
             <el-submenu index="2">
               <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
               <el-menu-item-group>
                 <el-menu-item index="2-1">分类管理</el-menu-item>
                 <el-menu-item index="2-2">内容列表</el-menu-item>
               </el-menu-item-group>
             </el-submenu>
           </el-menu>
         </el-aside>
   
         <el-container>
           <el-header style="text-align: right; font-size: 12px">
             <el-dropdown>
               <i class="el-icon-setting" style="margin-right: 15px"></i>
               <el-dropdown-menu slot="dropdown">
                 <el-dropdown-item>个人信息</el-dropdown-item>
                 <el-dropdown-item>退出登录</el-dropdown-item>
               </el-dropdown-menu>
             </el-dropdown>
           </el-header>
           <el-main>
             <!--在这里展示视图-->
             <router-view />
           </el-main>
         </el-container>
       </el-container>
     </div>
   </template>
   <script>
     export default {
       name: "Main"
     }
   </script>
   <style scoped lang="scss">
     .el-header {
       background-color: #B3C0D1;
       color: #333;
       line-height: 60px;
     }
     .el-aside {
       color: #333;
     }
   </style>
   ```

   配置嵌套路由修改 router 目录下的 index.js 路由配置文件，使用children放入main中写入子模块，代码如下

   **index.js**

   ```js
   import Vue from "vue";
   import Router from "vue-router";
   import Main from "../views/Main";
   import Login from "../views/Login";
   import UserList from "../views/user/List";
   import UserProfile from "../views/user/Profile";
   
   Vue.use(Router);
   
   export default new Router({
     routes: [
       {
         path: '/main',
         component: Main,
         //路由嵌套
         children: [
           {path: '/user/profile',component: UserProfile},
           {path: '/user/list',component: UserList}
         ]
       },
       {
         path: '/login',
         component: Login
       }
     ]
   });
   ```

   ## 十四、参数传递

   ### 1. Demo

   1. 前端传递参数

       此时我们在Main.vue中的route-link位置处 to 改为了 :to，是为了将这一属性当成对象使用，注意 router-link 中的 name 属性名称 一定要和 路由中的 name 属性名称 匹配，因为这样 Vue 才能找到对应的路由路径；

      ```txt
      <!--name：传组件名 params：传递参数，需要绑定对象：v-bind-->
      <router-link v-bind:to="{name: 'UserProfile', params: {id: 1}}">个人信息</router-link>
      ```

      修改路由配置，增加props：true属性

       主要是router下的index.js中的 path 属性中增加了 :id 这样的占位符

      ```vue
      {
        path: '/user/profile/:id',
        name: 'UserProfile',
        component: UserProfile,
        props:true
      }
      ```

      前端显示

      在要展示的组件Profile.vue中接收参数

      **Profile.vue：**

      ```vue
      <template>
        <div>
          个人信息
          {{ id }}
        </div>
      </template>
      <script>
          export default {
            props: ['id'],
            name: "UserProfile"
          }
      </script>
      <style scoped>
      </style>
      ```

      ### 组件重定向

       重定向的意思大家都明白，但 Vue 中的重定向是作用在路径不同但组件相同的情况下，比如：
      ​ 在router下面index.js的配置

      ```js
      {
        path: '/main',
        name: 'Main',
        component: Main
      },
      {
        path: '/goHome',
        redirect: '/main'
      }
      ```

      ## 十五、路由钩子与异步请求

      ### 1. 路由模式与 404

       **路由模式有两种**

      - hash：路径带 # 符号，如 http://localhost/#/login

      - history：路径不带 # 符号，如 http://localhost/login

        修改路由配置，代码如下：

```vue
export default new Router({
  mode: 'history',
  routes: [
  ]
});
```

**404界面：**

1. 创建一个NotFound.vue视图组件

```vue
<template>
    <div>
      <h1>404,你的页面走丢了</h1>
    </div>
</template>
<script>
    export default {
        name: "NotFound"
    }
</script>
<style scoped>
</style>
```

```js
import NotFound from '../views/NotFound'
{
   path: '*',
   component: NotFound
}
```

### 2. 路由钩子与异步请求

`beforeRouteEnter`：在进入路由前执行
`beforeRouteLeave`：在离开路由前执行

在Profile.vue中写:

```vue
 export default {
    name: "UserProfile",
    beforeRouteEnter: (to, from, next) => {
      console.log("准备进入个人信息页");
      next();
    },
    beforeRouteLeave: (to, from, next) => {
      console.log("准备离开个人信息页");
      next();
    }
  }
```

**参数说明:**

- to：路由将要跳转的路径信息
- from：路径跳转前的路径信息
- next：路由的控制参数
- next() 跳入下一个页面
- next(’/path’) 改变路由的跳转方向，使其跳到另一个路由
- next(false) 返回原来的页面
- next((vm)=>{}) 仅在 beforeRouteEnter 中可用，vm 是组件实例

### 3. 在钩子函数中使用异步请求

1. 安装 Axios

2. main.js引用 Axios

3. 准备数据 ： 只有**我们的 static 目录下的文件是可以被访问到的**，所以我们就把静态文件放入该目录下。
   数据和之前用的json数据一样 需要的去上述axios例子里

4. 在 beforeRouteEnter 中进行异步请求

   Profile.vue:

```vue
 export default {
    //第二种取值方式
    // props:['id'],
    name: "UserProfile",
    //钩子函数 过滤器
    beforeRouteEnter: (to, from, next) => {
      //加载数据
      console.log("进入路由之前")
      next(vm => {
        //进入路由之前执行getData方法
        vm.getData()
      });
    },
    beforeRouteLeave: (to, from, next) => {
      console.log("离开路由之前")
      next();
    },
    //axios
    methods: {
      getData: function () {
        this.axios({
          method: 'get',
          url: 'http://localhost:8080/static/mock/data.json'
        }).then(function (response) {
          console.log(response)
        })
      }
    }
  }
```

