# 标识注释

1. `obj` 对象
2. `arr` 数组
3. `str` 字符串
4. `RE` RegExp
5. `BL`/`bol` boolean
6. `length` 长度
7. `fn` function
8. `index` 下标
9. `——` 备注
10. `uri` 浏览器地址标识 uri
11. `num` 数字
12. `e` 元素
13. `start` 开始
14. `end` 结束
15. `:` 类型
16. `s`  元素

# 变量作用域与内存

## 变量声明

### let

### const 

1. const 声明的变量不能再次赋值
2. const 声明的对象可以对键进行赋值
3. 需要对对象无法赋值的情况使用 `Object.freeze()` 
4. const let 都是以块为作用域 有利于垃圾回收过早介入

### 标识符查找

> 在特定上下文读取或写入而引用一个标识符时

1. 开始：开始于作用域前端，找到及停
2. 未找到则沿作用域链进行查找
3. 原型链也会进行查找

## 垃圾回收

> 基本思路 ： 确定哪个变量不会使用即释放内存

### 标记清理

1. 垃圾回收程序运行时会标记内存中存储的变量
2. 然后会将所有在上下文中的变量及被引用的变量的标记去掉
3. 对仍有的标记的变量进行清除

### 引用计数

> 引用则+1 被覆盖则 -1 0 即释放

1. 循环引用会导致引用次数都是2
2. 函数多次循环引用会导致大量内存无法释放

### 内存管理

- 块级作用域在执行完超出上下文会自动解除引用
- 全局变量可以 赋值 null  `= null` 下次会被回收
- const let 有助于回收

### 内存泄漏

- 声明全局变量
- 定时器
- 闭包

# 基本引用类型

## Date

- 以1970.1.1开始计算
- 提供3个方法
  - `Date.parse()`  // 将传入的字符串时间转换为 毫秒 tips：当new Date('2012-12-5') 直接传入时间字符串时，会默认调用 Date.parse()
  - `Date.UTC()` // 将传入的参数转换为毫秒 格式固定
  - `Date.now()` // 返回代码执行时的时间毫秒
- 继承的方法
  - `toLoacleString()` // 返回与浏览器运行的本地时间一致的日期和时间
  - `toString()` // 返回带时区信息的
  - `valueOf()` // 不返回字符串 返回时间日期的毫秒表示
- 格式化
- 方法组件

## RegExp

> 正则表达式



### 示例方法

- `exec()`
- `test()`

## 原始包装类型

### Boolean

- `valueOf()` // 返回原始true或者false
- `toString()` // 返回 'true' 'false'

- `原始布尔值` `(true)` 和 `Boolean` `(new Boolean(false))`对象有区别
  - 对象在布尔运算中会转为true
  - typeof Boolean对象会转换为 object

### Number

#### 基础

- `valueOf()` // 返回原始数值
- `toString() toLoacleString()` // 返回数字的字符串

#### 数值格式化为字符串的方法

- `num.toFixed(n)` //  返回包含 n 位小数的字符串 ( 小数点过多会四舍五入 ，有精度问题)
- `num.toExponential()` // 返回科学记数法

#### isInteger

> 辨别一个数值是否保存为整数

### String

- `valueOf() toString() toLoacleString()` 都返回原始字符串

#### 操作方法

- `cancat`
- `slice(start,end)`
- `substr(start,length)`
- `substring(start,end)`
- `indexOf()`
- `lastIndexOf()`
- `startWith(str,start) => boolean`
- `endWith(str,start) => boolean`
- `includes(string) => boolean`
- `trim()`
- `trimLeft()`
- `trimRight()`
- `repeat() `复制多次并拼接
- `padStart(length,str) `复制字符串 如果小于长度则用 str 填充 如果没有str则是空
- `padEnd(length,str)`
- `[...str] `可以解构为数组
- `for-of `可以迭代遍历字符串
- `大小转换`
- **匹配**
- `match(RE)`
- `search(RE) => index`
- `replace(str/RE,str) `使用str替换符合RE的所有的字符 如果第一个是str则之替换第一个匹配 RE需要带全局标记才会修改全部匹配

 

## 单例内置对象

> 任何由 ECMAScript 实现提供、与宿主环境无关，并在 ECMAScript 程序开始执行时就存在的对象

### Global

> 全局的兜底对象

#### URL编码

1. `encodeURI(uri)` 对URI进行编码 仅转换空格
2. `encodeURIComponent(uri)` 对URI进行编码 替换所有无效字符  —— 常用
3. `decodeURI(uri) `对应解码
4. `decodeURIComponent(uri)` 对应解码

#### eval()

> ECMAScript解释器

1. 可以获取调用所在上下文
2. 外部无法访问

#### Global对象属性

- undefined
- NAN
- Infinity
- Object
- Array
- Function
- Boolean
- String
- Number
- Date
- RegExp
- Symbol
- Error
- EvalError
- RangeError
- ReferenceError
- SyntaxError
- TypeError
- URIError

#### window 对象

> 是Global对象的代理

**获取Global**

```js
let global = function() {
 return this;
}(); 
```

- 当一个函数在没有明确 （通过成为某个对象的方法，或者通过 call()/apply()）指定 this 值的情况下执行时，this 值等于 Global 对象

### Math

1. **比值**
2. `Math.min(num1,num2...)` 返回一组数字的最小值
3. `Math.max(num1,num2...)` 返回一组数字的最大值
4. **舍入**
5. `Math.ceil()` 向上舍入
6. `Math.floor()` 向下舍入
7. `Math.round()` 四舍五入
8. `Math.fround()` 方法返回数值最接近的单精度（32 位）浮点值表示
9. **获取值**
10. `Math.random()` 获取 0 - 1 之间的随机值
11. **绝对值**
12. `Math.abs(num)` 获取num的绝对值

# 集合引用类型

## Object

## Array

### 创建

1. `Array.from()` 将类数组转换为数组
2. `Array.of()` 将一组参数转换为数组

### 检测数组

1. `instanceof`
2. `Array.isArray(arr)`

### 迭代方法

- `arr.keys() `遍历下标
- `arr.values()` 遍历value
- `arr.entries()` 遍历之后生成 键对值 [key,value]
- `for(let [key,value] of arr.entries()) {}` 结构 + 迭代

### 复制填充

- `fill()` 填充
- `coprWithin(index1,index2)` 复制 从索引index1开始插到 index2的位置

### 转换方法

- `toLoacleString()` 返回字符串的 ，拼接 但是调用的是值得 toLoacleString()  方法
- `toString()`  返回每个值等效的字符串用 ，拼接
- `valueOf()`  返回数组本身

### 栈方法

- `push()`  
- `pop()`  删除最后一项 并返回删除的元素

### 队列方法

- `shift()` 删除第一项并返回
- `unshift()` 添加到数组钱

### 排序方法

- `reverse()`  反向排列
- `sort(fn)` 升序排列 或者 按照 fn 规定的规则排列 第一个值排第一则返回1 否则返回-1

### 操作方法

- `concat(arr1,arr2)` 数组合并 

  - 默认强制打平 `newColors[Symbol.isConcatSpreadable] = false`

  - 不打平

    ```js
    newColors[Symbol.isConcatSpreadable] = false; 
    let colors2 = colors.concat("yellow", newColors); 
     ["red", "green", "blue", "yellow", ["black", "brown"]] 
    ```

- `slice()`

  - 删除  `slice(index,length)`
  - 插入 `slice(index,0,e,e2...)` 从index位置删除0个元素 插入 e,e2...
  - 替换  `slice(index,length,e,e2...)` 从index位置删除length个元素 插入 e,e2...

### 搜索和位置方法

#### 严格相等

- `indexOf()`
- `lasetIndexOf()`
- `includes() => boolean` 表示是否至少找到一个与指定元素匹配的项 是全等 ===

#### 断言函数

- `find()`  返回第一个匹配的元素
- `findIndex() `返回第一个匹配的元素的下标

### 迭代方法

- `every(fn)`  每一项运行fn 都true才会返回true
- `filter(fn)` 返回符合fn的元素的数组
- `forEach(fn)` 每一项都运行 fn
- `map(fn)` 每一项都运行 fn 并返回运行fn组成的数组
- `some(fn)` 每一项运行fn 一项为true就返回true

### 归并方法

- `reduce(fn)`
- `reduceRight(fn)`

## 定型数组

## Map

> 键/值存储机制

### 基本API

-  `new Map([['key1','val1'],['key2','val2']])` 
- `set('key','val')` 
- `get(key)` 
- `has(key) => BL`
- `size`
- `delete(key)`
- `clear()`

### 顺序与迭代

- `for(let m of map.entries()) {}`
- `[...map]` 转数组
- `forEach`

## Set

### 基本API

- `new Set()`
- `add()`
- `has()`
- `size`
- `delete()`
- `clear()`

### 顺序迭代

- `values()`
- `entries()`

# 迭代器与生成器

## 迭代器

### 理解迭代

- 循环是迭代的基础
- 迭代在一个有序集合上进行

### 迭代器模式

- 把有些结构称为“可迭 代对象”（**iterable**）
- **元素是有限的**
- 无歧义的**遍历顺序**

### 可迭代协议

#### 实现Iterable需要具备的能力

- 支持迭代的自我识别能力
- 创建实现 Iterator 接口的对象的能力
- 在 ECMAScript 中，这意味着必须暴露一个属性作为“**默认迭代器**”，而 且这个属性必须使用特殊的 Symbol.iterator 作为键

#### 内置类型包含Iterable

- 字符串
- 数组
- 映射
- 集合
- arguments 对象
- NodeList 等 DOM 集合类型

#### 接收可迭代对象的原生特性

- for...of
- 数组解构
- 扩展操作符
- Array.form()
- 创建集合
- 创建映射
- Promise.all()
- Promise.race()
- yield* 操作符

#### 迭代器协议

##### 解释

- `迭代器`是一种一次性使用的对象，用于迭代与其关联的可迭代对象
- 使用 `next()` 迭代
- next() 调用成功后返回 `IteratorResult` 对象 
  - `done: BL` 为 true 结束
  - `value` 可迭代的下一个值
  - 当 `value` 值为 `undefined` 后 `done` 为 `true`
- 不同迭代器没有联系
- 可迭代对象被修改 next() 也会响应变化
- 迭代器指向可迭代对象的引用 会阻止垃圾回收

##### 自定义迭代器

> 为没有迭代器的添加自定义的迭代

- `[Symbol.iterator]() {}` 需要return {next () {}} 用于迭代

- `next()`

- ```js
  class Count {
  	constructor(limit) {
  		this.limit = limit
  	}
    	// 自定义的迭代器  
  	[Symbol.iterator]() {
  		let limit = this.limit,
  				count = 1
  		return {
              // 需要return next()
  			next() {
  				if(count <= limit) {
  					return {done:false,value:count++}
  				}else{
  					return {done:true,value:undefined}
  				}
  			}
  		}
  	}
  }
  
  let c = new Count(3)
  let f = new Count(10)
  // 有了 [Symbol.iterator]() 可以迭代
  for(let item of c) {
  	console.log(item)
  }
  for(let i of f) {
  	console.log(i)
  }
  ```

##### 终止迭代

- `for-of`  `return break continue throw`

- 结构操作并未消费所有值

  - return 需返回一个 `IteratorResult`

  - 迭代器没有关闭可以从上次地方继续迭代

  - ```js
    let arr = [1,1,1,2,3,5,4]
    let ite = arr[Symbol.iterator]()
    for(let i of ite) {
    	console.log(i)
    	if(i > 3) {
    		break
    	}
    }
    console.log('----------------------')
    for(let i of ite) {
    	console.log(i)
    }
    ```

## 生成器

### 基础

- 定义：`*fn(){}`
- 不能是箭头函数
- `*`不受两侧空格影响

### yield

- 可以用于停止和执行生成器

- 生成器在遇到yield关键字之前会正常执行

- 遇到yield知乎停止执行 函数作用域会保存

- 停止的生成器只能通过 next 继续执行

- **只能用在生成器中**

- 生成器可作为可迭代对象

- 可以循环生成

- ```js
  function* generatorFn(n) {
  	while(n--) {
  		yield n
  	}
  }
  ```

- 实现输入和输出

- 产生可迭代对象

  - `yield* [1,2,3]` 生成可迭代对象 类似于循环生成 `yield*`

- `yield*`实现递归算法

### 生成器作为默认迭代器

```js
class Foo {
	constructor() {
		this.value = [1,2,3,4,5,6]
	}
	*[Symbol.iterator]() {
		yield* this.value
	}
}
```

### 提前终止生成器

- `next()`
- `return()`
- `throw`

# 对象、类与面向对象变编程

## 理解对象

### 属性的类型

1. 数据属性

   - `[[Configurable]]` 
     - 是否可以delete
     - 是否可以重新定义
     - 是否可以修改特性
     - 特性是否可以改为访问器属性
     - 默认为true
   - `[[Enumerable]]`
     - 是否for-in
     - 默认true
   - `[[Writable]]`
     - 是否可修改
     - `Writable` 设置为`false`之后无法被修改 在严格模式时对其进行修改会报错
   - `[[Value]]`
     - 包含的实际值
   - `Object.defineProperty(obj,str,{})`
     - 对对象的 str 属性的 `Configurable` `Enumerable` `Writable` `Value` 进行操作

2. 访问器属性

   - `[[Configurable]]`

     - 可否delete 
     - 可否重定义
     - 可否改为数据属性
     - 默认true

   - `[[Enumerable]]`

     - 可否for-in

   - `[[Get]]`

     - 获取函数 默认undefined

   - `[[Set]]`

     - 设置函数 默认undefined
     - 不要给当前监听的属性赋值 会超出栈

   - ```js
     let obj = {
     	home:2020,
     	status:'init',
     	year:2020
     }
     Object.defineProperty(obj,"home",{
     	get() {
     		return this.home
     	},
     	set(newValue) {
     		if(newValue >= 2020) {
     			this.year = 2020
     			this.status = 'init'
     		}else{
     			this.year = newValue
     			this.status = 'edit'
     		}
     	}
     })
     obj.home = 2009
     console.log(obj.status)
     ```

### 定义多个属性

- `Object.defineproperties(obj,{name:{set(){},get(newValue){},value:value},...})` 
- 对多个属性同事进行操作

### 读取属性特性

- `Object.getOwnPropertyDescriptor(obj,name)`
  - 获取对象属性的特性
  - `value`  `configurable`  `set`  `get`  `enumerable`  `writable`
- `Object.getOwnPropertyDescriptors(obj)`
  - 获取对象所以属性的特性

### 合并对象

- `Object.assign(obj1,obj2) => obj`
- 是浅复制
- 前面的对象属性有同名会被后面的覆盖

### 对象标识及相等判断

- `Object.is(s1,s2)`

### 增强的对象语法

1. 属性简写
   - `{name}`
2. 可计算属性
   - 对象键名可以动态赋值
   - `{ [name] : '' }`
   - `{ [fn()] : ''}`
3. 简写方法
   - `{ set() { }}`
   - `{ [name] () {}}`

### 对象解构

1. 基础解构
   
   - `let {str} = {str:str}` 为str赋值
   - `let { str : name } = {str:''}` 为name赋值
   
2. 嵌套解构

   - ```js
     let obj = { name : str }
     let copy = {}
     // 需要用 ( ) 包裹代码
     ({ name : copy.name } = obj) // 为copy复制name参数
     ```

3. 部分解构

   - 涉及多个属性解构复制当有错误时整个解构只会完成一部分
   - `try{} catch {}`

4. 参数上下文匹配

   - 在`函数列表中`解构赋值 不影响`arguments`，单可以在函数中使用局部变变量
   - `function fn(name,{age,height}) {}`

## 创建对象

### 工厂模式

1. 使用函数`return obj`
2. 没有解决对象标识问题

### 构造函数模式

#### 基础

1. `创建构造函数` 使用 `new` 使用创建对象
2. 构造函数首字母大写
3. new 对象会执行
   - 在内存中创建一个对象
   - 这个新对象内部的 `[[Prototype]]` 特性被赋值为构造函数的`prototype`属性
   - 构造函数内部的this被赋值为这个新对象
   - 执行构造函数内部代码
   - 如果构造函数返回非空对象则返回该对象；否则，返回刚创建的新对象
4. `constructor` 可以获取构造函数
5. `instanceof`可以判断是否是该构造函数

#### 构造函数也是函数

1. 构造函数与普通函数区别是**调用方式不同**
2. 构造函数也可以直接调用 **this 执行window**

#### 构造函数的问题

- 相同构造函数创建的不同对象 方法不等 `console.log(person1.sayName == person2.sayName); // false`

- 可以将方法定义在外 内部引用即可 这样不同对象方法是同一个

- 虽然不同的对象现在公用一个共同全局作用域但是作用域也乱了 可用 **原型模式**来改变

- ```js
  function Person(name, age, job){
   this.name = name;
   this.age = age;
   this.job = job;
   this.sayName = sayName;
  } 
  ```

#### 原型模式

**理解原型**

1. 每个函数会创建一个 **prototype** 包含应该特定引用类型的实例共享的属性与方法

2. 可以通过 **prototype** 进行共享属性及方法

3. `Object.setPrototypeOf(obj = 需要设置原型的对象,prototype = 新原型 obj / null)` 想实例私有特性 `[[Prototype]]`写入新值

   - 会涉及所有访问`[[prototype]]`对象的代码
   - **会严重影响代码性能**

4. 使用 `Object.create(prototypeObj)`来创建新对象，并为其指定原型

   - 创建对象 并且对象的原型是 `prototypeObj`

   - ```js
     let biped = {
      numLegs: 2
     };
     let person = Object.create(biped);
     person.name = 'Matt';
     console.log(person.name); // Matt
     console.log(person.numLegs); // 2
     console.log(Object.getPrototypeOf(person) === biped); // true 
     ```

**原型层级**

- 读取对象属性会先查找 对象 再查找 prototype
- 当实例有参数时 **原型上的参数会被遮盖** 无法访问
  - `hasPrototypeProperty` 判断为false

**原型和in操作符**

- `str in obj => bol` 判断 原型是否有参数
- `hasOwnProperty` 仅判断实例

**属性枚举顺序**

1. 不确定
   - `for-in`
   - `Object.keys()`
2. 确定(升序枚举数值键)
   - `Object.getOwnPropertyNams()`
   - `Object.getOwnPropertySymbols()`
   - `Object.assign()`

### *对象迭代

- `Object.values(obj)`
- `Object.entries(obj)`

## 继承

### 原型链



# 代理与反射

## 代理基础

### 创建空代理

1. `new Proxy(obj = 代理对象,handler = 处理对象)`
2. 代理对象没有 `prototype`
3. 无法使用 `instanceof Proxy`

### 定义捕获器

> 基本操作的拦截器
>

1. 基本操作的拦截器

2. 可以包含对个捕获器

3. 代理在操作传播到目标对象之前先调用捕获器函数，从而拦截并修改相应的行为

4. ```js
   let obj = {
   	name: 'yu'
   }
   let handler = {
   	get() {
   		return 'editName'
   	}
   }
   const proxy = new Proxy(obj, handler)
   console.log(proxy.name) // 'editName'
   ```

### 捕获器参数和反射API

- `get(target = 目标对象,property = 属性键,receiver = proxy对象){}`
- `Reflect` 全局的 原始行为
  - 不需要 `return target [property]`
  - `return Reflect.get(...argument)`
  - `handler = {get:Reflect.get}`
  -  `new Proxy(target, Reflect)` 空代理

### 捕获器不变式

> 在数据不可改变时报错

### 可撤销代理

1. 解释：中断代理对象与目标对象之间的关系
2. revocable()
   - 支持撤销代理对象与目标对象的关联
   - 撤销操作不可逆
   - 撤销函数是幂等 （多次撤销效果一样）
   - 撤销之后调用代理报错

### 实用反射API

#### 反射API与对象API

1. 反射API不限于捕获处理程序
2. 大多数反射API方法在Object类型上有对应的方法

#### 状态标记

1. 含义：很多反射方法返回**状态标记**的布尔值 标识意图执行的操作是否成功
2. 提供状态标记的方法
   - `Reflect.defineProperty()`
   - `Reflect.preventExtensions()`
   - `Reflect.setPrototypeOf()`
   - `Reflect.set()`
   - `Reflect.deleteProperty()`

#### 用一等函数替代操作符

1. `Reflect.get()`  替代访问操作符
2. `Reflect.set()` 替代 = 赋值操作符
3. `Reflect.has()` 替代 in wit()
4. `Reflect.deleteProperty()` 替代delete
5. `Reflect.construct()` 替代 new 操作符

#### 安全的应用函数

1. 问题：通过apply调用函数时，被调用的函数可能定义了自己的apply 
2. 解决：`Reflect.apply(fn,val,argument)`

### 代理另一个代理

1. 意义：实现多层拦截

### 代理的问题与不足

1. **this问题**

2. 代理与内部槽位

   - 部分内置类型可能无法代理
   - Date类型的方法依赖于对象内部槽位 [[NumberDate]] 但是代理对象上没有这个槽位 操作报错

   

   



## 代理捕获器与反射方法

### get()

1. 基本：`get(target,property,receiver) {return Reflect.get(...arguments)}`
2. 反射API：`Reflect.get()`
3. 参数：
   - target = 目标对象
   - property = 键
   - receiver = proxy对象
4. 拦截方式
   - `proxy.property`
   - `proxy[property]`
   - `Object.creat(proxy) [property]`
   - `Reflect.get(peoxy,property,receiver)`
5. 捕获器不变式
   - `target.property` 如果不可修改 则处理返回值必须与target.property相同
   - `target.property` 不可配置切且 **[[Get]]** 为**undefined** 则处理返回值必须是**undefined**

### set()

1. 基本：`set(target,property,value,receiver){return Reflect.set(...arguments)}`
2. 返回值：BL 严格模式TypeError
3. 参数：
   - target = 目标对象
   - property = 键
   - value = 值 
   - receiver = proxy对象
4. 拦截方式：
   - `proxy.property = value`
   - `proxy[property] = value`
   - `Object.creat(proxy) [property] = value`
   - `Reflect.set(proxy,property,value,receiver)`
5. 捕获器不变式
   - target.property 不可变则不能修改
   - target.property不可配置切**[[Set]]** 为**undefined**则不可修改目标值

### has()

1. 基本：`has(target,property){return Reflect.has(...arguments)}`
2. 返回值：BL
3. 参数：
   - target = 目标对象
   - property = 键
4. 拦截方式
   - `property in proxy`
   - `property in Object.creat(proxy)`
   - `with(peoxy) { (property)}`
   - `Reflect.has(proxy,property)`
5. 捕获器不变式
   - target.property存在且不可配置 则必须返回true
   - target.property 存在且不可扩展 则必须返回true

### defineProperty()

### getOwnPropertyDescriptor()

### deleteProperty()

1. 基本：`deleteProperty(target,property){return Reflect.deleteProperty(...arguments)}`
2. 返回值：BL
3. 参数：
   - target = 目标对象
   - property = 键
4. 拦截方式
   - `delete proxy.property`
   - `delete proxy[property]`
   - `Reflect.deleteProperty(proxy,property)`
5. 捕获器不变式
   - target.property存在且不可配置 则必须返回true

### ownKeys()

### getPrototypeOf()

### setPrototypeOf()

### isExtensible()

### preventExtensions()

### apply()

1. 基本：`apply(target,thisArg,...argumentsLists) {return Reflect.apply(...arguments)}`
2. 参数：
   - target = 目标对象
   - thisArg = 调用函数时的this参数
   - argumentsLists = 调用函数时的参数列表
3. 拦截方式
   - `proxy(...argumentsList)` 
   - `Function.prototype.apply(thisArg, argumentsList)` 
   - `Function.prototype.call(thisArg, ...argumentsList)` 
   - `Reflect.apply(target, thisArgument, argumentsList)`
4. 捕获器不变式
   - target 必须是一个函数对象

### construct()

1. 基本：`construct(target,argumentsLists,newTarget) {return Reflect.construct(...arguments)}`
2. 返回值：obj
3. 参数：
   - target = 目标构造函数
   - argumentsLists = 传给目标函数的参数列表
   - newTarget = 最初被调用的构造函数
4. 拦截方式
   - `new proxy(...atgumentsLists)`
   - `Reflect.construct(target,argumentsLists,newTarget)`
5. 捕获器不变式
   - target必须可以用作构造函数

## 代理模式

> 使用代理可以在代码中实现一些有用的编程模式

1. 跟踪属性访问
   - 监控对象属性被访问 更改 查询
2. 隐藏属性
   - 方便隐藏属性
3. 属性验证
   - 判断是否需要set
4. 函数与构造函数参数验证
   - 对函数及构造函数进行审查
5. 数据绑定与可观察对象
   - proxy代理观察数据

# 函数

## 箭头函数

1. 语法：`() => {}`
2. 不能使用场景：
   - arguments
   - super
   - new.target
   - 不能用于构造函数
   - 没有 property 属性

## 函数名

1. 函数可以有多个名称

2. `newFn =  fnName` 函数赋值不带()时 仅赋值函数指针

   ```js
   function sum(num1, num2) {
    return num1 + num2;
   }
   let anotherSum = sum; 
   ```

## 理解参数

1. 参数数量没有限制
2. arguments
   - 非箭头函数可以访问arguments
   - 是一个类Array的对象
3. 箭头函数只能通过命名的参数进行访问

## 没有重载

1. 无法定义多个同名函数

## 默认参数

1. 基本：`fn(name = '') {}`
2. 参数：原始类型 对象 函数返回值
3. 使用：箭头函数也是可以使用 但是需要加上()
4. 注意：
   - 赋值为undefined相当于没有赋值
   - arguments不反应默认参数
5. 暂时性死区：
   - 前面定义参数不能引用后面定义的参数 反之可以
6. 参数作用域
   - 不能引用函数体的作用域

## 参数扩展与收集

### 扩展参数

1. 基本：传入的参数可以传递一个扩展的数组
2. 示例：`fn(a,b,c = 10) fn(...[1,2])`

### 收集参数

1. 基本：命名参数需要放在前面 收集参数只能放在最后
2. 收集参数不影响arguments对象
3. 箭头函数虽然没有arguments但是任然可用

## 函数声明与函数表达式

1. **函数声明提升**
   - 声明函数会在代码执行前 将函数声明提升到源码树顶部
   - 函数赋值变量时并不会发生提升
   - `fn()` 会 
   - `fn = function()` 不会

## 函数作为值

1. 基本：`fn(fn1,str)`
2. 参数：
   - fn1 = 函数名 不能加()

## 函数内部

### arguments

1. 基本：报含所有的参数的类数组对象

2. **callee**: 指向arguments对象所在函数的指针

3. 阶乘：

   ```js
   function fn(num) {
   	if (num <= 1) {
   		console.log('return 1')
   		return 1
   	} else {
   		console.log(num, num - 1)
   		return num * arguments.callee(num - 1)
   	}
   }
   console.log(fn(10))
   ```

### this

1. 箭头函数this指向定义该函数的上下文

### caller

1. 作用：调用当前函数的函数
2. 注意：
   - 在全局作用域调用为 unll
   - 严格模式下访问报错

### new.target

1. 引用调用new.target 是undefined
2. new 构建的返回构造函数

## 函数属性与方法

1. **属性**
   - `length`  返回函数定义的命名参数的个数  `fn.length`
   - `prototype` 保存所有实例方法  **不可枚举**
2. **方法**
   - `apply(this,arr/arguments)`
     - 用来指定this值调用函数
     - 参数 this 和Array或者arguments对象
   - `call(this,s,s...)`
     - 同上
     - 区别：参数
   - `bind(this,s,s...)`
     - 会创建一个新函数并且函数this指向提供的this
     - 除了第一个是this其他都是参数

## 函数表达式

**函数声明**

1. 会有**函数声明提升**的问题

**函数表达式**

1. `let fn = function(){}`
2. 是**匿名函数**
3. 不会有**函数声明提升**的问题

**注意**

```js
if (condition) {
 function sayHi() {
 console.log('Hi!');
 }
} else {
 function sayHi() {
 console.log('Yo!');
 }
} 
```

1. 因为函数声明提升这样写会有问题
2. js引擎会纠正其声明
3. 不同浏览器纠正方法不同
4. 可以使用函数表达式

## 递归

1. 解释：函数递归通常是自己调用自己
2. 问题
   - 函数如果赋值给其他变量可能会有问题
   - 使用 **arguments.call()**来调用
   - 严格模式下可能无法使用 arguments.callee()
   - 可以使用 函数表达式

## 尾调优化

1. 描述：es6新增的管理优化机制 暂时还不确认浏览器是否跟进

2. ```js
   function outerFunction() {
    return innerFunction(); // 尾调用
   } 
   (1) 执行到 outerFunction 函数体，第一个栈帧被推到栈上。
   (2) 执行 outerFunction 函数体，到 return 语句。计算返回值必须先计算 innerFunction。
   (3) 执行到 innerFunction 函数体，第二个栈帧被推到栈上。
   (4) 执行 innerFunction 函数体，计算其返回值。
   (5) 将返回值传回 outerFunction，然后 outerFunction 再返回值。
   (6) 将栈帧弹出栈外。
   在 ES6 优化之后，执行这个例子会在内存中发生如下操作。
   (1) 执行到 outerFunction 函数体，第一个栈帧被推到栈上。
   (2) 执行 outerFunction 函数体，到达 return 语句。为求值返回语句，必须先求值 innerFunction。
   (3) 引擎发现把第一个栈帧弹出栈外也没问题，因为 innerFunction 的返回值也是 outerFunction
   的返回值。
   (4) 弹出 outerFunction 的栈帧。
   (5) 执行到 innerFunction 函数体，栈帧被推到栈上。
   (6) 执行 innerFunction 函数体，计算其返回值。
   (7) 将 innerFunction 的栈帧弹出栈外。
   ```

3. 优化：优化之后会少一个栈

4. 条件：

   - 严格模式
   - 外部函数的返回值是对尾调函数的调用
   - 尾调函数返回后不需要执行额外的操作
   - 尾调函数不是引用外部函数作用域中自由变量的闭包、

5. ```js
   "use strict";
   // 基础框架
   function fib(n) {
    return fibImpl(0, 1, n);
   }
   // 执行递归
   function fibImpl(a, b, n) {
    if (n === 0) {
    return a;
    }
    return fibImpl(b, a + b, n - 1);
   } 
   ```

## 闭包

1. 定义：引用了另一函数作用域中变量的函数

2. 问题：闭包会引用函数内的变量导致函数执行完毕不会被回收

3. this：

   - 非箭头函数

     - 函数上下文

     - 全局调用时 非严格模式是 **window** 严格模式是 **undefined**

     - 作为某个对象的方法调用this等于这个对象

     - 在**对象中调用时** `匿名函数`不会绑定到对象 会绑定到window或者undefined  如下：

     - ```js
       window.name = 'ooo'
       let obj = {
       	name: 'yu',
       	con() {
       		return function () {
       			console.log(this.name)
       		}
       	}
       }
       obj.con()() // ‘ooo’
       ```

   - 箭头函数 this 指向调用他的上下文

4. 内存泄漏

   - ie9之前回收机制不同
   - HTML元素保存在闭包作用域中元素不能销毁
   - 必须手动将HTML元素设为null才行

## 立即调用的函数表达式

1. 解释：立即调用的匿名函数又叫 **立交调用的函数表表达式**
2. 表达式：`(function(){})()`
3. es6 IIFE没有那么必要 因为块级作用域无需IIFE就可以实现

## 私有变量

1. 含义：任何定义在函数或块中的变量都可以认为是私有的 
2. 特权方法：能够访问函数私有变量的共有方法

### 静态私有变量

1. 对象的变量
2. 可以为对象添加特权方法访问变量
3. `Obj.perototype.fn`私有变量

### 模块模式

1. 单例对象实现相同的隔离与封装

2. ```js
   let singleton = function() {
    // 私有变量和私有函数
        let privateVariable = 10;
        function privateFunction() {
        return false;
        }
    // 特权/公有方法和属性
    return {
        publicProperty: true,
        publicMethod() {
        privateVariable++;
        return privateFunction();
        }
    };
   }(); 
   ```

### 模块增强模式

1. 在返回对象之前对其进行增强

# Promise与异步函数

## 异步编程

### 同步与异步

1. **同步行为**：顺序执行处理器指令
2. **异步行为**：类似于系统中断，即当前进程外部的实体可以触发代码执行

### 以往的异步编程

1. 通过回调实现异步

## Promise

### Promise基础

#### 创建

1. `new Promise(fn)`

#### 状态

1. `pending `   待定
2. `fulfilled`  兑现 **(resolved  解决)**
3. `rejected` 拒绝
4. 状态**不可逆**
5. 状态是**私有的** js无法检测状态

#### 结果参数，拒绝理由 

1. 用途：
   - 表示异步
   - 访问返回值

#### 控制状态

1. `resolve()`  兑现
2. `reject()`  拒绝
3. 只能改变一次

#### Promise.resolve()

1. Promise并非已开始就是待定状态
2. 通过`Promise.resolve()` 可以实例化一个解决的Promise
3. 以上的方法可以将任何值转换为Promise
4. 幂等

#### Promise.reject(err)

1. 实例化一个错误状态的Promise
2. err: 错误理由
3. try/catch 不能捕获 Promise.reject()的报错

### Promise的实例方法

#### thenable接口

1. 任何对象都有then() 这个放大被认为实现了thenable接口

#### Promise.prototype.then(onResolved  ,onRejected )

1. 作用：为Promise 添加处理程序的主要方法
2. 参数
   - onResolved  成功执行 
   - onRejected   失败执行  ( 只想添加 onRejected 的话 onResolved = undefined )

#### Promise.prototype.catch()

1. 作用：为Promise添加拒绝方法
2. 参数：onRejected 方法
3. 语法糖：`Promise.then(null,onRejected)`

#### Promise.prototype.finally()

1. 作用：在解决或者拒绝状态时执行
2. 无法知道具体状态

#### 非重入Promise方法

1. then catch finally 会进入微任务

#### 邻近处理程序的执行顺序

1. then catch finally 的执行顺序按照书写顺序

#### 传递解决值和拒绝理由

1. `resolve(res)`
2. `reject(err)`
3. `finally(res)`

#### 拒绝与错误处理

#### Promise连锁与合成

**连锁 / 解决回调地狱**

1. 多个then实现顺序异步

2. ```js
   let p1 = new Promise((resolve, reject) => {
    console.log('p1 executor');
    setTimeout(resolve, 1000);
   });
   p1.then(() => new Promise((resolve, reject) => {
    console.log('p2 executor');
    setTimeout(resolve, 1000);
    }))
    .then(() => new Promise((resolve, reject) => {
    console.log('p3 executor');
    setTimeout(resolve, 1000);
    }))
    .then(() => new Promise((resolve, reject) => {
    console.log('p4 executor');
    setTimeout(resolve, 1000);
    })); 
   ```

**期约图**

1. 顺序：Promise的顺序是添加的顺序

**Promise.all()与Promise.race()**

1. `Promise.all([]) => promise`

   - 作用：多个Promise对象组合为一个Promise对象

   - 参数：Array

   - 说明：

     - 可迭代参数的元素会通过 Promise.resolve()转换为Promise对象
     - `[]` 相当于 `promise.resolve()`
     - Promise.all()  会报错

   - resolve: 只有包含的Promise都解决才会resolve，其中有一个reject整个都会reject

   - reject: 其中一个拒绝则会拒绝 则第一个拒绝的理由为最终理由

   - ```js
     // 没一个Promise的值 可以用与在Promise使用  有顺序
     let str = ''
     
     let p1 = new Promise((resolve, reject) => {
     	str = str + 'yu'
     	resolve(str)
     })
     let p2 = new Promise((resolve, reject) => {
     	str = str + 'hong'
     	resolve(str)
     })
     let p3 = new Promise((resolve, reject) => {
     	str = str + 'de'
     	resolve(str)
     })
     let p = Promise.all([p1, p2, p3])
     p.then(res => {
     	console.log(res)
     })
     ```

2. `Promise.race([]) => promise`

   - 作用：返回一组Promise中最先完成的那个 新Promise
   - 参数：可迭代的Promise集合
   - 说明：
     - 可迭代参数的元素会通过 Promise.resolve()转换为Promise对象
     - `[]` 相当于 `promise.resolve()`
     - Promise.race()  会报错
   - 状态：以第一个完成的Promise的状态为最终状态
   - 阻塞：第一个完成的Promise**不会阻塞**其他Promise的执行

**？串行Promise合成**

### Promise扩展

#### Promise取消 (ES6暂未实现)

1. 描述：当不需要继续执行Promise时，此时需中断取消Promise的执行，但是Es6并未提供相关方法

#### Promise进度

1. 描述：了解当前Promise的执行进度
2. Es6并未提供相关进度追踪方法

## 异步函数

> ES8 提供的 async/await

### 异步函数

#### async

1. 作用：用于声明异步函数
2. 范围：函数声明 、函数表达式 、箭头函数 、方法 
3. return 
   - 异步函数使用`return`返回值 会被Promise.resolve()包装成一个Promise对象
   - 没有return则返回undefined
   - 异步函数始终返回Promise对象
   - 返回常规值 `then()` 可以直接解包 可以直接使用
4. 拒绝：当Promise状态为rejected的话异步函数不会被捕获 即 `catch 无法捕获error`

#### await

1. 作用：暂停异步的执行 等待Promise的解决 
2. return 
   - 异步函数使用`return`返回值 会被Promise.resolve()包装成一个Promise对象
   - 没有return则返回undefined
   - 异步函数始终返回Promise对象
   - 返回常规值 `then()` 可以直接解包 可以直接使用
3. 拒绝：await能够捕获拒绝
4. 限制：
   - 必须在异步函数中使用
   - 异步函数的特质不会扩展到嵌套函数中

### 停止和恢复执行

### 异步函数策略

#### 实现 sleep

```js
function sleep(num) {
	return new Promise((resolve, reject) => setTimeout(resolve, num))
}
async function fn() {
	let date = Date.now()
	await sleep(1500)
	console.log(Date.now() - date)
}
fn()
```

#### 利用平行执行

1. 作用：对await的顺序没有需求的时候可以**平行执行**

2. ```js
   async function fn() {
   	let p1 = fn1()
   	let p2 = fn1()
   	let p3 = fn1()
   	await p1
   	await p2
   	await p3
   }
   ```

#### 串行执行Promise

```js
function add(num) {
	return num + 3
}
async function fn(num) {
	num = await add(num)
	console.log(num)
	num = await add(num)
	console.log(num)
	num = await add(num)
	return num
}
fn(3).then(res => console.log(res))
```

#### 栈追踪与内存处理

# BOM

## window

1. 两重身份
   - Global 对象
   - js接口

### Global作用域

1. **var** 声明的函数或变量都会变成window对象的属性或者方法
2. let const 不行

### 窗口关系

1. top ：指向最层窗口
2. parent ： 指向当前窗口的父窗口
3. 如果当前窗口是最上层窗口 则parent和top都指向window
4. self ：就是window窗口 

### 窗口位置与像素比

1. 窗口位置
   - screenLeft ：窗口相对于屏幕距离左边的位置
   - screenTop ：窗口相对于屏幕距离右边的位置
2. 移动
   - `moveTo(newX,newY)` 移动到新坐标
   - `moveBy(x,y)` 移动当前位置的距离
3. 像素比
   - `window.devicePixelRatio` ：表示物理与逻辑像素之间的比例

### 窗口大小

1. `innerWidth`   `innerHeight`  浏览器视口大小
2. `outerWdith` `outerHeight`  浏览器窗口自身大小
3. `resizeTo(newX,newY)`   
4. `resizeBy(x,y)`

### 视口位置

1. 获取文档相对于视口滚动距离

   - `pageXoffset / scrollX`
   - `pageYoffset / scrollY`

2. 滚动页面

   - `scroll(x,y)` 滚动到新位置

   - `scrollTo(x,y) `滚动到新位置

   - `scrollBy(x,y)`  当前位置继续滚动

   - `behavior`  平滑滚动属性

   - ```js
     window.scrollTo({
      left: 100,
      top: 100,
      behavior: 'smooth'
     }); 
     ```

### 导航与打开新窗口

### 定时器

1. setTimeout
2. setInterval

### 系统对话框

1. `alert()`
2. `confirm() => BL`
3. `prompt() => str`

### location对象

1. 作用：当前窗口的文档信息，以及导航功能
2. 即是window对象也是document对象
3. 参数
   - hash
   - host
   - hostname
   - href
   - port
   - pathname
   - search
   - username
   - password
   - origin
   - protocol

### 查询字符串

1. `URLSearchParams`：`new URLSearchParams(window.loaction.search)`
2. 作用：从location.search的参数中获取数据
3. 方法
   - get()
   - has()
   - delete()
   - toString()
4. 大多数支持`URLSearchParams`的浏览器也支持迭代

### 操作地址

1. 跳转：`location.assign(url)`
2. `window.location = location.href = location.assign(url)`
3. location修改属性,并且会刷新URL
   - hash
   - search
   - hostname
   - pathname
   - port
4. `reload()`  重载
   - reload() 从缓存重载
   - reload(true) 从服务器重载

### navigator对象

1. 浏览器的类型属性

### 检测插件

### screen对象

1. 用户端信息

### history对象

1. 作用：记录导航历史记录
2. 是window属性
3. 出于安全考虑这个对象不会暴露用户访问过的URL，但可以知道是前进和后退

#### 导航

1. `history.go(num)`：前进或者后退
2. `history.back()` 后退
3. `history.forward()` 前进
4. `history.length`  条数

#### 历史状态管理

1. `history.pushState(state对象,title,url)`
2. 改变URL不刷新浏览器
3. 会创建新的历史记录
4. `history.replaceState(Satet对象,title)`

# 客户端检测

# DOM

## 节点层级

1. 根节点：<HTML> 称之为 **文档元素** 

## Node类型

1. `nodeType`：每个节点都有nodeType属性，表示该节点的类型
2. `nodeName`和nodeValue： 保存节点信息
3. 节点关系
   - `childNodes`：每个节点都有 childNodes属性 包含 NodeLists 实例
   - `NodeLists` ：是一个类数组对象，是实时的
   - `parentNode`：指向DOM树的父元素
   - `previousSibling`：上一个节点（没有为null）
   - `nextSibling`：下一个节点（没有为null）
4. 操纵节点
   - `appendChild(e)`
   - `inserBefore(e,e = 参照节点)`
   - `cloneNode(BL = 表示是否深入复制)`

### Document类型



# 事件

## 事件流

### 事件冒泡

1. 解释：从具体的元素开始向上传播直到window对象

### 事件捕获

1. 解释：从上往下触发 从模糊到具体
2. 注意：旧浏览器不支持

### DOM事件流

1. 解释：DOM事件流分为三个阶段
   - 事件捕获
   - 到达目标
   - 事件冒泡

## 事件处理程序

> 用户或者浏览器执行某种动作























































