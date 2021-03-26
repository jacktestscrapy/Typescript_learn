# 泛型

软件工程中,我们不仅要创建一致的定义良好的API,同时也要考虑重用性.组件不仅能支持当前的数据类型,同时也能支持未来的数据类型,这在创建大型系统时十分灵活.在Java中国可以使用泛型来创建可重用的组件, 一个组件可以支持多种类型数据.用户可以以自己的数据类型来使用组件.

**泛型就是解决类、 接口、 方法的复用性、 以及对不特定数据类型的支持.** 通俗理解指,在定义函数、接口或类的时候,不预先制定具体的类型,而在使用的时候再指定具体类型.

## 一、引入范型及使用

### 1. 引入范型

我们现在有这样需求, 创建一个函数,它返回任何传入的值.若不用泛型,我们可能使用联合类型,例如:

```typescript
function returnValue(arg: number | string | boolean ): any{
  return arg
}
```

这样是非常麻烦,或者我们也可以使用any, 但是会失去类型检查, 我们传入的和返回的可以类型不一致.

```typescript
function returnValue(arg: any ): any{
  return arg
}
```

因此，我们需要一种方法使返回值的类型与传入参数的类型是相同的.这里，我们使用了 ***类型变量***，它是一种特殊的变量，只用于表示类型而不是值。

**我们把returnValue函数叫做泛型**. 不同于使用 `any`，它不会丢失信息,保证传入类型和返回类型一致. 

**泛型:不预先确定的数据类型,具体的类型在使用的时候才确定**

```typescript
/*
* T 代表Type.在定义泛型时通常⽤作第⼀个类型变量名称
* K 代表Key, 表示对象中国的键类型.
* V 代表Value 表示对象中的值类型
* E 代表Element 表示元素类型
*/

function returnValue<T>(arg: T):T{
  return arg
}
```

### 2.使用泛型函数

定义泛型函数后有2种使用方法: 

第一种: 传入所有的参数，包含类型参数.此方法使用麻烦.

```typescript
function returnValue<T>(arg: T):T{
  return arg
}

// 第一种,传入所有的参数,包括类型参数
let v = returnValue<string>("panda")
```

第二种:利用了*类型推论* -- 即编译器会根据传入的参数自动地帮助我们确定T的类型.

```typescript
function returnValue<T>(arg: T):T{
  return arg
}
let v = returnValue('panda')
console.log(v)
```

## 二、泛型接口

在定义接口时,为接口中的属性或者方法定义泛型类型,在是一个接口时,再指定具体的类型.

```typescript

interface Log {
	// 泛型类型的函数
	<T>(arg:T):T
}

// 泛型函数
function getLog<T>(arg: T): T{
	return arg
}

let myLog: Log = getLog;
console.log(myLog(20)) // 20, 正确
console.log(myLog('panda')) // 正确, panda
```

还有另外一种方式, 就是我们想清楚的知道使用的泛型类型, 我们就要把**泛型参数作为整个接口的一个参数**, 这样接口里的其他成员也能知道这个参数的类型.

```typescript
interface Log<T> {
	(arg: T): T;
}

// 泛型函数
function getLog<T>(arg: T): T{
	return arg;
}

// 使用时必须传入一个泛型类型,否则出错.

let myLog: Log<string> = getLog;
// console.log(myLog(20)) // 错误, 类型不一致,
console.log(myLog('panda')) // 正确
```

## 三、泛型类

**在定义类时, 为类中的属性或方法定义泛型类型 在创建类的实例时, 再指定特定的泛型类型.**

和泛型接口一样, 直接把泛型类型放在类后面,可以让我们知道类所有属性都是用相同类型.

```typescript
class GetValue<T>{
	
	constructor(tel: T) {}
	// 静态成员不能使用泛型类型
	// static add(x: T) { }

	public list: T[] = [];
	
	run(x: T, y: T){
		return x
	};
}
let v = new GetValue('13212345678')
v.run('12', '123')
```

## 四、泛型约束

泛型约束作用:望限制每个类型变量接受的类型数量

```typescript
interface Inter{
	length: number
}
// T extends Inter 表示使用接口给泛型添加约束
function interFn<T extends Inter>(value: T): T{
	console.log(value, value.length)
	return value
}
console.log(interFn('123'))
// console.log(interFn(123))// error number 类型没有length的属性
```

###  在泛型约束中使用类型参数

泛型约束的另⼀个常⻅的使⽤场景就是检查对象上的键是否存在. 可以使用keyof 来先获取某类型的所有键,keyof返回的是联合类型.然后再结合extends 约束,来限制输入的属性名包含在keyof返回的联合类型中.

```typescript
// 使用K extends keyof T 获取T的所有属性.
// 再使用K判定要查找的属性是否存在
// 函数的返回值是key所对应的值
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K]{
	return obj[key]
}

let x = { a: 1, b: 2, c: 3, d: 4 }

console.log(getProperty(x,'a')) // 1
console.log(getProperty(x,'e'))// error 'e'不在x的属性中
```

## 五、泛型默认类型

当使⽤泛型时没有在代码中直接指定类型参数，从实际值参数中也⽆法推断出类型时，这个默认类型就会起作⽤. 泛型参数默认类型与普通函数默认值类似

```typescript
// T=string,泛型默认类型为string
interface A<T=string> {
	name: T;
}
const strA: A = { name: "panda" };
// 不使用默认类型
const numB: A<number> = { name: 101 };
```

## 六、在泛型里使用类类型

在泛型里使用类类型,就是定义一个类, 把类做为参数来约束数据传入的类型.

现在我们有下面需求:定义User类和ArticleCate类映射数据库字段, 定义DBOper类操作数据库,然后把User类和ArticleCate类作为参数传入到DBOper中.

```typescript
class DBOper<T>{
    // 定义泛型方法,增加数据
    add(info:T):boolean{
        console.log(info);       
        return true;
    }
    // 更新数据
    updated(info:T,id:number):boolean {
        console.log(info);  
        console.log(id); 
        return true;
    }
}

// 定义User
class User{
    username:string | undefined;
    password:string | undefined;
}


class ArticleCate{
    title:string | undefined;
    desc:string | undefined;
    status:number | undefined;
  
    constructor(params:{
        title:string | undefined,
        desc:string | undefined,
        status?:number | undefined
    }){
        this.title=params.title;
        this.desc=params.desc;
        this.status=params.status;
    }
}


let u = new User()
u.username='panda'
u.password = '123456'

// 若不使用User类就可以传入任意类型, 然后都可以使用类中方法
var db = new DBOper<User>();
db.add(u)

var a=new ArticleCate({
    title:'分类',
    desc:'1111',
    status:1
});
// 把ArticleCate作为参数传入.
var Db = new DBOper<ArticleCate>()
Db.add(a)
Db.updated(a, 12)
```

