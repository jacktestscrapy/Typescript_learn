# 常用修饰符

## 一、Public、Private、Protected

在TypeScript里,public、private、protected修饰符是用来描述类内部的属性或者方法的可访问性.

成员都默认为public, 可也以明确将一个成员标记为**public,它代表公开的, 类内部或外部都可以访问**.

**private表示只能在类内部访问, 当成员被标记为private时,它就不能在声明它的类外部访问**

**protected表示类内部和子类可以访问.**

```typescript
class Animal{
	// 定义属性, 默认是public
	public name: string;
	// 构造函数,默认是public
	public constructor(n: string) {
		this.name = n
	}
	public run(distance: number) {
		console.log(`${this.name} 跑 ${distance}m`)
	}
}

class Dog extends Animal{
	// 定义私有属性, 只能类内部访问
	private age: number = 18
	// 定义受保护对象, 在类内部以及子类中访问
	protected sex: string = '男'

	// 重写父类的方法
	run(distance: number = 5) {
		super.run(distance)
		console.log('子类Dog的run方法')
	}


}

class QiuDog extends Dog{

	run(distance: number = 6) {
		super.run(distance)
		console.log(this.sex) // 子类能看到父类中受保护的成员
		// console.log(this.age) // age 为私有属性,不能在子类中访问
	}
}

console.log(new Dog('牧羊').name) // 牧羊. name 是公有属性,可以
console.log(new QiuDog('牧羊').run()) // 子类可以访问protected的属性.
// console.log(new QiuDog('牧羊').sex) // 受保护的不能在类外访问,只能在类里访问

// console.log(new Dog('牧羊').age) // 私有属性只能在类内部访问

```

## 二、readonly和参数属性

可以使用 `readonly` 关键字将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。

```typescript
class Person {
  readonly name: string = 'panda'
  constructor(name: string) {
    this.name = name
  }
}

let john = new Person('king')
// john.name = 'peter' // error name 是只读的不能修改
```

我们必须在 `Person` 类里定义一个只读成员 `name` 和一个参数为 `name` 的构造函数，并且立刻将 `name` 的值赋给 `this.name`，这种情况经常会遇到.**参数属性可以方便地让我们在一个地方定义并初始化一个成员。**

```typescript
class Person {
  private readonly age:number = 8;
  constructor(readonly name: string) {
    this.name = name
  }
}

let p = new Person('king')
// p.name = "hah" // 只读属性不能修改
// p.age //私有属性只能在类内部访问
```

## 三、存取器

`TypeScript` 支持通过 `getters/setters` 来截取对对象成员的访问.我们可以通过 getters 和 setters ⽅法来实现数据的封装和有效性校验，防⽌出现

异常数据。

```typescript
const nameLength = 16;

class UserInfo {

  private _userName:string = "panda_kings"
 // 获取用户名
  get userName(): string {
    return this._userName;
  }
  
 // 设置用户名
  set userName(newName: string) {
	  if (newName.length > nameLength) {
		console.log("用户名的最大长度为" + nameLength)
		throw new Error("用户名的最大长度为 " + nameLength);
    }
    this._userName = newName;
  }
}

let userInfo = new UserInfo();
// 获取名字,默认会调用get userName方法
console.log(userInfo.userName) // panda_kings
// 设置名字,默认会调用set userName方法
userInfo.userName = "panda1234567890-asdas"
```



