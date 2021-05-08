---
title: Vue 源码浅析
categories: [frontend]
comments: true
---

## 简介

version：Vue.js v2.6.12

Flow：静态类型检查

目录结构

## new Vue 初始化

Vue 构造函数定义：`src\core\instance\index.js`

```js
function Vue(options) {
  if (process.env.NODE_ENV !== "production" && !(this instanceof Vue)) {
    warn("Vue is a constructor and should be called with the `new` keyword");
  }
  this._init(options);
}
```

通过 instanceof 判断 Vue 构造函数是否在初始化后实例的原型链上，不通过 new 进行初始化会报错，且无法调用 `_init` 方法。warn 是封装好的控制台报错方法

```js
export let warn = noop;
warn = (msg, vm) => {
  const trace = vm ? generateComponentTrace(vm) : "";

  if (config.warnHandler) {
    config.warnHandler.call(null, msg, vm, trace);
  } else if (hasConsole && !config.silent) {
    console.error(`[Vue warn]: ${msg}${trace}`);
  }
};
```

使用 new 进行初始化会调用 `_init` 方法，该方法在 `src\core\instance\init.js` 中定义

```js
Vue.prototype._init = function(options?: Object) {
  const vm: Component = this;
  // a uid
  vm._uid = uid++;

  let startTag, endTag;
  /* istanbul ignore if */
  if (process.env.NODE_ENV !== "production" && config.performance && mark) {
    startTag = `vue-perf-start:${vm._uid}`;
    endTag = `vue-perf-end:${vm._uid}`;
    mark(startTag);
  }

  // a flag to avoid this being observed
  vm._isVue = true;
  // merge options
  if (options && options._isComponent) {
    // optimize internal component instantiation
    // since dynamic options merging is pretty slow, and none of the
    // internal component options needs special treatment.
    initInternalComponent(vm, options);
  } else {
    vm.$options = mergeOptions(
      resolveConstructorOptions(vm.constructor),
      options || {},
      vm
    );
  }
  /* istanbul ignore else */
  if (process.env.NODE_ENV !== "production") {
    initProxy(vm);
  } else {
    vm._renderProxy = vm;
  }
  // expose real self
  vm._self = vm;
  initLifecycle(vm);
  initEvents(vm);
  initRender(vm);
  callHook(vm, "beforeCreate");
  initInjections(vm); // resolve injections before data/props
  initState(vm);
  initProvide(vm); // resolve provide after data/props
  callHook(vm, "created");

  /* istanbul ignore if */
  if (process.env.NODE_ENV !== "production" && config.performance && mark) {
    vm._name = formatComponentName(vm, false);
    mark(endTag);
    measure(`vue ${vm._name} init`, startTag, endTag);
  }

  if (vm.$options.el) {
    vm.$mount(vm.$options.el);
  }
};
```

递增生成唯一的组件 `_uid`

对传入的参数进行合并

初始化生命周期 - `initLifecycle` 方法

初始化事件 - `initEvents` 方法

`initRender` 方法

`callHook` 方法

`initInjections` 方法

`initState` 方法

`initProvide` 方法

`callHook` 方法

最后调用 `$mount` 方法挂在 vm，将模板渲染成最终的 DOM

## Vue 挂载

`$mount` 方法在很多文件中都有定义，`src\platforms\web\entry-runtime-with-compiler.js` `src\platforms\web\runtime\index.js` `src\platforms\weex\runtime\index.js`，因为 `$mount` 方法是和平台、构建方式都相关的。重点分析 complier 版本的实现：

```js
const mount = Vue.prototype.$mount;
Vue.prototype.$mount = function(
  el?: string | Element,
  hydrating?: boolean
): Component {
  el = el && query(el);

  /* istanbul ignore if */
  if (el === document.body || el === document.documentElement) {
    process.env.NODE_ENV !== "production" &&
      warn(
        `Do not mount Vue to <html> or <body> - mount to normal elements instead.`
      );
    return this;
  }

  const options = this.$options;
  // resolve template/el and convert to render function
  if (!options.render) {
    let template = options.template;
    if (template) {
      if (typeof template === "string") {
        if (template.charAt(0) === "#") {
          template = idToTemplate(template);
          /* istanbul ignore if */
          if (process.env.NODE_ENV !== "production" && !template) {
            warn(
              `Template element not found or is empty: ${options.template}`,
              this
            );
          }
        }
      } else if (template.nodeType) {
        template = template.innerHTML;
      } else {
        if (process.env.NODE_ENV !== "production") {
          warn("invalid template option:" + template, this);
        }
        return this;
      }
    } else if (el) {
      template = getOuterHTML(el);
    }
    if (template) {
      /* istanbul ignore if */
      if (process.env.NODE_ENV !== "production" && config.performance && mark) {
        mark("compile");
      }

      const { render, staticRenderFns } = compileToFunctions(
        template,
        {
          outputSourceRange: process.env.NODE_ENV !== "production",
          shouldDecodeNewlines,
          shouldDecodeNewlinesForHref,
          delimiters: options.delimiters,
          comments: options.comments
        },
        this
      );
      options.render = render;
      options.staticRenderFns = staticRenderFns;

      /* istanbul ignore if */
      if (process.env.NODE_ENV !== "production" && config.performance && mark) {
        mark("compile end");
        measure(`vue ${this._name} compile`, "compile", "compile end");
      }
    }
  }
  return mount.call(this, el, hydrating);
};
```
