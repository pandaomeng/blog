---
title: 实现 promise
date: 2021-03-12
tag: [promise]
---

# 手写 promise

## 简单版实现

代码

```js
// 定义三种状态
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';

function MyPromise(fn) {
  this.state = PENDING;
  this.resolveCallback = [];
  this.rejectCallback = [];
  // fulfilled 或者 rejected 之后需要把结果报错在对象中
  this.value = null;

  // 使用箭头函数，保证函数体内的 this 指向生成的 promise 对象
  const resolve = (value) => {
    if (this.state === PENDING) {
      this.resolveCallback.map((each) => each(value));
      this.value = value;
      this.state = FULFILLED;
    }
  };

  const reject = (value) => {
    if (this.state === PENDING) {
      this.rejectCallback.map((each) => each(value));
      this.value = value;
      this.state = REJECTED;
    }
  };

  try {
    fn(resolve, reject);
  } catch (e) {
    reject(e);
  }
}

MyPromise.prototype.then = function (onFulfilled, onRejected) {
  const onFulfilledCallback =
    typeof onFulfilled === 'function' ? onFulfilled : (x) => x;
  const onRejectedCallback =
    typeof onRejected === 'function'
      ? onRejected
      : (r) => {
          throw r;
        };

  // 注册回调函数
  if (this.state === PENDING) {
    this.resolveCallback.push(onFulfilledCallback);
    this.rejectCallback.push(onRejectedCallback);
  }

  // 如果 promise 对象的当前状态已经 fulfilled，则直接执行 then 中的方法
  if (this.state === FULFILLED) {
    onFulfilled(this.value);
  }

  if (this.state === REJECTED) {
    onRejected(this.value);
  }

  return this;
};
```

测试用例 1

```js
// 测试用例
const p1 = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    resolve('hello');
  }, 1000);
})
  .then('world')
  .then(console.log);

setTimeout(() => {
  p1.then(console.log);
}, 2000);

// 1s 后输出 hello 
// 再过 1s 输出 hello
```

测试用例 2

```js
const p2 = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    reject('error');
  }, 1000);
}).then(null, (error) => {
  console.log(`error: `, error);
});

// 1s 后输出 error: error
```

## Promise.all 实现

代码

```js
Promise.myAll = function (promises) {
  let count = 0;
  return new Promise((resolve, reject) => {
    let result = new Array(promises.length);
    for (let i = 0; i < promises.length; i += 1) {
      const promise = promises[i];
      if (promise instanceof Promise) {
        promise
          .then((res) => {
            result[i] = res;
            count += 1;
            if (count === promises.length) {
              resolve(result);
            }
          })
          .catch((err) => {
            reject(err);
          });
      } else {
        result[i] = promise;
        count += 1;
        if (count === promises.length) {
          resolve(result);
        }
      }
    }
  });
};
```

测试 case

```js
const p1 = new Promise((resolve) => {
  setTimeout(() => {
    resolve('hello');
  }, 1000);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => {
    resolve('world');
  }, 500);
});

const p3 = 999;

const p4 = Promise.reject('this is an error');

Promise.myAll([p1, p2, p3]).then((res) => {
  console.log(`res: `, res);
});
// res: [ 'hello', 'world', 999 ]

Promise.myAll([p1, p2, p3, p4])
  .then((res) => {
    console.log(`res: `, res);
  })
  .catch((error) => {
    console.log(`error: `, error);
  })
 
// error: this is an error
```

Promise.allSettled 和 Promise.race 原理类似

