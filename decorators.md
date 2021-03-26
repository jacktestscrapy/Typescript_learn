# 装饰器

## 一、简介

装饰器是一种特殊类型的声明，它能够被附加到类声明，方法，属性或参数上，可以修改类的行为。通俗的讲,装饰器就是一个方法，可以注入到类、方法、属性参数上来扩展类、属性、方法、参数的功能. 更简单的理解可以认为就是在不修改源代码基础上能给源代码添加额外功能.

要想使用装饰器,必须在同时config.json里面启用`experimentalDecorators`编译器选项.

```json
{
  "compilerOptions": {
     "target": "es2015",
     // 启用装饰器
     "experimentalDecorators": true
  }
}
```

根据装饰器位置分类:  类装饰器、访问器装饰器、属性装饰器、方法装饰器、参数装饰器，但是没有函数装饰器。

根据装饰器是否有参数: 普通装饰器（无法传参） 、 装饰器工厂（可传参）

## 二、定义及使用

装饰器本质是一个函数,理论上忽略参数的话,任何函数都可以做装饰器使用.先定义一个函数，然后这个函数有一个参数，target就是要装饰的目标,定义了这个函数之后，它就可以作为装饰器，使用`@函数名`的形式，写在要装饰的内容前面.

```typescript
function helloWorld(target:any) {
	// 输出Function HelloWorldClass(){} 是当前装饰的类
  	console.log(typeof target, target)
}

// 使用装饰器
@helloWorld
class HelloWorldClass{

}

var hello = new HelloWorldClass()
console.log(hello)
```

##  三、传参和不传参装饰器

### 1. 不传参装饰器(普通装饰器)

不传参装饰器就是不能传递参数, 但是可以动态扩展属性或者方法.

```typescript

function helloWorld(target:any) {
	// 输出Function HelloWorldClass(){} 是当前装饰的类
	console.log(typeof target, target)
	// 动态扩展对象属性
	target.prototype.name = 'panda';
	// 动态扩展对象方法
	target.prototype.say = function (): any{
		console.log('我是划水的狗子')
	}
}

// 使用装饰器
@helloWorld
class Person{
	name: string;
	age?: number;

	constructor(n: string) {
		this.name = n
	}
}

var p:any = new Person("法外之徒");
console.log(p.name); // 法外之徒
p.say(); // 我是划水的狗子
```

修饰器对类的行为的改变，是代码编译时发生的(不是TypeScript编译，而是js在执行机中编译阶段),而不是运行时.这意味着，修饰器能在编译阶段运行代码。也就是说，修饰器本质就是编译时执行的函数。

但是在是场景中,大多时间我们希望向装饰器传入一些参数, 所以要借助JavaScript中函数柯里化特性.这样就能实现在装饰器传参.

### 2. 传参数装饰器

装饰器传入参数我们发现可以打印出类的名字,target就是类的名字; params是我们传入的参数,同时我们可以通过prototypek可以重载类中的方法.

```typescript
function decoratorsClass(params: string) {
	return function (target: any) {
		// 传入的参数
		console.log(params);
		console.log('----')
		// 被装饰的类
		console.log(target);
    // 构造函数
    console.log(target.prototype.constructor)
    
		target.prototype.say = function (): void{
			// 使用传入的参数
			console.log(params)
		}
	}
}

@decoratorsClass('张三这个法外之徒终于伏法')
class Person{

	say(){}
}
let p = new Person();
p.say() // 张三这个法外之徒终于伏法
```

## 四、常用装饰器

### 1. 类装饰器

从上面例子看出, 若类的装饰器返回一个值, 就会使用这个返回的值替换被装饰类的声明,所以可以使用此特性修改类的实现,可以通过装饰器，来覆盖类里一些操作.

```typescript
function desc(target:any) {
  return class extends target{ 
	  gender = '男';
	  name = 'padna';
	  age = 10
    say() {
      console.log('我是重载过的');
    }
    
    getData(){
      console.log('获取数据')
    }
  }
}

@desc
class Person {
 
  public name: string ;
  public age: number;

  constructor(name: string, age:number) {
    this.name = name;
    this.age = age;
  }
  // 会被覆盖
	say() {
	  console.log('能说会道!')
  }
}

let p:any = new Person('哈哈', 20);
console.log(p.name, p.age, p.gender);
p.say() // 我是重载过的
p.getData()
```

上面例子,首先定义了一个装饰器，它返回一个类，这个类继承要修饰的类，所以最后创建的实例不仅包含原 Person类中定义的实例属性，还包含装饰器中定义的实例属性;在装饰器里给实例添加属性，设置的属性值会覆盖被修饰的类里定义的实例属性,所以创建实例的时候虽然传入了字符串,但name的值还是panda.

### 2. 属性装饰器

属性装饰器会在运行时当作函数被调用,会传入下面2个参数:

- target: 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
- propertyKey: 属性名字

属性装饰器没法操作属性的属性描述符，它只能用来判断某各类中是否声明了某个名字的属性。

```typescript
function value(v: string) {
	return function (target: any, attr: string) {
		console.log(typeof attr, attr)
		target[attr] = v
	}
}

class Person{
	@value('panda')
	public name: string | undefined;
	
	getName() {
		console.log(this.name)
	}
}
let person = new Person()
person.getName()
```

### 3. 方法装饰器

方法装饰器用来处理类中方法，它可以处理方法的属性描述符，可以处理方法定义。方法装饰器在运行时也是被当做函数调用，含 3 个参数：

- Target: 装饰静态成员时是类的构造函数，装饰实例成员时是类的原型对象；
- propertyKey: 方法的名字；
- descriptor: 方法的属性描述符。

**方法装饰器可以没有返回值, 若有返回值会被当做方法的属性描述符的对象.**

### 无返回值

**注意:若代码输出目标版本小于`ES5`，*属性描述符*将会是`undefined`**

```typescript
function get(target: any, propertyKey: string, desc: any) {
	console.log(target)
	console.log("被装饰的方法", propertyKey)
  console.log(desc)
	console.log(desc.value)
  // 扩展属性
  target.prototype.method = 'Get'
}

class HttpClient{
	@get
	getData() {
		console.log('获取数据成功')
	}
}
var http = new HttpClient()

http.getData()
```

上面案例我们发现,desc.value 输出的是函数名, desc 输出的是writable(可读的), enumerable(可以枚举的), configurable(可配置的)和value 这些属性.

我们当然也可以在方法装饰器中扩展类的属性或方法.

### 有返回值

有返回值会被当做被装饰方法的属性描述符的对象,**代码输出目标版本小于`ES5`，装饰器的返回值会被忽略**

```typescript
function desc(params: any) {
  return function (target: any, key: any, descriptor:any) {
    // 修改被装饰的函数的
	  let method = descriptor.value;
	// 修改被装饰方法的功能  
	descriptor.value = function (...args: Array<any>) {
		// 把参数转换成字符串类型
      args = args.map(it => String(it));
       console.log(args);
    }
  }
}
class Person {
  public name: string | undefined;
  public age: number | 0;

  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  @desc('装饰器上的参数')
  say(...args:any[]) {
    console.log('说的方法')
  }
}


let p = new Person('哈哈', 20);
console.log(p);
p.say('ha') // ['ha]
p.say('张三', 20, [20,30]) // ['张三','20',"20,30"]
```

### 四、方法参数装饰器

*参数装饰器*声明在一个参数声明之前（紧靠着参数声明).参数装饰器应用于类构造函数或方法声明, 参数装饰器不能用在声明文件、重载或其它外部上下文（比如 `declare`的类）里。

用来修饰方法中的参数.参数装饰器需要三个参数:

- Target: 当前对象的原型.
- propertyKey: 参数的名字
- index: 参数数组中的位置

参数装饰器特殊之处可以提供信息，给比如给类原型添加了一个新的属性，属性中包含一系列信息，这些信息就被成为「元数据」，然后就可以使用另外一个装饰器来读取「元数据」.

```typescript
function desc(params: string) {
  return function (target: any, methodName:any,  index:any) {
    console.log(target); // 类的原型
	console.log(methodName); // 被装饰的名字
    console.log(index); // 序列化
	// 原型添加元数据
	target.message = params
  } 
}
class Person {
  public name: string | undefined;
  public age: number | 0;

  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  say(@desc('参数装饰器') age: number) {
    console.log('说的方法')
  }
}


let p:any = new Person('panda', 20);
console.log(p);
p.say(20);
console.log(p.message)
```



### 五、访问器装饰器

*访问器装饰器*声明在一个访问器的声明之前。 访问器装饰器应用于访问器的 *属性描述符*并且可以用来监视，修改或替换一个访问器的定义。 访问器装饰器不能用在声明文件中（.d.ts），或者任何外部上下文（比如 `declare`的类）里。

**注意:TypeScript不允许同时装饰一个成员的`get`和`set`访问器.取而代之的是，一个成员的所有装饰的必须应用在文档顺序的第一个访问器上。这是因为，在装饰器应用于一个*属性描述符*时，它联合了`get`和`set`访问器，而不是分开声明的**

访问器装饰器表达式会在运行时当作函数被调用，传入下列3个参数：

- Target: 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。

-  propertyKey:成员的名字。

- Descriptor: 成员的属性描述符。

```typescript
function desc(params: boolean) {
  return function (target: any, propertyKey:any,  descriptor:any) {
    console.log(target); // 类的原型
	console.log(propertyKey); // 被装饰的名字
	console.log(descriptor); // 序列化
	// 修改描述符的对象值
	descriptor.enumerable = params
  } 
}
class Person {
	public _name: string | undefined;
	public _age: number | 0;

	constructor(name, age) {
		this._name = name;
		this._age = age;
	}

  // 设置为true,该属性才能出现在对象的枚举属性中
	@desc(true)
	get name() {
		return this._name  
	}

	// @desc(false) //error 不能向同名的存储器使用多个装饰器
	set name(name: string) {
		this._name = name
	}
	
  // 设置为false,枚举属性会少一个
	@desc(false)
	get age() {
		return this._age;
	}
	
	set age(age:number) {
		this._age = age
	}

}


let p = new Person('panda', 20);

console.log(p.name)
console.log(p.name = 'king')
for (let prop in p) {
    console.log(`property = ${prop}`);
}

```

## 五、常用装饰器的执行顺序

类中不同声明上的装饰器将按以下规定的顺序应用：

属性装饰>函数参数装饰器> 函数装饰器>类装饰器

```typescript
function decCls(params: string) {
  return function (target: any) {
    console.log('4.类的装饰器');
  }
}

function decMethod(params: string) {
  return function (target: any, key: string, descriptor: {[propsName: string]: any}) {
    console.log('3.类的函数装饰器');
  }
}

function decParams(params: string) {
  return function (target: any, name: string) {
    console.log('1.类属性装饰器');
  }
}

function decQuery(params: string) {
  return function (target: any, key: string, index: number) {
    console.log('2.函数参数装饰器');
  }
}

@decCls('类的装饰器')
class Person{
  @decParams('属性装饰器')
  public name: string | undefined;

  @decMethod('函数装饰器')
  getData(@decQuery('函数参数装饰器') age: number, @decQuery('函数参数装饰器') gender: string) {
    console.log('----');
  }
}
```

