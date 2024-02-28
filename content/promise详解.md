---
title: "[JS] Promise 详解"
date: 2024/2/26 16:39:01
---

promise 的链式调用解决了异步编程中的回调地狱问题, 让异步编程变得更加直观

> async/await 可以不用手写 promise, 让代码更加直观. 就是简化 Promise 写法

> JavaScript 的本质上是单线程的，因此在任何时刻，只有一个任务会被执行，尽管控制权可以在不同的 Promise 之间切换，从而使 Promise 的执行看起来是并发的。在 JavaScript 中，并行执行只能通过 worker 线程实现

```js
// 在创建 Promise 对象时，参数的函数内的同步脚本会立即执行
new Promise(() => {
  console.log('promise output') // 会立刻输出
})
```

```js
function async1() {
  return new Promise((resolve, reject) => {
    console.log('log on new promise')
    setTimeout(() => {
      //   console.log('async');
      return resolve('ok');
    }, 1000);
  });
}

async function main() {
  const res = await async1();
  console.log(res); // micro task
  console.log('01');
}

function m1() {
  return async1()
    .then((res) => console.log(res)) // micro task
    .then(() => console.log('01'));
}

main();
m1();
```


```js
const pro = new Promise((resolve, reject) => {
  resolve('suc')
} )

// Promise.reject(new Error('xxx'))
throw new Error('xxx')

// 在非 async 函数中调用 async 函数,
// async 的作用就是声明当前函数是一个异步函数, 并且返回一个 Promise 对象
// async 函数是 AsyncFunction 构造函数的实例
async function wait() {
  await new Promise(resolve => setTimeout(resolve, 1000));

  return 10;
}

function f() {
  // 1 秒后显示 10
  wait().then(result => alert(result));
}

f();
```