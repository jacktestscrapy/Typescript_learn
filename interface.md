# 接口

## 一、接口初识

**接口的作用**：在面向对象的编程中，**接口是一种规范的定义，它定义了行为和动作的规范**，在程序设计里面，接口起到一种限制和规范的作用。

接口定义了某一批类所需要遵守的规范，接口不关心这些类的内部状态数据，也不关心这些类里方法的实现细节，它只规定这批类里必须提供某些方法，提供这些方法的类就可以满足实际需要。

### 1. 定义接口

**接口是对象状态(属性)和行为(方法)的抽象描述.** Typescript的核心原则之一,是对值所具有的结构进行类型检查, 我们可以定义接口(Interfaces)来定义对象类型.

```typescript
/* 
需求: 创建人的对象, 需要对人的属性进行一定的约束
  id是number类型, 必须有, 只读的
  name是string类型, 必须有
  age是number类型, 必须有
  sex是string类型, 可以没有
*/

// 定义人的接口
interface IPerson {
  id: number
  name: string
  age: number
  sex: string
}

const person1: IPerson = {
  id: 1,
  name: 'tom',
  age: 20,
  sex: '男'
}
```

类型检查器会查看对象内部的属性是否与IPerson接口描述一致, 如果不一致就会提示**类型错误**

### 2.可选属性和只读属性

上面需求中的sex 属性是可选的, 不是必须的. 我们可以在定义时在可选属性后面添加“?”符号.

可选属性的哈出就是对可能存在的属性进行预定义, 可以捕获引用了不存在属性时的错误.

```typescript
interface IPerson {
  id: number
  name: string
  age: number
  sex?: string // 定义可选属性
}

const person1: IPerson = {
  id: 1,
  name: "Tom",
  age: 20,
  sex: '男'
}
const person2: IPerson = {
  id: 1,
  name: "Tom",
  age: 20,
  //sex: 'female'// 可以没有
}
```

只读属性,就是只能在对象刚刚创建的时候修改,不能在使用过程中修改. 可以使用**readonly**来指定.

```typescript
interface IPerson {
  readonly id: number
  name: string
  age: number
  sex?: string
}

const person:IPerson{
  id:1,
  name: "panda",
  age:30
}
person.id = 2 // error
```

### 3. 任意属性

有时候我们希望⼀个接⼝中除了包含必选和可选属性之外，还允许有其他的任意属性，这时我们可以使⽤ **索引签名** 或者**传递对象**的形式来满⾜上述要求。

```typescript
interface IPerson {
	name: string;
	age?: number;
  // 定义索引签名
	[argName: string]:any
}

function UserInfo(info: IPerson):{name:string, age: number} {
	return {
		name: info.name,
		age: info.age
	}
}

/*
* 第一种方式使用索引签名, 可以传入任意的类型的属性
*/
let userInfo = UserInfo({
  name: 'panda',
  age: 31,
  sex:'男',
  height: 220
})
/*
*传入对象方式,接口不会进行过多的属性检查,只会检查传入的对象中是否有传入的属性.
* 此方法不建议使用.
let user = {
	name: 'panda',
	age: 31,
	sex: '男',
  height: 220
}
let userInfo = UserInfo(user)
*/
```



## 二、接口实现函数类型

为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它是对**参数和返回值类型的约束**。参数列表里的每个参数都需要名字和类型。

```typescript
// 定义接口
interface IAdd{
	// 函数的参数a和b以及返回类型
	(a: number, b: number): number
}
```

这样定义后，我们可以像使用其它接口一样使用这个函数类型的接口.下例展示了如何创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。

```typescript
//  add为函数类型的变量,冒号后面是函数的实现.
let add: IAdd: (a,b)=> a + b
console.log(add(10, 20)) // 返回30
```

## 三、类类型接口

我们可以定义类类型接口,然后在实现类,能够对类进行批量约束, 和抽象类相似.

**注意: 接口只能约束类的公有成员, 构造函数、私有成员和受保护成员不能被约束.**

```typescript
interface Animal{
	name: string;
	run(distance:number): void;
}

class Dog implements Animal{
	name: string;
	constructor(name: string) {
		this.name = name
	}
	run(distance: number = 10) { 
		console.log(`${this.name}能跑 ${distance} m 远 `)
	}
}

class Cat implements Animal{
	// 构造函数的另一种写法
	constructor(public name: string) {	
	}
	// 子类实现接口的方法,可以不传入参数
	run() {
		console.log(`${this.name}能跑远 `)
	}
	// 定义自己的方法
	eat(sth: string) {
		console.log(`${this.name}吃 ${sth}`)
	}
}

let dog = new Dog('田园犬')
dog.run()
let cat = new Cat('小橘')
cat.run()
cat.eat('鱼')
```

一个类也可以实现多个接口

```typescript
interface Person{
	name: string
	work(something: string ):void
}

interface Programmer{
	coding(str: string):void
}
// 实现多个接口时用","" 隔开
class TS implements Programmer, Person{
	constructor(public name: string) { }

	work(str: string) {
		console.log(`${this.name} 正在${str}`)
	}

	coding() {
		console.log(`${this.name} 正在撸代码`)
	}
}

let ts = new TS('panda')
ts.work('学习')
ts.coding()
```



## 四、接口的继承

就像类能继承一样, 接口也能继承, 这样能够实现多样功能.

```typescript
interface Person{
	name: string
	work(something: string ):void
}

interface Programmer{
	coding(str: string):void
}
// 接口也可以继承一个或多个接口, 多个接口用逗号分隔
interface FirstEnd extends Person, Programmer{
	doSth(str: string): void
}

// 实现接口
class TS implements FirstEnd{
	constructor(public name: string) { }

	work(str: string) {
		console.log(`${this.name} 正在${str}`)
	}

	coding() {
		console.log(`${this.name} 正在撸代码`)
	}
	doSth() {
		console.log('我是全干的程序员')
	}
}

let ts = new TS('panda')
ts.work('学习')
ts.coding()
ts.doSth()
```

## 五、接口和类的混合使用

既然接口和类都能继承,那接口和类是能够混合使用的, 但是要主要继承以及实现的层级关系.

```typescript
interface Person{
	name: string
	work(something: string ):void
}

interface Programmer{
	coding(str: string):void
}
// 接口也可以继承一个或多个接口, 多个接口用逗号分隔
interface FirstEnd extends Person, Programmer{
	doSth(str: string): void
}

// 定义类
class BackEnd{
	constructor(name: string) { }
	writer() {
		console.log('后端也能写页面')
	}
	
}

// 先继承基类后实现接口
class TS extends BackEnd implements FirstEnd{
	constructor(public name: string) { 
		super(name)
	}

	work(str: string) {
		console.log(`${this.name} 正在${str}`)
	}

	coding() {
		console.log(`${this.name} 正在撸代码`)
	}
	doSth() {
		console.log('我是全干的程序员')
	}
	// 重写父类的方法
	writer() {
		console.log('我是划水的狗子')
	}
}

let ts = new TS('panda')
ts.work('学习')
ts.coding()
ts.doSth()
// 若子类实现父类方法优先调用子类实现的方法, 否则调用父类的方法
ts.writer()
```

