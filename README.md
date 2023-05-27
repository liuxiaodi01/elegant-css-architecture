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
