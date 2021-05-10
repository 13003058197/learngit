# 递归 和 尾递归

```js
// 尾递归：函数的最后调用自身
1.阶加
function factorial(num){
  if(num <= 1){
    return num
  }
  return num + factorial(num - 1)
}

```

# 斐波拉契数列

```js
// 斐波拉契数列 0, 1, 1, 2, 3, 5, 8 , 13......
f(0) = 0
f(1) = 1
f(n) = f(n-2) + f(n -1)

//  普通递归
function factorial(n){
  if(n < == 1){
    return n
  }
  return factorial(n-1) + factorial(n-2)
}

// 尾递归
function factorial(num, num1 = 0, num2 = 1){
  if(num === 0){
    return num1
  }
  return factorial(num-1, num2, num1+num2)
}
// 循环法
// 公式法
```















