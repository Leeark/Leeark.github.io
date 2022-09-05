---
layout: post
title: JS知识梳理
categories: 面试
description: JS知识梳理
keywords: 前端, 面试
---

# JS数据类型

ECMAScript有6种简单数据类型（也称为原始类型）: Undefined、Null、Boolean、Number、String和Symbol。

Symbol（符号）是ECMAScript 6新增的。符号是原始值，且符号实例是唯一、不可变的。 符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。

符号没有字面量语法，这也是它们发挥作用的关键。按照规范，你只要创建 Symbol()实例并将其 用作对象的新属性，就可以保证它不会覆盖已有的对象属性，无论是符号属性还是字符串属性。

还有一种复杂数据类型叫Object（对象）。

# JS判断数据类型：

https://www.cnblogs.com/onepixel/p/5126046.html

## typeof操作符

对一个值使用typeof操作符会返回下列字符串之一：

❑ "string"表示值为字符串；❑ "number"表示值为数值；❑ "symbol"表示值为符号。

❑ "object"表示值为对象（而不是函数）或null；❑ "function"表示值为函数；

❑ "undefined"表示值未定义；❑ "boolean"表示值为布尔值；

缺点：

1,typeof null ; //'object'

2,无法区分对象（除了函数）的具体类型

3,new Boolean()；new Number()；new String()检测为'object'![image-20220714104045998](C:\Users\14211\AppData\Roaming\Typora\typora-user-images\image-20220714104045998.png)

## instanceof

instanceof 是用来判断 A 是否为 B 的实例，表达式为：A instanceof B，如果 A 是 B 的实例，则返回 true,否则返回 false。 （检测构造函数的prototype是否出现在某个实例对象的原型链上）

new出的实例同时是两个构造函数的实例![image-20220714104241794](C:\Users\14211\AppData\Roaming\Typora\typora-user-images\image-20220714104241794.png)

```js
[] instanceof Array; // true
{} instanceof Object;// true
new Date() instanceof Date;// true
 
function Person(){};
new Person() instanceof Person;// true
 
[] instanceof Object; // true
new Date() instanceof Object;// true
new Person instanceof Object;// true
```

因此，**instanceof 只能用来判断两个对象是否属于实例关系****， 而不能判断一个对象实例具体属于哪种类型。**

## constructor

当一个函数 F被定义时，JS引擎会为F添加 prototype 原型，然后再在 prototype上添加一个 constructor 属性，并让其指向 F 的引用。如下所示：

![img](https://images2015.cnblogs.com/blog/849589/201705/849589-20170508125250566-1896556617.png)

当执行 var f = new F() 时，F 被当成了构造函数，f 是F的实例对象，此时 F 原型上的 constructor 通过原型链传递到了 f 上，因此 f.constructor == F

![img](https://images2015.cnblogs.com/blog/849589/201705/849589-20170508125714941-1649387639.png)

## toString

toString() 是 Object 的原型方法。可以通过 `toString()` 来获取每个对象的类型。为了每个对象都能通过 `Object.prototype.toString()` 来检测，需要以 `Function.prototype.call()` 或者 `Function.prototype.apply()` 的形式来调用，传递要检查的对象作为第一个参数，称为 `thisArg`。

对于 Object 对象，直接调用 toString() 就能返回' [object Object] '。而对于其他对象，则需要通过 call / apply 来调用才能返回正确的类型信息。

```js
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(Symbol()); //[object Symbol]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
Object.prototype.toString.call(document) ; // [object HTMLDocument]
Object.prototype.toString.call(window) ; //[object global] window 是全局对象 global 的引用
```

# Object 构造函数的方法

## Object.create()

**`Object.create()`** 方法用于创建一个新对象，使用现有的对象来作为新创建对象的原型（prototype）。

```js
const person = {
  isHuman: false,
  printIntroduction: function() {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.printIntroduction();
// expected output: "My name is Matthew. Am I human? true"
```

## Object.keys()

ES5 引入了`Object.keys`方法，返回一个数组，对象自身所有可枚举的键名。

## Object.values()

ES2017引入，`Object.values`方法返回一个数组，对象自身所有可枚举的键值。

## Object.entries()

ES2017引入，`Object.entries`方法返回一个数组，对象自身所有可枚举的键值对。

## Object.is(a,b) 

它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

不同之处只有两个：一是 +0 不等于 -0 ，二是 NaN 等于自身。

## Object.assign() 

合并对象：将源对象（source）的所有**可枚举属性**，复制到目标对象（target）。

Object.assign方法的第一个参数是目标对象，后面的参数都是源对象。

**注意**：如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

```js
const target = { a: 1, b: 1 };
const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
console.log(target);    // {a:1, b:2, c:3}
```

## Object.freeze()

**`Object.freeze()`** 方法可以**冻结**一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。`freeze()` 返回和传入的参数相同的对象。

## Object.defineProperty()

`Object.defineProperty()` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。语法：

```reasonml
Object.defineProperty(obj, prop, descriptor)
```

参数说明：

> obj：必需。目标对象
> prop：必需。需定义或修改的属性的名字
> descriptor：必需。目标属性所拥有的特性（对象）

返回值：

> 传入函数的对象。即第一个参数obj

给对象的属性添加特性描述， 属性分两种：目前提供两种形式：数据描述和存取器描述。[数据属性和访问器属性。]

### 数据描述

 [[Configurable]]：表示属性是否可以通过 delete 删除并重新定义，是否可以修改它的特 性，以及是否可以把它改为访问器属性。默认情况下，所有直接定义在对象上的属性的这个特 性都是 true，如前面的例子所示。默认是 false。

虽然可以对同一个属性多次调用 Object.defineProperty()，但在把 configurable 设 置为 false 之后就会受限制了。

把 configurable 设置为 false，意味着这个属性不能从对象上删除。非严格模式下对 这个属性调用 delete 没有效果，严格模式下会抛出错误。此外，一个属性被定义为不可配置之后，就 不能再变回可配置的了。再次调用 Object.defineProperty()并修改任何非 writable 属性会导致 错误（修改writable为true也会报错）

  [[Enumerable]]：表示属性是否可以通过 for-in 循环返回。默认情况下，所有直接定义在对 象上的属性的这个特性都是 true，如前面的例子所示。 默认是 false。

 [[Writable]]：表示属性的值是否可以被修改。默认情况下，所有直接定义在对象上的属性的 这个特性都是 true，如前面的例子所示。

  [[Value]]：包含属性实际的值。这就是前面提到的那个读取和写入属性值的位置。这个特性 的默认值为 undefined。 

```js
var obj = {
    test:"hello"
}
//对象已有的属性添加特性描述
Object.defineProperty(obj,"test",{
    configurable:true | false,
    enumerable:true | false,
    value:任意类型的值,
    writable:true | false
});
//对象新添加的属性的特性描述
Object.defineProperty(obj,"newKey",{
    configurable:true | false,
    enumerable:true | false,
    value:任意类型的值,
    writable:true | false
});
```



### 存取器描述

**getter/setter**

当设置或获取对象的某个属性的值的时候，可以提供getter/setter方法。

-  [[Get]]：获取函数，在读取属性时调用。默认值为 undefined。
-   [[Set]]：设置函数，在写入属性时调用。默认值为 undefined。
-  访问器属性是不能直接定义的，必须使用 Object.defineProperty()。

**注意：当使用了getter或setter方法，不允许使用writable和value这两个属性**

## Object.defineProperties()

该方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。

```js
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```

## Object.setPrototypeOf() 

`Object.setPrototypeOf`方法的作用与`__proto__`相同，用来设置一个对象的原型对象（prototype），返回参数对象本身。它是 ES6 正式推荐的设置原型对象的方法。

```javascript
// 格式
Object.setPrototypeOf(object, prototype)

// 用法
const o = Object.setPrototypeOf({}, null);
```

该方法等同于下面的函数。

```javascript
function setPrototypeOf(obj, proto) {
  obj.__proto__ = proto;
  return obj;
}
```

警告 Object.setPrototypeOf()可能会严重影响代码性能。Mozilla 文档说得很清楚： “在所有浏览器和 JavaScript 引擎中，修改继承关系的影响都是微妙且深远的。这种影响并 不仅是执行 Object.setPrototypeOf()语句那么简单，而是会涉及所有访问了那些修 改过[[Prototype]]的对象的代码。”

为避免使用 Object.setPrototypeOf()可能造成的性能下降，可以通过 Object.create()来创 建一个新对象，同时为其指定原型：

```js
 let biped = { numLegs: 2 }; 
let person = Object.create(biped); 
person.name = 'Matt'; 
console.log(person.name);// Matt 
console.log(person.numLegs); // 2 
console.log(Object.getPrototypeOf(person) === biped); // true
```



## Object.getPrototypeOf()

该方法与`Object.setPrototypeOf`方法配套，用于读取一个对象的原型对象。

## Object.getOwnPropertyDescriptor()

**`Object.getOwnPropertyDescriptor()`** 方法返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

```js
const object1 = {
  property1: 42
};

const descriptor1 = Object.getOwnPropertyDescriptor(object1, 'property1');

console.log(descriptor1.configurable);
// expected output: true

console.log(descriptor1.value);
// expected output: 42
```



## Object.getOwnPropertyDescriptors()

ES5 的`Object.getOwnPropertyDescriptor()`方法会返回某个对象属性的描述对象（descriptor）。

ES2017 引入了`Object.getOwnPropertyDescriptors()`方法，返回指定对象所有自身属性（非继承属性）的描述对象。

```javascript
const obj = {
  foo: 123,
  get bar() { return 'abc' }
};

Object.getOwnPropertyDescriptors(obj)
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }
```

上面代码中，`Object.getOwnPropertyDescriptors()`方法返回一个对象，所有原对象的属性名都是该对象的属性名，对应的属性值就是该属性的描述对象。

## Object.getOwnPropertyNames()

列出所有实例属性，无论是否可以枚举。

**`Object.getOwnPropertyNames()`**方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括 Symbol 值作为名称的属性）组成的数组。

```js
Object.getOwnPropertyNames(obj)
```

## Object.getOwnPropertySymbols()

`Object.getOwnPropertySymbols()` 方法返回一个给定对象自身的所有 Symbol 属性的数组。

```js
Object.getOwnPropertySymbols(obj)
```



# 遍历

## for...in

以任意顺序迭代一个对象的除[Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)以外的[可枚举](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)属性，包括继承的可枚举属性。

关键点：遍历属性名，除了symbol，需可枚举，包含继承。

## Object.keys

遍历对象所有可枚举属性 不包括原型链上的属性。

## Object.getOwnPropertyNames()

**`Object.getOwnPropertyNames()`**方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括 Symbol 值作为名称的属性）组成的数组。

## Object.getOwnPropertySymbols()

`**Object.getOwnPropertySymbols()**` 方法返回一个给定对象自身的所有 Symbol 属性的数组。

|                                | 可枚举/不可枚举 | 有无继承属性 | 有无symbol属性 | 返回值       |
| ------------------------------ | --------------- | ------------ | -------------- | ------------ |
| for...in（迭代器）             | 仅可枚举属性    | 有           | 无             | ——           |
| Object.keys                    | 仅可枚举属性    | 无           | 无             | 属性名数组   |
| Object.getOwnPropertyNames()   | 都有            | 无           | 无             | 属性名数组   |
| Object.getOwnPropertySymbols() | ————            | ————         | 有             | 符号属性数组 |

## for...of

[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)（包括 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)，[`Map`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)，[`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)，[`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)，[`TypedArray`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)，[arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments) 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句。

关键词：可迭代对象，遍历属性值

Symbol类型是不可枚举的。Object.keys方法返回对象上的所有可枚举的键属性。Symbol类型是不可见的，并返回一个空数组。 记录整个对象时，所有属性都是可见的，甚至是不可枚举的属性。
这是Symbol的众多特性之一：除了表示完全唯一的值（防止对象意外名称冲突，例如当使用2个想要向同一对象添加属性的库时），您还可以隐藏这种方式对象的属性（尽管不完全。你仍然可以使用Object.getOwnPropertySymbols()方法访问 Symbol。

Symbol定义的属性不会出现在下面循环中：

for in:可获取原型属性，不可获取不可枚举属性
for of:不可遍历对象，可遍历数组
Object.keys：原型属性和不可枚举属性都不能获取
Object.getOwnPropertyByNames：不可获取原型属性，可获取不可枚举属性
JSON.stringify：原型属性和不可枚举属性都不能获取
Reflect.ownKeys：可获取不可枚举和Symbol，不可获取原型
————————————————

获取原型上属性的有：for in；
获取不可枚举属性的有：getOwnPropertyNames，Reflect.ownKeys
可获取Symbol属性的有：getOwnpropertySymbols,Reflect.ownKeys

## why  for...in ?

`for ... in`为遍历对象属性而构建，不建议与数组一起使用，数组可以用`Array.prototype.forEach()`和`for ... of`，那么`for ... in`的到底有什么用呢？

它最常用的地方应该是用于**调试，可以更方便的去检查对象属性**（通过输出到控制台或其他方式）。尽管对于处理存储数据，数组更实用些，但是你在处理有`key-value`数据（比如属性用作“键”），需要检查其中的任何键是否为某值的情况时，还是推荐用`for ... in`。

示例：

```js
var obj = {a:1, b:2, c:3};

for (var prop in obj) {
  console.log("obj." + prop + " = " + obj[prop]);
}

// Output:
// "obj.a = 1"
// "obj.b = 2"
// "obj.c = 3"
```



# 原型和in操作符

有两种方式使用 in 操作符：单独使用和在 for-in 循环中使用。

在单独使用时，in 操作符会在可以通过对象访问指定属性时返回 true，**无论该属性是在实例上还是在原型上。**

在上面整个例子中，name 随时可以通过实例或通过原型访问到（可枚举）。

因此，调用"name" in persoon1 时始终返回 true，无论这个属性是否在实例上。

如果要确定某个属性是否存在于原型上，则可以像下 面这样同时使用 hasOwnProperty()和 in 操作符： 

```js
function hasPrototypeProperty(object, name)
{ return !object.hasOwnProperty(name) && (name in object); } 
```

只要通过对象可以访问，in 操作符就返回 true，而 hasOwnProperty()只有属性存在于实例上 时才返回 true。因此，只要 in 操作符返回 true 且 hasOwnProperty()返回 false，就说明该属性 是一个原型属性。相当于`hasPrototypeProperty(person, "name")`

**要获得对象上所有可枚举的实例属性，可以使用 Object.keys()方法。**这个方法接收一个对象作 为参数，返回包含该对象所有**可枚举属性**名称的字符串数组。

如果想列出所有实例属性，无论是否可以枚举，都可以使用 **Object.getOwnPropertyNames()**

在 ECMAScript 6 新增符号类型之后，相应地出现了增加一个 Object.getOwnPropertyNames() 的兄弟方法的需求，因为以符号为键的属性没有名称的概念。因此，Object.getOwnPropertySymbols()方法就出现了，这个方法与 Object.getOwnPropertyNames()类似，只是针对符号而已

for-in 循环和 Object.keys() 的枚举顺序是不确定的，取决于 JavaScript 引擎，可能因浏览器而异。 Object.getOwnPropertyNames()、Object.getOwnPropertySymbols()和 Object.assign() 的枚举顺序是确定性的。先以升序枚举数值键，然后以插入顺序枚举字符串和符号键。在对象字面量中 定义的键以它们逗号分隔的顺序插入。

# 操作dom

## 获取

最常用的方法是`document.getElementById()`和`document.getElementsByTagName()`，以及CSS选择器`document.getElementsByClassName()`。

由于ID在HTML文档中是唯一的，所以`document.getElementById()`可以直接定位唯一的一个DOM节点。`document.getElementsByTagName()`和`document.getElementsByClassName()`总是返回一组DOM节点。

要精确地选择DOM，可以先定位父节点，再从父节点开始选择，以缩小范围。

第二种方法是使用`querySelector()`和`querySelectorAll()`，需要了解selector语法，然后使用条件来获取节点，更加方便：

Document.querySelector：方法返回文档中与指定选择器或选择器组匹配的第一个 [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element)对象。 如果找不到匹配项，则返回`null`。

Document.querySelectorAll：返回与指定的选择器组匹配的文档中的元素列表 (使用深度优先的先序遍历文档的节点)。返回的对象是 [`NodeList`](https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList) 。

## 添加

移动DOM节点也就是把这个节点插入到html文档中的某个地方，这里js给了我们两个方法：

1.appendChild()：把节点插入到父节点的末尾。

document.body.appendChild(oDiv);  //把div插入到body中，并且位于末尾

2.insertBefore()：把节点插入到父节点的某个兄弟节点的前面。

var oP = createElement('p'); //创建一个p节点
document.body.insertBefore(oP,oDiv); //把p节点插入到div的前面

## 删除

删除DOM节点的方法是removeChild()。

document.body.removeChild(oP); //删除p节点

## 替换

替换DOM节点的方法是replaceChild()。

var oSpan = document.createElement('span'); //创建一个span标签
document.body.replaceChild(oSpan,oBox); //用span标签替换div标签

![image-20220713105004111](C:\Users\14211\AppData\Roaming\Typora\typora-user-images\image-20220713105004111.png)

# 文档插入100节点，解决办法

解决办法：创建文档片段Fragment，将标签全部放入该片段中，再统一插入document，这样只会渲染一次

```javascript
<ul id="root"></ul>
<script>
var root = document.getElementById('root')
var fragment = document.createDocumentFragment()
for(let i = 0; i < 1000; i++){
	let li = document.createElement('li')
	li.innerHTML = '我是li标签'
    fragment.appendChild(li)
}
root.appendChild(fragment);
</script>
我们经常使用javascript来操作DOM元素，比如使用appendChild()方法。
每次调用该方法时，浏览器都会重新渲染页面。如果大量的更新DOM节点，则会非常消耗性能，影响用户体验.
javascript提供了一个文档碎片DocumentFragment的机制。
如果将文档中的节点添加到文档片段中，就会从文档树中移除该节点。
 
把所有要构造的节点都放在文档片段中执行，这样可以不影响文档树，也就不会造成页面渲染。
当节点都构造完成后，再将文档片段对象添加到页面中，这时所有的节点都会一次性渲染出来，
这样就能减少浏览器负担，提高页面渲染速度
```

# 作用域与作用域链

- 作用域分为全局作用域和局部作用域。
- 作用域其实就是规定了当前作用域中的变量和函数可被作用的范围。
- 作用域链其实就是规定了变量和函数的查找规则、是当前执行上下文的变量对象以及所有父级执行上下文的变量对象的集合。
- 当查找一个变量时，先从作用域的顶端查找，一直查找到作用域的底端，若查找完仍未找到，抛出错误。
- （块级作用域）
- var 声明是全局作用域或函数作用域，而 let 和 const 是块作用域。 var 变量可以在其范围内更新和重新声明； let 变量可以被更新但不能重新声明； const 变量既不能更新也不能重新声明。 它们都被提升到其作用域的顶端。

# 闭包

## 定义

闭包就是能够读取其他函数内部变量的函数。

```js
function test(){
    var age=18;
    function  addAge(){
        age++;
        alert(age);
    }
    return addAge;
}
    var fn=test();    fn();//弹出19

```

**三大特性：**

\- 函数嵌套函数。
\- 函数内部可以引用外部的参数和变量。
\- 参数和变量不会被垃圾回收机制回收。

## 优点

\- 希望一个变量长期存储在内存中。
\- 避免全局变量的污染。
\- 私有成员的存在。

## 缺点

\- 常驻内存，增加内存使用量。
\- 使用不当会很容易造成内存泄露。



# call/apply/bind

Function.prototype.call()

```
function.call(thisArg, arg1, arg2, ...)
```

`**call()**` 方法使用一个指定的 `this` 值和单独给出的一个或多个参数来调用一个函数。

如果没有传递第一个参数，`this` 的值将会被绑定为全局对象。

```js
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);//this占位
  this.category = 'food';
}

console.log(new Food('cheese', 5).name);
// expected output: "cheese"
```

```js
function greet() {
  var reply = [this.animal, 'typically sleep between', this.sleepDuration].join(' ');
  console.log(reply);
}

var obj = {
  animal: 'cats', sleepDuration: '12 and 16 hours'
};

greet.call(obj);  // cats typically sleep between 12 and 16 hours
```

**备注：**该方法的语法和作用与 [`apply()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法类似，只有一个区别，就是 `call()` 方法接受的是**一个参数列表**，而 `apply()` 方法接受的是**一个包含多个参数的数组**。

Function.prototype.bind()

`**bind()**` 方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

```js
const module = {
  x: 42,
  getX: function() {
    return this.x;
  }
};

const unboundGetX = module.getX;
console.log(unboundGetX()); // The function gets invoked at the global scope
// expected output: undefined

const boundGetX = unboundGetX.bind(module);
console.log(boundGetX());
// expected output: 42
```

在默认情况下，使用 [`window.setTimeout()`](https://developer.mozilla.org/zh-CN/docs/Web/API/setTimeout) 时，`this` 关键字会指向 [`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)（或 `global`）对象。当类的方法中需要 `this` 指向类的实例时，你可能需要显式地把 `this` 绑定到回调函数，就不会丢失该实例的引用。

```js
function LateBloomer() {
  this.petalCount = Math.ceil(Math.random() * 12) + 1;
}

// 在 1 秒钟后声明 bloom
LateBloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 1000);
};

LateBloomer.prototype.declare = function() {
  console.log('I am a beautiful flower with ' +
    this.petalCount + ' petals!');
};

var flower = new LateBloomer();
flower.bloom();  // 一秒钟后，调用 'declare' 方法
```



# 原型与原型链

![image-20220719190859486](C:\Users\14211\AppData\Roaming\Typora\typora-user-images\image-20220719190859486.png)

无论何时，只要创建一个函数，就会按照特定的规则为这个函数创建一个 prototype 属性（指向 原型对象）。默认情况下，所有原型对象自动获得一个名为 constructor 的属性，指回与之关联的构 造函数。对前面的例子而言，Person.prototype.constructor 指向 Person。

然后，因构造函数而异，可能会给原型对象添加其他属性和方法。 在自定义构造函数时，原型对象默认只会获得 constructor 属性，其他的所有方法都继承自 Object。

每次调用构造函数创建一个新实例，这个实例的内部[[Prototype]]指针就会被赋值为构造函数的原型对象。脚本中没有访问这个[[Prototype]]特性的标准方式，但 Firefox、Safari 和 Chrome 会在每个对象上暴露proto___属性，通过这个属性可以访问对象的原型。在其他实现中，这个特性 完全被隐藏了。

关键在于理解这一点：实例与构造函数原型之间有直接的联系，但实例与构造函数之 间没有。

## 原型链

重温一下构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型有 一个属性指回构造函数，而实例有一个内部指针指向原型。如果原型是另一个类型的实例呢？那就意味 着这个原型本身有一个内部指针指向另一个原型，相应地另一个原型也有一个指针指向另一个构造函 数。这样就在实例和原型之间构造了一条原型链。这就是原型链的基本构想。



# JavaScript执行机制

单线程 非阻塞：任务队列；事件轮询

js是一门单线程的非阻塞脚本语言，同一时刻只允许一个代码段执行。

单线程机制下，执行异步任务阻塞现象：等待结果返回，后面的代码无法执行。

单线程原因：JavaScript的Web端使用需要操作dom，线程1删除了这个DOM节点，线程2要操作这个DOM节点，产生矛盾。

js的另一大特点是非阻塞，实现这一点的关键在于任务队列。

我们讲了单线程的弊端，JavaScript是通过事件循环机制来解决这个弊端的，事件循环机制的执行规则。

1.所有代码作为宏任务进入主线程执行栈，开始执行。

2.执行过程中，同步代码会立即执行，宏任务进入宏任务队列，微任务进入到微任务队列。

3.当前宏任务执行完出队，读取微任务队列，有则执行，直到全部执行完毕。

4.执行浏览器ui进程渲染。

5.检查是否有webwork任务，有则执行。

6.本轮宏任务执行完成，回到第二步，继续执行，直到宏任务与微任务队列全部清空。

![image-20220730222153730](C:\Users\14211\AppData\Roaming\Typora\typora-user-images\image-20220730222153730.png)

https://www.ruanyifeng.com/blog/2014/10/event-loop.html

# 继承

## 原型链继承

基本操作：A（子类）构造函数的原型指向B（父类）构造函数的实例对象。

```js
function SuperType() { this.colors = ["red", "blue", "green"]; } 
function SubType() {}
// 继承 SuperType 
SubType.prototype = new SuperType(); //B类的单个实例对象，唯一的
let instance1 = new SubType(); 
instance1.colors.push("black"); 
console.log(instance1.colors); // "red,blue,green,black" 
let instance2 = new SubType(); 
console.log(instance2.colors); // "red,blue,green,black" 
```

问题：

1，原型中包含的引用值会在所有实例间共享，这也是为什么属性通常会 在构造函数中定义而不会定义在原型上的原因。

2，子类型在实例化时不能给父类型的构造函数传参。事实上，我们无法在不影响所有对象实例的情况下把参数传进父类的构造函数。再加上之前提到的原型中包含引用值的问题，就导致原型链基本不会被单独使用。

## 盗用构造函数

经典继承；盗用伪装

基本思路很简单：在子类构造函数中调用父类构造函数。

因为毕竟函数就是在特定上下文中执行代码的简单对象，所以可以使用 apply()和 call()方法以新创建的对象为上下文执行构造函数。

```js
function SuperType() {
 this.colors = ["red", "blue", "green"];
}
function SubType() {
 // 继承 SuperType
 SuperType.call(this);
}
let instance1 = new SubType();
instance1.colors.push("black");
console.log(instance1.colors); // "red,blue,green,black"
let instance2 = new SubType();
console.log(instance2.colors); // "red,blue,green"
```

优点：

1，**每个实例维护自己的属性**（主要解决的问题）

2，可以向父类构造函数传参

```js
function SuperType(name){
 this.name = name;
}
function SubType() {
 // 继承 SuperType 并传参
 SuperType.call(this, "Nicholas");
 // 实例属性
 this.age = 29;
}
let instance = new SubType();
console.log(instance.name); // "Nicholas";
console.log(instance.age); // 29
```

缺点：

盗用构造函数的主要缺点，也是使用构造函数模式自定义类型的问题：必须在构造函数中定义方法， 因此函数不能重用。此外，子类也不能访问父类原型上定义的方法，因此所有类型只能使用构造函数模 式。由于存在这些问题，盗用构造函数基本上也不能单独使用。





## 组合继承

组合继承（有时候也叫伪经典继承）综合了原型链和盗用构造函数，将两者的优点集中了起来。基本的思路是使用**原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性。**这样既可以把方 法定义在原型上以实现重用，又可以让每个实例都有自己的属性。

```js
function SuperType(name){
 this.name = name;
 this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function() {
 console.log(this.name);
};
function SubType(name, age){
 // 继承属性，使得实例维护不同属性
 SuperType.call(this, name);
 this.age = age;
}
// 继承方法，使得方法能够复用
SubType.prototype = new SuperType();
SubType.prototype.sayAge = function() {
 console.log(this.age);
};
let instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
console.log(instance1.colors); // "red,blue,green,black"
instance1.sayName(); // "Nicholas";
instance1.sayAge(); // 29
let instance2 = new SubType("Greg", 27);
console.log(instance2.colors); // "red,blue,green"
instance2.sayName(); // "Greg";
instance2.sayAge(); // 27
```

组合继承弥补了原型链和盗用构造函数的不足，是 JavaScript 中使用最多的继承模式。而且组合继 承也保留了 instanceof 操作符和 isPrototypeOf()方法识别合成对象的能力。

缺点：组合继承其实也存在效率问题。最主要的效率问题就是父类构造函数始终会被调用两次：一次在是 创建子类原型时调用，另一次是在子类构造函数中调用。

## 原型式继承

总结：不需要子构造函数，将一个对象设置为A临时构造函数的原型，并将new A返回

```js
//2006 年，Douglas Crockford 写了一篇文章：《JavaScript 中的原型式继承》（“Prototypal Inheritance in JavaScript”）。这篇文章介绍了一种不涉及严格意义上构造函数的继承方法。他的出发点是即使不自定义类型也可以通过原型实现对象之间的信息共享。文章最终给出了一个函数：
function object(o) {
 function F() {} //工具构造函数
 F.prototype = o; //改写原型
 return new F(); //新建对象并返回
}
let person = {
 name: "Nicholas",
 friends: ["Shelby", "Court", "Van"]
};
let anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");
let yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");
console.log(person.friends); // "Shelby,Court,Van,Rob,Barbie"
```

本质上，object()是对传入的对象执行了一次浅复制。

ECMAScript 5 通过增加 Object.create()方法将原型式继承的概念规范化了。这个方法接收两个 参数：作为新对象原型的对象，以及给新对象定义额外属性的对象（第二个可选）。在只有一个参数时， Object.create()与这里的 object()方法效果相同。

Object.create()的第二个参数与 Object.defineProperties()的第二个参数一样：每个新增 属性都通过各自的描述符来描述。

## 寄生式继承

与原型式继承比较接近的一种继承方式是寄生式继承（parasitic inheritance）.寄生式继承背后的思路类似于寄生构造函数和工厂模式：创建一个实现继承的函数，以某种 方式增强对象，然后返回这个对象。基本的寄生继承模式如下：

```js
function object(o) {
 function F() {}
 F.prototype = o;
 return new F();
}
function createAnother(original){
 let clone = object(original); // 通过调用函数创建一个新对象
 clone.sayHi = function() { // 以某种方式增强这个对象
 console.log("hi");
 };
 return clone; // 返回这个对象
}
```

**注意 通过寄生式继承给对象添加函数会导致函数难以重用，与构造函数模式类似。**

## 寄生式组合继承

寄生式组合继承通过盗用构造函数继承属性，但使用混合式原型链继承方法。基本思路是不通过调用父类构造函数给子类原型赋值，而是取得父类原型的一个副本。说到底就是使用寄生式继承来继承父 类原型，然后将返回的新对象赋值给子类原型。寄生式组合继承的基本模式如下所示：

```js
function inheritPrototype(subType, superType) {
 let prototype = object(superType.prototype); // 创建对象
 prototype.constructor = subType; // 增强对象
 subType.prototype = prototype; // 赋值对象
}
```

这个 inheritPrototype()函数实现了寄生式组合继承的核心逻辑。这个函数接收两个参数：子 类构造函数和父类构造函数。在这个函数内部，第一步是创建父类原型的一个副本。然后，给返回的 prototype 对象设置 constructor 属性，**解决由于重写原型导致默认 constructor 丢失的问题**。最 后将新创建的对象赋值给子类型的原型。

```js
function SuperType(name) {
 this.name = name;
 this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function() {
 console.log(this.name);
};
function SubType(name, age) {
 SuperType.call(this, name);
 this.age = age;
}
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function() {
 console.log(this.age);
};
```

这里只调用了一次 SuperType 构造函数，避免了 SubType.prototype 上不必要也用不到的属性， 因此可以说这个例子的效率更高。而且，原型链仍然保持不变，因此 instanceof 操作符和 isPrototypeOf()方法正常有效。寄生式组合继承可以算是引用类型继承的最佳模式。

## 类继承

ES6 类支持单继承。使用 extends 关键字，就可以继承任何拥有[[Construct]]和原型的对象。 很大程度上，这意味着不仅可以继承一个类，也可以继承普通的构造函数（保持向后兼容）

派生类都会通过原型链访问到类和原型上定义的方法。this 的值会反映调用相应方法的实例或者类

# New做了什么

创建 2绑 返回

new调用构造函数的过程：
1，创建新的临时对象a
2，把a的原型属性(<u>__</u>proto<u>__</u>)指向该构造函数原型属性(prototype)
3，把this绑定到a上
4，如果构造函数没返回其他对象，则返回a

# 类

类可以包含构造函数方法、实例方法、获取函数、设置函数和静态类方法，但这些都不是必需的。

constructor 关键字用于在类定义块内部创建类的构造函数。方法名 constructor 会告诉解释器 在使用 new 操作符创建类的新实例时，应该调用这个函数。

使用 new 操作符实例化 Person 的操作等于使用 new 调用其构造函数。唯一可感知的不同之处就 是，JavaScript 解释器知道使用 new 和类意味着应该使用 constructor 函数进行实例化。

类的写法

```js
class Person {
 constructor() {
 // 添加到 this 的所有内容都会存在于不同的实例上
 this.locate = () => console.log('instance', this);
 }
 // 定义在类的原型对象上
 locate() {
 console.log('prototype', this);
 }
 // 定义在类本身上
 static locate() {
 console.log('class', this);
 }
}
let p = new Person();
p.locate(); // instance, Person {}
Person.prototype.locate(); // prototype, {constructor: ... }
Person.locate(); // class, class Person {}
```

派生类的方法可以通过 super 关键字引用它们的原型。这个关键字只能在派生类中使用，而且仅限于类构造函数、实例方法和静态方法内部。在类构造函数中使用 super 可以调用父类构造函数。

```js
class Vehicle {
 constructor() {
 this.hasEngine = true;
 }
}
class Bus extends Vehicle {
 constructor() {
 // 不要在调用 super()之前引用 this，否则会抛出 ReferenceError
 super(); // 相当于 super.constructor()
 console.log(this instanceof Vehicle); // true
 console.log(this); // Bus { hasEngine: true }
 }
}
new Bus();
```

在静态方法中可以通过 super 调用继承的类上定义的静态方法：

```js
class Vehicle {
 static identify() {
 console.log('vehicle');
 }
}
class Bus extends Vehicle {
 static identify() {
 super.identify();
 }
}
Bus.identify(); // vehicle
```



# 数组方法





# 扩展运算符

# 解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

# 箭头函数



# Promise

Promise 是异步编程的一种解决方案，解决了回调地狱的问题。

`Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

promise对象有两个特点：

1，对象有三种状态：pending，fulfilled，rejected。只有异步操作的结果可以决定当前是那种状态。

2，一旦状态改变就不可再变，会保持这个结果，统称为resolved。状态改变的两种可能：pending→fulfilled；pending→rejected。

ES6入门：为了行文方便`resolved`统一只指`fulfilled`状态，不包含`rejected`状态。

`Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。

`then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`resolved`时调用，第二个回调函数是`Promise`对象的状态变为`rejected`时调用。

这两个函数都是可选的，不一定要提供。它们都接受`Promise`对象传出的值作为参数。

promise缺点：

一旦创建无法中途取消；

不设置回调函数，内部抛出的错误不会反映到外部；

pending状态无法得知目前的进展。

# Generator

## 1 什么是生成器

语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。

执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

形式上，Generator 函数是一个普通函数，但是有两个特征。一是，`function`关键字与函数名之间有一个星号；二是，函数体内部使用`yield`表达式，定义不同的内部状态（`yield`在英语里的意思就是“产出”）。

Generator 函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。不同的是，调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是上一章介绍的遍历器对象（Iterator Object）。

下一步，必须调用遍历器对象的`next`方法，使得指针移向下一个状态。也就是说，每次调用`next`方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个`yield`表达式（或`return`语句）为止。换言之，Generator 函数是分段执行的，`yield`表达式是暂停执行的标记，而`next`方法可以恢复执行。

总结：调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的`next`方法，就会返回一个有着`value`和`done`两个属性的对象。`value`属性表示当前的内部状态的值，是`yield`表达式后面那个表达式的值；`done`属性是一个布尔值，表示是否遍历结束。

## 2 定义生成器

```js
function* 生成器名() {
     yield 值;
     yield 值;
     yield 值;
     yield 值;
}
```

## 3 yield 关键字

1）yeild 关键字返回一个值， 遍历的时候每次得到就是 yield 的值。

2）调用 next()，执行到 yield 就会停止； 下一次调用 next()，执行到下一个 yield 停止。

> 调用生成器函数的时候，函数内代码不会执行；当调用 next() 的时候，才开始执行生成器函数内的代码； 执行到 yield 停止。

## 4 使用生成器函数给对象部署 iterator 接口

```js
let obj = {
    name: '曹操',
    age: 12,
    sorce: 90,
    height: 170
};

// 把obj变为一个 iterable
// 部署iterator接口
obj[Symbol.iterator] = function* (){
    for (let i in obj) {
        yield [i, obj[i]];
    }
};

for (let i of obj) {
    console.log(i);
}
```

# async/await

https://learn.fuming.site/front-end/Promise/async%E4%B8%8Eawait.html

async 函数是什么？一句话，它就是 Generator 函数的语法糖。async 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里。

一比较就会发现，`async`函数就是将 Generator 函数的星号（`*`）替换成`async`，将`yield`替换成`await`，仅此而已。



# this指向

`this`指的是函数运行时所在的环境（上下文）。

1；浏览器：全局上下文中this指向window

2；普通函数：谁调用指向谁

全局函数：指向window； 对象调用方法：指向该对象。

3；箭头函数：外部指向谁就指向谁；在哪里定义就指向谁；没有自己的this；

4；构造函数中的this，指向新建出的实例对象。



5；dom事件中的this指向dom标签

# 深拷贝

浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象共享堆内存。

深拷贝开辟新的内存空间，新对象跟原对象不共享内存，数据独立。

## JSON.parse(JSON.stringify(obj))

缺点：

会忽略 `undefined`和`symbol`；

不可以对`Function`进行拷贝，因为`JSON`格式字符串不支持`Function`，在序列化的时候会自动删除；

诸如 `Map`, `Set`, `RegExp`, `Date`, `ArrayBuffer `和其他内置类型在进行序列化时会丢失；

不支持循环引用对象的拷贝;（循环引用的可以大概地理解为一个对象里面的某一个属性的值是它自己）

## 递归

```js
function deepClone(target) {
 // 如果是对象，且不是原始值null
 if (typeof target === 'object' && target) { //注解一
  // 创建容器
  const result = Array.isArray(target) ? [] : {}; //注解三
  const keys = Object.keys(target); //注解二
 // Object.keys()会过滤掉原型链上的属性
  keys.forEach(key => {
   result[key] = deepClone(target[key]) // 注解三
  })
  return result;
 }
 // 如果是原始值，则直接返回
 return target;
}
```

- typeof null 返回的结果是 object，而众所周知 null 是一个基本类型，不是对象，所以我们判断是否为对象需要将它排除。
- 这里我们没有使用 for...in 操作符来遍历对象，因为in操作符会查找原型链上的属性，使用 `Object.keys()`更能节约性能。
- 我们提前判断了是否是对象或者数组，并创建了`result`容器。在之后，无论是数组还是对象，`result[key] = value`都能进行赋值。

缺点：循环引用报错。

# 闭包 

闭包Closure是1964年提出的一个计算机编程的概念，按照当时的论文《表达式的机器执行》中的定义，它包含控制和环境两个部分，在JavaScript中，以函数能够访问定义时的环境中变量的方式得以实现。

# 模块化

我们现在有了运行大量 JavaScript 脚本的复杂程序，还有一些被用在其他环境（例如 [Node.js](https://developer.mozilla.org/en-US/docs/Glossary/Node.js)）。因此，近年来，有必要开始考虑提供一种将 JavaScript 程序拆分为可按需导入的单独模块的机制。

# 防抖与节流

## 防抖

1.事件触发n秒后执行回调（延迟执行）2.n秒内再次触发事件，重新计时。

```js
// 实现 2
// immediate 表示第一次是否立即执行
function debounce(fn, wait = 50, immediate) {
    let timer = null
    return function(...args) {
        if (timer) clearTimeout(timer)
      
      	// ------ 新增部分 start ------ 
      	// immediate 为 true 表示第一次触发后执行
      	// timer 为空表示首次触发
        if (immediate && !timer) {
            fn.apply(this, args)
        }
      	// ------ 新增部分 end ------ 
      	
        timer = setTimeout(() => {
            fn.apply(this, args)
        }, wait)
    }
}

// DEMO
// 执行 debounce 函数返回新函数
const betterFn = debounce(() => console.log('fn 防抖执行了'), 1000, true)
// 第一次触发 scroll 执行一次 fn，后续只有在停止滑动 1 秒后才执行函数 fn
document.addEventListener('scroll', betterFn)
```

## 节流

限制一个函数在一定时间内只能执行一次。

```js
// fn 是需要执行的函数
// wait 是时间间隔
const throttle = (fn, wait = 50) => {
  // 上一次执行 fn 的时间
  let previous = 0
  // 将 throttle 处理结果当作函数返回
  return function(...args) {
    // 获取当前时间，转换成时间戳，单位毫秒
    let now = +new Date()
    // 将当前时间和上一次执行函数的时间进行对比
    // 大于等待时间就把 previous 设置为当前时间并执行函数 fn
    if (now - previous > wait) {
      previous = now
      fn.apply(this, args)
    }
  }
}

// DEMO
// 执行 throttle 函数返回新函数
const betterFn = throttle(() => console.log('fn 函数执行了'), 1000)
// 每 10 毫秒执行一次 betterFn 函数，但是只有时间差大于 1000 时才会执行 fn
setInterval(betterFn, 10)
```

# Cookie/storage

为了满足各种各样的需求，会经常性在本地存储大量的数据，传统方式我们以document.cookie来进行存储的，但是由于其存储大小只有4k左右，并且解析也相当的复杂，每一次发送请求都会携带上cookie，会造成带宽的浪费，给开发带来诸多不便，HTML5规范则提出解决方案。

| 项           | cookie                                                       | localStorage                                                 | sessionStorage                                               |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 存储大小     | 4K                                                           | 5M                                                           | 5M                                                           |
| 生命周期     | 若不设置有效期，则临时存放在内存中，会话结束后自动删除。若设置了有效期，则存储在硬盘里，有效期到了，自动删除。 | 永久有效。窗口或浏览器关闭也一直保存。只有手动删除才去除。   | 当前网页会话下有效，关闭页面或浏览器后会被清除。             |
| 生成方式     | 服务器生成。可设置失效时间。服务器通过响应头“Set-Cookie”传给前端。 | 前端生成。                                                   | 前端生成。                                                   |
| 与服务器通信 | 每次请求都会携带在HTTP头中。（名为：“Cookie”）               | 不发给客户端。纯粹为了保存数据                               | 不发给客户端。纯粹为了保存数据                               |
| 数据共享     | 相同浏览器不同页面可共享（同源页面）                         | 相同浏览器不同页面可共享（同源页面）                         | 相同浏览器同一个页面可以共享，不同页面不可共享（同源页面）   |
| 易用性       | 易用性差。原生的cookie接口不够友好，需要自己封装setCookie, getCookie | 原生接口很好用。<br/><br/>localStorage.setItem(key, value) //存<br/>localStorage.getItem(key)  //读<br/>localStorage.removeItem(key)  //删<br/>localStorage.clear()  //删除所有 | 原生接口很好用。<br/><br/>sessionStorage.setItem(key, value)  //存<br/>sessionStorage.getItem(key)  //读<br/>sessionStorage.removeItem(key)  //删<br/>sessionStorage.clear()  //删除所有 |

https://www.cnblogs.com/fundebug/p/about-browser-storage.html

# requestAnimationFrame

### 函数`requestAnimationFrame()`

前面讲解threejs动画效果，使用了`setInterval()`函数，实际开发中，为了更好的利用浏览器渲染，可以使用函数`requestAnimationFrame()`代替`setInterval()`函数，`requestAnimationFrame()`和`setInterval()`一样都是浏览器`window`对象的方法。

`requestAnimationFrame()`参数是将要被调用函数的函数名，`requestAnimationFrame()`调用一个函数不是立即调用而是向浏览器发起一个执行某函数的请求， 什么时候会执行由浏览器决定，一般默认保持60FPS的频率，大约每16.7ms调用一次`requestAnimationFrame()`方法指定的函数，60FPS是理想的情况下，如果渲染的场景比较复杂或者说硬件性能有限可能会低于这个频率。可以查看文章[《requestAnimationFrame()》](http://www.yanhuangxueyuan.com/HTML5/time.html)了解更多`requestAnimationFrame()`函数的知识。

https://javascript.ruanyifeng.com/htmlapi/requestanimationframe.html#