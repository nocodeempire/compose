```js
let init = (...args) => args.reduce((ele1, ele2) => ele1 + ele2, 0);
let step2 = (val) => val + 2;
let step3 = (val) => val + 3;
let step4 = (val) => val + 4;
let steps = [step4, step3, step2, init];
 
// 实现如下方法

let composeFunc = compose(...steps)
console.log(composeFunc(1, 2, 3))
```

方法一
```js
// 递归队列
const compose = (...args) => {
  let length = args.length,
    index = length - 1,
    result;
  return function f(...args1) {
    result = args[index].apply(this, args1);
    index --;
    if(index < 0) return result;
    return f(result);
  }
}
```

方法二
```js
// 方法调用后的返回结果给下一个方法
const compose = (...funcs) => funcs.reverse().reduce((f, g) => {
  return (...args) => g.call(null, f.apply(null, args));
}, funcs.shift())
```

方法三
```js
// Promise
const compose = (...funcs) => (...args) => {
  const init = funcs.pop();
  return funcs.reverse().reduce((prev, curr) => {
    return prev.then(res => curr.call(null, res));
  }, Promise.resolve(init.apply(null, args)))
}
```
