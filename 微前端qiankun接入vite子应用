### vite子应用接入qiankun上下文

因为我们的项目需要兼容老旧浏览器，所以直接采用了@vitejs/plugin-legacy插件，会把项目中的代码全部转化成systemjs格式。

以下是根据vite官方文档用react模版创建的项目。

先用vite build构建出html模版，由于qiankun不支持在非module script标签内解析esm格式的代码，所以我把所有的module格式的sciprt脚本全部注释，同时把legacy sciprt标签上的nomodule全部去掉这样才可以加载。

**注意，我目前没采用动态publicpath插件，所以vite的base写死成我们的主应用域名。后期可以考虑使用动态路径。**

### html模版改造

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="http://dev.xtrfr.cn:4173/assets/favicon.17e50649.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
    <!-- <script type="module" crossorigin src="http://dev.xtrfr.cn:4173/assets/index.344d46c1.js"></script> -->
    <!-- <link rel="modulepreload" href="http://dev.xtrfr.cn:4173/assets/vendor.876e63ed.js"> -->
    <link rel="stylesheet" href="http://dev.xtrfr.cn:4173/assets/index.cd9c0392.css">
    <!-- <script type="module">!function(){try{new Function("m","return import(m)")}catch(o){console.warn("vite: loading legacy build because dynamic import is unsupported, syntax error above should be ignored");var e=document.getElementById("vite-legacy-polyfill"),n=document.createElement("script");n.src=e.src,n.onload=function(){System.import(document.getElementById('vite-legacy-entry').getAttribute('data-src'))},document.body.appendChild(n)}}();</script> -->
  </head>
  <body>
    <div id="root"></div>
    
    <script>!function(){var e=document,t=e.createElement("script");if(!("noModule"in t)&&"onbeforeload"in t){var n=!1;e.addEventListener("beforeload",(function(e){if(e.target===t)n=!0;else if(!e.target.hasAttribute("nomodule")||!n)return;e.preventDefault()}),!0),t.type="module",t.src=".",e.head.appendChild(t),t.remove()}}();</script>
    <script id="vite-legacy-polyfill" src="http://dev.xtrfr.cn:4173/assets/polyfills-legacy.1b8c3f41.js"></script>
    <script id="vite-legacy-entry" data-src="http://dev.xtrfr.cn:4173/assets/index-legacy.c4abd9c4.js">System.import(document.getElementById('vite-legacy-entry').getAttribute('data-src'))</script>
    <script src="/assets/entry.js"></script>
  </body>
</html>
​
```

**在html模版中插入了entry.js文件**

```js
console.log('before')
​
async function bootstrap() {
  // 异步等到promise resolve才会去调用mount，保证mount的时候window上一定有函数。
 return System.import('http://dev.xtrfr.cn:4173/assets/index-legacy.c4abd9c4.js').then(() => {
    console.log('bootstrap', window.purehtml)
    window.purehtml.bootstrap()
  })
}
​
/**
 * 应用每次进入都会调用 mount 方法，通常我们在这里触发应用的渲染方法
 */
async function mount(props) {
  console.log('mount', window.purehtml)
  window.purehtml.mount(props)
}
​
/**
 * 应用每次 切出/卸载 会调用的方法，通常在这里我们会卸载微应用的应用实例
 */
async function unmount(props) {
  console.log('unmount', window.purehtml)
  window.purehtml.unmount(props)
}
​
((global) => {
  global["purehtml"] = {
    bootstrap,
    mount,
    unmount,
  };
})(window);
console.log('after', window.purehtml)
​
```

### 应用入口改造

同时我们的main.tsx代码也改造成如下

```js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App, { a } from "./App";
console.log("purehtml", window.purehtml);
// ReactDOM.render(
//   <React.StrictMode>
//     <App />
//   </React.StrictMode>,
//   document.getElementById("root")
// );
​
export async function bootstrap() {
  console.log("react app bootstraped");
}
​
/**
 * 应用每次进入都会调用 mount 方法，通常我们在这里触发应用的渲染方法
 */
export async function mount(props: any) {
  console.log("mountProps", props);
  ReactDOM.render(
    <App />,
    props.container
      ? props.container.querySelector("#root")
      : document.getElementById("root")
  );
}
​
/**
 * 应用每次 切出/卸载 会调用的方法，通常在这里我们会卸载微应用的应用实例
 */
export async function unmount(props: any) {
  console.log("unmountProps", props);
  ReactDOM.unmountComponentAtNode(
    props.container
      ? props.container.querySelector("#root")
      : document.getElementById("root")
  );
}
​
((global) => {
  global["purehtml"] = {
    bootstrap,
    mount,
    unmount,
  };
})(window);
​
```

### 总结

先改造vite的base，然后通过插件给html文件注入entry.js，同时过滤掉module的script并且把legacy script的nomodule去掉就可以成功的接入主应用啦。插件等后续有时间再写吧。

如果大家有更好的方法希望大家能提点建议，谢谢！
