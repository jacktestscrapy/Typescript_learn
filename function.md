# 函数

## 一、定义函数

和 JavaScript 一样，TypeScript 函数可以创建有名字的函数和匿名函数。

```typescript
// 命名函数
function fullName(x: number, y: nunber): {
	return x+y
}

console.log(fullName(123, 123)) // 246

// 匿名函数
let myName = function (x: number, y: string) {
	return x+y
}
console.log(myName(12,'12')) // 1212
```

## 二、可选参数和默认参数和剩余参数

在 TypeScript 里，我们也可以为参数提供一个默认值当用户没有传递这个参数或传递的是undefined时,我们可以使用默认值.

```typescript
function buildName(firstName: string='panda', lastName?: string): string {
  if (lastName) {
    return firstName + '-' + lastName
  } else {
    return firstName
  }
}
```

默认参数和可选参数有个共同点：它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来.此时我们可以使用**剩余参数**, 它可以接收0个或者任意个参数.

```typescript
// 剩余参数 接受新参传过来的任意多个值
function count(...result:number[]) {
	var sum = 0;
	for (var i = 0; i < result.length; i++){
		sum += result[i]
	}
	return sum
}
console.log(count(2,3,4,56,456))
```

## 三、函数重载

函数重载: 函数名相同, 而形参不同的多个函数.在JS中, 由于弱类型的特点和形参与实参可以不匹配, 是没有函数重载这一说的 但在TS中, 与其它面向对象的语言(如Java)就存在此语法.

```typescript
//函数重载
function add (x: string, y: string): string
function add (x: number, y: number): number

// 定义函数实现
function add(x: string | number, y: string | number): string | number {
  // 在实现上我们要注意严格判断两个参数的类型是否相等，而不能简单的写一个 x + y
  if (typeof x === 'string' && typeof y === 'string') {
    return x + y
  } else if (typeof x === 'number' && typeof y === 'number') {
    return x + y
  }
}

console.log(add(1, 2))
console.log(add('a', 'b'))
// console.log(add(1, 'a')) // error

```

