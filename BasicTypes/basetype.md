# 基础类型

## 一、Boolean、 Number、String、Symbol

### 1. Boolean 类型

```jav
let flag: boolean = false;

// flag=1 错误 
```

### 2. Number 类型

```javascript
let count:number = 20;
console.log(count = 12.3)
```

### 3. String 类型

```javas
let name:string = "Panda"
```

### 4. Symbol类型

Symbol 是不可变的唯一的. 使用Symbol 构造函数创建.

```typescript
// 使用Symbol进行构造
let sym1 = Symbol()
// 不可变唯一
let sym2 = Symbol('key')
let sym3 = Symbol('key')
// 返回false
console.log(sym2 === sym3)
```



## 二、Array、  Tuple、enum

### 1. Array 类型

第一种定义方式:

```javascript
let list: nunber[] = [1,2,3]
```

第二种定义方式:

```javascript
let list: Array<number> = [1,2,3]
```

### 2. Tuple 类型

**众所周知，数组⼀般由同种类型的值组成，但有时我们需要在单个变量中存储不同类型的值，这时候我们就可以使⽤元组**。在 JavaScript 中是没有元组的，元组是 TypeScript 中特有的类型，其⼯作⽅式类似于数组。

元组可⽤于定义具有有限数量的未命名属性的类型。每个属性都有⼀个关联的类型。使⽤元组时，必须提供每个属性的值。为了更直观地理解元组的概念，我们来看⼀个具体的例⼦：

```typescript
let tupleType: [string, boolean];
tupleType = ["panda", true];
```

在上⾯代码中，我们定义了⼀个名为 tupleType 的变量，它的类型是⼀个类型数组 [string,boolean] ，然后我们按照正确的类型依次初始化 tupleType 变量。与数组⼀样，我们可以通过下标来访问元组中的元素：

```typescript
console.log(tupleType[0]); // panda
console.log(tupleType[1]); // true
```

在元组初始化的时候，如果出现类型不匹配的话，⽐如：

```typescript
tupleType = [true, "panda"];
```



此时，TypeScript 编译器会提示以下错误信息：

```shell
[0]: Type 'true' is not assignable to type 'string'.
[1]: Type 'string' is not assignable to type 'boolean'.
```



很明显是因为类型不匹配导致的。在元组初始化的时候，我们还必须提供每个属性的值，不然也会出现错误，⽐如：

```typescript
tupleType = ["panda"];
```



此时，TypeScript 编译器会提示以下错误信息：

```shell
Property '1' is missing in type '[string]' but required in type '[string,boolean]'.
```

### 3. Enum 类型

使⽤枚举我们可以定义⼀些带名字的常量。 使⽤枚举可以清晰地表达意图或创建⼀组有区别的⽤例.**TypeScript ⽀持数字的和基于字符串的枚举。**

#### 1. 数字枚举

```typescript
enum Names{
	panda,
	alex,
	queen,
	king
}
let first_name: Names = Names.panda
console.log(first_name) // 输出0
```

默认情况下，panda 的初始值为 0，其余的成员会从 1 开始⾃动增⻓。也可以设置初始值.

```typescript
enum Names{
	panda=3,
	alex,
	queen,
	king
}
// 默认输出枚举成员的值
let first_name: Names = Names.panda
// 可以根据值获取枚举成员
let sec_name: string = Names[4]
console.log(first_name)
console.log(sec_name)
```

#### 2. 字符串枚举

在⼀个字符串枚举⾥，每个成员都必须⽤字符串字⾯量，或另外⼀个字符串枚举成员进⾏初始化。

```typescript
enum Names{
	panda="Panda",
	alex="Alex",
	queen="Queen",
	king="King"
}
// 默认输出枚举成员的值, 两种效果一样
let first_name = Names.panda
let sec_name = Names['panda']
console.log(first_name)
console.log(sec_name)
```

#### 3. 常量枚举

除了数字枚举和字符串枚举之外，还有⼀种特殊的枚举 —— 常量枚举。它是使⽤ const 关键字修饰的枚举，**常量枚举在编译阶段不会参与编译,会被移除**.当我们需要的是常量的值而不是对象时可以使用常量枚举,这样可以节省编译时间.例如:

```typescript
const enum Names{
	panda,
	alex,
	queen,
	king
}

let first_name = Names.panda
console.log(first_name) // 输出0
```

## 三、Any、Unknown

### 1. Any 类型

表示任意类型, 变量类型设置为any 相当于关闭了TS的类型检查. 所以在使用TS 时,不建议使用any 类型.

```typescript
let notSure: any
notSure = 666
notSure = "hello"
```

TypeScript 允许我们对 any 类型的值执⾏任何操作，⽽⽆需事先执⾏任何形式的检查.在许多场景下，这太宽松了。使⽤ any 类型，可以很容易地编写类型正确但在运⾏时有问题的代码。如果我们使⽤ any 类型，就⽆法使⽤ TypeScript 提供的⼤量的保护机制。为了解决 any 带来的问题，引入unknown

### 2. unKnown 类型

unknown 类型只能被赋值给 any 类型和 unknown 类型本身。直观地说：只有能够保存任意类型值的容器才能保存 unknown 类型的值。

```typescript
let value: unknown;
value = true;
value = 42;
value = "hello world"
value = []
// unknown 只能赋值给any 类型或者unknown
let value1: unknown = value;
let value2: any = value;
// 会爬出错误:不能把类型unknown 分配给类型number
// let value3: number = value;
```

## 四、Void Null Undefined

### 1. Void

某种程度上来说，void 类型像是与 any 类型相反，它表示没有任何类型.当⼀个函数没有返回值时,可以设置返回值类型是void.

```typescript
function sum(): void{
	console.log('警告信息')
}

console.log(sum())
// 返回 警告信息和undefined

```

**需要注意**的是，声明⼀个 void 类型的变量没有什么作⽤，因为在严格模式下，它的值只能为undefined ：

### 2. Null 和Undefined

undefined 和 null 两者有各⾃的类型分别为 undefined 和 null 。当使用**严格模式,**null和undefined 只能分配给unknown和any类型使用.

若给一个变量传递多个类型可以使用联合类型.

```typescript
let u: undefined = undefined;
let n: null = null;

let d: string | number | undefined
```

## 五、Never、Object、Function 

### 1. Never

never 类型表示的是值不会存在的类型。例如， never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型

```typescript
// 返回never的函数必须存在⽆法达到的终点
function error(message: string): never {
	throw new Error(message);
}

// 死循环无返回值
function infiniteLoop(): never {
	while (true) {}
}
```

### 2.Object

```typescript
let obj: object = { x: 1, y: 2 }
// 会抛出object不存在属性x
//obj.x = 4
let obj1: { x: number, y: number } = { x: 1, y: 2 }
obj1.x = 3
```

### 3.Function

使用函数类型,可以先声明函数以及函数的返回值类型,或者函数中参数类型,可以直接调用不用重复声明.

```typescript
// 定义函数类型
let computer: (x: number, y: number) => number
// 定义函数
computer = (a,b)=> a+b
```

## 六、联合类型、类型断言、类型推断

### 1.联合类型

联合类型（Union Types）表示取值可以为多种类型中的一种.例如, 注册网站时名字可以是字符串,也可以是手机号.此时就可以使用联合类型

```typescript
let names: string | Object | number

names = "panda"
names = { "name": 12, "age": "13" }
names = 1761234678
```

### 2. 类型断言

类型断言和其他语言的类型转换相似, 但是不进行特殊的数据检查和解构.它只在编译阶段起作用,没有运行时影响.

类型断言两种方式,效果一样: 

- 尖括号语法

  ```typescript
  let someValue: any = "this is a string";
  let strLength: number = (<string>someValue).length;
  ```

- as 语法

  ```typescript
  let someValue: any = "this is a string";
  let strLength: number = (someValue as string).length;
  ```

### 3. 类型推断

类型推断: TS会在没有明确的指定类型的时候推测出一个类型.

有下面2种情况: 1. 定义变量时赋值了, 推断为对应的类型. 2. 定义变量时没有赋值, 推断为any类型

```typescript
/* 定义变量时赋值了, 推断为对应的类型 */
let b9 = 123 // number
// b9 = 'abc' // error

/* 定义变量时没有赋值, 推断为any类型 */
let b10  // any类型
b10 = 123
b10 = 'abc'
```



