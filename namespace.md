# 模块和命名空间

## 一、模块

模块是在自己范围内执行,不再全局范围内执行, 这样模块中声明的变量、函数、类等在模块外不可见. 要想使用模块内的变量、函数、类等必须先在模块中使用**export** 关键字导出,**然后在使用import**关键字导入才能使用.

模块是自声明的, 两个模块之间的关系是通过在文件级别上使用imports和exports建立的.

### 1. 导出和导入

导出使用export关键字, 导入使用import关键字.

```typescript
// db.ts
interface DBI<T>{
    add(info:T):boolean;
    update(info:T,id:number):boolean;
    delete(id:number):boolean;
    get(id:number):any[];
}
// 第一种导出方式
// export class MysqlDb<T> implements DBI<T>{
class MySqlDb<T> implements DBI<T>{

    constructor(){
        console.log('数据库建立连接');
	}
	// 增加数据
    add(info: T): boolean {
        console.log(info);
        return true;
    }    
    // 更新数据
	update(info: T, id: number): boolean {
		console.log('更新数据')
        throw new Error("方法没实现");
	}
	// 删除数据
    delete(id: number): boolean {
		console.log('删除数据')
		return true
	}
	// 获取数据
    get(id: number): any[] {
        var list=[
            {
                title:'xxxx',
                desc:'xxxxxxxxxx'
            },
            {
                title:'xxxx',
                desc:'xxxxxxxxxx'
            }
        ]
        return list;
    }
}

// 第二种导出方式
export { MySqlDb };
```

导入

```typescript
// user.ts
// 导入
import {MsSqlDb} from '../modules/db';

//定义数据库的映射
class UserClass{
    username:string | undefined;
    password:string | undefined;
}

// 类作为泛型类的参数
var UserModel = new MySqlDb<UserClass>();
// 导出
export {
    UserClass,
    UserModel
}
```

使用

```typescript
// index.ts
import {UserClass,UserModel} from './model/user';
//增加数据
var u=new UserClass();
u.username='张三';
u.password='12345655654757';
UserModel.add(u);


//获取user表数据
var res=UserModel.get(123);
console.log(res);
```

### 2. 默认导出

默认导出使用 `default`关键字标记；并且一个模块只能够有一个`default`导出.标记为默认导出的类和函数或变量的名字,在使用时可以省略.

```typescript
// b.ts
export default function getUser(): any[]{
	console.log('获取用户数据')
	return [
		{ "id": 1, "name": "张三" },
		{"id":2, "name":"法外之徒"}
	]
}
```

使用

```typescript
// index.ts
import getUser from '../modules/b'
console.log(getUser())
// 和上面效果一样,使用默认导出的类或函数或变量可以换成其它的
/*
*import a from '../modules/b'
*console.log(a())
*/
```



## 二、命名空间

在代码量较大的情况下，为了避免各种变量命名相冲突，可将相似功能的函数、类、接口等放置到命名空间内.

在TypeScript中, 命明空间就是之前的“内部模块”.TypeScript的命名空间可以将代码包裹起来，只对外暴露需要在外部访问的对象。命名空间内的对象通过export关键字对外暴露.只能访问通过export 暴露的变量、函数、类等, 否则错误.

```typescript
namespace X{
	interface Animal{
		name: string;
		run(): void;	
	}
	export class Dog implements Animal {
		name: string;
		constructor(theName: string) {
			this.name = theName
		}
		run() {
			console.log(`${this.name}在跑`)
		}
	}
}

namespace X{
	interface Animal{
		name: string;
		run(): void;	
	}
	export class Cat implements Animal {
		name: string;
		constructor(theName: string) {
			this.name = theName
		}
		run() {
			console.log(`${this.name}在跑`)
		}
	}
}

let dog = new X.Dog('田园犬')
dog.run()

let cat = new X.Cat('小橘')
cat.run()
```

### 分离到多个文件

可以把命名空间的定义以及实现部分拆分为一个文件, 而使用在其他文件, 只需要导入使用的命名空间就行.

```typescript
// modules/b.ts

// 注意: 此处要暴露整个命名空间
export namespace X{
	interface Animal{
		name: string;
		run(): void;	
	}
	export class Cat implements Animal {
		name: string;
		constructor(theName: string) {
			this.name = theName
		}
		run() {
			console.log(`${this.name}在跑`)
		}
	}

	export class Dog implements Animal {
		name: string;
		constructor(theName: string) {
			this.name = theName
		}
		run() {
			console.log(`${this.name}在跑`)
		}
	}
}
```

然后可以在使用的地方进行导入

```typescript
// index.ts
import { X } from "../modules/b";

let dog = new X.Dog('田园犬')
dog.run()

let cat = new X.Cat('小橘')
cat.run()
```



