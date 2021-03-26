# 类的属性和方法

在⾯向对象语⾔中，类是⼀种⾯向对象计算机编程语⾔的构造，是创建对象的蓝图，描述了所创建的对象共同的属性和⽅法.

在 TypeScript 中，我们可以通过 Class 关键字来定义⼀个类.类中的属性和方法分为静态属性、对象属性、静态方法、对象方法.

**静态方法和静态属性,使用类进行调用, 静态属性的值可以修改; 对象方法和对象属性使用类对象进行调用**.

**构造函数在创建类对象时优先初始化**

```typescript
class Greeter {
	// 静态属性
	static cname: string = "King";
	// 成员属性
	greeting: string="Panda";
	// 构造函数 - 执⾏初始化操作
	constructor(message: string) {
		console.log("我优先执行!!")
		this.greeting = message;
	}
	// 静态⽅法
	static getClassName() {
		return "Class name is Greeter";
	}
	// 成员⽅法
	greet() {
		return "Hello, " + this.greeting;
	}
}

let greeter = new Greeter("world");

// 调用静态属性
// greeter.cname 静态属性不能通过类对象进行调用
greeter.greeting // 成员属性可以使用对象调用
console.log(Greeter.cname) // 静态属性使用类调用
// 修改静态属性的值和成员属性的值
Greeter.cname = "panda"
greeter.greeting = "king"
console.log(Greeter.cname, greeter.greeting)
// 调用实例方法
console.log(greeter.greet())
// 调用静态方法
console.log(Greeter.getClassName())
```

