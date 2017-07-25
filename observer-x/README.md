
中文版本 | [English Version](./README.en.md)

# Observer-X

本项目设计思想与部分代码实现逻辑借鉴了[ Icaro ](https://github.com/GianlucaGuarini/icaro) 以及[ Watch.JS ](https://github.com/melanke/Watch.JS)。目前 Observer-X 实现了基于 Proxy 的动态监测方案，正在准备完成基于 ES5 的 defineProperty 方案，尽可能保持二者 API 的一致性。

## Installation

- 标签方式引入

```html
<script src="https://unpkg.com/observer-x@0.0.7/dist/observer-x.js"></script>
  ```
此时 observe 挂载在全局作用域名 ObserverX 下，使用 `ObserverX.observe` 方式引用：
```
observerX.observe({ value: 0 });
```

- npm / yarn

```shell
$ npm install observer-x -S
or
$ yarn add observer-x
``` 
然后使用 import 引入：
```shell
import { observe } from 'observer-x';

// 引入 ES6 类，用于 Rollup 等打包
import { observe } from 'observer-x/dist/observer-x.es.js';
```

## Usage

- Node 中使用
```

import { observe } from '../../dist/observer-x';

const obj = observe(
  {},
  {
    recursive: true
  }
);

obj.property = {};

obj.property.listen(changes => {
  console.log(changes);
  console.log('changes in obj');
});

obj.property.name = 1;

obj.property.arr = [];

obj.property.arr.listen(changes => {
  // console.log('changes in obj.arr');
});

// changes in the single event loop will be print out

setTimeout(() => {
  obj.property.arr.push(1);

  obj.property.arr.push(2);

  obj.property.arr.splice(0, 0, 3);
}, 500);

```


- HTML 中进行 DOM 更新

```
const counter = observerX.observe({ value: 0 });

// 自动触发
function update() {
    display.innerHTML = counter.value;
}

// 变化监听
counter.listen(update);
(function loop() {
    counter.value++;
    requestAnimationFrame(loop);
})();
```

# RoadMap

[react-intersection-observer](https://github.com/thebuilder/react-intersection-observer)