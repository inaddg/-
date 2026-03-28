[TOC]



# 医诊陪护项目

# [admin端]

# 前置准备

## 1.环境配置和项目搭建

### 环境配置

1.node.js在18版本以上

2.node版本管理工具nvm：https://nvm.uihtm.com/  （当项目需要频繁切换node版本的时候）

注意：下载新对应版本之前需要把已经下了的老版本删掉

​	2.1下载版本： `nvm install 24.12.0`

​	2.2切换到指定版本：`nvm use 24.12.0`

​	2.3查看当前node版本：`node -v`

​	2.4查看下过的所有node 版本：`nvm list`

3.安装 pnpm : `npm install pnpm -g`

![img](file:///C:/Users/Amelia/Desktop/%E5%89%8D%E7%AB%AF%E7%BB%83%E4%B9%A0/%E5%89%8D%E7%AB%AF%E5%AD%A6%E4%B9%A0/vue2+3/%E7%AC%94%E8%AE%B0/11-day12-day14-%E5%A4%A7%E4%BA%8B%E4%BB%B6%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F/11-day12-day14-%E5%A4%A7%E4%BA%8B%E4%BB%B6%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F/assets/image-20230710172832242.png?lastModify=1768643812)

### 项目搭建

1.Vite + Vue：`pnpm create vite`

![76671399523](C:\Users\Amelia\AppData\Local\Temp\1766713995237.png)

项目启动也可以用：`pnpm create vue` 更方便 (我用的vue，更加方便)

## 2.Router路由配置引入

**如果用的是 vite 创建项目：需要手动引入路由**

1.安装 vue-router : `npm install vue-router@4`

2.创建路由和对应页面：src下创建新文件夹router,在其下新建文件 index.js:

//这里路由用的是hash模式，不是history模式！！

```javascript
import { createRouter, createWebHashHistory } from 'vue-router'

import Layout from '@/views/indexMain.vue'
import Login from '@/views/login/indexLogin.vue'

const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      component: Layout ,
    },
    {
      path: '/login',
      component: Login ,
    }
  ],
})

export default router
```

2.1新建页面目录文件夹 views ，在其下新建文件 indexMain.vue ：

```vue
<template>
  <div>
    layout
  </div>
</template>

<script setup></script>
```

2.2在 views 下新建登录页面目录文件夹 login ，在其下新建文件 indexLogin.vue ：内容同上

3.在main.js 引入 router：

```javascript
import router from './router'
app.use(router)
```

4.对应页面使用<RouterView />配置路由显示：

```vue
<template>
  <router-view></router-view>
</template>
```

## 3.elementPlus引入和按需加载

### UI框架

Element Plus： https://element-plus.org/zh-CN

安装步骤

- 安装包管理器 ：pnpm install element-plus

- 按需引入

  - 执行命令-安装两款插件

    pnpm install -D unplugin-vue-components unplugin-auto-import

- vite.config.js 配置修改，**注意：修改配置后需要重启项目**

```js
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueDevTools from 'vite-plugin-vue-devtools'

import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

// https://vite.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    vueDevTools(),

      AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    },
  },
})
```

# 左侧Aside菜单页面+顶部导航布局

## 4.layout布局和菜单aside组件创建

//如果用 vite 创建项目，此时需要把 style.css 里的内容清空

1.element里的 cotainer组件 布局`src/views/indexMain.vue`：

```vue
<template>
  <div class="common-layout">
    <el-container>
<!-- <el-aside width="200px">Aside</el-aside> -->
      <Aside></Aside>
      <el-container>
        <el-header>Header</el-header>
        <el-main>Main</el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script setup>
  import Aside from '@/components/indexAside.vue'
</script>
```

2.src/style.css（没有就新建）作用：统一浏览器默认样式、消除布局差异：

```css
html, body, div, span, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
abbr, address, cite, code,
del, dfn, em, img, ins, kbd, q, samp,
small, strong, sub, sup, var,
b, i,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, figcaption, figure,
footer, header, hgroup, menu, nav, section, summary,
time, mark, audio, video {
  margin: 0;
  padding: 0;
  border: 0;
  outline: 0;
  /* font-size:100%; */
  vertical-align: baseline;
  background: transparent;
}

a {
  text-decoration: none;
}

li {
  list-style: none;
}

body {
  height: 100vh;
  background-color: #f1f4f6;
}

#app {
  height: 100%;
}

.el-header {
  height: 50px !important;
  padding: 0 !important;
  box-shadow: 0px 1px 1px rgba(0,0,0,0.05);
  border-bottom: 1px solid transparent;
}

.pagination-info {
  padding: 10px 0;
  background-color: #fff;
  .el-pagination {
    justify-content: end;
  }
}
```

### 菜单aside组件创建

3.在 src 下新建组件库文件夹 components ，在其下新建组件 indexAside.vue（左侧菜单部分）：

```vue
<template>
  <el-menu
        active-text-color="#ffd04b"
        background-color="#545c64"
        class="aside-container"
        default-active="2"
        text-color="#fff"
        @open="handleOpen"
        @close="handleClose"
      >
        <el-sub-menu index="1">
          <template #title>
            <el-icon><location /></el-icon>
            <span>Navigator One</span>
          </template>
          <el-menu-item-group title="Group One">
            <el-menu-item index="1-1">item one</el-menu-item>
            <el-menu-item index="1-2">item two</el-menu-item>
          </el-menu-item-group>
          <el-menu-item-group title="Group Two">
            <el-menu-item index="1-3">item three</el-menu-item>
          </el-menu-item-group>
          <el-sub-menu index="1-4">
            <template #title>item four</template>
            <el-menu-item index="1-4-1">item one</el-menu-item>
          </el-sub-menu>
        </el-sub-menu>
        <el-menu-item index="2">
          <el-icon><icon-menu /></el-icon>
          <span>Navigator Two</span>
        </el-menu-item>
        <el-menu-item index="3" disabled>
          <el-icon><document /></el-icon>
          <span>Navigator Three</span>
        </el-menu-item>
        <el-menu-item index="4">
          <el-icon><setting /></el-icon>
          <span>Navigator Four</span>
        </el-menu-item>
      </el-menu>
</template>

<script setup>
const handleOpen = () => {

}
const handleClose = () => {

}
</script>

<style lang="scss" scoped>
</style>
```

## 5.aside样式问题和treeMenu组件拆分

### 5.1 样式

1.安装 less ：pnpm add less

src/components/indexAside.vue：

```vue
<template>
  <el-menu
    active-text-color="#ffd04b"
    background-color="#545c64"
    class="aside-container"
    default-active="2"
    text-color="#fff"
    @open="handleOpen"
    @close="handleClose"
      >
      <p class="logo-lg">DIDI陪诊</p>
      <TreeMenu />
  </el-menu>
</template>

<script setup>
import TreeMenu from '@/components/treeMenu.vue';

const handleOpen = () => {}
const handleClose = () => {}
</script>

<style lang="less" scoped>
.aside-container {
  height: 100%;
  .logo-lg {
    text-align: center;
    height: 50px;
    color: #fff;
    font-size: 20px;
    line-height: 50px;
  }
}
</style>
```

src/views/indexMain.vue

```vue
<template>
  <div class="common-layout">
    <el-container>
<!-- <el-aside width="200px">Aside</el-aside> -->
      <Aside />
      <el-container>
        <el-header>Header</el-header>
        <el-main>Main</el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script setup>
  import Aside from '@/components/indexAside.vue'
</script>
<style lang="less" scoped>
.common-layout {
  height: 100vh;
  .el-container {
    height: 100%;
  }
}
</style>

```

### 5.2 treeMenu组件拆分（左侧菜单的下部分）

新建：src/components/treeMenu.vue：

```vue
<template>
  <el-sub-menu index="1">
    <template #title>
      <el-icon><location /></el-icon>
      <span>Navigator One</span>
    </template>
    <el-menu-item-group title="Group One">
      <el-menu-item index="1-1">item one</el-menu-item>
      <el-menu-item index="1-2">item two</el-menu-item>
    </el-menu-item-group>
    <el-menu-item-group title="Group Two">
      <el-menu-item index="1-3">item three</el-menu-item>
    </el-menu-item-group>
    <el-sub-menu index="1-4">
      <template #title>item four</template>
      <el-menu-item index="1-4-1">item one</el-menu-item>
    </el-sub-menu>
  </el-sub-menu>
  <el-menu-item index="2">
    <el-icon><icon-menu /></el-icon>
    <span>Navigator Two</span>
  </el-menu-item>
  <el-menu-item index="3" disabled>
    <el-icon><document /></el-icon>
    <span>Navigator Three</span>
  </el-menu-item>
  <el-menu-item index="4">
    <el-icon><setting /></el-icon>
    <span>Navigator Four</span>
  </el-menu-item>
</template>

<script setup>
</script>

<style lang="scss" scoped>
</style>
```

## 6.treeMenu组件递归实现

1.实现路由 src/router/index,js ：

```javascript
import { createRouter, createWebHashHistory } from 'vue-router'

import Layout from '@/views/indexMain.vue'
import Login from '@/views/login/indexLogin.vue'
import Admin from '@/views/auth/admin/indexAdmin.vue'
import Group from '@/views/auth/group/indexGroup.vue'
import Staff from '@/views/vppz/staff/indexStaff.vue'
import Order from '@/views/vppz/order/indexOrder.vue'
import Dashboard from '@/views/dashboard/indexDashboard.vue'

const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes:  [
  {
    path: '/',
    component: Layout,
    name: 'main',
    children: [
      {
        path: 'dashboard',
        meta: { id: '1', name: '控制台', icon: 'Platform', path: '/dashboard', describe: '用于展示当前系统中的统计数据、统计报表及重要实时数据' },
        component: Dashboard
      },
      {
        path: 'auth',
        meta: { id: '2' ,name: '权限管理', icon: 'Grid' },
        children: [
          {
            path: '',
            alias: ['admin'],
            meta: { id: '1', name: '账号管理', icon: 'Avatar', path: '/auth/admin', describe: '管理员可以进行编辑，权限修改后需要登出才会生效' },
            component: Admin
          },
          {
            path: 'group',
            meta: { id: '2', name: '菜单管理', icon: 'Menu', path: '/auth/group', describe: '菜单规则通常对应一个控制器的方法,同时菜单栏数据也从规则中获取' },
            component: Group
          }
        ]
      },
      {
        path: 'vppz',
        meta: { id: '3', name: 'DIDI陪诊', icon: 'BellFilled' },
        children: [
          {
            path: '',
            alias: ['staff'],
            meta: { id: '1', name: '陪护管理', icon: 'Checked', path: '/vppz/staff', describe: '陪护师可以进行创建和修改，设置对应生效状态控制C端选择' },
            component: Staff
          },
          {
            path: 'order',
            meta: { id: '2', name: '订单管理', icon: 'List', path: '/vppz/order', describe: 'C端下单后可以查看所有订单状态，已支付的订单可以完成陪护状态修改' },
            component: Order
          }
        ]
      }
    ]
  },
  {
    path: '/login',
    component: Login
  },
],
})

export default router
```

2.根据路由，注册子页面，并引入（层级结构要和路径匹配）：

- 在 src/views 下新建子文件夹：auth、dashboard、vppz

- auth下新建文件夹admin和group、vppz下新建文件夹order、satff

- 在上述子文件夹下新建文件：index...vue（里面写好初始化结构）

- 在 index.js 中引入上述文件（已补充写在上面代码中）

  ![76682056183](C:\Users\Amelia\AppData\Local\Temp\1766820561833.png)

3.获取路由数据 src/components/indexAside.vue：

```vue
<template>
  <el-menu
    active-text-color="#ffd04b"
    background-color="#545c64"
    class="aside-container"
    default-active="2"
    text-color="#fff"
    @open="handleOpen"
    @close="handleClose"
      >
      <p class="logo-lg">DIDI陪诊</p>

      <!-- 通过父子组件通信把menuData数据传到子组件 -->
      <TreeMenu :index="1" :menuData="menuData" />
  </el-menu>
</template>

<script setup>
import TreeMenu from '@/components/treeMenu.vue';
// 通过vue-router获取当前路由数据
import { useRouter } from 'vue-router'
import { ref } from 'vue'

const router = useRouter() //调用，获取当前路由实例
const menuData = ref(router.options.routes[0].children) //获取当前路由数据
// console.log(router);


const handleOpen = () => { }
const handleClose = () => {}
</script>

<style lang="less" scoped>
... 
</style>

```

补充父子组件通信 indexAside（父组件）treeMenu（子组件）：

1. 父组件

- 拿到数据

  ![76682196804](C:\Users\Amelia\AppData\Local\Temp\1766821968046.png)


- 动态传递

  ![76682202966](C:\Users\Amelia\AppData\Local\Temp\1766822029661.png)

2. 子组件
   - 子组件利用 defineProps 获取数据![76682433784](C:\Users\Amelia\AppData\Local\Temp\1766824337846.png)

4.递归组件：src/components/treeMenu.vue：

```vue
<template>

  <template v-for="(item, index) in props.menuData">
        <!-- 遍历数据 -->
        <!-- 1.如果没有子菜单（也就是数据中没有children） -->
    <el-menu-item
      v-if="!item.children || item.children.length == 0"
      :index="`${props.index}-${item.meta.id}`"
      :key="`${props.index}-${item.meta.id}`"
      >
      <!-- <el-icon><setting /></el-icon>
      <span>Navigator Four</span> -->
      <el-icon size="20">
        <!-- 动态组件 -->
         <component :is="item.meta.icon" />
      </el-icon>
      <span>{{ item.meta.name }}</span>
    </el-menu-item>

        <!-- 2.有子菜单 -->
    <el-sub-menu
      v-else
      :index="`${props.index}-${item.meta.id}`"
      :key="index + 1"
      >
      <template #title>
        <el-icon>
          <component :is="item.meta.icon" />
        </el-icon>
        <span>{{ item.meta.name }}</span>
      </template>

      <!-- 有子菜单下面的 需要渲染的内容  -->
       <!-- 递归组件：保证当前组件名称==下面所要写的组件 -->
      <tree-menu
        :menuData="item.children"
        :index="`${props.index}-${item.meta.id}`"
       />
    </el-sub-menu>
  </template>

</template>

<script setup>
// 子组件获取数据
const props = defineProps(['menuData', 'index'])
console.log(props);

</script>

<style lang="scss" scoped>
</style>
```

补充：

- **递归组件的实现**：组件自身调用自身，是实现树形菜单的核心

  ![76682760918](C:\Users\Amelia\AppData\Local\Temp\1766827609182.png)

- **动态组件 <component :is="item.meta.icon" />**：需提前全局注册 Element Plus 图标（需在入口文件`main.js`注册），否则会失效（在后面）。

- **index 唯一性**：`el-menu` 要求所有菜单项的 `index` 必须唯一（否则激活 / 展开状态异常），因此通过 `props.index`（父索引） + `item.meta.id`（当前节点 ID）拼接（如 `1-101`、`1-101-102`），保证每一层菜单的 `index` 不重复

  ## 7.菜单点击跳转和样式问题

  1.添加点击事件 src/components/treeMenu.vue：

  ```vue
  <template>
    <template v-for="(item, index) in props.menuData">
  	  <el-menu-item
        @click="handleClick(item, `${props.index}-${item.meta.id}`)"
        v-if="!item.children || item.children.length == 0"
        :index="`${props.index}-${item.meta.id}`"
        :key="`${props.index}-${item.meta.id}`"
        >
            ...
        </el-menu-item>
        ...
  </template>    
  <script setup>
  	import {useRouter} from 'vue-router'
  ...
  // 点击菜单事件( active索引值 )
  	const router = useRouter() // 创建router路由实例
  	const handleClick = (item, active) => {
    // console.log(item,'item');
    	router.push(item.meta.path)
  }
  </script>
  ...
  ```

  2.页面main部分引入路由 src/views/indexMain.vue：

  ```vue
  <template>
  ...
  	<el-main>
         <router-view />
      </el-main>
  ...
  </template>
  ...
  ```

  ### 样式-菜单栏图标显示

  1.之前是通过动态组件加载  src/components/treeMenu.vue：

     需全局注册安装

  - pnpm install @element-plus/icons-vue

  - 导入注册：main.js：

    ```javascript
    // main.ts

    // 如果您正在使用CDN引入，请删除下面一行。
    import * as ElementPlusIconsVue from '@element-plus/icons-vue'

    const app = createApp(App)
    for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
      app.component(key, component)
    }
    ```

    ​

  2. 给菜单增加宽度（内部）：src/components/indexAside.vue：

  ```vue
  <template>
    <el-menu
      :style="{ width: '230px'}"
       ...
        >
        ...
    </el-menu>
  </template>
  ...
  ```

  # 顶部Header组件页面布局

## 8.header组件创建和样式编写

1.Main中封装组件 src/views/indexMain.vue：

```vue
<template>
  ...
      <el-container>
        <!-- <el-header>Header</el-header> -->
        <Header />
        ...
      </el-container>
...
</template>
<script setup>
...
  import Header from '@/components/navHeader.vue'
</script>
<!-- main中样式我自己稍微修改了一下 -->
<style lang="less" scoped>
.common-layout {
  background-color: #f5f7fa;
  font-family: soft;
  height: 100vh;
  .el-container {
    height: 100%;
    .el-header {
      padding: 0;
    }
  }
}
</style>
```

2.components中新建文件 navHeader.vue：

```vue
<template>
  <div class="header-container">
    <div class="header-left flex-box" >
      <el-icon class="icon" size="23"><Fold /></el-icon>
    </div>
    <div class="header-right">
      <el-dropdown>
        <div class="el-dropdown-link flex-box">
          <el-avatar
            src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
          />
          <p class="user-name">admin</p>
        </div>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item>Action 1</el-dropdown-item>
            <el-dropdown-item>Action 2</el-dropdown-item>
            <el-dropdown-item>Action 3</el-dropdown-item>
            <el-dropdown-item disabled>Action 4</el-dropdown-item>
            <el-dropdown-item divided>Action 5</el-dropdown-item>
          </el-dropdown-menu>
        </template>
    </el-dropdown>
    </div>
  </div>
</template>
<script setup></script>
<style lang="less" scoped>
  .flex-box{
    display: flex;
    align-items: center;
  }
.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 100%;
  background-color: #fff;
  padding-right: 25px;
  // 添加阴影
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
.header-left {
  height: 100%;
  .icon{
      width: 45px;
      height: 100%;
    }
    .icon:hover{
      background-color: #f5f5f5;
      cursor: pointer;;
    }
  }
  .header-right {
    .user-name {
      margin-left: 10px;
    }
  }
}
</style>

```

## 9.VUEX引入和菜单展开收起功能实现( 我改用**pinia**)

1.状态管理工具 vuex ：https://v3.vuex.vuejs.org/zh/

切换到v4.x 安装命令： pnpm install vuex@next --save

2. 我这里的状态管理工具想用 **pinia** ：https://pinia.vuejs.org/zh/getting-started.html

- 插件：pnpm install pinia-plugin-persistedstate --save


- 安装：pnpm i pinia

3. 老师用vuex是：

   3.1. 在 src 下新建文件夹 store ，其下新建 index.js 文件

   3.2 .在store下新建文件 menu.js 

   3.3 在main.js挂载

   3.4 在 navHeader.vue 中调用方法 并 在Aside组件里实现相关样式判断

4. 我用的是pinia：

   4.1 main.js：

   ```javascript
   //原：import store from './store'
   import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
   const pinia = createPinia()
   pinia.use(piniaPluginPersistedstate)
   // 3. 挂载Pinia（替换原app.use(store)）
   app.use(pinia)
   ```

   4.2 stores/menuStore.js：

   ```javascript
   import { defineStore } from 'pinia'

   // 定义并导出Menu Store（Pinia的核心是defineStore，第一个参数为唯一ID）
   export const useMenuStore = defineStore('menu', {
     // 状态（替代Vuex的state，返回函数避免单例问题）
     state: () => ({
       isCollapse: false,
       selectMenu: [],
     }),
        // 方法（替代Vuex的mutations，支持同步/异步，可直接修改state）
     actions: {
       // 折叠菜单（对应原mutations的collapseMenu）
       collapseMenu() {
         this.isCollapse = !this.isCollapse
       },
     }
   })
   ```

   4.3 src/components/navHeader.vue

   ```vue
   <template>
     <div class="header-container">
       <div class="header-left flex-box">
         <!-- 调用Pinia的collapseMenu方法 -->
         <el-icon class="icon" size="23" @click="handleCollapse"><Fold /></el-icon>
       </div>
         ...
     </div>
   </template>

   <script setup>
   // 移除Vuex的useStore
   // import { useStore } from 'vuex'
   // const store = useStore()

   // 引入Pinia的menuStore
   import { useMenuStore } from '@/stores/menuStore'
   // 获取store实例（Pinia实例是响应式的，无需手动解构）
   const menuStore = useMenuStore()

   // 折叠菜单方法（替代原store.commit）
   const handleCollapse = () => {
     menuStore.collapseMenu()
   }

   // 【可选】访问状态示例（替代this.$store.state.menu.isCollapse）
   // console.log('菜单折叠状态：', menuStore.isCollapse)
   </script>
   ```

   4.4 改变展开和收起 src/components/indexAside.vue：

   ```vue
   <template>
     <el-menu
       :style="{ width: !isCollapse ? '230px' : '64px' }"
       active-text-color="#ffd04b"
       background-color="#545c64"
       class="aside-container"
       default-active="2"
       text-color="#fff"
       :collapse="isCollapse"
       @open="handleOpen"
       @close="handleClose"
         >
         <p class="logo-lg">{{ isCollapse ? "DIDI" : "DIDI陪诊" }}</p>

         <!-- 通过父子组件通信把menuData数据传到子组件 -->
         <TreeMenu :index="1" :menuData="menuData" />
     </el-menu>
   </template>

   <script setup>
   import TreeMenu from '@/components/treeMenu.vue';
   // 通过vue-router获取当前路由数据
   import { useRouter } from 'vue-router'
   import { ref } from 'vue'

   import { computed } from "vue";
   import { useMenuStore } from '@/stores/menuStore'
   const menuStore = useMenuStore()
   // 替换折叠状态访问：store.state.menu.isCollapse → menuStore.isCollapse
   const isCollapse = computed(() => menuStore.isCollapse);

   const router = useRouter() //调用，获取当前路由实例
   const menuData = ref(router.options.routes[0].children) //获取当前路由数据
   // console.log(router);
     const handleOpen = () => { }
      const handleClose = () => {}
   </script>
   ```


   补充逻辑-展开收起功能：

   1、menuStore.js中写方法和数据

   2、navHeader组件引入stores仓库，获取menuStore中的collapseMenu方法，并注册点击事件

   3、在Aside中

   ​	3.1 同上，引入并获取isCollapse数据以及计算属性

   ​	3.2 el-menu组件上加入动态属性 :collapse

   ​	3.3 给盒子宽度加上判断条件

   ​	3.4 完善功能，给logo文字加上判断条件	

## 10.tag样式编写和高亮效果实现

1.src/components/treeMenu.vue：

   ```vue
...
<script setup>
import { useRouter } from 'vue-router'
// 1. 移除Vuex的useStore引入
import { useMenuStore } from '@/stores/menuStore'

// 2. 替换Vuex store实例为Pinia的menuStore实例
// const store = useStore()
const menuStore = useMenuStore()
// 子组件获取数据
const props = defineProps(['menuData', 'index'])
// console.log(props);

// 点击菜单事件( active索引值 )
const router = useRouter() // 创建router路由实例
const handleClick = (item, active) => {
  // console.log(item,'item');
  
 // 3. 替换Vuex的commit调用为Pinia的actions方法
  // store.commit('addMenu', item.meta)
  menuStore.addMenu(item.meta)

  router.push(item.meta.path)
}
</script>
   ```

2.src/stores/menuStore.js：

```javascript
 actions: {
     // 添加菜单（对应原mutations的addMenu）
    addMenu(payload) {
      if (this.selectMenu.findIndex(item => item.path === payload.path) === -1) {
        this.selectMenu.push(payload)
      }
    },
 }
```

3.做显示 src/components/navHeader.vue：

```vue
<template>
  <div class="header-container">
    <div class="header-left flex-box" >
      <el-icon class="icon" size="23" @click="handleCollapse"><Fold /></el-icon>
      <ul class="flex-box">
        <li
          v-for="(item,index) in selectMenu"
          :key="item.path"
          :class="{ selected : route.path == item.path}"
          class="tab flex-box">
          <el-icon size="16"><component :is="item.icon" /></el-icon>
          <router-link class=" text flex-box" :to="{ path: item.path }">
            {{ item.name }}
          </router-link>

          <el-icon size="16" class="close"><Close /></el-icon>
        </li>
      </ul>
    </div>
    <div class="header-right">
      <el-dropdown>
        <div class="el-dropdown-link flex-box">
          <el-avatar
            src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
          />
          <p class="user-name">admin</p>
        </div>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item>Action 1</el-dropdown-item>
            <el-dropdown-item>Action 2</el-dropdown-item>
            <el-dropdown-item>Action 3</el-dropdown-item>
            <el-dropdown-item disabled>Action 4</el-dropdown-item>
            <el-dropdown-item divided>Action 5</el-dropdown-item>
          </el-dropdown-menu>
        </template>
    </el-dropdown>
    </div>
  </div>
</template>

<script setup>
  //原：import { useStore } from 'vuex'
import { useMenuStore } from '@/stores/menuStore'
import { computed } from "vue";
import { useRoute } from 'vue-router' //当前路由对象

const route = useRoute() // 获取当前路由对象

// 获取store实例,代替：const store = useStore()
const menuStore = useMenuStore()
 // 调用折叠菜单方法（替代this.$store.commit('collapseMenu')）
const handleCollapse = () => {
  menuStore.collapseMenu()
}

// 3. 替换状态访问：store.state.menu.selectMenu → menuStore.selectMenu
const selectMenu = computed(() => menuStore.selectMenu);


// 访问状态（替代this.$store.state.menu.isCollapse）
console.log('菜单折叠状态：', menuStore.isCollapse)
</script>

<style lang="less" scoped>
  .flex-box{
    display: flex;
    align-items: center;
    height: 100%;
  }
.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 100%;
  background-color: #fff;
  padding-right: 25px;
  // 添加阴影
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
.header-left {
  height: 100%;
  .icon{
      width: 45px;
      height: 100%;
    }
    .icon:hover{
      background-color: #f5f5f5;
      cursor: pointer;;
    }
    .tab {
      height: 100%;
      padding: 0 10px;
      .text {
        margin: 0 5px;
      }
      .close {
        visibility: hidden;
      }
      // &.selected表示 “当前元素同时拥有selected类时” 的样式（这里的&是 当前写样式的这个父元素 ）
      &.selected {
        a {
          color: #409eff;
        }
        i {
          color: #409eff;
        }
        background-color: #f5f5f5;
      }
    }
    .tab:hover {
      background-color: #f5f5f5;
      .close {
        visibility: inherit; // 显示关闭按钮
        cursor: pointer;
        color: #000;
      }
    }
  }
  .header-right {
    .user-name {
      margin-left: 10px;
    }
  }
  a {
    height: 100%;
    color: #333;
    font-size: 15px;
   }
}
</style>
```

## 11.关闭tag功能实现

1.src/components/navHeader.vue：

```vue
<template>
  ...
          <el-icon size="16" class="close" @click="closeTab(item, index)"><Close /></el-icon>
    ...
</template>

<script setup>
  //原：import { useStore } from 'vuex'
import router from '@/router';
...
// 3. 替换状态访问：store.state.menu.selectMenu → menuStore.selectMenu
const selectMenu = computed(() => menuStore.selectMenu);
// 访问状态（替代this.$store.state.menu.isCollapse）
// console.log('菜单折叠状态：', menuStore.isCollapse)

// 点击关闭标签页
const closeTab = (item, index) => {
  // 4. 替换commit调用为Pinia的actions方法
  // store.commit("closeMenu", item);
  menuStore.closeMenu(item);
  // 删除非当前页tag
  if (route.path !== item.path) {
    return;
  }
  const selectMenuData = selectMenu.value

  // 删除的是最后一项
  if (index === selectMenuData.length ) {
    // 如果tag只有一个元素
    if (!selectMenuData.length) {
      router.push('/')
    } else { // 删除最后一项后，跳转到前一项
      router.push({
          path: selectMenuData[index - 1].path
        })
      }
    }
    // 删除的不是最后一项 是中间位置tag
    else {
    router.push({
        path: selectMenuData[index].path
      })
    }
  }
</script>

<style lang="less" scoped>
...
</style>

```

2.在src/stores/menuStore.js下写新方法：

```javascript
actions: {
    ...
 	 // 关闭菜单（对应原mutations的closeMenu）
    closeMenu(item) {
      const index = this.selectMenu.findIndex(val => val.name === item.name)
      // 通过索引删除数组指定元素（arr.splice(index, 1)）
      // arr.splice(1, 0, 6, 7):从索引 1 开始，不删除元素，插入 6、7:
      // arr.splice(1, 0, 6, 7)
      // console.log(index);
      this.selectMenu.splice(index, 1)
    },
    ...
}
```

# 登录页面

## 12.接口文档介绍和登陆页面编写

Vite（静态资源处理）  将资源引入为URL：

![76742788921](C:\Users\Amelia\AppData\Local\Temp\1767427889218.png)

```jsx
const imgUrl = new URL('./img.png', import.meta.url).href
document.getElementById('hero-img').src = imgUrl
```

登录：src/views/login/indexLogin.vue：

```vue
<template>
  <el-row class="login-container" justify="center" :align="'middle'">
    <el-card style="max-width: 480px">
      <template #header>
        <div class="card-header">
          <img :src="imgUrl" alt="">
        </div>
      </template>
      <div class="jump-link">
        <el-link type="primary" href="/#/register" @click="handleChange">{{formType ? '已有账号，去登录' : '没有账号，去注册'}}</el-link>
      </div>
      <el-form :model="loginForm" label-width="120px">
        <!-- prop: 绑定表单数据对象-->
        <el-form-item prop="userName">
          <el-input v-model="loginForm.userName" placeholder="手机号" :prefix-icon="UserFilled"></el-input>
        </el-form-item>

        <el-form-item prop="passWord">
          <el-input type="password" v-model="loginForm.passWord" placeholder="密码" :prefix-icon="Lock"></el-input>
        </el-form-item>

        <el-form-item prop="validCode">
          <el-input></el-input>
        </el-form-item>
      </el-form>
    </el-card>
  </el-row>
</template>

<script setup>
import { UserFilled, Lock} from '@element-plus/icons-vue'
import { ref } from 'vue'

  const imgUrl = new URL('../../../public/login-head.png', import.meta.url).href

// 表单数据
const loginForm = ref({
  validCode: '',
  userName: '',
  passWord: ''
})

// 切换表单（0-登录 1-注册）
const formType = ref(0)
// 点击切换登录和注册路由
  const handleChange = () => {
    formType.value = formType.value ? 0 : 1
}
</script>

<style lang="less" scoped>
  // 需要 :deep() 穿透作用域；
  // 选中 el-card 组件内部的 .el-card__header 类，设置 padding: 0（清除卡片头部默认的内边距）
:deep(.el-card__header) {
    padding: 0
  }
  .login-container {
    height: 100vh;
    .card-header{
      background-color: #899fe1;
      img {
        width: 430px;
      }
    }
    .jump-link {
      text-align: right;
      margin-bottom: 10px;
    }
  }
</style>
```

## 12.验证码倒计时和手机号验证

src/views/login/indexLogin.vue：

```vue
<template>
  <el-row class="login-container" justify="center" :align="'middle'">
    <el-card style="max-width: 480px">
      <template #header>
        <div class="card-header">
          <img :src="imgUrl" alt="">
        </div>
      </template>
      <div class="jump-link">
        <el-link @click="handleChange" type="primary" underline>{{formType ? '返回登录' : '注册账号'}}</el-link>
      </div>
      <el-form
        style="max-width: 600px;"
       :model="loginForm"
       class="demo-ruleForm"
       >
        <!-- prop: 绑定表单数据对象-->
        <el-form-item prop="userName">
          <el-input v-model="loginForm.userName" placeholder="手机号" :prefix-icon="UserFilled"></el-input>
        </el-form-item>

        <el-form-item prop="passWord">
          <el-input type="password" v-model="loginForm.passWord" placeholder="密码" :prefix-icon="Lock"></el-input>
        </el-form-item>

        <el-form-item
          v-if="formType"
         prop="validCode">
          <el-input v-model="loginForm.validCode" placeholder="验证码" :prefix-icon="Lock">
            <template #append>
              <span @click="countdownChange">{{ countdown.validText }}</span>
            </template>
          </el-input>
        </el-form-item>
      </el-form>
    </el-card>
  </el-row>
</template>

<script setup>
import { UserFilled, Lock} from '@element-plus/icons-vue'
import { ref } from 'vue'

  const imgUrl = new URL('../../../public/login-head.png', import.meta.url).href

// 表单数据
const loginForm = ref({
  validCode: '',
  userName: '',
  passWord: ''
})

// 切换表单（0-登录 1-注册）
const formType = ref(0)
// 点击切换登录和注册路由
  const handleChange = () => {
    formType.value = formType.value ? 0 : 1
  }

// 获取验证码之后倒计时逻辑 - 发送短信
const countdown = ref({
  validText: '获取验证码',
  time:60
})
let flag = false; // 定时器标识
const countdownChange = () => {
  if (flag) return; // 防止多次点击
  // 判断手机号是否正确
  const phoneReg = /^(?:(?:\+|00)86)?1[3-9]\d{9}$/
  // 如果当前手机号不存在/手机号格式不正确
  if (!loginForm.value.userName || !phoneReg.test(loginForm.value.userName)) {
    return ElMessage({
      message: '请输入正确的手机号',
      type: 'warning'
    })
  }
  const time = setInterval(() => {
    if (countdown.value.time <= 0) {
      countdown.value.validText = `获取验证码`
      countdown.value.time = 60
      flag = false;
      clearInterval(time)
    } else {
      countdown.value.time -= 1;
      countdown.value.validText = `剩余${countdown.value.time}s`
    }
  }, 1000)
  flag = true;
}
</script>

<style lang="less" scoped>
  // 需要 :deep() 穿透作用域；
  // 选中 el-card 组件内部的 .el-card__header 类，设置 padding: 0（清除卡片头部默认的内边距）
:deep(.el-card__header) {
    padding: 0
  }
  .login-container {
    height: 100vh;
    .card-header{
      background-color: #899fe1;
      img {
        width: 430px;
      }
    }
    .jump-link {
      text-align: right;
      margin-bottom: 10px;
    }
  }
</style>
```

## 13.登录form表单校验功能

src/views/login/indexLogin.vue：

```vue
<template>
  <el-row class="login-container" justify="center" :align="'middle'">
    <el-card style="max-width: 480px">
      <template #header>
        <div class="card-header">
          <img :src="imgUrl" alt="">
        </div>
      </template>
      <div class="jump-link">
        <el-link @click="handleChange" type="primary" underline>{{formType ? '返回登录' : '注册账号'}}</el-link>
      </div>
      <el-form
        style="max-width: 600px;"
       :model="loginForm"
       class="demo-ruleForm"
       :rules="rules"
       >
        <!-- prop: 绑定表单数据对象-->
        <el-form-item prop="userName">
          <el-input v-model="loginForm.userName" placeholder="手机号" :prefix-icon="UserFilled"></el-input>
        </el-form-item>

        <el-form-item prop="passWord">
          <el-input type="password" v-model="loginForm.passWord" placeholder="密码" :prefix-icon="Lock"></el-input>
        </el-form-item>

        <el-form-item
          v-if="formType"
         prop="validCode">
          <el-input v-model="loginForm.validCode" placeholder="验证码" :prefix-icon="Lock">
            <template #append>
              <span @click="countdownChange">{{ countdown.validText }}</span>
            </template>
          </el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" style="width: 100%" @click="submitForm">{{formType ? '注册' : '登录'}}</el-button>
        </el-form-item>

      </el-form>
    </el-card>
  </el-row>
</template>

<script setup>
import { UserFilled, Lock} from '@element-plus/icons-vue'
import { ref } from 'vue'

  const imgUrl = new URL('../../../public/login-head.png', import.meta.url).href

// 表单数据
const loginForm = ref({
  validCode: '',
  userName: '',
  passWord: ''
})

// 切换表单（0-登录 1-注册）
const formType = ref(0)
// 点击切换登录和注册路由
  const handleChange = () => {
    formType.value = formType.value ? 0 : 1
  }

// 账号自定义校验规则
const validateUser = (rule, value, callback) => {
  //不能为空
  if (value === '') {
    callback(new Error('请输入账号'))
  } else {
    const phoneReg = /^(?:(?:\+|00)86)?1[3-9]\d{9}$/
    phoneReg.test(value) ? callback() : callback(new Error('手机号格式错误，请输入正确的手机号'))
  }
}

// 密码自定义校验规则
const validatePass = (rule, value, callback) => {
  //不能为空
  if (value === '') {
    callback(new Error('请输入密码'))
  } else {
    const passReg = /^[a-zA-Z0-9_-]{4,16}$/
    passReg.test(value) ? callback() : callback(new Error('密码格式错误, 请输入4-16位字符'))
  }
}

// 表单校验规则
const rules = ref({
  userName: [{ validator: validateUser, trigger: 'blur' }],
  passWord: [{ validator: validatePass, trigger: 'blur' }],
})

// 获取验证码之后倒计时逻辑 - 发送短信
const countdown = ref({
  validText: '获取验证码',
  time:60
})
let flag = false; // 定时器标识
const countdownChange = () => {
  if (flag) return; // 防止多次点击
  // 判断手机号是否正确
  const phoneReg = /^(?:(?:\+|00)86)?1[3-9]\d{9}$/
  // 如果当前手机号不存在/手机号格式不正确
  if (!loginForm.value.userName || !phoneReg.test(loginForm.value.userName)) {
    return ElMessage({
      message: '请输入正确的手机号',
      type: 'warning'
    })
  }
  const time = setInterval(() => {
    if (countdown.value.time <= 0) {
      countdown.value.validText = `获取验证码`
      countdown.value.time = 60
      flag = false;
      clearInterval(time)
    } else {
      countdown.value.time -= 1;
      countdown.value.validText = `剩余${countdown.value.time}s`
    }
  }, 1000)
  flag = true;
}

// 提交表单
const submitForm = () => {
}
</script>

<style lang="less" scoped>
...
</style>

```

## 14.axios引入和二次封装

https://www.axios-http.cn/docs/intro

1. 安装axios：pnpm add axios

2. 使用：

   新建 src/utils文件夹，其下新建 request.js ：

```javascript
import axios from 'axios'
import { ElMessage } from 'element-plus'

const http = axios.create({
    // 通用请求的地址前缀
    // baseURL: 'https://wechatopen.mynatapp.cc/v3pz',
    baseURL: 'https:/v3pz.itndedu.com/v3pz',
    timeout: 10000, // 超时时间
})

// 添加请求拦截器
http.interceptors.request.use(function (config) {
  // 在发送请求之前做些什么

    // 不需要添加token的api
    const whiteUrl = ['/get/code','/user/authentication','/login']
    const token = localStorage.getItem('pz_token')
    if (token && !whiteUrl.includes(config.url)) {
      config.headers['x-token'] = token
      // 临时
      // config.headers['auth'] = '13797053405'
    }
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
http.interceptors.response.use(function (response) {
  // 对响应数据做点什么

    // 对于接口返回异常的数据，给用户一点提示
    if (response.data.code === -1) {
      ElMessage.warning(response.data.message)
    }
    return response;
  }, function (error) {
    // 对响应错误做点什么
    ElMessage.error('网络异常，请检查！')
    return Promise.reject(error);
  });

export default http

```

3.新建 src/api文件夹，其下新建indexApi.js ：

```javascript
import request from '@/utils/request'

// 发送验证码
export const getCode = (data) => {
  return request.post('/get/code', data)
}
```

4.调用getCode方法  src/views/login/indexLogin.vue：

```javascript
...
const countdownChange = () => {
  ...
  getCode({ tel: loginForm.value.userName }).then(({ data }) => {
    console.log(data, 'data')
    if (data.code === 10000) {
      ElMessage.success('验证码发送成功')
    }
  })
}
...
```

## 15.注册接口和登录接口联调

src/views/login/indexLogin.vue：

```vue
<template>
  ...
      <el-form
        ref="loginFormRef"
        style="max-width: 600px;"
       :model="loginForm"
       class="demo-ruleForm"
       :rules="rules"
       >
		...

        <el-form-item>
          <el-button type="primary" style="width: 100%" @click="submitForm(loginFormRef)">{{formType ? '注册' : '登录'}}</el-button>
        </el-form-item>

      </el-form>
    </el-card>
  </el-row>
</template>

<script setup>
import { UserFilled, Lock} from '@element-plus/icons-vue'
import { ref } from 'vue'
import { getCode, userAuthentication, login } from '@/api/indexApi'
import { ElMessage } from 'element-plus'
...

const loginFormRef = ref();
// 提交表单
const submitForm = async (formEl) => {
  if (!formEl) return;
  // 手动触发校验表单
  await formEl.validate((valid) => {
    if (valid) {
      if (formType.value === 0) {
        // 登录逻辑
        console.log('登录成功', loginForm.value)
        login(loginForm.value).then(({ data })=>{
          if(data.code === 10000){
            ElMessage.success('登录成功！')
            // 存储token
            localStorage.setItem('pz_token', data.data.token)
            // 用户信息（转字符串）
            localStorage.setItem('pz_userInfo', JSON.stringify(data.data.userInfo))
          }
        })
      } else {
        // 注册逻辑
        console.log('注册成功', loginForm.value)
        userAuthentication(loginForm.value).then(({ data })=>{
          if(data.code === 10000){
            ElMessage.success('注册成功，请登录！')
            formType.value = 0; // 切换到登录表单
          }
        })
      }
    } else {
      console.log('表单校验失败')
      return false;
    }
  })

}
</script>

<style lang="less" scoped>
...
</style>
```

src/api/indexApi.js：

```javascript
...
// 注册用户
export const userAuthentication = (data) => {
  return request.post('/user/authentication', data)
}

// 用户登录
export const login = (data) => {
  return request.post('/login', data)
}
```

## 16.用户鉴权和路由守卫添加

导入router ：src/views/login/indexLogin.vue：

```vue
<script setup>
import { useRouter } from 'vue-router'
    ...
</script>
```

src/main.js（路由守卫判断）

```javascript
...
// 路由导航守卫
router.beforeEach((to, from) => {
  const token = localStorage.getItem('pz_token')
  // 非登陆页面token不存在，跳转到登录页
  if (to.path !== '/login' && !token) {
    return '/login'
  }
  // 登录页面存在token，跳转到首页
  else if (to.path === '/login' && token) {
    return '/'
  } else {
    return true
  }
});
...
```

src/api/indexApi.js：

```javascript
...
//权限管理列表
export const authAdmin = (params) => {
  return request.get('/auth/admin', { params })
}
```

权限管理 - 账号管理页面 :

 src/views/auth/admin/indexAdmin.vue：

```vue
<template>
  <div>
    admin
  </div>
</template>

<script setup>
import { authAdmin } from '@/api/indexApi';
import { ref, onMounted } from 'vue';

const paginationData = ref({
  pageNum: 1,
  pageSize: 10,
});

onMounted(() => {
  authAdmin(paginationData.value).then(({ data }) => {
    console.log('管理员数据：', data);
  });
});

</script>

<style lang="scss" scoped>
</style>

```

 src/utils/request.js ：（判断token是否过期）

```javascript
// 添加响应拦截器
http.interceptors.response.use(function (response) {
  // 对响应数据做点什么

    // 对于接口返回异常的数据，给用户一点提示
    if (response.data.code === -1) {
      ElMessage.warning(response.data.message)
    }
    if (response.data.code === -2) { // token失效
      localStorage.removeItem('pz_token')
      localStorage.removeItem('pz_userInfo')
      // window.location.origin => 获取当前根目录地址
      window.location.href = window.location.origin
    }

    return response;
  }, function (error) {
    // 对响应错误做点什么
    ElMessage.error('网络异常，请检查！')
    return Promise.reject(error);
  });
```

# 菜单管理页面

## 17.菜单管理添加弹窗显示

1.弹窗组件dialog + Tree树形控件：src/views/auth/group/indexGroup.vue

```vue
<template>
  <button @click="dialogFormVisible = true">打开</button>
  <el-dialog
    v-model="dialogFormVisible"
    :before-close="beforeClose"
    title="添加权限"
    width="500"
  >
    <el-form
      ref="formRef"
      label-width="100px"
      label-position="left"
      :model="form"
    >
      <el-form-item label="名称" prop="name">
        <el-input v-model="form.name" placeholder="请填写权限名称" />
      </el-form-item>

      <el-form-item label="权限" prop="permissions">
        <!-- node-key => 数据唯一标识符(每个数据的工号) -->
        <el-tree
          ref="treeRef"
          style="max-width: 600px;"
          :data="permissionData"
          node-key="id"
          show-checkbox
          :default-checked-keys="defaultKeys"
        />
      </el-form-item>

    </el-form>
  </el-dialog>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { userGetMenu } from '@/api/indexApi';

onMounted(() => {
  userGetMenu().then(({ data }) => {
    console.log('权限数据：', data);
    permissionData.value = data.data;
  });
});

// 表单form数据
const form = ref({
  name: '',
  permissions: ''
});

// 树形控件数据
const permissionData = ref([]);

// 对话框显示与否
const dialogFormVisible = ref(false);

// 关闭弹窗前的回调
const beforeClose = () => {
  dialogFormVisible.value = false;
};

// 选中权限
const defaultKeys = [4, 5];

// 获取树形控件实例
const treeRef = ref();
</script>

<style lang="scss" scoped>
</style>
```

2.菜单权限数据 接口：src/api/indexApi.js

```javascript
...
//菜单权限数据
export const userGetMenu = () => {
  return request.get('/user/getmenu')
}
```

## 18.菜单管理添加接口联调

 src/views/auth/group/indexGroup.vue：

```vue
<template>
  <button @click="dialogFormVisible = true">打开</button>
  <el-dialog
    v-model="dialogFormVisible"
    :before-close="beforeClose"
    title="添加权限"
    width="500"
  >
    <el-form
      ref="formRef"
      label-width="100px"
      label-position="left"
      :model="form"
      :rules="rules"
    >
    <!-- 隐藏的表单域 -->
      <el-form-item v-show="false" prop="id">
        <el-input v-model="form.id" />
      </el-form-item>

      <el-form-item label="名称" prop="name">
        <el-input v-model="form.name" placeholder="请填写权限名称" />
      </el-form-item>

      <el-form-item label="权限" prop="permissions">
        <!-- node-key =》 数据唯一标识符(每个数据的工号) -->
        <!-- :default-expand-all="true" =》默认展开所有节点 -->
        <el-tree
          ref="treeRef"
          style="max-width: 600px;"
          :data="permissionData"
          node-key="id"
          show-checkbox
          :default-checked-keys="defaultKeys"
          :default-expanded-keys="[2]"
        />
      </el-form-item>
    </el-form>

    <template #footer>
      <div class="dialog-footer">
        <el-button type="primary" @click="confirm(formRef)">确定</el-button>
        <el-button @click="dialogFormVisible = false">取消</el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { userGetMenu, userSetMenu, menuList } from '@/api/indexApi';

onMounted(() => {
  userGetMenu().then(({ data }) => {
    console.log('权限数据：', data);
    permissionData.value = data.data;
  });
  getListData();
});

const paginationData = ref({
  pageNum: 1,
  pageSize: 10,
});

//请求列表数据
const getListData = () => {
  menuList(paginationData.value).then(({ data }) => {
    console.log('菜单列表数据：', data);
  });
}

const formRef = ref();

// 表单form数据
const form = ref({
  id: '',
  name: '',
  permissions: ''
});

// 树形控件数据
const permissionData = ref([]);

// 对话框显示与否
const dialogFormVisible = ref(false);

// 关闭弹窗前的回调
const beforeClose = () => {
  dialogFormVisible.value = false;
};

// 选中权限
const defaultKeys = [4, 5];

// 获取树形控件实例
const treeRef = ref();

const rules = ref({
  name: [{
    required: true,
    message: '请输入权限名称',
    trigger: 'blur'
  }]
})
// 确认按钮回调(表单提交)
const confirm = async (formEl) => {
  if (!formEl) return;
  await formEl.validate((valid, fields) => {
    if (valid) {
      // 获取到选择的checkbox数据
      const permissions = JSON.stringify(treeRef.value.getCheckedKeys());
      userSetMenu({
        name: form.value.name,
        permissions,
        id: form.value.id,
      }).then(({ data }) => {
        console.log('修改权限结果：', data);
      })
    } else {
      console.log('error submit!', fields);
      return false;
    }
  });
}
</script>

<style lang="scss" scoped>
</style>
```

src/api/indexApi.js：

```javascript
...
//菜单权限修改
export const userSetMenu = (data) => {
  return request.post('/user/setmenu', data)
}

//菜单权限列表
export const menuList = (params) => {
  return request.get('/menu/list', { params })
}
```

## 19.菜单管理列表和编辑逻辑

src/views/auth/group/indexGroup.vue：

1.利用table组件搭建了一个列表，往里面动态传入数据

2.区分添加和编辑的逻辑，利用nextTick异步往里面传数据

3.完善关闭弹窗的逻辑，如果是添加需要清空之前的数据

```vue
<template>
  <button @click="open(null)">打开</button>

  <el-table :data="tableData.list" style="width: 100%">
    <el-table-column prop="id" label="id" />
    <el-table-column prop="name" label="昵称" />
    <el-table-column prop="permissionName" label="菜单权限" width="500px"/>
    <el-table-column label="操作">
      <template #default="scope">
        <el-button type="primary" @click="open(scope.row)">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>

 ...
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue';
import { userGetMenu, userSetMenu, menuList } from '@/api/indexApi';

onMounted(() => {
  userGetMenu().then(({ data }) => {
    console.log('权限数据：', data);
    permissionData.value = data.data;
  });
  getListData();
});

//列表数据
const tableData = ref({
  list: [],
  total: 0,
});

// 打开弹窗
const open = (rowData = {}) => {
  dialogFormVisible.value = true;
  // 弹窗打开form生成，是异步的,所以需要使用nextTick
  nextTick(() => { // nextTick:弹窗里的所有元素都渲染完成后，再执行后面的赋值操作
    if (rowData) {
      Object.assign(form.value, { id: rowData.id, name: rowData.name }); // 把rowData的数据赋值给form表单
      treeRef.value.setCheckedKeys(rowData.permission);
    }
  });
};

const paginationData = ref({
  pageNum: 1,
  pageSize: 10,
});

//请求列表数据
const getListData = () => {
  menuList(paginationData.value).then(({ data }) => {
    const { list, total } = data.data;
    tableData.value.list = list;
    tableData.value.total = total;
  });
}

const formRef = ref();

// 表单form数据
const form = ref({
  id: '',
  name: '',
  permissions: '',
});

// 树形控件数据
const permissionData = ref([]);

// 对话框显示与否
const dialogFormVisible = ref(false);

// 关闭弹窗前的回调
const beforeClose = () => {
  dialogFormVisible.value = false;
  formRef.value.resetFields(); // 重置表单
  treeRef.value.setCheckedKeys(defaultKeys); // 重置树形控件
};
    
...
</script>

<style lang="scss" scoped>
</style>

```

## 20.菜单管理剩余问题处理（面板显示、按钮样式显示效果、点击编辑和新增的逻辑...）

src/views/auth/group/indexGroup.vue：

1.在 编辑 或者 添加 操作完成后，要先关闭弹窗（beforeClose），然后页面更新新数据（getListData）

```vue
<script>
    ...
    
// 确认按钮回调(表单提交)
const confirm = async (formEl) => {
  ...
      }).then(({ data }) => {
        console.log('修改权限结果：', data);
        beforeClose();
        getListData();
      })
  ...
}
</script>
```

src/views/auth/group/indexGroup.vue：

2.分页逻辑

```vue
<template>
  ...
  <el-table :data="tableData.list" style="width: 100%">
    ...
  </el-table>
 <!-- 分页 -->
  <div class="pagenation-info">
    <el-pagination
      v-model:current-page="paginationData.pageNum"
      :page-size="paginationData.pageSize"
      :background="false"
      layout="total, prev, pager, next"
      :total="tableData.total"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      size="small"
      style="float: right;"
      />
  </div>
...
</template>

<script setup>
...

//改变每页条数回调
const handleSizeChange = (val) => {
  paginationData.value.pageSize = val;
  getListData();
}
//改变页码回调
const handleCurrentChange = (val) => {
  paginationData.value.pageNum = val;
  getListData();
}

...
</script>

<style lang="scss" scoped>
</style>
```

3.列表顶部面板显示 - 封装成组件复用

components下新建文件panelHead.vue：

```vue
<template>
  <div class="panel-heading">
    <div class="panel-lead">
      <div class="title">菜单管理</div>
      <p class="description">菜单规则通常对应一个控制器的方法，同时菜单栏数据也从规则中获取</p>
    </div>
  </div>
</template>

<script setup>

</script>

<style lang="scss" scoped>
  .panel-heading {
    padding: 15px;
    background: #e8edf0;
    border-color: #e8edf0;
    position: relative;
    .panel-lead {
      font-size: 14px;
      .title {
        font-weight: bold;
        font-style: normal;
      }
      .description {
        margin-top: 5px;
      }
    }
  }
</style>
```

注册以上为全局组件panel，在main.js：

```javascript
...
// 引入panelhead组件
import PanelHead from '@/components/panelHead.vue'
...
// 全局注册panelhead组件
app.component('PanelHead', PanelHead)
```

页面复用：src/views/auth/group/indexGroup.vue：

```vue
<template>
  <PanelHead />
...
```

4.按钮样式 - src/views/auth/group/indexGroup.vue：

```vue
<template>
  <PanelHead />

  <div class="btns">
    <el-button :icon="Plus" type="primary" @click="open(null)" size="small">新增</el-button>
  </div>

...
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue';
import { userGetMenu, userSetMenu, menuList } from '@/api/indexApi';
import { Plus } from '@element-plus/icons-vue';
...
</script>

<style lang="scss" scoped>
.btns {
  padding: 10px;
  background-color: #fff;
}
</style>
```

# 账号管理

## 21.账号管理列表（时间戳的转换）

1.src/views/auth/admin/indexAdmin.vue：

- 跟菜单管理的表格类似，直接用 el-table组件
- ***所属组别***  要用到后台数据，根据权限id匹配权限名称
- ***状态***  利用 tag组件 
- ***创建时间***  将时间戳 => 时间：利用 dayjs插件（https://dayjs.fenxianglu.cn/）
  - 下载： pnpm add dayjs
  - 当前页面引入： import  dayjs  from 'dayjs'
  - 在onMounted里面调用即可

```vue
<template>
  <div>
    admin
  </div>

  <el-table :data="tableData.list" style="width: 100%">
    <el-table-column prop="id" label="id" />
    <el-table-column prop="name" label="昵称" />

    <el-table-column prop="permissions_id" label="所属组别">
      <template #default="scope">
        {{ permissionName(scope.row.permissions_id) }}
      </template>
    </el-table-column>
    <el-table-column prop="mobile" label="手机号" />

    <el-table-column prop="active" label="状态">
      <template #default="scope">
         <el-tag :type="scope.row.active ? 'success' : 'danger'">{{ scope.row.active ? '正常' : '失效'}}</el-tag>
      </template>
    </el-table-column>

    <el-table-column label="创建时间">
      <template #default="scope">
         <div class="flex-box">
            <el-icon><Clock /></el-icon>
            <span style="margin-left: 10px">{{ scope.row.created_time }}</span>
         </div>
      </template>
    </el-table-column>

    <el-table-column label="操作">
      <template #default="scope">
        <el-button type="primary" @click="open(scope.row)">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script setup>
import { authAdmin, menuSelectList } from '@/api/indexApi';
import { ref, onMounted } from 'vue';
import  dayjs  from 'dayjs'

const paginationData = ref({
  pageNum: 1,
  pageSize: 10,
});

onMounted(() => {
  authAdmin(paginationData.value).then(({ data }) => {
    console.log('管理员数据：', data);
    const { list, total } = data.data;
    list.forEach(item => {
      item.created_time = dayjs(item.created_at).format('YYYY-MM-DD HH:mm:ss');
    });
    tableData.value.list = list;
    tableData.value.total = total;
  });
  menuSelectList().then(({ data }) => {
    options.value = data.data;
  });
});

const options = ref([])
//根据权限id匹配权限名称
const permissionName = (id) => {
  const data = options.value.find(el => el.id === id);
  return data ? data.name : '超级管理员';
};

//列表数据
const tableData = ref({
  list: [],
  total: 0,
});

const open = (row) => {
  console.log(row);
}
</script>

<style lang="scss" scoped>
  .flex-box {
    display: flex;
    align-items: center;
  }
</style>
```

2.菜单下拉权限 - src/api/indexApi.js：

```javascript
...
//菜单权限下拉列表
export const menuSelectList = () => {
  return request.get('/menu/selectlist')
}
```

## 22.账号管理编辑分页功能完成

1.src/views/auth/admin/indexAdmin.vue：

分页：跟菜单管理的分页一样，直接用 pagenation 组件

弹窗内容：下拉组件 el-select

顶部选项卡：复用自定义的组件panelHead（暂时还没设置动态）

```vue
<template>
  <PanelHead />

  <el-table :data="tableData.list" style="width: 100%">
    <el-table-column prop="id" label="id" />
    <el-table-column prop="name" label="昵称" />

    <el-table-column prop="permissions_id" label="所属组别">
      <template #default="scope">
        {{ permissionName(scope.row.permissions_id) }}
      </template>
    </el-table-column>
    <el-table-column prop="mobile" label="手机号" />

    <el-table-column prop="active" label="状态">
      <template #default="scope">
         <el-tag :type="scope.row.active ? 'success' : 'danger'">{{ scope.row.active ? '正常' : '失效'}}</el-tag>
      </template>
    </el-table-column>

    <el-table-column label="创建时间">
      <template #default="scope">
         <div class="flex-box">
            <el-icon><Clock /></el-icon>
            <span style="margin-left: 10px">{{ scope.row.created_time }}</span>
         </div>
      </template>
    </el-table-column>

    <el-table-column label="操作">
      <template #default="scope">
        <el-button type="primary" @click="open(scope.row)">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- 分页 -->
  <div class="pagenation-info">
    <el-pagination
      v-model:current-page="paginationData.pageNum"
      :page-size="paginationData.pageSize"
      :background="false"
      layout="total, prev, pager, next"
      :total="tableData.total"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      size="small"
      style="float: right;"
      />
  </div>
  <!-- 弹窗 -->
  <el-dialog
    v-model="dialogFormVisible"
    :before-close="beforeClose"
    title="添加权限"
    width="500"
  >
  <!-- 弹窗内容 -->
    <el-form
        ref="formRef"
        label-width="100px"
        label-position="left"
        :model="form"
        :rules="rules"
      >
         <!-- 表单域 (手机号不可编辑=>disabled) -->
        <el-form-item label="手机号" prop="mobile">
          <el-input v-model="form.mobile" disabled />
        </el-form-item>
        <el-form-item label="昵称" prop="name">
          <el-input v-model="form.name" />
        </el-form-item>

        <el-form-item label="菜单权限" prop="permissions_id">
          <el-select
          v-model="form.permissions_id"
          placeholder="请选择菜单权限"
          style="width: 240px"
          >
            <el-option
              v-for="item in options"
              :key="item.id"
              :label="item.name"
              :value="item.id"
            />
          </el-select>
        </el-form-item>
    </el-form>
    <template #footer>
      <div class="dialog-footer">
        <el-button type="primary" @click="confirm(formRef)">确定</el-button>
        <el-button @click="dialogFormVisible = false">取消</el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script setup>
import { authAdmin, menuSelectList, updateUser } from '@/api/indexApi';
import { ref, onMounted } from 'vue';
import dayjs from 'dayjs'

const paginationData = ref({
  pageNum: 1,
  pageSize: 10,
});

onMounted(() => {
  getListData();
  menuSelectList().then(({ data }) => {
    options.value = data.data;
  });
});

// 获取列表数据(在onMouneted基础上改的)
const getListData = () => {
  authAdmin(paginationData.value).then(({ data }) => {
    console.log('管理员数据：', data);
    const { list, total } = data.data;
    list.forEach(item => {
      item.created_time = dayjs(item.created_at).format('YYYY-MM-DD HH:mm:ss');
    });
    tableData.value.list = list;
    tableData.value.total = total;
  });
};

//改变每页条数回调
const handleSizeChange = (val) => {
  paginationData.value.pageSize = val;
  getListData();
}
//改变页码回调
const handleCurrentChange = (val) => {
  paginationData.value.pageNum = val;
  getListData();
}

// 弹窗显示控制
const dialogFormVisible = ref(false);
// 关闭弹窗的方法
const beforeClose = (done) => {
  done();
};

// 表单验证
const rules = ref({
  name: [
    { required: true, message: '请输入昵称', trigger: 'blur' },
  ],
  // 下拉选择change事件触发验证
  permissions_id: [
    { required: true, message: '请选择菜单权限', trigger: 'change' },
  ],
})

// 编辑表单数据
const formRef = ref()
const form = ref({
  name: '',
  permissions_id: '',
})

// 表单提交
const confirm = async(formEl) => {
  if (!formEl) return;
  await formEl.validate(( valid, fields ) => {
    if (valid) {
      //console.log('表单数据：', form.value);
      const { name, permissions_id } = form.value;
      updateUser({ name,permissions_id }).then(({ data }) => {
        //console.log('更新管理员信息：', data);
        if(data.code === 10000) {
          dialogFormVisible.value = false;
          getListData();
        }
      });
    } else {
      console.log('error submit!', fields);
      return false;
    }
  })
}

const options = ref([])
//根据权限id匹配权限名称
const permissionName = (id) => {
  const data = options.value.find(el => el.id === id);
  return data ? data.name : '超级管理员';
};

//列表数据
const tableData = ref({
  list: [],
  total: 0,
});

const open = (rowData) => {
  //console.log(rowData);
  dialogFormVisible.value = true;
  //无需考虑新增情况，所以直接赋值（用Object.assign）
  Object.assign(form.value, { mobile: rowData.mobile, name: rowData.name, permissions_id: rowData.permissions_id });
}
</script>

<style lang="scss" scoped>
  .flex-box {
    display: flex;
    align-items: center;
  }
</style>
```

2.用户信息修改 - src/api/indexApi.js：

```javascript
...
//用户信息修改
export const updateUser = (data) => {
  return request.post('/update/user', data)
}
```

## 23.用户权限接口联调和动态路由数据组装（vuex改用pinia）

1.src/views/auth/admin/indexAdmin.vue：

- 补全关闭弹窗的逻辑

```haxe
// 关闭弹窗的方法
const beforeClose = () => {
  dialogFormVisible.value = false;
};
```

2.src/components/navHeader.vue：

- 点击触发用户头像下拉菜单指令事件 - 退出

```vue
<template>
 ...
    <div class="header-right">
      <el-dropdown @command="handleClick">
        <div class="el-dropdown-link flex-box">
          <el-avatar
            src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
          />
          <p class="user-name">admin</p>
        </div>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item command="cancel">退出</el-dropdown-item>
          </el-dropdown-menu>
        </template>
    </el-dropdown>
    </div>
  </div>
</template>

<script setup>
...

// 处理下拉菜单点击事件
const handleClick = (command) => {
  if (command === 'cancel') {
    // 退出登录 清除token和用户信息
    localStorage.removeItem('pz_token')
    localStorage.removeItem('pz_userInfo')
    // 跳转到网站的登录地址（可以用路由跳转代替）
    window.location.href = window.location.origin
  }
}
</script>

<style lang="less" scoped>
...
</style>

```

3.用户菜单权限 - src/api/indexApi.js：

```javascript
...
//用户菜单权限
export const menuPermissions = () => {
  return request.get('/menu/permissions')
}
```

4.动态路由：在登录的时候调用以上接口（vuex改用pinia） - src/views/login/indexLogin.vue：

![img](file:///C:/Users/Amelia/AppData/Local/Temp/1769588563447.png?lastModify=1769588554)

```vue
<template>
...
</template>

<script setup>
import { UserFilled, Lock} from '@element-plus/icons-vue'
import { ref, computed, toRaw } from 'vue'
import { getCode, userAuthentication, login, menuPermissions } from '@/api/indexApi'
import { ElMessage } from 'element-plus'
import { useRouter } from 'vue-router'
// 【修改点1】删除Vuex的useStore导入，新增Pinia的menuStore导入
// 原：import { useStore } from "vuex";
import { useMenuStore } from '@/stores/menuStore'

...

const router = useRouter();
const loginFormRef = ref();
// 【修改点2】删除Vuex的store实例，创建Pinia的menuStore实例
// 原：const store = useStore();
const menuStore = useMenuStore()
// 【修改点3】替换Vuex的state获取方式，直接从Pinia实例取routerList
const routerList = computed(() => menuStore.routerList)
// 提交表单
const submitForm = async (formEl) => {
  if (!formEl) return;
  // 手动触发校验表单
  await formEl.validate((valid) => {
    if (valid) {
      if (formType.value === 0) {
        // 登录逻辑
        console.log('登录成功', loginForm.value)
        login(loginForm.value).then(({ data })=>{
          if(data.code === 10000){
            ElMessage.success('登录成功！')
            // 存储token
            localStorage.setItem('pz_token', data.data.token)
            // 用户信息（转字符串）
            localStorage.setItem('pz_userInfo', JSON.stringify(data.data.userInfo))
            // 获取用户菜单权限
             menuPermissions().then(({ data: permission }) => {
              // 【修改点4】替换Vuex的commit提交，直接调用Pinia的actions方法dynamicMenu
              // 原：store.commit('dynamicMenu', permission.data)
              menuStore.dynamicMenu(permission.data)
              console.log(toRaw(routerList.value), 'routerList')
              console.log(router)
              //toRow => 获取响应式数据的原始数据
              toRaw(routerList.value).forEach(item => {
                router.addRoute("main", item)
              })
            }).then(() => {
              router.push('/')
            })
          }
        })
      } else {
        // 注册逻辑
        console.log('注册成功', loginForm.value)
        userAuthentication(loginForm.value).then(({ data })=>{
          if(data.code === 10000){
            ElMessage.success('注册成功，请登录！')
            formType.value = 0; // 切换到登录表单
          }
        })
      }
    } else {
      console.log('表单校验失败')
      return false;
    }
  })

}
</script>

<style lang="less" scoped>
...
</style>
```

5.vuex改用pinia - stores/menuStore.js：

-  dynamicMenu 的实现 
  - vite方法glob导入：https://cn.vitejs.dev/guide/features#glob-import-caveats

```javascript
import { defineStore } from 'pinia'

// 定义并导出Menu Store（Pinia的核心是defineStore，第一个参数为唯一ID）
export const useMenuStore = defineStore('menu', {
  // 状态（替代Vuex的state，返回函数避免单例问题）
  state: () => ({
    isCollapse: false, // 是否折叠菜单
    selectMenu: [], // 存储已打开的菜单
    routerList: [], // 存储路由列表
    menuActive: '1-1' // 存储激活菜单
  }),

  // 持久化配置（替代vuex-persistedstate）
  persist: {
    key: 'v3pz', // 与原Vuex的key保持一致
    storage: window.localStorage, // 存储到localStorage
    paths: ['isCollapse', 'selectMenu', 'routerList', 'menuActive'] // 可选：指定要持久化的字段
  },

  // 方法（替代Vuex的mutations，支持同步/异步，可直接修改state）
  actions: {
    // 折叠菜单（对应原mutations的collapseMenu）
    collapseMenu() {
      this.isCollapse = !this.isCollapse
    },

    // 添加菜单（对应原mutations的addMenu）
    addMenu(payload) {
      // 对数据进行去重
      if (this.selectMenu.findIndex(item => item.path === payload.path) === -1) { // 等于-1 => 不存在
        this.selectMenu.push(payload)
      }
    },

    // 更新激活菜单（对应原mutations的updateMenuActive）
    updateMenuActive(value) {
      this.menuActive = value
    },

    // 关闭菜单（对应原mutations的closeMenu）
    closeMenu(item) {
      const index = this.selectMenu.findIndex(val => val.name === item.name)
      // 通过索引删除数组指定元素（arr.splice(index, 1)）
      // arr.splice(1, 0, 6, 7):从索引 1 开始，不删除元素，插入 6、7:
      // arr.splice(1, 0, 6, 7)
      // console.log(index);
      this.selectMenu.splice(index, 1)
    },

    // 动态生成路由（对应原mutations的dynamicMenu）
    dynamicMenu(payload) {
      // 通过glob导入文件 => 动态导入指定目录下的所有.vue文件
      const modules = import.meta.glob('../views/**/**/*.vue')
      function routerSet(router) {
        router.forEach(route => {
          // 如果没有子路由（说明它是一个最终页面），则拼接路由数据
          if (!route.children) {
            const url = `../views${route.meta.path}/${route.name}index.vue`
            //去 “文件通讯录” modules 里找到这个文件，把它绑定给这条路由
            route.component = modules[url]
          } else {
            // 如果这条路由有子菜单（else），就递归调用自己，去处理它的子菜单，直到所有层级的路由都绑定好页面
            routerSet(route.children)
          }
        })
      }
      //把后端传来的原始菜单数据（payload）丢给上面那个 “递归处理工具”，让它给所有路由都配上对应的页面文件。
      routerSet(payload)
      // 把处理好的路由数据，存到routerList里
      this.routerList = payload
    }
  }
})

```

### ***动态路由的作用**：**让「前端的菜单 / 页面访问权限」完全由后端控制**，前端不用写死任何页面路由。

1. **不同角色看不同菜单，后端一句话就能改**

   比如管理员能看「用户管理、数据统计、系统设置」，普通员工只能看「个人中心、我的任务」—— 这些权限不用前端改代码，后端只需要在数据库里给不同角色配置不同的菜单数据，前端就能自动渲染对应的菜单、加载对应的页面，不用前端参与。

2. **新增 / 删除 / 修改菜单，前端不用改代码、不用重新发版**

   比如公司要加一个「订单审核」菜单，或者删掉「旧版报表」菜单，后端直接在后台配置里改一下，前端刷新页面就生效了；如果没有这段代码，前端得手动写路由、改代码、打包、重新部署上线，多一步都不行。

3. **适配多租户 / 多公司场景（中大型项目必备）**

   如果系统是给多个公司用的（比如 SaaS 系统），A 公司要「财务模块」，B 公司不要，后端直接给 A 公司返回带财务的菜单数据，B 公司返回不带的，前端自动适配，不用为不同公司做多个前端版本。

## 24.动态路由添加和vuex持久化实现（改用pinia）

src/components/indexAside.vue：

```vue
<template>
 ...
</template>

<script setup>
import TreeMenu from '@/components/treeMenu.vue';
// 通过vue-router获取当前路由数据
import { useRouter } from 'vue-router'
import { ref } from 'vue'

import { computed } from "vue";
import { useMenuStore } from '@/stores/menuStore'
const menuStore = useMenuStore()
// 替换折叠状态访问：store.state.menu.isCollapse → menuStore.isCollapse
const isCollapse = computed(() => menuStore.isCollapse);

const router = useRouter() //调用，获取当前路由实例
//const menuData = ref(router.options.routes[0].children) //获取当前路由数据
const menuData = computed(() => menuStore.routerList);
 console.log(router);


const handleOpen = () => { }
const handleClose = () => {}
</script>

<style lang="less" scoped>
...
</style>

```

改一下menuData的数据，则会根据不同用户的权限显示页面

![76975851923](C:\Users\Amelia\AppData\Local\Temp\1769758519235.png)

### pinia 和 vuex 持久化插件（我用的是pinia）:

vuex 插件下载命令：npm i vuex-persistedstate

pinia 插件下载命令：npm i pinia-plugin-persistedstate

src/stores/menuStore.js：（pinia持久化 - persist字段）（初始化在main.js）

```javascript
import { defineStore } from 'pinia'

// 定义并导出Menu Store（Pinia的核心是defineStore，第一个参数为唯一ID）
export const useMenuStore = defineStore('menu', {
  // 状态（替代Vuex的state，返回函数避免单例问题）
  state: () => ({
    isCollapse: false, // 是否折叠菜单
    selectMenu: [], // 存储已打开的菜单
    routerList: [], // 存储路由列表
    menuActive: '1-1' // 存储激活菜单
  }),

  // 持久化配置（替代vuex-persistedstate）
  persist: {
    key: 'v3pz', // 与原Vuex的key保持一致
    storage: window.localStorage, // 存储到localStorage
    paths: ['isCollapse', 'selectMenu', 'routerList', 'menuActive'] // 可选：指定要持久化的字段
  },

  // 方法（替代Vuex的mutations，支持同步/异步，可直接修改state）
  actions: {
    // 折叠菜单（对应原mutations的collapseMenu）
    collapseMenu() {
      this.isCollapse = !this.isCollapse
    },

    // 添加菜单（对应原mutations的addMenu）
    addMenu(payload) {
      // 对数据进行去重
      if (this.selectMenu.findIndex(item => item.path === payload.path) === -1) { // 等于-1 => 不存在
        this.selectMenu.push(payload)
      }
    },

    // 更新激活菜单（对应原mutations的updateMenuActive）
    updateMenuActive(value) {
      this.menuActive = value
    },

    // 关闭菜单（对应原mutations的closeMenu）
    closeMenu(item) {
      const index = this.selectMenu.findIndex(val => val.name === item.name)
      // 通过索引删除数组指定元素（arr.splice(index, 1)）
      // arr.splice(1, 0, 6, 7):从索引 1 开始，不删除元素，插入 6、7:
      // arr.splice(1, 0, 6, 7)
      // console.log(index);
      this.selectMenu.splice(index, 1)
    },

    // 动态生成路由（对应原mutations的dynamicMenu）
    dynamicMenu(payload) {
      // 通过glob导入文件 => 动态导入指定目录下的所有.vue文件
      const modules = import.meta.glob('../views/**/**/*.vue')
      function routerSet(router) {
        router.forEach(route => {
          // 如果没有子路由（说明它是一个最终页面），则拼接路由数据
          if (!route.children) {
            // meta => 路由元信息，path => 路由路径
            const url = `../views${route.meta.path}/${route.name}index.vue`
            //去 “文件通讯录” modules 里找到这个文件，把它绑定给这条路由
            route.component = modules[url]
          } else {
            // 如果这条路由有子菜单（else），就递归调用自己，去处理它的子菜单，直到所有层级的路由都绑定好页面
            routerSet(route.children)
          }
        })
      }
      //把后端传来的原始菜单数据（payload）丢给上面那个 “递归处理工具”，让它给所有路由都配上对应的页面文件。
      routerSet(payload)
      // 把处理好的路由数据，存到routerList里
      this.routerList = payload
    }
  }
})
```

main.js：

```javascript
import { createApp } from 'vue'
import { createPinia } from 'pinia'

import App from './App.vue'
import router from './router'

import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
// main.ts

// 在 main.js 中添加（只引入 ElMessage 样式，体积更小）
import 'element-plus/es/components/message/style/css'
// 如果您正在使用CDN引入，请删除下面一行。
import * as ElementPlusIconsVue from '@element-plus/icons-vue'

// 引入panelhead组件
import PanelHead from '@/components/panelHead.vue'

import { useMenuStore } from './stores/menuStore'
const storageStore = localStorage.getItem('v3pz')

// 路由导航守卫
router.beforeEach((to ) => {
  const token = localStorage.getItem('pz_token')
  // 非登陆页面token不存在，跳转到登录页
  if (to.path !== '/login' && !token) {
    return '/login'
  }
  // 登录页面存在token，跳转到首页
  else if (to.path === '/login' && token) {
    return '/'
  } else {
    return true
  }
});

const app = createApp(App)
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}

app.use(createPinia())
app.use(router)

const pinia = createPinia()
pinia.use(piniaPluginPersistedstate)

// 3. 挂载Pinia（替换原app.use(store)）
app.use(pinia)

//刷新后的动态路由重新添加
if (storageStore) {
  const menuStore = useMenuStore()
  menuStore.dynamicMenu(JSON.parse(storageStore).routerList)
  menuStore.routerList.forEach(item => {
    router.addRoute("main", item)
  })
}

app.mount('#app')

// 全局注册panelhead组件
app.component('PanelHead', PanelHead)

```

剩余逻辑补充：
退出逻辑（src/components/navHeader.vue）：

```javascript
...
// 处理下拉菜单点击事件
const handleClick = (command) => {
  if (command === 'cancel') {
    // 退出登录 清除token和用户信息
    localStorage.removeItem('pz_token')
    localStorage.removeItem('pz_userInfo')
      //清除持久化
    localStorage.removeItem('v3pz')
    // 跳转到网站的登录地址（可以用路由跳转代替）
    window.location.href = window.location.origin
  }
}
...
```

接口返回异常清除逻辑（src/utils/request.js）：

```javascript
// 添加响应拦截器
http.interceptors.response.use(function (response) {
...
    if (response.data.code === -2) { // token失效
      localStorage.removeItem('pz_token')
      localStorage.removeItem('pz_userInfo')
      localStorage.removeItem('v3pz')
...

```

把route作为一个动态属性传递到顶部文案条里 - 
src/views/auth/admin/indexAdmin.vue：

src/views/auth/auth/indexGroup.vue（同理）：

```vue
<template>
  <PanelHead :route="route"/>
...
</template>

<script setup>
...
import { useRoute } from 'vue-router'

const route = useRoute()
console.log('当前路由对象：', route);
...
</script>

<style lang="scss" scoped>
...
</style>

```

动态显示顶部 src/components/panelHead.vue：

```vue
<template>
  <div class="panel-heading">
    <div class="panel-lead">
      <div class="title">{{ props.route.meta.name }}</div>
      <p class="description">{{  props.route.meta.describe }}</p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({//用来定义组件接收的外部属性（props）
  route: {
    type: Object
  }
})
</script>

<style lang="scss" scoped>
...
</style>

```

defineProps逻辑：

![76976483298](C:\Users\Amelia\AppData\Local\Temp\1769764832989.png)

## 25.菜单高亮显示和用户信息显示问题解决

动态设置用户名称与头像 - src/components/navHeader.vue：

```vue
<template>
...
          <el-avatar
            :src="userInfo.avatar"
          />
          <p class="user-name">{{ userInfo.name }}</p>
        </div>
...
</template>

<script setup>
...
// 获取用户信息
const userInfo = JSON.parse(localStorage.getItem('pz_userInfo'))
...
</script>

<style lang="less" scoped>
...
</style>
```

菜单高亮 - src/components/indexAside.vue：

- 初始值需要去 menuStore.js 中定义一下

```vue
<template>
  <el-menu
...
    :default-active="active"
...
</template>

<script setup>
...

// 获取当前路由数据
const active = computed(() => menuStore.menuActive);
...
</script>

<style lang="less" scoped>
...
</style>

```

menuStore.js ：

```javascript
import { defineStore } from 'pinia'

// 定义并导出Menu Store（Pinia的核心是defineStore，第一个参数为唯一ID）
export const useMenuStore = defineStore('menu', {
  // 状态（替代Vuex的state，返回函数避免单例问题）
  state: () => ({
    isCollapse: false, // 是否折叠菜单
    selectMenu: [], // 存储已打开的菜单
    routerList: [], // 存储路由列表
    menuActive: '1-1' // 存储激活菜单
  }),
...
    // 更新激活菜单（对应原mutations的updateMenuActive）
    updateMenuActive(value) {
      this.menuActive = value
    },
...
```

src/components/treeMenu.vue：

```javascript
// 点击菜单事件( active索引值 )
const router = useRouter() // 创建router路由实例
const handleClick = (item， active) => {
  // console.log(item,'item');

 // 3. 替换Vuex的commit调用为Pinia的actions方法
  // store.commit('addMenu', item.meta)
  menuStore.addMenu(item.meta)

  menuStore.updateMenuActive(active)

  router.push(item.meta.path)
}
```

路由重定向 - src/router/index.js：

```javascript
...
const localData = localStorage.getItem('v3pz')

const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes:  [
  {
    path: '/',
    component: Layout,
    name: 'main',
     redirect: to => {
      if (localData) {
        // 是否有二级菜单
        const child = JSON.parse(localData).routerList[0].children
        if (child) {
          return child[0].meta.path
        } else {
          return JSON.parse(localData).routerList[0].meta.path
        }
      } else {
        return '/dashboard'
      }
    },
...
export default router

```

# 陪护管理

## 26.陪护管理新增陪护师弹窗

src/views/vppz/staff/indexStaff.vue：

```vue
<template>
   <div class="btns">
    <el-button :icon="Plus" type="primary" @click="open(null)" size="small">新增</el-button>
  </div>

  <el-dialog
    v-model="dialogFormVisable"
    :before-close="beforeClose"
    title="陪护师添加"
    width="500"
  >
    <el-form
      ref="formRef"
      label-width="100px"
      label-position="left"
      :model="form"
      :rules="rules"
    >
      <!-- 隐藏的表单域 -->
      <el-form-item v-show="false" prop="id">
        <el-input v-model="form.id" />
      </el-form-item>

      <el-form-item label="昵称" prop="name">
        <el-input v-model="form.name" placeholder="请输入昵称" />
      </el-form-item>

      <el-form-item label="头像" prop="avatar">
        <el-button v-if="!form.avatar" type="primary">点击上传</el-button>
        <el-image
        v-else
        :src="form.avatar"
        style="width: 100px; height: 100px"
        />
      </el-form-item>

      <el-form-item label="性别" prop="sex">
        <el-select v-model="form.sex" placeholder="请选择性别">
          <el-option label="男" value="1" />
          <el-option label="女" value="2" />
        </el-select>
      </el-form-item>

      <el-form-item label="年龄" prop="age">
        <el-input-number v-model="form.age" :min="18" :max="50" />
      </el-form-item>

      <el-form-item label="手机号" prop="mobile">
        <el-input v-model="form.mobile" placeholder="请输入手机号" />
      </el-form-item>

      <el-form-item label="是否生效" prop="active">
        <el-radio-group v-model="form.active">
          <el-radio :value="0">失效</el-radio>
          <el-radio :value="1">生效</el-radio>
        </el-radio-group>
      </el-form-item>
    </el-form>

    <template #footer>
      <div class="dialog-footer">
        <el-button type="primary" @click="confirm(formRef)">确定</el-button>
        <el-button @click="dialogFormVisible = false">取消</el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script setup>
import { ref } from 'vue'
import { Plus } from '@element-plus/icons-vue';

const dialogFormVisable = ref(false)
const beforeClose = () => {
  dialogFormVisable.value = false
}

const formRef = ref()
const form = ref({
  id: '',
  mobile: '',
  active: 1,
  age: 28,
  avatar: '',
  name: '',
  sex: '',
})
const rules = ref({

})

// 确认按钮回调(表单提交)
const confirm = async (formEl) => {
  if (!formEl) return;
  await formEl.validate((valid, fields) => {
    if (valid) {
      console.log('submit!', form.value);
      // 关闭弹窗
      dialogFormVisable.value = false
    } else {
      console.log('error submit!', fields);
      return false;
    }
  });
}

// 打开弹窗
const open = () => {
  dialogFormVisable.value = true
}
</script>

<style lang="scss" scoped>
  .btns {
    padding: 10px 0 10px 10px;
    background-color: #fff;
}
.image-list {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  .img-box {
    position: relative;
    .select {
      position: absolute;
      left: 0px;
      top: 0px;
      width: 24px;
      height: 24px;
      background-color: #67c23a;
      z-index: 999;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
  .el-image {
    margin-right: 10px;
    margin-bottom: 10px;
  }
}
</style>
```

## 27.陪护新增表单校验和接口联调

陪护师头像弹窗 + 表单校验 - src/views/vppz/staff/indexStaff.vue： 

```vue
<template>
   <div class="btns">
    <el-button :icon="Plus" type="primary" @click="open(null)" size="small">新增</el-button>
  </div>

  <el-dialog
    v-model="dialogFormVisable"
    :before-close="beforeClose"
    title="陪护师添加"
    width="500"
  >
    <el-form
      ref="formRef"
      label-width="100px"
      label-position="left"
      :model="form"
      :rules="rules"
    >
      <!-- 隐藏的表单域 -->
      <el-form-item v-show="false" prop="id">
        <el-input v-model="form.id" />
      </el-form-item>

      <el-form-item label="昵称" prop="name">
        <el-input v-model="form.name" placeholder="请输入昵称" />
      </el-form-item>

      <el-form-item label="头像" prop="avatar">
        <el-button
        v-if="!form.avatar"
        type="primary"
        @click="dialogImgVisable = true"
        >
        点击上传
      </el-button>
        <el-image
        v-else
        :src="form.avatar"
        style="width: 148px; height: 148px"
        />
      </el-form-item>

      <el-form-item label="性别" prop="sex">
        <el-select v-model="form.sex" placeholder="请选择性别">
          <el-option label="男" value="1" />
          <el-option label="女" value="2" />
        </el-select>
      </el-form-item>

      <el-form-item label="年龄" prop="age">
        <el-input-number v-model="form.age" :min="18" :max="50" />
      </el-form-item>

      <el-form-item label="手机号" prop="mobile">
        <el-input v-model="form.mobile" placeholder="请输入手机号" />
      </el-form-item>

      <el-form-item label="是否生效" prop="active">
        <el-radio-group v-model="form.active">
          <el-radio :value="0">失效</el-radio>
          <el-radio :value="1">生效</el-radio>
        </el-radio-group>
      </el-form-item>
    </el-form>

    <template #footer>
      <div class="dialog-footer">
        <el-button type="primary" @click="confirm(formRef)">确定</el-button>
        <el-button @click="dialogFormVisible = false">取消</el-button>
      </div>
    </template>
  </el-dialog>

  <el-dialog
    v-model="dialogImgVisable"
    :before-close="beforeClose"
    title="选择头像"
    width="680"
  >
    <div class="image-list">
      <div
      v-for="(item, index) in fileList"
      :key="item.id"
      class="img-box"
      @click="selectIndex = index"
      >
      <!-- 手写选中图片效果 -->
        <div v-if="selectIndex === index" class="select">
          <el-icon color="#fff"><Check /></el-icon>
        </div>
        <el-image
          :src="item.url"
          style="width: 148px; height: 148px"
        />
      </div>
    </div>
    <template #footer>
      <div class="dialog-footer">
        <el-button type="primary" @click="confirmImage">确定</el-button>
        <el-button @click="dialogImgVisable = false">取消</el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { Plus } from '@element-plus/icons-vue';
import { ElMessage } from 'element-plus'
import { photoList, companion } from '@/api/indexApi';

onMounted(() => {
  // 获取图片列表
  photoList().then(({ data }) => {
    fileList.value = data.data
    //console.log('图片列表：', data.data);
  })
})

const dialogFormVisable = ref(false)
const beforeClose = () => {
  dialogFormVisable.value = false
  form.value.resetFields()
  dialogImgVisable.value = false
}

const formRef = ref()
const form = ref({
  id: '',
  mobile: '',
  active: 1,
  age: 28,
  avatar: '',
  name: '',
  sex: '',
})
const rules = ref({
  name: [{ required: true, message: '请输入昵称', trigger: 'blur' }],
  avatar: [{ required: true, message: '请选择头像' }],
  sex: [{ required: true, message: '请选择性别', trigger: 'change' }],
  age: [{ required: true, message: '请输入年龄', trigger: 'blur' }],
  mobile: [{ required: true, message: '请输入手机号', trigger: 'blur' }]
})

// 确认按钮回调(表单提交)
const confirm = async (formEl) => {
  if (!formEl) return;
  await formEl.validate((valid, fields) => {
    if (valid) {
      //console.log('submit!', form.value)
      companion(form.value).then(({ data }) => {
        if (data.code === 10000) {
          ElMessage.success('操作成功')
          beforeClose()
          // 刷新列表,请求列表数据
          
        } else {
          ElMessage.error(data.message)
        }
      })
    } else {
      console.log('error submit!', fields);
      return false;
    }
  })
}

// 打开弹窗
const open = () => {
  dialogFormVisable.value = true
}

// 选择头像弹窗
const dialogImgVisable = ref(false)
const fileList = ref([])
// 选中头像
const selectIndex = ref(0)
const confirmImage = () => {
  form.value.avatar = fileList.value[selectIndex.value].url
  dialogImgVisable.value = false
}

</script>

<style lang="scss" scoped>
  .btns {
    padding: 10px 0 10px 10px;
    background-color: #fff;
}
.image-list {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  .img-box {
    position: relative;
    .select {
      position: absolute;
      left: 0px;
      top: 0px;
      width: 24px;
      height: 24px;
      background-color: #67c23a;
      z-index: 999;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
  .el-image {
    margin-right: 10px;
    margin-bottom: 10px;
  }
}
</style>
```

陪护师头像接口 + 陪护师创建 - src/api/indexApi.js：

```javascript
...
//陪护师头像列表
export const photoList = () => {
  return request.get('/photo/list')
}

//陪护师创建
export const companion = (data) => {
  return request.post('/companion', data)
}
```

## 28.陪护列表显示和接口联调

调陪护列表的接口 - src/api/indexApi.js：

```javascript
...
//陪护师列表
export const companionList = (params) => {
  return request.get('/companion/list', { params })
}
```

陪护列表 el-table 显示（时间戳复用见 21.） + 多选框功能 src/views/vppz/staff/indexStaff.vue： 

```vue
<template>
   <div class="btns">
    <el-button :icon="Plus" type="primary" @click="open(null)" size="small">新增</el-button>
  </div>

  <!-- 陪护师列表 -->
  <el-table
  :data="tableData.list"
  style="width: 100%"
  @selection-change="handleSelectionChange"
  >
    <!-- 多选框 -->
    <el-table-column type="selection" :selectable="selectable" width="55" />

    <el-table-column prop="id" label="id" />
    <el-table-column prop="name" label="昵称" />

    <el-table-column label="头像">
      <!-- 原来是<template #default="scope">,解构一下-->
      <template #default="{ row }">
        <el-image
        :src="row.avatar"
        style="width: 50px; height: 50px"
        />
      </template>
    </el-table-column>

    <el-table-column prop="sex" label="性别">
      <template #default="{ row }">
        {{ row.sex === 1 ? '男' : '女' }}
      </template>
    </el-table-column>

    <el-table-column prop="mobile" label="手机号" />

    <el-table-column prop="active" label="状态">
      <template #default="{ row }">
        <el-tag :type="row.active ? 'success' : 'danger'">
          {{ row.active ? '正常' : '失效' }}
        </el-tag>
      </template>
    </el-table-column>

     <el-table-column label="创建时间">
      <template #default="{ row }">
         <div class="flex-box">
            <el-icon><Clock /></el-icon>
            <span style="margin-left: 10px">{{ row.create_time }}</span>
         </div>
      </template>
    </el-table-column>

    <el-table-column label="操作">
      <template #default="{ row }">
        <el-button type="primary" @click="open(row)">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>

 <!-- 分页 -->
  <div class="pagenation-info">
    <el-pagination
      v-model:current-page="paginationData.pageNum"
      :page-size="paginationData.pageSize"
      :background="false"
      layout="total, prev, pager, next"
      :total="tableData.total"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      size="small"
      style="float: right;"
      />
  </div>
 ...
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { Plus } from '@element-plus/icons-vue';
import { ElMessage } from 'element-plus'
import { photoList, companion, companionList } from '@/api/indexApi';
import dayjs from 'dayjs'

onMounted(() => {
  // 获取图片列表
  photoList().then(({ data }) => {
    fileList.value = data.data
    //console.log('图片列表：', data.data);
  })
  // 获取列表数据
  getListData()
})

// 分页数据
const paginationData = ref({
  pageNum: 1,
  pageSize: 10,
});
// 列表数据
const tableData = ref({
  list: [],
  total: 0,
});

// 调用接口的方法
const getListData = () => {
  companionList(paginationData.value).then(({ data }) => {
    //console.log('陪护师数据：', data);
    const { list, total } = data.data;
    // 格式化时间
     list.forEach(item => {
      item.create_time = dayjs(item.created_at).format('YYYY-MM-DD HH:mm:ss');
    });
    tableData.value.list = list;
    tableData.value.total = total;
  })
}

const dialogFormVisable = ref(false)
const beforeClose = () => {
  dialogFormVisable.value = false
  formRef.value.resetFields()
  //dialogImgVisable.value = false
}

const formRef = ref()
const form = ref({
  id: '',
  mobile: '',
  active: 1,
  age: 28,
  avatar: '',
  name: '',
  sex: '',
})
const rules = ref({
  name: [{ required: true, message: '请输入昵称', trigger: 'blur' }],
  avatar: [{ required: true, message: '请选择头像' }],
  sex: [{ required: true, message: '请选择性别', trigger: 'change' }],
  age: [{ required: true, message: '请输入年龄', trigger: 'blur' }],
  mobile: [{ required: true, message: '请输入手机号', trigger: 'blur' }]
})

// 确认按钮回调(表单提交)
const confirm = async (formEl) => {
  if (!formEl) return;
  await formEl.validate((valid, fields) => {
    if (valid) {
      //console.log('submit!', form.value)
      companion(form.value).then(({ data }) => {
        if (data.code === 10000) {
          ElMessage.success('操作成功')
          beforeClose()
          // 刷新列表,请求列表数据
          getListData()
        } else {
          ElMessage.error(data.message)
        }
      })
    } else {
      console.log('error submit!', fields);
      return false;
    }
  })
}

// 打开弹窗
const open = () => {
  dialogFormVisable.value = true
}

// 选择头像弹窗
const dialogImgVisable = ref(false)
const fileList = ref([])
// 选中头像
const selectIndex = ref(0)
const confirmImage = () => {
  form.value.avatar = fileList.value[selectIndex.value].url
  dialogImgVisable.value = false
}

//改变每页条数回调
const handleSizeChange = (val) => {
  paginationData.value.pageSize = val;
  getListData();
}
//改变页码回调
const handleCurrentChange = (val) => {
  paginationData.value.pageNum = val;
  getListData();
}

// 多选框相关
const handleSelectionChange = () => {
}
</script>

<style lang="scss" scoped>
 ...
</style>
```

## 29.陪护师编辑和批量删除接口联调

陪护师删除的接口 - src/api/indexApi.js：

```javascript
...
//陪护师删除
export const deleteCompanion = (data) => {
  return request.post('/delete/companion', data)
}
```

src/views/vppz/staff/indexStaff.vue： 

 - 气泡弹窗组件 Popconfirm
 -  完善多选框相关函数逻辑
 - 完善删除相关函数逻辑
 - 添加顶部panel文字

```javascript
<template>
    <PanelHead :route="route"/>

   <div class="btns">
    <el-button :icon="Plus" type="primary" @click="open(null)" size="small">新增</el-button>
    <!-- 批量删除按钮 -->
    <el-popconfirm
      confirm-button-text="是"
      cancel-button-text="否"
      :icon="InfoFilled"
      icon-color="#626AEF"
      title="是否确认删除？"
      @confirm="confirmEvent"
    >
      <template #reference>
        <el-button :icon="Delete" type="danger" size="small">删除</el-button>
      </template>
    </el-popconfirm>
  </div>

  ...
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'
import { Plus, InfoFilled, Delete } from '@element-plus/icons-vue';
import { ElMessage } from 'element-plus'
import { photoList, companion, companionList, deleteCompanion } from '@/api/indexApi';
import dayjs from 'dayjs'
import { useRoute } from 'vue-router'

const route = useRoute()

...

const dialogFormVisable = ref(false)
const beforeClose = () => {
  dialogFormVisable.value = false
  formRef.value.resetFields()
}

...

// 确认按钮回调(表单提交)
const confirm = async (formEl) => {
  if (!formEl) return;
  await formEl.validate((valid, fields) => {
    if (valid) {
      //console.log('submit!', form.value)
      companion(form.value).then(({ data }) => {
        if (data.code === 10000) {
          ElMessage.success('操作成功')
          beforeClose()
          // 刷新列表,请求列表数据
          getListData()
        } else {
          ElMessage.error(data.message)
        }
      })
    } else {
      console.log('error submit!', fields);
      return false;
    }
  })
}

// 打开弹窗
const open = (rowData={}) => {
  dialogFormVisable.value = true
  // nextTick 保证在下次DOM更新循环结束之后执行其回调
  nextTick(() => { //点击编辑时，表单数据回显
    // 如果是编辑
    if (rowData) {
      //需要通过Object.assign进行赋值，否则表单不会更新
      Object.assign(form.value, rowData)
    }
  })
}

...

// 多选框相关
// 存储选中的数据
const selectTableData = ref([])
const handleSelectionChange = (val) => {
  // 当我们选中当前行时，需要给当前数据重新赋值
  selectTableData.value = val.map(item => ({id: item.id})) // 只需要id，是对象的结构，所以需要加上{}，不然就是数组的结构了
  //console.log('选中的数据：', selectTableData.value)
}

// 批量删除回调
const confirmEvent = () => {
  // 没有选中数据
  if (!selectTableData.value.length) {
    return ElMessage.warning('请至少选择一条数据')
  }
  deleteCompanion({ id: selectTableData.value }).then(({
    data }) => {
      if(data.code === 10000) {
        ElMessage.success('删除成功')
        getListData()
      } else {
        ElMessage.error(data.message)
      }
  })
}
</script>

<style lang="scss" scoped>
...
</style>
```

------------------------------前置数据到这里已经完成-------------------------------------

# 【C端】

## 1.项目创建引入 router 和 vant UI

### 1.命令行用 vite 创建项目 (如果用vue则同之前一样)：

#### pnpm create vite  ( vite网址：https://cn.vitejs.dev/guide/ )

然后cd pzh5：

### 2.下载 vue-router：pnpm i vue-router

### 3.下载安装 vant UI

#### （https://vant-ui.github.io/vant/#/zh-CN）：pnpm add vant





### 引入组件（按需加载）

#### 1.安装插件：pnpm add unplugin-vue-components -D

#### 2.配置插件：vite.config.js (替换原来的)

![77079941361](C:\Users\Amelia\AppData\Local\Temp\1770799413612.png)

```javascript
import vue from '@vitejs/plugin-vue';
import Components from 'unplugin-vue-components/vite';
import { VantResolver } from 'unplugin-vue-components/resolvers';

export default {
  plugins: [
    vue(),
    Components({
      resolvers: [VantResolver()],
    }),
  ],
};

```

### 4.启动项目：pnpm dev

注：如果出现端口冲突的情况可以在 vite.config.js 中的export default中配置自定义端口：

```javascript
import vue from '@vitejs/plugin-vue';
import Components from 'unplugin-vue-components/vite';
import { VantResolver } from 'unplugin-vue-components/resolvers';

export default {
  server: {
    port: 4500,
  },
  plugins: [
    vue(),
    Components({
      resolvers: [VantResolver()],
    }),
  ],
};
```

#### 5.src下新建文件夹router，其下新建index.js文件：

```javascript
import { createWebHashHistory, createRouter } from 'vue-router'

import Layout from '../pages/Main.vue'
import Home from '../pages/home/index.vue'
import Order from '../pages/order/index.vue'
import User from '../pages/user/index.vue'
import Login from '../pages/login/index.vue'
import createOrder from '../pages/createOrder/index.vue'
import detail from '../pages/detail/index.vue'

const routes = [
  { 
    path: '/',
    component: Layout,
    redirect: '/home',
    children: [
      {
        path: 'home',
        meta: { 
          icon: 'home-o',
          name: '首页'
        },
        component: Home
      },
      {
        path: 'order',
        meta: { 
          icon: 'orders-o',
          name: '订单'
        },
        component: Order
      },
      {
        path: 'user',
        meta: {
          icon: 'user-circle-o',
          name: '我的'
        },
        component: User
      }
    ]
  },
  {
    path: '/login',
    name:"login",
    component: Login
    
  },
  {
    path: '/createOrder',
    name:"createOrder",
    component: createOrder
  },
  {
    path: '/detail',
    name:"detail",
    component: detail
  },
]

export default createRouter({
  history: createWebHashHistory(),
  routes
})
```

按照路由注册对应页面：src 下新建文件夹pages，按照上面代码路径注册（用vite可以不用强制小驼峰命名）

![77080045244](C:\Users\Amelia\AppData\Local\Temp\1770800452442.png)

在其中的Main.vue中需要有路由出口：

```vue
<template>
  <router-view />
</template>
<script setup>
</script>
```

在main.js中注册router：

```javascript
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router'

const app = createApp(App)

// 路由挂载
app.use(router)

app.mount('#app')
```

app.vue 添加路由出口：

```vue
<template>
	<router-view />
</template>
<script setup>
</script>
```

## 2.tabbar引入和layout组件实现

1.引入标签栏UI组件 tabbar，实现动态底部导航条 - src/pages/Main.vue：

```vue
<template>
  <div>
    layout
  </div>
  <router-view />
  <van-tabbar v-model="active">
    <van-tabbar-item 
      v-for="item in router.options.routes[0].children" :key="item.path" 
      :icon="item.meta.icon"
      :url="`#/${item.path}`"
      >
      {{ item.meta.name }}
    </van-tabbar-item>
  </van-tabbar>

</template>
<script setup>
import { ref, onMounted } from 'vue';
// useRouter 用来获取路由实例，useRoute 用来获取当前路由对象
import { useRouter, useRoute } from 'vue-router';

const active = ref(0);

const router = useRouter();
const route = useRoute(); // 当前路由对象
onMounted(() => {
  const data = router.options.routes[0]
  // findIndex遍历数组，找到与当前路由路径匹配的项，并返回其索引，如果没有找到匹配项，则返回-1
  active.value = data.children.findIndex(item => '/' + item.path === route.path)
})
</script> 
```

# 登录页面

## 3.登录页面显示效果实现 

  1.登录页面

- 登录接口同上面 陪诊后台 一样
- style 下载 less：pnpm i less


- 移动端不需要vite自带的样式，注释掉mian.js里面的： import './style.css'

src/pages/login/index.vue：

```vue
<template>
  <h1>用户登录</h1>
  <van-form @submit="onSubmit">
     <van-cell-group inset>
       <van-field
          v-model="form.userName"
          name="用户名"
          label="用户名"
          placeholder="用户名"
          :rules="[{ required: true, message: '请填写用户名' }]"
      />
       <van-field
          v-model="form.passWord"
          type="password" 
          name="密码"
          label="密码"
          placeholder="密码"
          :rules="[{ required: true, message: '请填写密码' }]"
      />
     </van-cell-group>
     <!-- 提交按钮（按钮为回车也能提交,圆角,块级） -->
      <div class="btn">
        <van-button native-type="submit" round block type="primary">提交</van-button>
      </div>
  </van-form>
</template>

<script setup>
import { ref } from 'vue';

// 表单数据
const form = ref({
  userName: '',
  passWord: ''
})
// 表单提交
const onSubmit = (values) => {
  console.log(values);
}
</script>

<style lang="less" scoped>
h1 {  // 字体居中
  text-align: center;
}
.btn {  // 按钮外层添加间距
  margin: 16px;
}
</style>
```

## 4.axios二次封装和登录接口联调

axios： https://www.axios-http.cn/docs/intro

1. cd 到 pzh5，安装axios：pnpm add axios

2. 使用：

   新建 src/utils文件夹，其下新建 request.js ：

```javascript
import axios from 'axios'

const http = axios.create({
  // 通用请求的地址前缀
  // baseURL: 'https://wechatopen.mynatapp.cc/v3pz',
  baseURL: 'https:/v3pz.itndedu.com/v3pz',
  timeout: 100000, // 超时时间
  headers: { "terminal": "h5" } // 终端标识，h5端固定为h5
})

// 添加请求拦截器
http.interceptors.request.use(function (config) {
  // 在发送请求之前做些什么
  const token = localStorage.getItem('h5_token')
  // 不需要添加token的api
  const whiteUrl = ['/login']
  if (token && !whiteUrl.includes(config.url)) {
    config.headers['h-token'] = token
    // 临时
    // config.headers['auth'] = '13797053405'
  }
  return config;
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error);
});

// 添加响应拦截器
http.interceptors.response.use(function (response) {
  // 对响应数据做点什么

  // 对于接口返回异常的数据，给用户一点提示
  if (response.data.code === -1) {
    //ElMessage.warning(response.data.message)
  }
  if (response.data.code === -2) { // token失效
    localStorage.removeItem('h5_token')
    localStorage.removeItem('h5_userInfo')
    // window.location.origin => 获取当前根目录地址
    window.location.href = window.location.origin
  }

  return response;
}, function (error) {
  // 对响应错误做点什么
  ElMessage.error('网络异常，请检查！')
  return Promise.reject(error);
});

export default http
```

登录接口，新建 src/api 文件夹，其下新建文件 index.js：

```javascript
import request from '@/utils/request'

export default {
  login(data) {
    return request.post('/login', data)
  }
}
```

main.js 引入 api：

```javascript
import { createApp } from 'vue'
//import './style.css'
import App from './App.vue'
import router from './router'
import api from './api'

const app = createApp(App)

// 挂载api,在实例上绑定属性,这样在组件中就可以通过this.$api来访问api中的方法了
app.config.globalProperties.$api = api 

// 路由挂载
app.use(router)
app.mount('#app')
```

Login 页面使用 api 方法 - src/pages/login/index.vue：

```vue
<template>
...
</template>

<script setup>
// 获取vue实例方法:getCurrentInstance
import { ref, getCurrentInstance } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// 获取vue实例，可以通过proxy访问到全局属性和方法
const { proxy } = getCurrentInstance(); 

// 表单数据
const form = ref({
  userName: '',
  passWord: ''
})
// 表单提交
const onSubmit = async () => { 
  // 调用全局属性$api中的login方法，传入表单数据
  const { data } = await proxy.$api.login(form.value)
  //console.log(res)
  if (data.code === 10000) { 
    localStorage.setItem('h5_token', data.data.token) // 存储token
    // userInfo是一个对象，不能直接存储在localStorage中，需要先转换成字符串
    localStorage.setItem('h5_userInfo', JSON.stringify(data.data.userInfo)) // 存储用户信息
    router.push('/home') // 登录成功后跳转到主页面
  }
}
</script>

<style lang="less" scoped>
...
</style>
```

# 首页页面

## 5.首页页面效果实现

调用接口：src/api/index.js:

```javascript
import request from '../utils/request'
 
// API 接口封装写法,统一管理接口请求的文件.
// 在组件中直接引入这个文件,就可以调用里面的方法进行接口请求了
export default {
  login(data) {
    return request.post('/login', data)
  },
  // 首页数据
  index() {
    return request.get('/Index/index')
  }
}
```

src/pages/home/index.vue：

顶部箭头图标组件、搜索栏组件、轮播图组件

引入api接口数据，在 onMounted 里面给对应的数据赋值

```javascript
<template>
  <div class="header">
    <div class="header-left">
      中部地区
      <van-icon name="arrow" />
    </div>
    <!-- 顶部右侧搜索栏 -->
    <van-search
      v-model="searchValue"
      shape="round"
      placeholder="请输入搜索关键词"
      input-align="center" 
    />
  </div>
  <!-- 轮播图 -->
  <van-swipe class="my-swiper" height="170" :autoplay="3000" indicator-color="white">
    <van-swipe-item 
      v-for="item in homeData.slides" 
      :key="item.id"
      >
      <van-image
        radius="5"
        :src="item.pic_image_url"
      />
    </van-swipe-item>
  </van-swipe>
  <!-- 栅格组件实现页面中间的左右图片 -->
    <van-row justify="space-around"><!--主轴对齐-->
      <van-col 
        span="11"
        v-for="(item, index) in homeData.nav2s"
        :key="item.id"
        class="center-img"
        @click="goOrderTwo(index)"
        >
        <van-image
          :src="item.pic_image_url"
        />
      </van-col>
    </van-row>
    <!-- 栅格组件实现医院列表 -->
    <van-row 
     v-for="item in homeData.hospitals" 
     :key="item.id" 
     justify="space-around"
     class="yy-list"
     >
      <van-col span="6">
        <van-image
          width="100"
          height="90"
          :src="item.avatar_url"
        />
      </van-col>
      <van-col span="15" class="yy">
        <div class="yy-name">{{ item.name }}</div>
        <div class="yy-type">
          <span>{{ item.rank }}</span>
          &nbsp <!-- 空格 -->
          <span>{{ item.label }}</span>
        </div>
        <div class="yy-text">{{ item.intro }}</div>
      </van-col>
    </van-row>
</template>  

<script setup>
import { ref, onMounted, getCurrentInstance } from 'vue';

const searchValue = ref('');

// 获取vue实例方法:getCurrentInstance
const { proxy } = getCurrentInstance();

// 首页数据
const homeData = ref({
  hospitals: [],
  nav2s: [],
  navs: [],
  now: '',
  slides: [] // 轮播图列表，接口字段为 slides
});

// 快捷入口
const goOrderTwo = (index) => {
  
}

onMounted(async () => {
  const { data } = await proxy.$api.index()
  // 属性合并赋值
  Object.assign(homeData.value, data.data)
  //console.log(homeData.value, '轮播图数据')
})
</script>

<style lang="less" scoped>  
.header {
    display: flex;
    justify-content: space-between;
    margin: 5px;
    line-height: 54px;
    .header-left {
      padding-left: 22px;
       //太多省略了
      background: url(data:image/png;base64,iV...CC)
        no-repeat left center;
      background-size: 20px;
      font-size: 20px;
      font-weight: bold;
      color: #666666;
    }
  }
  .my-swiper {
    margin: 5px;
  }
  .yy-list {
    padding-bottom: 10px;
    margin: 20px 0;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 1px 6px 0 rgba(0, 0, 0, 0.04), 0 1px 6px 0 rgba(0, 0, 0, 0.04);
    .yy {
      .yy-name {
        font-size: 20px;
        line-height: 30px;
        font-weight: bold;
      }
      .yy-type {
        font-weight: bold;
        line-height: 25px;
        font-size: 15px;
        color: #0ca7ae;
      }
      .yy-text {
        font-size: 14px;
        color: #999999;
      }
    }
  }
  .bottom-text {
    line-height: 50px;
    text-align: center;
    color: #999999;
  }
</style>
```

## 6.订单详情自定义头部和状态显示

src/pages/home/index.vue：

完善快捷入口的逻辑

完善点击医院列表卡片跳转逻辑

```vue
<template>
 ...
    <!-- 栅格组件实现医院列表 -->
    <van-row
     @click="goOrder(item)" 
 ...
</template>  

<script setup>
import { ref, onMounted, getCurrentInstance } from 'vue';
import { useRouter } from 'vue-router';
...

const router = useRouter();
// 快捷入口
const goOrderTwo = (index) => {
  router.push(`/createOrder?id=${homeData.value.hospitals[index].id}`)
}

// 点击医院列表
const goOrder = (data) => {
  router.push(`/createOrder?id=${data.id}`)
}

...
</script>

<style lang="less" scoped>  
...
</style>
```

# 订单详情页面

src/pages/createOrder/index.vue：

```vue
<template>
  <div class="container">
    <div class="header">
      <van-icon 
        @click="goBack" 
        name="arrow-left" 
        class="header-left" 
        size="30"
      />
      填写服务订单
    </div>
    <statusBar :item="0" />
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router';
import statusBar from '../../components/statusBar.vue';

const router = useRouter();

// 返回上一页
const goBack = () => {
  router.go(-1);
}
</script>

<style lang="less" scoped>
.container {
  background-color: #f0f0f0;
  height: 100vh;
}
.header {
  padding: 10px 0;
  text-align: center;
  line-height: 30px;
  font-size: 15px;
  .header-left {
    float: left;
  }
}

.cell {
  width: 95%;
  margin: 5px auto;
  background-color: #fff;
  ::v-deep(.van-field__control) {
    color: var(--van-cell-value-color);
  }
  ::v-deep(.van-cell__title) {
	display: flex;
	align-items: center;
  }
  .server-name {
	  margin-left: 10px;
  }
}
.service-text {
  background: url(data:image/png;base64,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)
    no-repeat center center;
  background-size: 20px;
}
.sumbit {
  position: absolute;
  bottom: 0;
}
::v-deep(.van-dialog__content) {
  text-align: center;
  padding: 20px;
  .close {
    position: absolute;
    left: 20px;
  }
}
</style>
```

在 components 下新建组件-动态进度条 statusBar.vue：

把要用到的图片 h5_images 放到 public 下

```vue
<template>
    <div class="od-banner">
      <img class="od-banner-icon" src="/h5_images/od_bg_icon.png" mode="widthFix" />
      <div class="od-jd" :class="[`od-jd-${item}`]">
        <div class="od-jd-out">
          <div class="od-jd-in"></div>
        </div>
        <div class="vp-flex od-jd-text">
          <div class="vp-flex_1">
            <text class="od-jd-st-0">填写订单</text>
          </div>
          <div class="vp-flex_1">
            <text class="od-jd-st-10">在线支付</text>
          </div>
          <div class="vp-flex_1">
            <text class="od-jd-st-20">专人服务</text>
          </div>
          <div class="vp-flex_1">
            <text class="od-jd-st-30">服务完成</text>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, reactive, getCurrentInstance, onMounted } from "vue";
  const { proxy } = getCurrentInstance();
  const { item } = defineProps(["item"]);
  </script>
  
  <style lang="less" scoped>
  .vp-flex {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
  }
  .vp-flex_1 {
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    -ms-flex: 1;
    flex: 1;
    -webkit-tap-highlight-color: transparent;
  }
  .od-banner {
    overflow: hidden;
    position: relative;
    background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAyAAAAABCAIAAACpCl0xAAAABGdBTUEAAK/INwWK6QAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAHrSURBVHjadFVBcgMxCEP3Prtv6vfUrI1A2JvMtJPYGIQQgJ+/X0YgPh+uv/V1nTCI9ZORJthGaVyWrFehU9qX/cvP9d+d9Ieyf4uVqMxPRpf/jfm5SRs+vsxVzOd19Yq8gbmZrnaUJueJtrBjPHd+klJ3aNSRA8uAF0o8rnJg1ihOTizZ5tKeJttN7AHvKNYNu3jAWeJWyMVJKJ2T/9e4h+dR4vPJi4eLik2Rp4uhZOabjTNLK1IhreUzp8MFB8Mj06YaSwxU0Z8LsYQkVDfLmNWe1ZhWTHwcwcCYp0MNb9pbqt0pmkyWu4aBBdy0yLNXXorl6nbGfdqEpQydLz4oTm7N4xgFEXkUUhst68VhkpTw6YKpFnB+8CrLcZ6xNnMFuSLegU51Cm/pBkbOaLpVSND4FgbUcCihz5Igrh68dSL9LoaiNIxWqJ17W4XNjcLcspF2JbMYhLSQtvjwOmu6UZl9wtUEPX811zn1CGt1Sxv3KknFbZcZBTWqsqjDLHAkqx66V0mSUOqh2kmnsHL4oCHjy3iTWrLv1bYs1bF7xJpq20Snfw4T06EG+15qFJ3FftQIDMYlqeRjLC8czUWtbFiXsReJ+FVKYcSf67sHQl/5KGkaplpt/aiWyeK39WS2tJWL6CmUMv8XYABQPZBxCZSO0gAAAABJRU5ErkJggg==)
      repeat-y center;
    background-size: 100%;
  }
  .od-banner-icon {
    position: absolute;
    top: 15px;
    right: -10px;
    width: 65px;
    opacity: 0.6;
  }
  
  .od-jd {
    margin: 30px 20px;
  }
  .od-jd-out {
    background: #ffffff;
    border: 2.5px solid #ffffff;
    height: 10px;
    line-height: 10px;
    border-radius: 25px;
    overflow: hidden;
    position: relative;
  }
  .od-jd-in {
    height: 10px;
    line-height: 10px;
    border-radius: 25px;
    overflow: hidden;
    width: 0%;
    background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAyAAAAABCAIAAACpCl0xAAAABGdBTUEAAK/INwWK6QAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAHrSURBVHjadFVBcgMxCEP3Prtv6vfUrI1A2JvMtJPYGIQQgJ+/X0YgPh+uv/V1nTCI9ZORJthGaVyWrFehU9qX/cvP9d+d9Ieyf4uVqMxPRpf/jfm5SRs+vsxVzOd19Yq8gbmZrnaUJueJtrBjPHd+klJ3aNSRA8uAF0o8rnJg1ihOTizZ5tKeJttN7AHvKNYNu3jAWeJWyMVJKJ2T/9e4h+dR4vPJi4eLik2Rp4uhZOabjTNLK1IhreUzp8MFB8Mj06YaSwxU0Z8LsYQkVDfLmNWe1ZhWTHwcwcCYp0MNb9pbqt0pmkyWu4aBBdy0yLNXXorl6nbGfdqEpQydLz4oTm7N4xgFEXkUUhst68VhkpTw6YKpFnB+8CrLcZ6xNnMFuSLegU51Cm/pBkbOaLpVSND4FgbUcCihz5Igrh68dSL9LoaiNIxWqJ17W4XNjcLcspF2JbMYhLSQtvjwOmu6UZl9wtUEPX811zn1CGt1Sxv3KknFbZcZBTWqsqjDLHAkqx66V0mSUOqh2kmnsHL4oCHjy3iTWrLv1bYs1bF7xJpq20Snfw4T06EG+15qFJ3FftQIDMYlqeRjLC8czUWtbFiXsReJ+FVKYcSf67sHQl/5KGkaplpt/aiWyeK39WS2tJWL6CmUMv8XYABQPZBxCZSO0gAAAABJRU5ErkJggg==)
      repeat-y center;
    background-size: 100%;
  }
  .od-jd-text {
    text-align: center;
    padding-top: 15px;
  }
  .od-jd-text text {
    color: #ffffff;
    font-size: 13px;
    opacity: 0.7;
  }
  
  .od-jd-0 .od-jd-in {
    width: 12%;
  }
  .od-jd-0 .od-jd-st-0 {
    opacity: 1;
    font-weight: bold;
  }
  .od-jd-10 .od-jd-in {
    width: 37%;
  }
  .od-jd-10 .od-jd-st-10 {
    opacity: 1;
    font-weight: bold;
  }
  .od-jd-20 .od-jd-in {
    width: 64%;
  }
  .od-jd-20 .od-jd-st-20 {
    opacity: 1;
    font-weight: bold;
  }
  .od-jd-30 .od-jd-in {
    width: 100%;
  }
  .od-jd-30 .od-jd-st-30 {
    opacity: 1;
    font-weight: bold;
  }
  .od-jd-40 .od-jd-in {
    width: 100%;
    background: #999999;
  }
  </style>
```

## 7.订单详情选择医院功能

 src/pages/createOrder/index.vue：

```vue
<template>
  <div class="container">
    <div class="header">
      <van-icon 
        @click="goBack" 
        name="arrow-left" 
        class="header-left" 
        size="30"
      />
      填写服务订单
    </div>
    <statusBar :item="0" />
    <van-cell class="cell">
      <!-- 左侧内容 -->
      <template #title>
         <van-image
          width="25"
          height="25"
          :src="createInfo.service.serviceImg"
         />
         <span class="server-name">{{ createInfo.service.serviceName }}</span>
      </template>
      <!-- 右侧内容 -->
      <template #default> 
        <div class="service-text">服务内容</div>
      </template>
    </van-cell>

    <!-- form表单 -->
    <van-cell-group class="cell">
      <van-cell>
        <!-- 左侧内容 -->
        <template #title>就诊医院</template>
        <!-- 右侧下拉选择-弹出层 -->
        <template #default> 
          <div @click="showHospital = true">
            {{ form.hospital_name || '请选择就诊医院' }}
            <van-icon name="arrow" />
          </div>
        </template>
      </van-cell>
    </van-cell-group>

    <!-- 就诊医院弹出层 -->
    <van-popup
      v-model:show="showHospital"
      position="bottom"
      :style="{ height: '30%' }"
    >
    <!-- 嵌入一个下拉选择的组件DatePicker -->
      <van-picker
        :columns="showHospColumns"
        @confirm="showHospConfirm"
        @cancel="showHospital = false" 
      />
    </van-popup>

  </div>
</template>

<script setup>
import { ref, onMounted, getCurrentInstance, computed } from 'vue';
import { useRouter } from 'vue-router';
import statusBar from '../../components/statusBar.vue';

// 获取当前vue实例
const { proxy } = getCurrentInstance();

const createInfo = ref({
  companion: [],
  hospitals: [],
  service: {}
})

onMounted (async () => {
  const { data } = await proxy.$api.h5Companion()
  // console.log(data);
  Object.assign(createInfo.value, data.data)
  console.log(createInfo.value);
}) 

const router = useRouter();

// 返回上一页
const goBack = () => {
  router.go(-1);
}

// form表单数据
const form = ref({})

// 就诊医院
const showHospital = ref(false) // 弹出层显示控制
// 计算属性，用于获取可选的医院列表
const showHospColumns = computed(() => {
  // 监听createInfo.hospitals的变化，返回医院名称列表
  return createInfo.value.hospitals.map(item => {
    return { text: item.name, value: item.id }
  });
});

// 就诊医院选择确认
const showHospConfirm = (item) => {
  console.log(item, 'item')
  form.value.hospital_id = item.selectedOptions[0].value
  form.value.hospital_name = item.selectedOptions[0].text
  showHospital.value = false // 关闭弹出层
}
</script>

<style lang="less" scoped>
...
</style>
```

调用接口：src/api/index.js:

```javascript
import request from '../utils/request'
 
// API 接口封装写法,统一管理接口请求的文件.
// 在组件中直接引入这个文件,就可以调用里面的方法进行接口请求了
export default {
  login(data) {
    return request.post('/login', data)
  },
  // 首页数据
  index() {
    return request.get('/Index/index')
  },
  // 订单详情
  h5Companion() {
    return request.get('/h5/companion')
  }
}
```

## 8.订单详情选择就诊时间功能

 src/pages/createOrder/index.vue：

```vue
<template>
...
    <!-- 就诊时间弹出层 -->
    <van-popup
      v-model:show="showStartTime"
      position="bottom"
      :style="{ height: '30%' }"
    >
    <!-- 嵌入一个下拉选择的组件itemPicker -->
      <van-date-picker
        @confirm="showTimeConfirm"
        @cancel="showStartTime = false"
        title="选择日期"
        :min-date="minDate"
      />
    </van-popup>

  </div>
</template>

<script setup>
...

// 选择就诊时间
const showStartTime = ref(false)
const currentDate = ref() // 当前选择的日期
const minDate = ref(new Date()) // 最小日期为当前日期
// 就诊时间选择确认
const showTimeConfirm = (item) => {
  // console.log(item, 'item')
  // 将选择的日期格式化为字符串，存储到form表单中
  const dateStr = item.selectedValues.join('-')
  // console.log(dateStr,'data'); // 2026-03-05 data
  currentDate.value = dateStr // 更新当前选择的日期
  form.value.starttime = new Date(dateStr).getTime() // 将日期字符串转换为时间戳
  showStartTime.value = false // 关闭弹出层
}
</script>

<style lang="less" scoped>
...
</style>
```

## 9.订单详情剩余表单显示

 src/pages/createOrder/index.vue：

```vue
<template>
  <div class="container">
    <div class="header">
      <van-icon 
        @click="goBack" 
        name="arrow-left" 
        class="header-left" 
        size="30"
      />
      填写服务订单
    </div>
    <statusBar :item="0" />
    <van-cell class="cell">
      <!-- 左侧内容 -->
      <template #title>
         <van-image
          width="25"
          height="25"
          :src="createInfo.service.serviceImg"
         />
         <span class="server-name">{{ createInfo.service.serviceName }}</span>
      </template>
      <!-- 右侧内容 -->
      <template #default> 
        <div class="service-text">服务内容</div>
      </template>
    </van-cell>

    <!-- form表单 -->
    <van-cell-group class="cell">
      <van-cell>
        <!-- 左侧内容 -->
        <template #title>就诊医院</template>
        <!-- 右侧下拉选择-弹出层 -->
        <template #default> 
          <div @click="showHospital = true">
            {{ form.hospital_name || '请选择就诊医院' }}
            <van-icon name="arrow" />
          </div>
        </template>
      </van-cell>
      <!-- 就诊时间 -->
      <van-cell>
        <!-- 左侧内容 -->
        <template #title>就诊时间</template>
        <!-- 右侧下拉选择-弹出层 -->
        <template #default> 
          <div @click="showStartTime = true">
            {{ currentDate || '请选择就诊时间' }}
            <van-icon name="arrow" />
          </div>
        </template>
      </van-cell>
      
      <!-- 就诊师 -->
      <van-cell>
        <!-- 左侧内容 -->
        <template #title>就诊师</template>
        <!-- 右侧下拉选择-弹出层 -->
        <template #default> 
          <div @click="showComponion = true">
            {{ componionName || '请选择就诊师' }}
            <van-icon name="arrow" />
          </div>
        </template>
      </van-cell>

      <!-- 接送地址 -->
      <van-cell>
        <!-- 左侧内容 -->
        <template #title>接送地址</template>
        <!-- 右侧输入框 -->
        <template #default> 
          <van-field 
            class="text"
            input-align="right"
            v-model="form.receiveAddress" 
            placeholder="请输入接送地址" 
          />
        </template>
      </van-cell>

      <!-- 联系电话 -->
      <van-cell>
        <!-- 左侧内容 -->
        <template #title>联系电话</template>
        <!-- 右侧输入框 -->
        <template #default> 
          <van-field 
            class="text"
            input-align="right"
            v-model="form.tel" 
            placeholder="请输入联系电话" 
          />
        </template>
      </van-cell>
    </van-cell-group>

    <!-- 服务需求-表单域 -->
    <van-cell-group title="服务需求" class="cell">
      <van-field 
        class="text"
        style="height: 100px;"
        v-model="form.demand" 
        placeholder="请简单描述您要就诊的科室.." 
      />
    </van-cell-group>

    <!-- 提交按钮 -->
    <van-button @click="submit" class="submit" type="primary" size="large">
      提交订单
    </van-button> 

    <!-- 就诊医院弹出层 -->
    <van-popup
      v-model:show="showHospital"
      position="bottom"
      :style="{ height: '30%' }"
    >
      <!-- 嵌入一个下拉选择的组件 Picker -->
      <van-picker
        :columns="showHospColumns"
        @confirm="showHospConfirm"
        @cancel="showHospital = false" 
      />
    </van-popup>

    <!-- 就诊时间弹出层 -->
    <van-popup
      v-model:show="showStartTime"
      position="bottom"
      :style="{ height: '30%' }"
    >
      <!-- 嵌入一个下拉选择的组件itemPicker -->
      <van-date-picker
        @confirm="showTimeConfirm"
        @cancel="showStartTime = false"
        title="选择日期"
        :min-date="minDate"
      />
    </van-popup>

    <!-- 就诊师弹出层 -->
    <van-popup
      v-model:show="showComponion"
      position="bottom"
      :style="{ height: '30%' }"
    >
      <!-- 嵌入一个下拉选择的组件 Picker -->
      <van-picker
        :columns="componionColumns"
        @confirm="showComponionConfirm"
        @cancel="showComponion = false" 
      />
    </van-popup>

  </div>
</template>

<script setup>
import { ref, onMounted, getCurrentInstance, computed } from 'vue';
import { useRouter } from 'vue-router';
import statusBar from '../../components/statusBar.vue';

// 获取当前vue实例
const { proxy } = getCurrentInstance();

const createInfo = ref({
  companion: [],
  hospitals: [],
  service: {},

})

onMounted (async () => {
  const { data } = await proxy.$api.h5Companion()
  // console.log(data);
  Object.assign(createInfo.value, data.data)
  console.log(createInfo.value);
}) 

const router = useRouter();

// 返回上一页
const goBack = () => {
  router.go(-1);
}

// form表单数据
const form = ref({})

// 就诊医院
const showHospital = ref(false) // 弹出层显示控制
// 计算属性，用于获取可选的医院列表
const showHospColumns = computed(() => {
  // 监听createInfo.hospitals的变化，返回医院名称列表
  return createInfo.value.hospitals.map(item => {
    return { text: item.name, value: item.id }
  });
});



// 就诊医院选择确认
const showHospConfirm = (item) => {
  // console.log(item, 'item')
  form.value.hospital_id = item.selectedOptions[0].value
  form.value.hospital_name = item.selectedOptions[0].text
  showHospital.value = false // 关闭弹出层
}

// 选择就诊时间
const showStartTime = ref(false)
const currentDate = ref() // 当前选择的日期
const minDate = ref(new Date()) // 最小日期为当前日期
// 就诊时间选择确认
const showTimeConfirm = (item) => {
  // console.log(item, 'item')
  // 将选择的日期格式化为字符串，存储到form表单中
  const dateStr = item.selectedValues.join('-')
  // console.log(dateStr,'data'); // 2026-03-05 data
  currentDate.value = dateStr // 更新当前选择的日期
  form.value.starttime = new Date(dateStr).getTime() // 将日期字符串转换为时间戳
  showStartTime.value = false // 关闭弹出层
}

// 选择就诊师
const showComponion = ref(false)
const componionName = ref() // 当前选择的就诊师名称
const showComponionConfirm = (item) => {
  form.value.companion_id = item.selectedOptions[0].value
  componionName.value = item.selectedOptions[0].text
  showComponion.value = false // 关闭弹出层
}

const componionColumns = computed(() => {
  return createInfo.value.companion.map(item => {
    return { text: item.name, value: item.id }
  });
});

// 提交按钮
const submit = async () => {
  // console.log(form.value);
  const { data } = await proxy.$api.h5CreateOrder(form.value)
  console.log(data);
  if (data.code === 200) {
    router.push('/orderList')
  }
}
</script>

<style lang="less" scoped>
.container {
  background-color: #f0f0f0;
  height: 100vh;
}
.header {
  padding: 10px 0;
  text-align: center;
  line-height: 30px;
  font-size: 15px;
  .header-left {
    float: left;
  }
}

.cell {
  width: 95%;
  margin: 5px auto;
  background-color: #fff;
  ::v-deep(.van-field__control) {
    color: var(--van-cell-value-color);
  }
  ::v-deep(.van-cell__title) {
	display: flex;
	align-items: center;
  }
  .server-name {
	  margin-left: 10px;
  }
}
.service-text {
  background: url(data:image/png;base64,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)
    no-repeat center center;
  background-size: 20px;
}
.submit {
  position: absolute;
  bottom: 0;
}
::v-deep(.van-dialog__content) {
  text-align: center;
  padding: 20px;
  .close {
    position: absolute;
    left: 20px;
  }
}
</style>
```

## 10.订单下单和支付功能实现

  src/pages/createOrder/index.vue：

```vue
<template>
  ...

    <!-- 支付二维码弹窗 -->
     <van-dialog 
      :show-confirm-button="false" 
      v-model:show="showCode" 
      >
        <van-icon name="cross" class="close" @click="closeCode" />
        <div>微信支付</div>
        <van-image width="150" height="150" :src="codeImg" />
        <div>请使用本人微信扫描二维码支付</div>
     </van-dialog>

  </div>
</template>

<script setup>
import { ref, onMounted, getCurrentInstance, computed } from 'vue';
import { useRouter } from 'vue-router';
import statusBar from '../../components/statusBar.vue';
import { showNotify } from 'vant';
import Qrcode from 'qrcode';
    
...

// 支付弹窗
const showCode = ref(false)
const codeImg = ref('')
// 关闭支付弹窗
const closeCode = () => {
  showCode.value = false
  router.push('/order') // 跳转订单列表页
}

// 提交按钮
const submit = async () => {
  const params = [
    'hospital_id',
    'hospital_name',
    'demand',
    'companion_id',
    'receiveAddress',
    'tel',
    'starttime',
  ]
  for (const i of params) {
    if (!form.value[i]) {
      showNotify({ type: 'danger', message: '请填写完整信息' });
      return;
    }
  }
  const { data: orderRes } = await proxy.$api.createOrder(form.value)
  console.log(orderRes.data.wx_code)
  Qrcode.toDataURL(orderRes.data.wx_code).then((url) => {
    showCode.value = true // 显示支付弹窗
    codeImg.value = url // 将生成的二维码图片URL赋值给codeImg
  })
}
</script>

<style lang="less" scoped>
...
</style>
```

src/api/index.js:

```javascript
...
  // 订单提交
  createOrder(data) {
    return request.post('/createOrder', data)
  }
}
```

!!!![77293380704](C:\Users\Amelia\AppData\Local\Temp\1772933807040.png)

提交订单时，网络响应消息：https://v3pz.itndedu.com/v3pz/createOrder

这是生成的微信二维码用于支付，需要一个插件能把这一串转成code二维码：

- 在终端cmd：cd pzh5
- 下载插件：pnpm i qrcode@latest
- 在当前需要使用的页面引入：import Qrcode from 'qrcode'
- 在对应函数中调用：

```js
Qrcode.toDataURL(orderRes.data.wx_code).then((url) => {
    showCode.value = true // 显示支付弹窗
    codeImg.value = url // 将生成的二维码图片URL赋值给codeImg
  })
```

# 订单列表页面

## 11.订单列表显示效果实现

src/pages/order/index.vue：

1. 定义接口
2. 在相关页面利用  getCurrentInstance 获取实例
3.  封装函数
4. 拿到数据，渲染页面
5. 倒计时组件

```vue
<template>
  <div class="container">
    <!-- 导航栏 -->
    <div class="header">我的订单</div>
    <van-tabs v-model:active="active" @click-tab="onClickTab">
      <van-tab title="全部" name="" />
      <van-tab title="待支付" name="1" />
      <van-tab title="待服务" name="2" />
      <van-tab title="已完成" name="3" />
      <van-tab title="已取消" name="4" />
    </van-tabs>

    <!-- 订单列表 -->
    <van-row @click="goDetail(item)" v-for="item in order" :key="item.out_trade_no">
      <!-- 左侧图片 -->
      <van-col span="5">
        <van-image 
          width="50" height="50" 
          :src="item.serviceImg" 
          radius="5" 
          />
      </van-col>
      <!-- 中间内容 -->
      <van-col span="14">
        <div class="text1">
          {{ item.service_name }}
        </div>
        <div class="text2">{{ item.hospital_name }}</div>
        <div class="text2">预约时间：{{ item.starttime }}</div>
      </van-col>
      <!-- 右侧订单状态 -->
      <van-col span="5" class="text2" :style="{color: colorMap[item.trade_state]}">
        {{ item.trade_state }}
        <counter :second="item.timer" v-if="item.trade_state === '待支付'" />
      </van-col>
    </van-row>
    <!-- 底部提示 -->
    <div class="bottom-text">没有更多订单了</div>
  </div>
</template>

<script setup>
import { ref, getCurrentInstance, onMounted } from 'vue'
import counter from '../../components/counter.vue'

// 订单列表
const order = ref([])

// 订单状态对应的颜色
const colorMap = {
  '待支付': '#ffa200',
  '待服务': '#1da6fd',
  '已完成': '#21c521',
  '已取消': '#999999'
}

onMounted(() => {
  getOrderList('') // 首次进来就获取订单列表
})

// 获取当前vue实例
const { proxy } = getCurrentInstance();

// 获取订单列表
const getOrderList = async (state) => {
  const { data } = await proxy.$api.orderList({ state })
  console.log(data)
  data.data.forEach(item => { // 计算订单剩余时间
    // 剩余时间 =  2小时-（当前时间-订单创建时间 ）（单位：毫秒）
    item.timer = 7200000 - (new Date() - item.order_start_time)

  });
  order.value = data.data
}
const onClickTab = (item) => {
  getOrderList(item.name)
}

const active = ref("")

// 跳转到订单详情页
const goDetail = (item) => {
}
</script>

<style lang="less" scoped>
.container {
    background-color: #f0f0f0;
    height: 100vh;
  }
  .header {
    background-color: #fff;
    line-height: 40px;
    text-align: center;
  }
  .van-row {
    background-color: #fff;
    padding: 10px;
    margin: 5px;
    border-radius: 5px;
    .text1 {
      font-size: 16px;
      line-height: 25px;
      font-weight: bold;
    }
    .text2 {
      font-size: 14px;
      line-height: 20px;
      color: #999999;
    }
  }
  .bottom-text {
    line-height: 50px;
    text-align: center;
    color: #999999;
  }
</style>
```

列表接口：src/api/index.js:

```javascript
...
  // 订单列表
  orderList(params) {
    return request.get('/order/list', { params })
  },
}
```

在components下新建倒计时组件counter.vue：

```vue
<template>
    {{ formater }}
  </template>
  
  <script setup>
  import { ref, onMounted, defineProps, defineEmits, watch } from "vue";
  const props = defineProps({
    second: {
      type: Number,
      default: 0,
    },
    format: {
      type: String,
      default: "MM-dd hh:mm",
    },
    sformat: {
      type: String,
      default: "hh:mm:ss",
    },
    suffix: {
      type: String,
      default: "",
    },
  });
  const emit = defineEmits(["counterOver"]);
  // 倒计时显示
  const formater = ref("");
  
  onMounted(() => {
    formater.value = TIME_FORMAT(props.second);
  });
  // 倒计时逻辑处理
  const TIME_FORMAT = (ts) => {
    let res;
  
    const showtime = () => {
      if (ts <= 0) {
        clearInterval(run);
        emit("counterOver");
        return TIME_SFORMAT(0, props.sformat, props.suffix);
      }
      res = TIME_SFORMAT(ts, props.sformat, props.suffix);
      return res;
    };
    const run = setInterval(() => {
      ts -= 1000;
      res = showtime();
      formater.value = res;
    }, 1000);
    return showtime();
  };
  const TIME_FORMIN = (param) => {
    if (param < 0) {
      param = 0;
    }
    if (param < 10) {
      param = "0" + param;
    }
    return param;
  };
  
  const TIME_SFORMAT = (ts, sfmt, suffix) => {
    const time = {
      "h+": TIME_FORMIN(Math.floor((ts / 1000 / 60 / 60) % 24)),
      "m+": TIME_FORMIN(Math.floor((ts / 1000 / 60) % 60)),
      "s+": TIME_FORMIN(Math.floor((ts / 1000) % 60)),
    };
    for (let k in time) {
      if (new RegExp("(" + k + ")").test(sfmt)) {
        sfmt = sfmt.replace(
          RegExp.$1,
          RegExp.$1.length == 1
            ? time[k]
            : ("00" + time[k]).substr(("" + time[k]).length)
        );
      }
    }
    return sfmt + suffix;
  };
  </script>
  
  <style></style>
 
```

在原有基础上加了一个加载界面，如下完整代码：

src/pages/order/index.vue：

```vue
<template>
  <div class="container">
    <!-- 导航栏 -->
    <div class="header">我的订单</div>
    <van-tabs v-model:active="active" @click-tab="onClickTab">
      <van-tab title="全部" name="" />
      <van-tab title="待支付" name="1" />
      <van-tab title="待服务" name="2" />
      <van-tab title="已完成" name="3" />
      <van-tab title="已取消" name="4" />
    </van-tabs>

    <!-- 订单列表 -->
    <van-loading v-if="loading" class="loading" size="24px">加载中...</van-loading>
    <van-row v-else @click="goDetail(item)" v-for="item in order" :key="item.out_trade_no">
      <!-- 左侧图片 -->
      <van-col span="5">
        <van-image 
          width="50" height="50" 
          :src="item.serviceImg" 
          radius="5" 
          />
      </van-col>
      <!-- 中间内容 -->
      <van-col span="14">
        <div class="text1">
          {{ item.service_name }}
        </div>
        <div class="text2">{{ item.hospital_name }}</div>
        <div class="text2">预约时间：{{ item.starttime }}</div>
      </van-col>
      <!-- 右侧订单状态 -->
      <van-col span="5" class="text2" :style="{color: colorMap[item.trade_state]}">
        {{ item.trade_state }}
        <counter :second="item.timer" v-if="item.trade_state === '待支付'" />
      </van-col>
    </van-row>
    <!-- 底部提示 -->
    <div v-if="!loading && order.length === 0" class="bottom-text">没有更多订单了</div>
  </div>
</template>

<script setup>
import { ref, getCurrentInstance, onMounted } from 'vue'
import counter from '../../components/counter.vue'
import { useRouter } from 'vue-router'

// 订单列表
const order = ref([])
// 加载状态
const loading = ref(false)

// 订单状态对应的颜色
const colorMap = {
  '待支付': '#ffa200',
  '待服务': '#1da6fd',
  '已完成': '#21c521',
  '已取消': '#999999'
}

onMounted(() => {
  getOrderList('') // 首次进来就获取订单列表
})

// 获取当前vue实例
const { proxy } = getCurrentInstance();

// 获取订单列表
const getOrderList = async (state) => {
  loading.value = true
  try {
    const { data } = await proxy.$api.orderList({ state })
    console.log(data)
    data.data.forEach(item => { // 计算订单剩余时间
      // 剩余时间 =  2小时-（当前时间-订单创建时间 ）（单位：毫秒）
      item.timer = 7200000 - (new Date() - item.order_start_time)
    });
    order.value = data.data
  } catch (error) {
    console.error('获取订单列表失败:', error)
  } finally {
    loading.value = false
  }
}
const onClickTab = (item) => {
  getOrderList(item.name)
}

const active = ref("")

const router = useRouter()
// 跳转到订单详情页
const goDetail = (item) => {
  // 订单号作为参数传递
  router.push(`/detail?oid=${item.out_trade_no}`) 
}
</script>

<style lang="less" scoped>
.container {
    background-color: #f0f0f0;
    height: 100vh;
  }
  .header {
    background-color: #fff;
    line-height: 40px;
    text-align: center;
  }
  .van-row {
    background-color: #fff;
    padding: 10px;
    margin: 5px;
    border-radius: 5px;
    .text1 {
      font-size: 16px;
      line-height: 25px;
      font-weight: bold;
    }
    .text2 {
      font-size: 14px;
      line-height: 20px;
      color: #999999;
    }
  }
  .bottom-text {
    line-height: 50px;
    text-align: center;
    color: #999999;
  }
  .loading {
    text-align: center;
    padding: 20px;
  }
</style>
```



# 订单详情页面

## 12.订单详情显示效果（上）

src/pages/order/index.vue：

完善跳转逻辑 goDetail 函数

```vue
<template>
...

    <!-- 订单列表 -->
    <van-row @click="goDetail(item)" v-for="item in order" :key="item.out_trade_no">
     ...
</template>

<script setup>
import { ref, getCurrentInstance, onMounted } from 'vue'
import counter from '../../components/counter.vue'
import { useRouter } from 'vue-router'
...

const router = useRouter()
// 跳转到订单详情页
const goDetail = (item) => {
  // 订单号作为参数传递
  router.push(`/detail?oid=${item.out_trade_no}`) 
}
</script>

<style lang="less" scoped>
...
</style>
```

src/pages/detail/index.vue：

- style：c_detail.css

```vue
<template>
  <div class="container">
    <div class="header">
      <van-icon @click="goBack" name="arrow-left" class="header-left" size="30" />
      订单详情
    </div>
    <statusBar :item="stateMap[detailData.trade_state]" />
    <div class="tips">
      <div class="dzf" v-if="detailData.trade_state === '待支付'">
        <div class="text1">订单待支付</div>
        <div class="text2">
          请在 <counter :second="second" /> 内完成支付，逾期订单将自动取消
        </div>
        <div class="text3">
          <van-button @click="showCode = true" type="success">立即支付(0.5元)</van-button>
        </div>
      </div>
    </div>
    <!-- 支付二维码弹窗 -->
     <van-dialog 
      :show-confirm-button="false" 
      v-model:show="showCode" 
      >
        <van-icon name="cross" class="close" @click="closeCode" />
        <div>微信支付</div>
        <van-image width="150" height="150" :src="codeImg" />
        <div>请使用本人微信扫描二维码支付</div>
     </van-dialog>
  </div>
</template>

<script setup>
import { useRouter, useRoute } from 'vue-router'
import statusBar from '../../components/statusBar.vue'
import { ref, onMounted, getCurrentInstance, computed } from 'vue';
import counter from '../../components/counter.vue'; // 导入倒计时组件
import Qrcode from 'qrcode'; // 导入qrcode库，用于生成二维码图片

const router = useRouter();
const route = useRoute();

// 获取当前vue实例
const { proxy } = getCurrentInstance();

// 详情页数据
const detailData = ref({})

// 计算属性 - 计算倒计时
const second = computed(() => {
  return detailData.value.order_start_time ? 7200000 - (new Date() - detailData.value.order_start_time) : 0
})

// 支付二维码相关
const showCode = ref(false) // 是否显示二维码弹窗
const codeImg = ref('') // 二维码图片地址
const closeCode = () => {// 关闭支付弹窗
  showCode.value = false
  router.push('/order') // 跳转订单列表页
}

// 订单状态
const stateMap = {
  '待支付': 10,
  '待服务': 20,
  '已完成': 30,
  '已取消': 40
}

onMounted( async () => {
  const { data } = await proxy.$api.orderDetail({ oid: route.query.oid })
  Object.assign(detailData.value, data.data)
   // 如果订单状态为待支付，则生成支付二维码
  Qrcode.toDataURL(data.data.wx_code).then((url) => {
    showCode.value = true // 显示支付弹窗
    codeImg.value = url // 将生成的二维码图片URL赋值给codeImg
  })
})

// 返回上一页
const goBack = () => {
  router.go(-1);
}
</script>

<style scoped>
.container {
    background-color: #f0f0f0;
    height: 100vh;
  }
  .header {
    background-color: #fff;
    line-height: 40px;
    text-align: center;
    .header-left {
      float: left;
    }
  }
  .card {
    margin: 15px 0;
    padding: 10px;
    .header-text {
      padding-left: 5px;
      line-height: 30px;
      font-size: 16px;
      font-weight: bold;
      border-left: 4px solid red;
    }
  }
  .dzf {
    padding: 20px;
    .text1 {
      font-size: 20px;
      font-weight: bold;
      line-height: 30px;
      color: #666;
    }
    .text2 {
      font-size: 14px;
      color: #666;
    }
    .text3 {
      text-align: center;
      .van-button {
        margin-top: 10px;
        margin-left: 10px;
        width: 80%;
        font-weight: bold;
      }
    }
  }
  ::v-deep(.van-dialog__content) {
    text-align: center;
    padding: 20px;
    .close {
      position: absolute;
      left: 20px;
    }
  }
</style>
```

订单详情接口：src/api/index.js:

```javascript
...
// 订单详情
  orderDetail(params) {
    return request.get('/order/detail', { params })
  },
```

## 13.订单详情显示效果（下）

src/pages/detail/index.vue：

- 渲染逻辑：通过对象（makeInfo、orderInfo）的数据，判断：如果是获取就诊人的话，则看数据中有无 client.name 这种带 “.” 的符号，如果有，则获取到就诊人数据，其他则直接读取后端返回对象的数据的具体的值就可，时间戳需要做额外处理
- 新增加载逻辑（自编）和进页面就加载

```vue
<template>
 ...
      <!-- 待服务 -->
      <div class="dzf" v-if="detailData.trade_state === '待服务'">
        <div class="text1">正在为您安排服务专员...</div>
        <div class="text2">请保持手机畅通，稍后会有服务专员与您联系</div>
      </div>
      <!-- 已完成 -->
      <div class="dzf" v-if="detailData.trade_state === '已完成'">
        <div class="text1">服务已完成</div>
        <div class="text2">感谢您的使用，如有售后问题请联系客服</div>
      </div>
      <!-- 已取消 -->
       <div class="dzf" v-if="detailData.trade_state === '已取消'">
        <div class="text1">订单已取消</div>
        <div class="text2">期待下次为您服务，如需帮助可咨询客服</div>
      </div>
    </div>

    <!-- 预约信息 -->
    <van-cell-group v-if="!loading" class="card">
      <div class="header-text">预约信息</div>
      <van-cell 
        v-for="(item, key) in makeInfo" 
        :key="key" 
        :title="item" 
        :value="formatData(key)" 
      />
    </van-cell-group>
    <!-- 订单信息 -->
    <van-cell-group v-if="!loading" class="card">
      <div class="header-text">订单信息</div>
      <van-cell 
        v-for="(item, key) in orderInfo" 
        :key="key" 
        :title="item" 
        :value="formatData(key)" 
      />
    </van-cell-group>

    <!-- 加载中 -->
    <van-loading v-else class="loading" size="24px">加载中...</van-loading>

    <!-- 支付二维码弹窗 -->
     ...
  </div>
</template>

<script setup>
import { useRouter, useRoute } from 'vue-router'
import statusBar from '../../components/statusBar.vue'
import { ref, onMounted, getCurrentInstance, computed } from 'vue';
import counter from '../../components/counter.vue'; // 导入倒计时组件
import Qrcode from 'qrcode'; // 导入qrcode库，用于生成二维码图片

const router = useRouter();
const route = useRoute();

// 获取当前vue实例
const { proxy } = getCurrentInstance();

// 详情页数据
const detailData = ref({})
// 加载状态
const loading = ref(true)

// 计算属性 - 计算倒计时
const second = computed(() => {
  return detailData.value.order_start_time ? 7200000 - (new Date() - detailData.value.order_start_time) : 0
})

// 支付二维码相关
const showCode = ref(false) // 是否显示二维码弹窗
const codeImg = ref('') // 二维码图片地址
const closeCode = () => {// 关闭支付弹窗
  showCode.value = false
  router.push('/order') // 跳转订单列表页
}

// 订单状态
const stateMap = {
  '待支付': 10,
  '待服务': 20,
  '已完成': 30,
  '已取消': 40
}

// 获取订单详情（上半部分内容）
const makeInfo = {
  service_name: '预约服务',
  hospital_name: '就诊医院',
  starttime: '期望就诊时间',
  'client.name': '就诊人',
  'client.mobile': '就诊人电话',
  receiveAddress: '接送地址',
  demand: '其他需求'
}
// 获取订单详情（下半部分内容）
const orderInfo = {
  tel: '联系人电话',
  order_start_time: '下单时间',
  price: '应付金额',
  out_trade_no: '订单编号'
}

// 时间戳格式化函数
const formatData = (key) => {
  if (key.indexOf(".") === -1) { // 如果key中没有点，说明是一级属性
    if (key === "order_start_time") {
      return formatTimestamp(detailData.value[key]);// 对订单创建时间进行格式化
    }
    return detailData.value[key];// 直接返回一级属性的值
  }
  let arr = key.split(".").reduce((o, p) => {// 如果key中有点，说明是嵌套属性，需要通过reduce逐层访问
    return (o || {})[p];// 访问嵌套属性，如果某层属性不存在，则返回undefined，避免报错
  }, detailData.value);// 从detailData开始访问，逐层访问到最终属性
  return arr;
}
function formatTimestamp(timestamp) {
  const date = new Date(timestamp);
  const year = date.getFullYear();
  const month = String(date.getMonth() + 1).padStart(2, "0"); // 月份是从0开始的，所以需要+1
  const day = String(date.getDate()).padStart(2, "0");

  return `${year}-${month}-${day}`;
}

onMounted( async () => {
  loading.value = true
  try {
    const { data } = await proxy.$api.orderDetail({ oid: route.query.oid })
    Object.assign(detailData.value, data.data)
     // 如果订单状态为待支付，则生成支付二维码
    Qrcode.toDataURL(data.data.code_url).then((url) => {
      codeImg.value = url // 将生成的二维码图片URL赋值给codeImg
    })
  } catch (error) {
    console.error('获取订单详情失败:', error)
  } finally {
    loading.value = false
  }
})

// 返回上一页
const goBack = () => {
  router.go(-1);
}
</script>

<style scoped>
...
</style>
```

# 我的 页面

## 14.我的页面显示效果

src/pages/user/index.vue：

style：c_user.css

```vue
<template>
  <div class="container">
    <!-- 用户图片＋用户名 -->
    <div class="user">
      <van-image class="img" width="100" height="100" :src="userInfo.avatar" />
      <div class="text">{{ userInfo.name }}</div>
    </div>

    <!-- 我的订单 -->
    <div class="order">
      <!-- 顶部 -->
      <div class="top">
        <div class="text1">我的订单</div>
        <div class="text2">全部</div>
      </div>
      <!-- 图标1 -->
      <div class="buttom">
        <div class="item">
          <van-image
            @click="goOrder(1)"
            width="40"
            height="40"
            src="/h5_images/od_10.png"
          />
          <div>待支付</div>
        </div>
        <!-- 图标2 -->
        <div class="item" @click="goOrder(2)">
          <van-image width="40" height="40" src="/h5_images/od_20.png" />
          <div>待服务</div>
        </div>
        <!-- 图标3 -->
        <div class="item"@click="goOrder(3)">
          <van-image width="40" height="40" src="/h5_images/od_30.png" />
          <div>已完成</div>
        </div>
        <!-- 图标4 -->
        <div class="item" @click="goOrder(4)">
          <van-image width="40" height="40" src="/h5_images/od_40.png"/>
          <div>已取消</div>
        </div>
      </div>

      <!-- 底部 -->
      <div class="foot">
        <div class="foot1">
          <div class="text1">
            <van-image width="20" height="20" src="/h5_images/ic_clients.png" />
            服务对象管理
          </div>
          <div class="text2"><van-icon name="arrow"/></div>       
        </div>
        <div class="foot2">
          <div class="text1">
            <van-image width="20" height="20" src="/h5_images/ic_share.png" />
            分享转发
          </div>
          <div class="text2"><van-icon name="arrow"/></div>  
        </div>
      </div>
    </div>

    <!-- 退出按钮 -->
    <van-button @click="show = true" type="danger" class="quit" size="large">退出登录</van-button>
      <!-- 退出弹窗 -->
      <van-dialog 
        v-model:show="show"
        title="提示"
        @cancel="show = false"
        @confirm="logout"
        show-cancel-button
      >
        <div class="quit_text">是否确认退出登录</div>
      </van-dialog>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const userInfo = computed(() => {
  return JSON.parse(localStorage.getItem('h5_userInfo')) || {}
});

// 跳转订单列表页
const goOrder = (active) => {
  router.push(`/order?active=${active}`)
}

// 控制退出弹窗的显示隐藏
const show = ref(false);
// 退出登录方法
const logout = () => {
  
}
</script>

<style lang="less" scoped>
.container {
    background-color: #f0f0f0;
    height: 100vh;
    overflow: hidden;
    .user {
      width: 95%;
      height: 200px;
      background-color: #fff;
      text-align: center;
      border-radius: 10px;
      margin: 10px;
      .img {
        margin-top: 30px;
      }
      .text {
        line-height: 30px;
        font-weight: bold;
      }
    }
    .order {
      width: 90%;
      margin: 10px;
      border-radius: 5px;
      background-color: #fff;
      padding: 10px;
      .top {
        margin: 10px;
        line-height: 50px;
        display: flex;
        justify-content: space-between;
        .text1 {
          color: #333;
        }
        .text2 {
          color: #999;
        }
        border-bottom: 0.5px solid #f5f5f5;
      }
      .buttom {
        padding: 10px;
        display: flex;
        justify-content: space-around;
        .item {
          font-size: 14px;
          color: #999;
        }
      }
    }
    .foot {
      margin: 0 10px;
      padding: 10px;
      line-height: 50px;
      background-color: #fff;
      .foot1,
      .foot2 {
        display: flex;
        justify-content: space-between;
        color: #555;
        align-items: center;
      }
      .foot1 {
        border-bottom: 0.5px solid #f5f5f5;
      }
      .van-image {
        vertical-align: middle;
        margin-right: 5px;
      }
    }
    .quit {
      width: 90%;
      margin: 20px;
    }
    .quit_text {
      margin: 20px 0;
      text-align: center;
    }
  }  
</style>
```

user的头像信息在本地存储的h5_userInfo中，读取：

- 引入 ref、computed，创建userInfo，通过计算属性监听变化
- 通过JSON把对象转变，得到 h5_userInfo

![77373084581](C:\Users\Amelia\AppData\Local\Temp\1773730845818.png)

## 15.登出逻辑补充和订单列表页实现效果

登出逻辑补充 - src/pages/user/index.vue：

```javascript
...
const logout = () => {
  // 清除本地存储
  localStorage.removeItem('h5_token')
  localStorage.removeItem('h5_userInfo')
  // 跳转到登录页
  router.push('/login')
}
...
```

# 【admin端】- 订单列表页实现效果

## 订单管理页面

请求接口：src/api/indexApi.js：

```javascript
...
//订单列表
export const adminOrder = (params) => {
  return request.get('/admin/order', { params })
}
```

src/views/vppz/order/indexOrder.vue：

- 当屏幕比较小的时候，做一个定位滚动的效果

1. 要给一些看起来比较挤的列给一个具体的宽度，使其能在一行显示，不会分多行
2. 不让内容压缩，所以要对两边内容进行一个固定
   - 固定在右侧，则在属性加上 fixed="right"，左侧 fixed="left"

- 在 el-form 组件中要控制 el-form-item们 在一行显示可以在el-form中设置属性：inline=“true”

```vue
<template>
  <PanelHead :route="route" />

  <!-- 搜索栏 -->
  <div class="form">
    <el-form inline="true" :model="searchForm">
      <el-form-item prop="out_trade_no">
        <el-input v-model="searchForm.out_trade_no" placeholder="订单号" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>
  </div>

  <el-table :data="tableData.list">
    <el-table-column prop="out_trade_no" label="订单号" width="150" fixed="left"></el-table-column>
    <el-table-column prop="hospital_name" label="就诊医院"></el-table-column>
    <el-table-column prop="service_name" label="陪诊服务"></el-table-column>
    <el-table-column label="陪护师">
      <template #default="scope"> <!--scope 是插槽参数，包含当前行数据（scope.row）-->
        <el-image style="width: 40px;height: 40px;" :src="scope.row.companion.avatar" />
      </template>
    </el-table-column>
    <el-table-column label="陪护师手机号" width="120">
      <template #default="scope"> <!--scope 是插槽参数，包含当前行数据（scope.row）-->
        {{ scope.row.companion.mobile }}
      </template>
    </el-table-column>
    <el-table-column prop="price" label="总价"></el-table-column>
    <el-table-column prop="paidPrice" label="已付"></el-table-column>
    <el-table-column label="下单时间" width="120">
      <template #default="scope"> <!--dayjs处理时间戳YYYY-MM-DD HH:mm:ss-->
        {{ dayjs(scope.row.order_start_time).format('YYYY-MM-DD')}}
      </template>
    </el-table-column>
    <el-table-column label="订单状态">
      <template #default="scope">
        <el-tag :type="statusMap(scope.row.trade_state)">
          {{ scope.row.trade_state }}
        </el-tag>
      </template>
    </el-table-column>
    <el-table-column prop="service_state" label="接单状态"></el-table-column>
    <el-table-column prop="tel" label="联系人手机号" width="120"></el-table-column>
    <el-table-column prop="create_time" label="操作" width="100" fixed="right">
      <template #default="scope">
        <!-- 只有已经支付之后，待服务的订单才显示“服务完成”，其他都是置灰+暂无服务 -->
        <!-- 点击服务完成，弹出气泡弹窗 -->
         <el-popconfirm
         v-if="scope.row.trade_state === '待服务'"
          confirm-button-text="是"
          cancel-button-text="否"
          :icon="InfoFilled"
          icon-color="#626AEF"
          title="是否确认完成？"
          @confirm="confirmEvent"
        >
          <template #reference><!--文字类型button按钮用 link -->
            <el-button type="primary" link>服务完成</el-button>
          </template>
        </el-popconfirm><!--暂无服务且置灰 -->
        <el-button v-else type="primary" link disabled>暂无服务</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- 分页 -->
  <div class="pagenation-info">
    <el-pagination
      v-model:current-page="paginationData.pageNum"
      :page-size="paginationData.pageSize"
      :background="false"
      layout="total, prev, pager, next"
      :total="tableData.total"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      size="small"
      style="float: right;"
      />
  </div>
</template>

<script setup>
import { adminOrder } from '@/api/indexApi';
import { onMounted,ref } from 'vue';
import dayjs from 'dayjs';
import { useRoute } from 'vue-router';
import { InfoFilled } from '@element-plus/icons-vue';


const route = useRoute()

onMounted(() => {
  getListData()
})

const paginationData = ref({
  pageNum: 1,
  pageSize: 10,
})

// 搜索表单
const searchForm = ref({
  out_trade_no: '',
})
// 搜索按钮点击事件
const onSubmit = () => {
  // console.log('搜索表单：', searchForm.value);
  getListData()
}

// 列表数据
const tableData = ref({
  list: [],
  total: 0
})

// 分页相关
//改变每页条数回调
const handleSizeChange = (val) => {
  paginationData.value.pageSize = val;
  getListData();
}
//改变页码回调
const handleCurrentChange = (val) => {
  paginationData.value.pageNum = val;
  getListData();
}

// 调用接口的方法
const getListData = () => {
  adminOrder(paginationData.value).then(({ data }) => {
    //console.log('陪护师数据：', data);
    const { list, total } = data.data;
    // 格式化时间
     list.forEach(item => {
      item.create_time = dayjs(item.created_at).format('YYYY-MM-DD HH:mm:ss');
    });
    tableData.value.list = list;
    tableData.value.total = total;
  })
}

// 状态
const statusMap = (key) => {
  const obj = {
     '待支付': 'warning',
     '已取消': 'info',
     '已完成': 'success',
  }
  return obj[key]
}

// 服务完成确认
const confirmEvent = () => {
  // console.log('服务完成确认');
}

</script>

<style lang="scss" scoped>
.form {
  display: flex;
  justify-content: flex-end;
  padding: 10px 0 10px 10px;
  background-color: #fff;
}
</style>
```

## 订单列表服务状态扭转和问题解决

调用接口 ：src/api/indexApi.js：

```javascript
...
//服务状态完成
export const updateOrder = (data) => {
  return request.post('/update/order', data)
}
```

src/views/vppz/order/indexOrder.vue：

- ...paginationData, ...params ：合并多个对象

  你原来的代码：

```js
adminOrder({ ...paginationData, ...params })
```

​	这里错了！

​	`paginationData` 是 `ref` 对象

​	直接解构，传过去的不是数字，后端就会报：

​	`pageNum should be a number;`

- 正确写法必须加 .value

```
adminOrder({
  ...paginationData.value, // 这里必须 .value！
  ...params
})
```

```vue
<template>
  <PanelHead :route="route" />

  <!-- 搜索栏 -->
  <div class="form">
    <el-form inline="true" :model="searchForm">
      <el-form-item prop="out_trade_no">
        <el-input v-model="searchForm.out_trade_no" placeholder="订单号" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>
  </div>

  <el-table :data="tableData.list">
    <el-table-column prop="out_trade_no" label="订单号" width="150" fixed="left"></el-table-column>
    <el-table-column prop="hospital_name" label="就诊医院"></el-table-column>
    <el-table-column prop="service_name" label="陪诊服务"></el-table-column>
    <el-table-column label="陪护师">
      <template #default="scope"> <!--scope 是插槽参数，包含当前行数据（scope.row）-->
        <el-image style="width: 40px;height: 40px;" :src="scope.row.companion.avatar" />
      </template>
    </el-table-column>
    <el-table-column label="陪护师手机号" width="120">
      <template #default="scope"> <!--scope 是插槽参数，包含当前行数据（scope.row）-->
        {{ scope.row.companion.mobile }}
      </template>
    </el-table-column>
    <el-table-column prop="price" label="总价"></el-table-column>
    <el-table-column prop="paidPrice" label="已付"></el-table-column>
    <el-table-column label="下单时间" width="120">
      <template #default="scope"> <!--dayjs处理时间戳YYYY-MM-DD HH:mm:ss-->
        {{ dayjs(scope.row.order_start_time).format('YYYY-MM-DD')}}
      </template>
    </el-table-column>
    <el-table-column label="订单状态">
      <template #default="scope">
        <el-tag :type="statusMap(scope.row.trade_state)">
          {{ scope.row.trade_state }}
        </el-tag>
      </template>
    </el-table-column>
    <el-table-column prop="service_state" label="接单状态"></el-table-column>
    <el-table-column prop="tel" label="联系人手机号" width="120"></el-table-column>
    <el-table-column prop="create_time" label="操作" width="100" fixed="right">
      <template #default="scope">
        <!-- 只有已经支付之后，待服务的订单才显示“服务完成”，其他都是置灰+暂无服务 -->
        <!-- 点击服务完成，弹出气泡弹窗 -->
         <el-popconfirm
         v-if="scope.row.trade_state === '待服务'"
          confirm-button-text="是"
          cancel-button-text="否"
          :icon="InfoFilled"
          icon-color="#626AEF"
          title="是否确认完成？"
          @confirm="confirmEvent(scope.row.out_trade_no)"
        >
          <template #reference><!--文字类型button按钮用 link -->
            <el-button type="primary" link>服务完成</el-button>
          </template>
        </el-popconfirm><!--暂无服务且置灰 -->
        <el-button v-else type="primary" link disabled>暂无服务</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- 分页 -->
  <div class="pagenation-info">
    <el-pagination
      v-model:current-page="paginationData.pageNum"
      :page-size="paginationData.pageSize"
      :background="false"
      layout="total, prev, pager, next"
      :total="tableData.total"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      size="small"
      style="float: right;"
      />
  </div>
</template>

<script setup>
import { adminOrder, updateOrder } from '@/api/indexApi';
import { onMounted,ref } from 'vue';
import dayjs from 'dayjs';
import { useRoute } from 'vue-router';
import { InfoFilled } from '@element-plus/icons-vue';

const route = useRoute()

onMounted(() => {
  getListData()
})

const paginationData = ref({
  pageNum: 1,
  pageSize: 10,
})

// 搜索表单
const searchForm = ref({
  out_trade_no: '',
})
// 搜索按钮点击事件
const onSubmit = () => {
  // console.log('搜索表单：', searchForm.value);
  getListData(searchForm.value)
}

// 列表数据
const tableData = ref({
  list: [],
  total: 0
})

// 分页相关
//改变每页条数回调
const handleSizeChange = (val) => {
  paginationData.value.pageSize = val;
  getListData();
}
//改变页码回调
const handleCurrentChange = (val) => {
  paginationData.value.pageNum = val;
  getListData();
}

// 调用接口的方法
const getListData = (params = {}) => {
  adminOrder({ ...paginationData.value, ...params }).then(({ data }) => {
    //console.log('陪护师数据：', data);
    const { list, total } = data.data;
    // 格式化时间
     list.forEach(item => {
      item.create_time = dayjs(item.created_at).format('YYYY-MM-DD HH:mm:ss');
    });
    tableData.value.list = list;
    tableData.value.total = total;
  })
}

// 状态
const statusMap = (key) => {
  const obj = {
     '待支付': 'warning',
     '已取消': 'info',
     '已完成': 'success',
  }
  return obj[key]
}

// 服务完成确认,扭转订单状态
const confirmEvent = (id) => {
  // console.log('服务完成确认');
  updateOrder({ id }).then(({ data }) => {
    if (data.code === 10000) {
      getListData()
    }
  })
}

</script>

<style lang="scss" scoped>
.form {
  display: flex;
  justify-content: flex-end;
  padding: 10px 0 10px 10px;
  background-color: #fff;
}
</style>
```

搜索功能：

- **搜索框 = 筛选条件**
- **分页 = 控制一页显示多少**
- 点查询 → 从第 1 页开始，按订单号查
- 点分页 → 在当前搜索结果里翻页
- 传给后端的参数永远是：

```
pageNum、pageSize、out_trade_no
```



# 【C端】重定向问题解决

启动C端之后，网页会出现闪烁的情况，解决：

main.js：

```javascript
import { createApp } from 'vue'
//import './style.css'
import App from './App.vue'
import router from './router'
import api from './api'
import { Notify } from 'vant'
import 'vant/lib/index.css'

const app = createApp(App)

// 挂载api,在实例上绑定属性,这样在组件中就可以通过this.$api来访问api中的方法了
app.config.globalProperties.$api = api 

// 路由守卫
router.beforeEach((to, from) => {
  //如果未登录
  if (to.path !== 'login') {
    if (localStorage.getItem('h5_token') == null) { //判断token是否存在
      return '/login'
    }
  }
})

// 路由挂载
app.use(router)
// 注册Vant Notify组件
app.use(Notify)

app.mount('#app')
```

# 完结撒花！！！！！