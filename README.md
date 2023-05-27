# ITCSS 架构结合 SMACSS 架构

- OOCSS
  OOCSS 为组件化思想。
  在 vue 中每个页面都是组件,使用不必单独在 style 里去定义,直接在组件中写就可以了。

- SMACSS
  分类：
  Base（基础）
  Layout（布局）
  Module（模块）
  State（状态）
  Theme（主题）
  其实现在项目开发，Layout 往往直接被当做组件直接使用了，例如 vue 中组件，然后你可以在他们内部去遵循 Module 规则
- ITCSS
  作者意识到了 ITCSS 的缺点，所以提出了 ITCSS 的灵活性来进行弥补。作者认为，对于 ITCSS 的层数，你没必要完全遵守，可根据项目需要进行删减保留，如果你不需要 OOCSS 的概念就可以删除 Objects 层，如果你需要 theme 层，就可以往 ITCSS 的分层里面添加

### css 架构之 settings 层代码实现

#### 颜色 边框 字体大小 阴影 层级等变量定义

```scss
// 定义变量 settings/_variables.scss

// colors
$colors: (
  "primary": #db9e3f,
  "info": #4b67af,
  "danger": #791a15,
  "blue-1": #1f3695,
  "blue": #4394e4,
  "white": #fff,
  "white-1": #fcfcfc,
  "white-2": #eceef0,
  "light": #f9f9f9,
  "light-1": #d4d9de,
  "grey": #999,
  "grey-1": #666,
  "dark-1": #343440,
  "dark": #222,
  "black": #000,
);
$boder-color: map-get($colors, "black");
// font-sizes
$font-size: 16px;
```

#### vue.config 配置共享全局变量

```js
const { defineConfig } = require("@vue/cli-service");
const { resolve } = require("node:path");
console.log(resolve, "__dirname");
module.exports = defineConfig({
  transpileDependencies: true,
  configureWebpack: {
    resolve: {
      alias: {
        "~styles": resolve(__dirname, "./src/styles"),
      },
    },
  },
  // 将_var变量导入成全局,在任何页面去使用
  css: {
    loaderOptions: {
      scss: {
        additionalData: `@import "@/styles/settings/_var.scss";`,
      },
    },
  },
});
```

### css 架构之 tools 层代码实现

- 引入[SassMagic](https://github.com/W3cplus/SassMagic) 工具库

### css 架构之 base 层代码实现

- Generic 引入 normalize.css 重置浏览器默认样式
  npm install --save normalize.css

### css 架构之 components 层代码实现

- OOCSS => BEM
- 经典的组件：栅格，布局组件
- 自定义组件

```vue
<template>
  <div class="media">
    <div class="media_img"></div>
    <div class="media_content"></div>
  </div>
  <!-- 组件 -->
  <c-media class="fiexd"></c-media>
</template>
<style>
.media {
}
.media .media_img {
}
.media .media_content {
}
/* 扩展的名称 */
.fiexd {
}
</style>
```

### Acss 层代码实现

- 让样式极限复用
- 解决 Acss 无语义化缺点：属性选择器
- Settings 与 Acss 层的关系

```html
<div fl></div>
[fl]{ background:pink }
```

### Theme 层代码实现

#### 将 data-theme 放置到 html 标签上

##### 方式一

- 通过 JS 更改 data-theme 属性进行切换改变样式
- 在 app.vue 中可以通过 JS 注入属性

```js
window.document.documentElement.setAttribute("data-theme", "cool");
```

```css
// styles/index.scss
// 主题
@import "./theme/css/default.scss";
@import "./theme/css/cool.scss";
@import "./theme/css/dark.scss";
@import "./theme/css/warn.scss";
```

```css
// default.scss
[data-theme="defalut"] .text {
  color: #409eff;
}
[data-theme="defalut"] .box {
  background: #409eff;
}
[data-theme="defalut"] .box-item-text {
  color: #409eff;
}
[data-theme="defalut"] .box-item-text {
  background: #409eff;
}
```

```css
// cool.scss
[data-theme="cool"] .text {
  color: #f56c6c;
}
[data-theme="cool"] .box {
  background: #f56c6c;
}
//  针对性换肤
[data-theme="cool"] .box-item-text {
  color: #e6a23c;
}
[data-theme="cool"] .box-item-text {
  background: #e6a23c;
}
```

```vue
<template>
  <div class="box">
    <div class="text"></div>
  </div>
  <div class="box-item-text">asdads</div>
</template>
<script>
import { onMounted } from "vue";
export default {
  setup() {
    onMounted(() => {
      window.document.documentElement.setAttribute("data-theme", "coo1");
    });
  },
};
</script>
<style></style>
```

##### 方式二

```scss
// theme/scss/_theme.scss
$data-theme: (
  defalut: (
    text-color: #409eff,
    bg-color: #409eff,
  ),
  cool: (
    text-color: #f56c6c,
    bg-color: #f56c6c,
  ),
  warn: (
    text-color: #e6a23c,
    bg-color: #e6a23c,
  ),
  dark: (
    text-color: #303133,
    bg-color: #000000,
  ),
);
// _handle.scss
@mixin themeify {
  @each $theme-name, $theme-map in $data-theme {
    $theme-map: $theme-map !global;
    //判断html的data-theme的属性值  #{}是sass的插值表达式
    [data-theme="#{$theme-name}"] & {
      @content;
    }
  }
}
@function themed($key) {
  @return map-get($theme-map, $key);
}
```

```vue
<template>
  <div class="app-wrapper">qweqweqwe</div>
</template>
<script setup lang="ts">
import { onMounted } from "vue";
onMounted(() => {
  document.documentElement.setAttribute("data-theme", "cool");
});
</script>
<style scoped lang="scss">
@include b(app-wrapper) {
  @include themeify {
    color: themed(text-color);
  }
}
</style>
```

```js
import { fileURLToPath, URL } from "node:url";
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import vueJsx from "@vitejs/plugin-vue-jsx";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(), vueJsx()],
  resolve: {
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
    },
  },
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: `@import "@/styles/settings/_variable.scss";@import "@/styles/tools/_sassMagic.scss";@import "@/styles/theme/scss/theme.scss";`,
      },
    },
  },
});
```

...
