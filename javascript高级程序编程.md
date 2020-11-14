# 标识注释

1. `obj` 对象
2. `arr` 是做
3. `str` 字符串
4. `RE` RegExp
5. `BL` boolean
6. `length` 长度
7. `fn` function
8. `index` 下标
9. `——` 备注
10. `uri` 浏览器地址标识 uri
11. `num` 数字
12. `e` 元素
13. `start` 开始
14. `end` 结束

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
- `[...str] `可以结构为数组
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













