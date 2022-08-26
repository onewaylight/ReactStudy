```js
function compose(...args) {
  return args.reduce(function(prevFunc, nextFunc) {
    return function(...values) {
      return nextFunc(prevFunc(...values));
    }
  }, k => k);
}

/// (((x * 2) + 5) * 3) + 4

const formula = compose(
  multiplyX(2),
  addX(5),
  multiplyX(3),
  addX(4),
);

formula(10);
```
