# Vue 基础

[toc]

## 初识 Vue

### Vue 第一步

1. 想让Vue工作，就必须创建一个 Vue 实例，且要传入一个配置对象；

2. root 容器里的代码依然符合 html 规范，只不过混入了一些特殊的Vue语法；

3. root容器里的代码被称为【Vue模板】；

4. Vue 实例和容器是一一对应的；

5. 真实开发中只有一个 Vue 实例，并且会配合着组件一起使用；

6. {{ xxx }} 中的 xxx 要写js表达式，且 xxx 可以自动读取到 data 中的所有属性；

7. 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新；

   注意区分：js表达式 和 js代码(语句)

   - 表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方：
        - `a`
        - `a + b`
        - `demo(1)`
        - `x === y ? 'a' : 'b'`
    - js代码(语句)
      - `if(){}`
      - `for(){}`

```html
<!-- 引入Vue -->
<script type="text/javascript" src="../js/vue.js"></script>

<!-- 准备好一个容器 -->
<div id="demo">
    <h1>Hello，{{name.toUpperCase()}}，{{address}}</h1>
</div>

<script type="text/javascript" >
    Vue.config.productionTip = false // 阻止 vue 在启动时生成生产提示。

    //创建Vue实例
    new Vue({
        el:'#demo', // el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串。
        data:{ // data中用于存储数据，数据供el所指定的容器去使用，值我们暂时先写成一个对象。
            name:'atguigu',
            address:'北京'
        }
    })
```

---

## 模板语法 data

Vue模板语法有2大类：

#### 插值语法：

- 功能：用于解析标签体内容。

- 写法：{{ xxx }}，xxx 是 js 表达式，且可以直接读取到data中的所有属性。

#### 指令语法：

- 功能：用于解析标签（包括：标签属性、标签体内容、绑定事件.....）。

- 举例：`v-bind:href="xxx"` 或  简写为 `:href="xxx"`，xxx 同样要写 js 表达式，且可以直接读取到 data 中的所有属性。

- 备注：Vue 中有很多的指令，且形式都是：`v-????`，此处我们只是拿v-bind举个例子。

```html
<!-- 准备好一个容器-->
<div id="root">
    <h1>插值语法</h1>
    <h3>你好，{{name}}</h3>
    <hr/>
    <h1>指令语法</h1>
    <a v-bind:href="school.url.toUpperCase()" x="hello">点我去{{school.name}}学习1</a>
    <a :href="school.url" x="hello">点我去{{school.name}}学习2</a>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'jack',
            school:{
                name:'吕不韦',
                url:'http://www.atguigu.com',
            }
        }
    })
</script>
```

---

## 数据绑定 data

**Vue 中有2种数据绑定的方式**：

#### 单向绑定 (v-bind)

数据只能从 data 流向页面。

#### 双向绑定 (v-model)

数据不仅能从data流向页面，还可以从页面流向 data。

#### 备注

   1. 双向绑定一般都应用在**表单类元素**上（如：input、select等）
2. `v-model:value` 可以简写为 v-model，因为 v-model 默认收集的就是 value 值。

```html
<!-- 准备好一个容器-->
<div id="root">
    <!-- 普通写法 -->
	单向数据绑定：<input type="text" v-bind:value="name"><br/>
    双向数据绑定：<input type="text" v-model:value="name"><br/>

    <!-- 简写 -->
    单向数据绑定：<input type="text" :value="name"><br/>
    双向数据绑定：<input type="text" v-model="name"><br/>

    <!-- 如下代码是错误的，因为v-model只能应用在表单类元素（输入类元素）上 -->
    <!-- <h2 v-model:x="name">你好啊</h2> -->
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'刘备'
        }
    })
</script>
```

---

## el 和 data 的两种写法

#### el 有 2 种写法

1. new Vue时候配置 el 属性。
2. 先创建Vue实例，随后再通过vm.$mount('#root')指定el的值。

    ```javascript
    // 第一种写法
    new Vue({
        el: '#root'
    })
    
    // 第二种写法
    const vm = new Vue({})
    vm.$mount('#root')  // mount 有挂载的意思，将 Vue 挂载到 #root
    ```

#### data 有 2 种写法

1. 对象式

2. 函数式
   - 如何选择：目前哪种写法都可以，以后学习到组件时，data必须使用函数式，否则会报错。
   
   ```javascript
   new Vue({
       el: '#root',
       // 第一种写法
       data: {
           name: '李白',
           age: '35',
           school:{
               name: '皇家学院'
           }
       },
       // 第二种写法, 组件时，必须使用函数式，可以创建多个实例对象，不互相作用
       data(){
           return{
               name: '白居易',
               age: 17
           }
       }
   })
   ```

#### 一个重要的原则：

**由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了**。

---

## MVVM 模型

![image-20211017132925124](https://github.com/EthanWhite134/learn_Vue/blob/main/images/image-20211017132925124.png)

#### MVVM 模型

1. **M**：模型(Model) ：data中的**数据**
2. **V**：视图(View) ：**模板**代码
3. **VM**：**视图**模型 (ViewModel)：Vue实例

#### 观察发现

1. data 中所有的属性，最后都出现在了 vm 身上。
2. vm 身上所有的属性及 Vue 原型上所有属性，在 Vue 模板中都可以直接使用。

---

## 数据代理 data

数据代理主要使用 get 和 set 方法

### Object.defineproperty 方法

详情见 ES6

```javascript
let person = {
    name: '吕不韦',
    sex: '男',
}
Object.defineProperty(person, 'age', {
    // 当有人读取 person 的 age 属性时，get 函数（getter）就会被调用，且返回值就是 age 的值
    get(){
    	console.log('有人读取 age 的值');
        return number
    },
    // 当有人修改 person 的 age 属性时，set 函数（setter）就会被调用，且会收到修改的具体值
    set(value){
        console.log('有人修改了 age 属性，且值是', value);
        number = value;
    }
})
```

### 数据代理

通过一个对象代理对另一个对象中属性的操作（读 / 写）

1. Vue 中的数据代理：通过 vm 对象来代理 data 对象中属性的操作（读 / 写）
2. Vue 中数据代理的好处：更加方便的操作 data 中的数据
3. 基本原理
   - 通过 `Object.defineProperty()`把 data 对象中所有属性添加到 vm 上
   - 为每一个添加到 vm 上的属性，都指定了一个 `getter `、`setter`
   - 在 `getter `、 `setter` 内部去操作（读 / 写）data 中对应的属性

```javascript
const vm = new Vue({
    el: '#root',
    data: {
        name: '蒙骜',
        skoll: '打仗',
    }
})
// 创建一个 Vue 实例（vm）时，会生成一个 _data 属性，里面存着 data 的值，可以通过 vm._data.name 获得 '蒙骜',也可以直接通过 vm.name 获得，并且两者全等 
```

---

## 事件处理 methods

### 事件的基本使用

1. 使用 v-on:xxx 或者 @ xxx 绑定事件，其中 xxx 是事件名（例如 click）
2. 事件的回调需要配置在methods 对象中，最终会在 vm 上
3. methods 中配置的函数，不要用箭头函数，否则 this 就不是 vm，而是 Window
4. methods 中配置的函数，都是被 Vue 管理的函数，this 的指向时 **vm** 或者 **组件实例对象**
5. `@click='demo'` 和 `@click="demo($event)"` 效果一致，但是后者可以传参

```html
<div id="root">
    <!-- <button v-on:click="showInfo">点我提示信息</button> -->
    <button @click="showInfo1">点我提示信息1（不传参）</button>
    <button @click="showInfo2($event,66)">点我提示信息2（传参）</button>
</div>

<script type="text/javascript">
    new Vue({
        el:'#root',
        data{...}
        methods:{
            showInfo1(event){
                // console.log(event.target.innerText)
                // console.log(this) //此处的 this 是 vm
                alert('同学你好！')
            },
            showInfo2(event, number){
                console.log(event, number)
                // console.log(event.target.innerText)
                // console.log(this) //此处的 this 是 vm
                alert('同学你好！！')
            }
        }
    })
</script>
```

### 事件修饰符

#### Vue中的事件修饰符：

1. **prevent**：阻止默认事件（常用）；
2. **stop**：阻止事件冒泡（常用）；
3. **once**：事件只触发一次（常用）；
4. **capture**：使用事件的捕获模式；
5. **self**：只有event.target是当前操作的元素时才触发事件；
6. **passive**：事件的默认行为立即执行，无需等待事件回调执行完毕；

#### 使用方法

```html
<h2>事件修饰符</h2>
<!-- 阻止默认事件（常用） -->
<a href="http://www.atguigu.com" @click.prevent="showInfo">点我提示信息</a>

<!-- 阻止事件冒泡（常用） -->
<div class="demo1" @click="showInfo">
    <button @click.stop="showInfo">点我提示信息</button>
    <!-- 修饰符可以连续写 -->
    <a href="http://www.atguigu.com" @click.prevent.stop="showInfo">点我提示信息</a>
</div>
```

### 键盘事件

1. Vue中常用的按键别名：
    - 回车 => enter
    - 删除 => delete (捕获“删除”和“退格”键)
    - 退出 => esc
    - 空格 => space
    - 换行 => tab (特殊，必须配合keydown去使用)
    - 上 => up
    - 下 => down
    - 左 => left
    - 右 => right

2. Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为 kebab-case（短横线命名）

3. 系统修饰键（用法特殊）：ctrl、alt、shift、meta
    - 配合 keyup 使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发。
    - 配合 keydown 使用：正常触发事件。

4. 也可以使用 keyCode 去指定具体的按键（不推荐）

5. `Vue.config.keyCodes.自定义键名 = 键码`，可以去定制按键别名

```html
<div id="root">
    <h2>欢迎来到{{name}}学习</h2>
    <input type="text" placeholder="按下回车提示输入" @keydown.enter="showInfo"> <!-- 按下 Enter 键的时候调用函数 showInfo -->
    <input type="text" placeholder="按下回车提示输入" @keydown.huiche="showInfo"> 
</div>

<script>
    Vue.config.keyCodes.huiche = 13 //定义了一个别名按键
    new Vue({
        methods:{
            showInfo(){}
        }
    })
</script>
```

---

## 计算属性  computed

1. 定义：要用的属性不存在，要通过已有属性计算得来。
2. 原理：底层借助了`Objcet.defineproperty`方法提供的 getter 和 setter。
3. get函数什么时候执行？
    - **初次读取时会执行一次。**
    - **当依赖的数据发生改变时会被再次调用。**
4. 优势：与 methods 实现相比，内部有缓存机制（复用），效率更高，调试方便。
5. 备注：
	- 计算属性最终会出现在vm上，直接读取使用即可。
	- 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变。

```html
<div id="root">
    姓：<input type="text" v-model="firstName"> <br/><br/>
    名：<input type="text" v-model="lastName"> <br/><br/>
    全名：<span>{{ firstName }}-{{ lastName }}</span>  <!-- 插值语法实现 -->
    全名：<span>{{ fullName }}</span>  <!-- method 方法实现 -->
    全名：<span>{{ fullName }}</span>  <!-- 计算属性实现 -->
</div>

<script type="text/javascript">
    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三'
        },
        methods: {
            fullName(){
                console.log('@---fullName')
                return this.firstName + '-' + this.lastName
            }
        },
        computed:{
            // 简写(当只有 get 的时候可以简写)
            fullName(){
                console.log('get被调用了')
                return this.firstName + '-' + this.lastName
            },
            
            // 完整写法
            fullName:{
                //get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                //get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。
                get(){
                    // console.log(this) // 此处的this是vm
                    return this.firstName + '-' + this.lastName
                },
                // set什么时候调用? 当 fullName被修改 时。
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

---

## 监视属性 watch

### 监视属性 watch：

1. 当**被监视的属性变化时**, 回调函数自动调用, 进行相关操作

2. 监视的属性必须存在，才能进行监视！！

3. 监视的两种写法：

	- new Vue 时传入watch 配置

	- 通过 vm.$watch 监视

```javascript
new Vue({
    data:{
        isHot: true,
    },
    // 第一种写法
    watch:{
    	isHot:{
            // handler 当 isHot 发生改变时调用
            handler(newValue, oldValue){
                console.log('isHot 被修改了', newValue, oldValue)
            }
        },
        // 简写
        isHot(newValue, oldValue){
                console.log('isHot 被修改了', newValue, oldValue)
            }
	}  
})

// 第二种写法
const vm = new Vue(){}
vm.$watch('isHot', {
    handler(newValue, oldValue){
        console.log('isHot 被修改了', newValue, oldValue)
    }
})

// 简写
vm.$watch('isHot', (newValue, oldValue){
        console.log('isHot 被修改了', newValue, oldValue)
})
```

### 深度监视

1. Vue 中的 watch 默认不监测对象内部值的改变（一层）。
2. 配置 deep:true 可以监测对象内部值改变（多层）。
3. 备注：
    - Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以！
    - 使用watch时根据数据的具体结构，决定是否采用深度监视。

```javascript
new Vue({
    data:{
        numbers:{
            a: 1,
            b: 2,
            c: {
                d: 12;
            }
        }
    },
    watch:{
    // 监视多级结构中所有属性的变化
    numbers:{
        deep:true, // 如果不配置此属性，改变 a,b,c,d 的值 number 不会被监视到改变
        handler(){
            console.log('numbers 改变了')
        	}
        }
    }
})
```

### computed 和 watch 的区别

1. computed 能完成的功能，watch 都可以完成
2. watch 能完成的功能，computed 不一定能完成，例如：watch 可以进行异步操作（比如网页加载之后一秒钟再执行）

**两个重要小原则**

- 所有被 Vue 管理的函数，最后写成普通函数，这样 this 的指向才是 vm 或者 组件实例对象
- 所有不被 Vue 管理的函数（定时器的回调函数，ajax 的回调函数等，Promise 的回调函数），最好写成箭头函数，这样 this 的指向才是 vm 或者 组件实例对象

---

## 绑定样式

1. class样式

    写法:class="xxx" xxx可以是字符串、对象、数组。

    - 字符串写法适用于：类名不确定，要动态获取。

    - 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定。

    - 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。

2. style样式

    - :style="{fontSize: xxx}"其中xxx是动态值。

    - :style="[a,b]"其中a、b是样式对象。

```html
<style>
	
</style>

<div id='root'>
    <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
    <div class="basic" :class="mood">{{name}}</div> <br/><br/>

    <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
    <div class="basic" :class="classArr">{{name}}</div> <br/><br/>

    <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
    <div class="basic" :class="classObj">{{name}}</div> <br/><br/>

    <!-- 绑定style样式--对象写法 -->
    <div class="basic" :style="styleObj">{{name}}</div> <br/><br/>
    <!-- 绑定style样式--数组写法 -->
    <div class="basic" :style="styleArr">{{name}}</div>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    const vm = new Vue({
        el:'#root',
        data:{
            name:'吕不韦',
            mood:'normal',
            classArr:['atguigu1','atguigu2','atguigu3'],
            classObj:{ // 对象写法，样式在 CSS 中
                atguigu1:false,
                atguigu2:false,
            },
            styleObj:{  // 对象写法，样式直接写在 data 中
                fontSize: '40px',
                color:'red',
            },
            styleArr:[  // 数组写法
                {
                    fontSize: '40px',
                    color:'blue',
                },
                {
                    backgroundColor:'gray'
                }
            ]
        },
</script>
```

---

## 条件渲染

1. v-if

	- 写法：
        - `v-if="表达式" `

        - `v-else-if="表达式"`

        - `v-else="表达式"`

    - 适用于：切换频率较低的场景。

    - 特点：不展示的 **DOM 元素直接被移除**。

    - 注意：v-if可以和:v-else-if、v-else一起使用，但要求结构不能被“打断”。


2. v-show

    - 写法：v-show="表达式"

    - 适用于：切换频率较高的场景。

    - 特点：不展示的 **DOM 元素**未被移除，仅仅是使用样式**隐藏**掉


3. 备注：使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到。

```html
<div id="root">
    <!-- 使用v-show做条件渲染 -->
    <h2 v-show="false">欢迎来到{{name}}</h2>
    <h2 v-show="1 === 1">欢迎来到{{name}}</h2>

    <!-- 使用v-if做条件渲染 -->
    <h2 v-if="false">欢迎来到{{name}}</h2>
    <h2 v-if="1 === 1">欢迎来到{{name}}</h2>

    <!-- v-else和v-else-if  中间不能被间断--> 
    <div v-if="n === 1">Angular</div>
    <div v-else-if="n === 2">React</div>
    <div v-else-if="n === 3">Vue</div>
    <div v-else>哈哈</div>

    <!-- v-if与template的配合使用，template 不显示在页面上 -->
    <template v-if="n === 1">
        <h2>你好</h2>
        <h2>吕不韦</h2>
        <h2>北京</h2>
    </template>

</div>
```

---

## 列表渲染

### `v-for  ` 指令:

1. 用于展示列表数据
2. 语法：v-for="(item, index) in xxx" :key="yyy"
3. 可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）

```html
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
```



### Key 的原理

面试题：react、vue中的key有什么作用？（key的内部原理）

1. 虚拟DOM中key的作用：

    - key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】, 

    - 随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：


2. 对比规则：

    - 旧虚拟 DOM 中找到了与新虚拟 DOM 相同的key：

        - 若虚拟 DOM 中内容没变, 直接使用之前的真实DOM！

        - 若虚拟 DOM 中内容变了, 则生成新的真实 DOM，随后替换掉页面中之前的真实 DOM。

    - 旧虚拟 DOM 中未找到与新虚拟 DOM 相同的 key
    
        - 创建新的真实 DOM，随后渲染到到页面。

3. 用 index 作为 key 可能会引发的问题：

    - 若对数据进行：逆序添加、逆序删除等破坏顺序操作:

    	- 会产生没有必要的真实 DOM 更新 ==> 界面效果没问题, 但效率低。

    - 如果结构中还包含输入类的 DOM：

        - 会产生错误 DOM 更新 ==> 界面有问题。

4. 开发中如何选择 key?:

    - 最好使用每条数据的唯一标识作为 key, 比如 id、手机号、身份证号、学号等唯一值。

    - 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用 index 作为 key 是没有问题的。

### 列表过滤（排序）

```html
<div id="root">
    <h2>人员列表</h2>
    <input type="text" placeholder="请输入名字" v-model="keyWord">
    <ul>
        <li v-for="(p,index) of filPerons" :key="index">
            {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
    </ul>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    //用watch实现
    //#region 
   new Vue({
				el:'#root',
				data:{
					keyWord:'',
					persons:[
						{id:'001',name:'马冬梅',age:19,sex:'女'},
						{id:'002',name:'周冬雨',age:20,sex:'女'},
						{id:'003',name:'周杰伦',age:21,sex:'男'},
						{id:'004',name:'温兆伦',age:22,sex:'男'}
					],
					filPerons:[]
				},
				watch:{ // 实现过滤功能
					keyWord:{
						immediate:true,
						handler(val){
							this.filPerons = this.persons.filter((p)=>{
								return p.name.indexOf(val) !== -1
							})
						}
					}
				}
			})
    //#endregion

    //用computed实现
    new Vue({
        el:'#root',
        data:{
            keyWord:'',
            persons:[
                {id:'001',name:'马冬梅',age:19,sex:'女'},
                {id:'002',name:'周冬雨',age:20,sex:'女'},
                {id:'003',name:'周杰伦',age:21,sex:'男'},
                {id:'004',name:'温兆伦',age:22,sex:'男'}
            ]
        },
        computed:{
            filPerons(){  // 实现排序功能
                const arr = this.persons.filter((p)=>{
                    return p.name.indexOf(this.keyWord) !== -1
                })
                //判断一下是否需要排序
                if(this.sortType){
                    arr.sort((p1,p2)=>{
                        return this.sortType === 1 ? p2.age-p1.age : p1.age-p2.age
                    })
                }
                return arr
            }
        }
    }) 
</script>
```

---

## Vue 监测数据的原理

1. vue会监视data中所有层次的数据。

2. 如何监测对象中的数据？

    - 通过setter实现监视，且要在new Vue时就传入要监测的数据。

        - 对象中后追加的属性，Vue默认不做响应式处理

        - 如需给后添加的属性做响应式，请使用如下API：

            `Vue.set(target，propertyName/index，value)` 或 

            `vm.$set(target，propertyName/index，value)`

3. 如何监测数组中的数据？

	- 通过包裹数组更新元素的方法实现，本质就是做了两件事：

        - 调用原生对应的方法对数组进行更新。

        - 重新解析模板，进而更新页面。

4. 在Vue修改数组中的某个元素一定要用如下方法：

    - 使用这些API:`push()、pop()、shift()、unshift()、splice()、sort()、reverse()`

    - `Vue.set() 或 vm.$set()`

特别注意：`Vue.set() 和 vm.$set()` 不能给vm 或 vm的根数据对象 添加属性！！！

```javascript
const vm = new Vue({
    el:'#root',
    data:{
        student:{
            name:'tom',
            age:{
                rAge:40,
                sAge:29,
            },
            hobby:['抽烟','喝酒','烫头'],
        }
    },
    methods: {
        addSex(){
            // Vue.set(this.student,'sex','男')
            this.$set(this.student,'sex','男')
        }
    }
})
```



## 收集表单数据

通过 v-model 收集表单数据，然后存储之后发送给服务器

1. 若：`<input type="text"/>`，则v-model收集的是value值，用户输入的就是value值。
2. 若：`<input type="radio"/>`，则v-model收集的是value值，且要给标签配置value值。
3. 若：`<input type="checkbox"/>`
    - 没有配置 input的value 属性，那么收集的就是 checked（勾选 or 未勾选，是布尔值）
    - 配置 input 的 value 属性:
        - v-model的初始值是非数组，那么收集的就是 checked（勾选 or 未勾选，是布尔值）
        - v-model 的初始值是数组，那么收集的的就是 value 组成的数组
        备注：v-model 的三个修饰符：
    - `lazy`：失去焦点再收集数据
    - `number`：输入字符串转为有效的数字
    - `trim`：输入首尾空格过滤

---

## 过滤器 filter

定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。

语法：

1. 注册过滤器：`Vue.filter(name,callback) 或 new Vue{filters:{}}`
2. 使用过滤器：`{{ xxx | 过滤器名}}  或  v-bind:属性 = "xxx | 过滤器名"`

备注：

1. 过滤器也可以接收额外参数、多个过滤器也可以串联
2. 并没有改变原本的数据, 是产生新的对应的数据

```html
<div id="root">
    <h2>显示格式化后的时间</h2>
    <!-- 计算属性实现 -->
    <h3>现在是：{{fmtTime}}</h3>
    <!-- methods实现 -->
    <h3>现在是：{{getFmtTime()}}</h3>
    <!-- 过滤器实现 -->
    <h3>现在是：{{time | timeFormater}}</h3>
    <!-- 过滤器实现（传参） -->
    <h3>现在是：{{time | timeFormater('YYYY_MM_DD') | mySlice}}</h3>
    <h3 :x="msg | mySlice">吕不韦</h3>
</div>

<div id="root2">
    <h2>{{msg | mySlice}}</h2>
</div>
</body>

<script type="text/javascript">
    // 全局过滤器，其他 Vue 实例也能使用
    Vue.filter('mySlice',function(value){
        return value.slice(0,4)
    })

    new Vue({
        el:'#root',
        data:{
            time:1621561377603, //时间戳
        },
        methods: {
            getFmtTime(){
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        // 局部过滤器
        filters:{
            timeFormater(value,str='YYYY年MM月DD日 HH:mm:ss'){
                // console.log('@',value)
                return dayjs(value).format(str)
            }
        }
    })
</script>
```

---

## 指令

### 内置指令

1. v-bind  : 单向绑定解析表达式, 可简写为 :xxx
2. v-model : 双向数据绑定
3. v-for  : 遍历数组/对象/字符串
4. v-on   : 绑定事件监听, 可简写为@
5. v-if     : 条件渲染（动态控制节点是否存存在）
6. v-else  : 条件渲染（动态控制节点是否存存在）
7. v-show  : 条件渲染 (动态控制节点是否展示)
8. v-text指令：
	- 作用：向其所在的节点中渲染文本内容。
   	- 与插值语法的区别：v-text 会替换掉节点中的内容，{{xx}} 则不会。
10. v-html指令：
	- 作用：向指定节点中渲染包含html结构的内容。
    - 与插值语法的区别：
        - v-html 会替换掉节点中所有的内容，{{xx}} 则不会。
        - v-html 可以识别 html 结构。
    - 严重注意：v-html有安全性问题！！！！
        - .在网站上动态渲染任意 HTML 是非常危险的，容易导致XSS攻击。
        - .一定要在可信的内容上使用 v-html，永不要用在用户提交的内容上！
10. v-cloak指令（没有值）：
    - 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉 v-cloak 属性。
    - 使用css配合 v-cloak 可以解决网速慢时页面展示出 {{xxx}} 的问题。
      - CSS 中属性选择器 v-clock 然后设置 display 为 none
11. v-once指令：
    - v-once 所在节点在初次动态渲染后，就视为静态内容了。
    - 以后数据的改变不会引起 v-once 所在结构的更新，可以用于优化性能。
12. v-pre指令：
    - 跳过其所在节点的编译过程。
    - 可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译。

```html
<div id="root">
    <div>你好，{{name}}</div>
    <div v-text="name"></div>  <!-- name 的值会出现在 div 元素中 -->
    <div v-html="str"></div>	<!-- str 的值会出现在 div 元素中，且能够解析 HTML 元素 -->
    <h2 v-cloak>{{name}}</h2>
    <h2 v-once>初始化的n值是:{{n}}</h2>	<!-- n 的值不能更改 -->
    <h2 v-pre>Vue其实很简单</h2>	<!-- 当有 v-pre 时， Vue 编译的时候会直接跳过，不会检查里面是否有插值语法或者方法 -->
</div>

<script type="text/javascript">

    new Vue({
        el:'#root',
        data:{
            n:1,
            name:'曹操',
            str:'<h3>你好啊！</h3>',
        }
    })
</script>
```

### 自定义指令 directives


1. 定义语法：
  - 局部指令：

    ```javascript
    new Vue({
        directives:{指令名:配置对象}
    })
    
    new Vue（{
    	directives{指令名:回调函数}
    }）
    ```

  - 全局指令：

	`Vue.directive(指令名, 配置对象) `或  ` Vue.directive(指令名, 回调函数)`

2. 配置对象中常用的3个回调：
    - bind：指令与元素成功绑定时调用。
    - inserted：指令所在元素被插入页面时调用。
    - update：指令所在模板结构被重新解析时调用。

3. 备注：
    - 指令定义时不加 v-，但使用时要加 v-；
    - 指令名如果是多个单词，要使用 `kebab-case` 命名方式，不要用 `camelCase `命名。

```html
<div id="root">
    <h2>放大10倍后的n值是：<span v-big="n"></span> </h2>
    <hr/>
    <input type="text" v-fbind:value="n">
</div>

<script type="text/javascript">

    //定义全局指令
    Vue.directive('fbind',{
        //指令与元素成功绑定时（一上来）
        bind(element, binding){
            element.value = binding.value
        },
        //指令所在元素被插入页面时
        inserted(element, binding){
            element.focus()
        },
        //指令所在的模板被重新解析时
        update(element, binding){
            element.value = binding.value
        }
    })

    new Vue({
        el:'#root',
        data:{
            name:'吕不韦',
            n:1
        },
        directives:{
            'big-number'(element, binding){ 代码块 }, // 名字由多个单词组成时写成这种形式
            // big函数何时会被调用？
            // 1.指令与元素成功绑定时（一上来）。
            // 2.指令所在的模板被重新解析时。
            big(element, binding){
                console.log('big',this) //注意此处的 this 是 window
                // console.log('big')
                element.innerText = binding.value * 10
            },
            fbind:{
                //指令与元素成功绑定时（一上来）
                bind(element, binding){
                    element.value = binding.value
                },
                //指令所在元素被插入页面时
                inserted(element, binding){
                    element.focus()
                },
                //指令所在的模板被重新解析时
                update(element, binding){
                    element.value = binding.value
                }
            }
        }
    })
</script>
```

---

## 生命周期

![生命周期](https://github.com/EthanWhite134/learn_Vue/blob/main/images/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

1. 又名：生命周期回调函数、生命周期函数、生命周期钩子。
2. 是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数。
3. 生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。
4. 生命周期函数中的this指向是vm 或 组件实例对象。

### 常用的生命周期钩子：

1. mounted: 发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】。

2. beforeDestroy: 清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。

### 关于销毁Vue实例

1. 销毁后借助Vue开发者工具看不到任何信息。

2. 销毁后自定义事件会失效，但原生DOM事件依然有效。

3. 一般不会在beforeDestroy操作数据，因为即便操作数据，也不会再触发更新流程了。

```javascript
new Vue({
    el:'#root',
    // template:`
    // 	<div>
    // 		<h2>当前的n值是：{{n}}</h2>
    // 		<button @click="add">点我n+1</button>
    // 	</div>
    // `,
    data:{
        n:1
    },
    methods: {
        add(){
            console.log('add')
            this.n++
        },
        bye(){
            console.log('bye')
            this.$destroy()
        }
    },
    watch:{
        n(){
            console.log('n变了')
        }
    },
    beforeCreate() {
        console.log('beforeCreate')
    },
    created() {
        console.log('created')
    },
    beforeMount() {
        console.log('beforeMount')
    },
    mounted() {
        console.log('mounted')
    },
    beforeUpdate() {
        console.log('beforeUpdate')
    },
    updated() {
        console.log('updated')
    },
    beforeDestroy() {
        console.log('beforeDestroy')
    },
    destroyed() {
        console.log('destroyed')
    },
})
```

---

## 组件

### 非单文件组件

##### Vue中使用组件的三大步骤：

1. 定义组件(创建组件)
2. 注册组件
3. 使用组件(写组件标签)

##### 如何定义一个组件？

1. 使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options几乎一样，但也有点区别；
2. 区别如下：
    - el不要写，为什么？

       最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器。

    - data必须写成函数，为什么？ 

       避免组件被复用时，数据存在引用关系。

    备注：使用template可以配置组件结构。

##### 如何注册组件？

1. 局部注册：靠new Vue的时候传入components选项
2. 全局注册：靠Vue.component('组件名',组件)

##### 编写组件标签：

`<school></school>`

```html
<div id="root">
    <!-- 第三步：编写组件标签 -->
    <school></school>
    <student></student>
</div>

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
                schoolName:'MIT',
                address:'US'
            }
        },
        methods: {
            showName(){
                alert(this.schoolName)
            }
        },
    })

    //第一步：创建 student 组件
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
    const hello = Vue.extend({ })
    // 简写
    const hello = ({})
    //第二步：全局注册组件
    Vue.component('hello',hello)
 //创建vm
    new Vue({
        el:'#root',
        data:{
            msg:'你好啊！'
        },
        //第二步：注册组件（局部注册）
        components:{
            school,
            student
        }
    })

    new Vue({
        el:'#root2',
    })
</script>
```

#### 几个注意点
##### 关于组件名
1. 一个单词组成
    - 第一种写法(首字母小写)：school
    - 第二种写法(首字母大写)：School
2. 多个单词组成
    - 第一种写法(kebab-case命名)：my-school
    - 第二种写法(CamelCase命名)：MySchool (需要Vue脚手架支持)
3. 备注：
    - 组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行。
    - 可以使用name配置项指定组件在开发者工具中呈现的名字。

##### 关于组件标签:
1. 第一种写法：<school></school>
2. 第二种写法：<school/>
3. 备注：不用使用脚手架时，<school/>会导致后续组件不能渲染。

##### 一个简写方式
`const school= Vue.extend(options) 可简写为：const school = options`

#### 组件的嵌套

就你想的那样，嵌套就行

#### VueComponent

1. school组件本质是一个名为`VueComponent`的构造函数，且不是程序员定义的，是Vue.extend生成的。

2. 我们只需要写`<school/>`或``<school></school>``，Vue解析时会帮我们创建school组件的实例对象，
	- 即Vue帮我们执行的：new VueComponent(options)。

3. 特别注意：每次调用Vue.extend，返回的都是一个全新的`VueComponent`！！！！

4. 关于this指向：
    - 组件配置中：
    	- data函数、methods中的函数、watch中的函数、`computed `中的函数 它们的this均是【`VueComponent`实例对象】。
    - `new Vue(options)`配置中：
		- data 函数、methods中的函数、watch中的函数、computed中的函数 它们的 `this` 均是【Vue实例对象】。

5. VueComponent 的实例对象，以后简称 vc（也可称之为：组件实例对象）。
	- Vue 的实例对象，以后简称 vm。

####         一个重要的内置关系

1. `VueComponent.prototype.__proto__ === Vue.prototype`
2. 为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。

### 单文件组件

```html
<!-- index.html 文件 -->
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

```javascript
// main.js 文件，入口文件
import App from './App.vue'
new Vue({
	el:'#root',
	template:`<App></App>`,
	components:{App},
})
```

```vue
App.vue 文件，负责管理所有的 组件
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

```vue
School.vue 文件，一个 Vue的组件
<template>
	html 文档
	<div class="demo">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
		<button @click="showName">点我提示学校名</button>	
	</div>
</template>

<script>
    脚本
	 export default {
		name:'School',
		data(){
			return {
				name:'皇家学院',
				address:'咸阳'
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
    样式
	.demo{
		background-color: orange;
	}
</style>
```

```vue
Student.vue 文件，一个 Vue 的组件
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
