# 抽象类和类方法重载

## 一、抽象类

使⽤ abstract 关键字声明的类，我们称之为抽象类。抽象类做为其它派生类的基类使用,抽象类不能被实例化，因为它⾥⾯包含⼀个或多个抽象⽅法。所谓的抽象⽅法，是指不包含具体实现的⽅法.

**使用原则:**

- 抽象类不能被实例化, 只能继承使用
- 抽象类中的抽象方法,在派生类中要全部实现,否则报错.
- **抽象方法可当做类的实例方法，添加访问修饰符**

```typescript
abstract class Person {
	constructor(public name: string){}
    // 抽象⽅法, 抽象方法必须在派生类中实现
    abstract say(words: string) :void; 
}

class Developer extends Person {
		constructor(name: string) {
			super(name);
 		}
		say(words: string): void {
			console.log(`${this.name} says ${words}`);
 		}
}

const lolo = new Developer("lolo");
lolo.say("I love ts!"); // lolo says I love ts!
```



## 二、类方法重载

类方法重载和函数的重载一样.在下面例子中我们重载了getProducts方法.

```typescript
class ProductService {
	getProducts(): void;
	getProducts(id: number): void;
	getProducts(id?: number) {
		if(typeof id === 'number') {
			console.log(`获取id为 ${id} 的产品信息`);
		} else {
			console.log(`获取所有的产品信息`);
		} 
	}
}
const productService = new ProductService();
productService.getProducts(666); // 获取id为 666 的产品信息
productService.getProducts(); // 获取所有的产品信息
```

