# Vue
MVVM思想
* M：model 包括数据和一些基本操作
* V：view 视图，页面渲染结果
* VM：View-model，模型与视图间的双向操作（无需开发人员干涉）

~~~
view     ViewModel  Model
只要我们Model发生了改变，View上自然就会表现出来。
当用户修改了View，Model中的数据也会跟着改变。
把开发人员从烦琐的D0M操作中解放出来，把关注点放在如何操作Model上。
~~~
安装
* 下载js并用 <script> 标签引入：<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
* 或者在VScode控制台使用npm install vue导入。步骤分为：
    1先npm init -y初始化项目，生成了一个package.json文件，说明他是一个npm管理的项目
    类似于maven的pom.xml
    2npm install vue，安装后在项目node_modules里有vue
    类似maven install拉取远程到本地
  
使用：
* new Vue
* 在dom中{{name}}代表从模型中放到view中
* v-model实现双向绑定
~~~
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>

    <div id="app">
        <input type="text" v-model="num">
        v-model实现双向绑定。此处代表输入框和vue里的data绑定
        
        <button v-on:click="num++">点赞</button>
        v-on:click绑定事件，实现自增。
        
        <button v-on:click="cancel">取消</button>
        回调自定义的方法。 此时字符串里代表的函数
        
        <h1> {{name}} ,非常帅，有{{num}}个人为他点赞{{hello()}}</h1>
        先从vue中拿到值填充到dom，input再改变num值，vue实例更新，然后此处也更新
    </div>

    <!-- 导入依赖 -->
    <script src="./node_modules/vue/dist/vue.js"></script>

    <script>
        //1、vue声明式渲染
        let vm = new Vue({ //生成vue对象
            el: "#app",//绑定元素 div id="app" // 可以指定恰标签，但是不可以指定body标签
            data: {  //封装数据
                name: "张三",  // 也可以使用{} //表单中可以取出
                num: 1
            },
            methods:{  //封装方法
                cancel(){
                    this.num -- ;
                },
                hello(){
                    return "1"
                }
            }
        });
        // 还可以在html控制台vm.name

        //2、双向绑定,模型变化，视图变化。反之亦然。
        //3、事件处理

        //v-xx：指令

        //1、创建vue实例，关联页面的模板，将自己的数据（data）渲染到关联的模板，响应式的
        //2、指令来简化对dom的一些操作。
        //3、声明方法来做更复杂的操作。methods里面可以封装方法。

    </script>
</body>

</html>

~~~
在VSCode中安装vue 2 snippets语法提示插件，在谷歌浏览器中安装vue-devtool
  
### 1、v-text、v-html、v-ref
>插值闪烁：
  
>使用{{}}方式在网速较慢时会出现问题。在数据未加载完成时，页面会显示出原始的{{}}，
  
>加载完毕后才显示正确数据，我们称为插值闪烁。
  
>我们将网速调慢一些，然后刷新页面，试试看刚才的案例
  
~~~
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
   
    <div id="app">
        {{msg}}  {{1+1}}  {{hello()}} 前面的内容如果网速慢的话会先显示括号，然后才替换成数据。
        v-html 和v-text能解决这个问题
        <br/>
        
        用v-html取内容
        <span v-html="msg"></span>
        
        <br/>
        原样显示
        <span v-text="msg"></span>  
    </div>
   
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        new Vue({
            el:"#app",
            data:{
                msg:"<h1>Hello</h1>",
                link:"http://www.baidu.com"
            },
            methods:{
                hello(){
                    return "World"
                }
            }
        })
    </>
    
</body>
</html>

 ~~~
  
  ### 2、单向绑定v-bind:
  ~~~
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
</head>
<body>

    <!-- 给html标签的属性绑定 -->
    <div id="app"> 

        <a v-bind:href="link">跳转</a>

        <!-- class,style  {class名：vue值}-->
        <span v-bind:class="{active:isActive,'text-danger':hasError}"
          :style="{color: color1,fontSize: size}">你好</span>

    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        let vm = new Vue({
            el:"#app",
            data:{
                link: "http://www.baidu.com",
                isActive:true,
                hasError:true,
                color1:'red',
                size:'36px'
            }
        })
    </script>

</body>
</html>

  ~~~
### 样式绑定v-bind:class

 ~~~
  
实例中将 isActive 设置为 true 显示了一个绿色的 div 块，如果设置为 false 则不显示：
<div v-bind:class="{ 'active': isActive }"></div>


text-danger 类背景颜色覆盖了 active 类的背景色：
<div class="static"
     v-bind:class="{ 'active' : isActive, 'text-danger' : hasError }">
</div>
~~~
  
~~~
  <div id="app">
    <div v-bind:class="classObject"></div>
</div>

new Vue({
    el: '#app',
    data: {
        isActive: true,
        error: {
            value: true,
            type: 'fatal'
        }
    },
    computed: {
        classObject: function () {
            return {
                base: true,
                active: this.isActive && !this.error.value,
                'text-danger': this.error.value && this.error.type === 'fatal',
            }
        }
    }
})

  ~~~
### 3、双向绑定v-model
  ~~~
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>

    <!-- 表单项，自定义组件 -->
    <div id="app">

        精通的语言：如果是多选框，那么会把每个value值赋值给vue数据
            <input type="checkbox" v-model="language" value="Java"> java<br/>
            <input type="checkbox" v-model="language" value="PHP"> PHP<br/>
            <input type="checkbox" v-model="language" value="Python"> Python<br/>
        选中了 {{language.join(",")}}
    </div>
    
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        let vm = new Vue({
            el:"#app",
            data:{
                language: []
            }
        })
    </script>

</body>
</html>

  ~~~
 ###4、v-on事件
  事件监听可以使用 v-on 指令

v-on:事件类型="方法" ，可以简写成@事件类型="方法"
  
>事件冒泡：大小div都有单机事件，点了内部div相当于外部div也点击到了。
  
>如果不想点击内部div冒泡到外部div，可以使用.prevent阻止事件冒泡
  
>用法是v-on:事件类型.事件修饰符="方法"
  
>还可以绑定按键修饰符
  
>v-on:keyup.up=“num+=2” @keyup.down=“num-=2” @click.ctrl=“num=10”
  
按键修饰符
~~~
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
                
        <!--事件中直接写js片段-->
        <button v-on:click="num++">点赞</button>
        <!--事件指定一个回调函数，必须是Vue实例中定义的函数-->
        <button @click="cancel">取消</button>
        <!--  -->
        <h1>有{{num}}个赞</h1>


        <!-- 事件修饰符 -->
        <div style="border: 1px solid red;padding: 20px;" v-on:click.once="hello">
            大div
            <div style="border: 1px solid blue;padding: 20px;" @click.stop="hello">
                小div <br />
                <a href="http://www.baidu.com" @click.prevent.stop="hello">去百度</a>
            </div>
        </div>



        <!-- 按键修饰符： -->
        <input type="text" v-model="num" v-on:keyup.up="num+=2" @keyup.down="num-=2" @click.ctrl="num=10"><br />

        提示：

    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        new Vue({
            el:"#app",
            data:{
                num: 1
            },
            methods:{
                cancel(){
                    this.num--;
                },
                hello(){
                    alert("点击了")
                }
            }
        })
    </script>
</body>

</html>

~~~

### 5、v-for遍历
~~~
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>

    <div id="app">
        <ul>
            <!-- 4、遍历的时候都加上:key来区分不同数据，提高vue渲染效率 -->
            <li v-for="(user,index) in users" :key="user.name" v-if="user.gender == '女'">
                <!-- 1、显示user信息：v-for="item in items" -->
               当前索引：{{index}} ==> {{user.name}}  ==>   
                  {{user.gender}} ==>{{user.age}} <br>
                <!-- 2、获取数组下标：v-for="(item,index) in items" -->
                <!-- 3、遍历对象：
                        v-for="value in object"
                        v-for="(value,key) in object"
                        v-for="(value,key,index) in object" 
                -->
                对象信息：
                <span v-for="(v,k,i) in user">{{k}}=={{v}}=={{i}}；</span>
                <!-- 4、遍历的时候都加上:key来区分不同数据，提高vue渲染效率 -->
            </li>

            
        </ul>

        <ul>
            <li v-for="(num,index) in nums" :key="index"></li>
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>         
        let app = new Vue({
            el: "#app",
            data: {
                users: [
                { name: '柳岩', gender: '女', age: 21 },
                { name: '张三', gender: '男', age: 18 },
                { name: '范冰冰', gender: '女', age: 24 },
                { name: '刘亦菲', gender: '女', age: 18 },
                { name: '古力娜扎', gender: '女', age: 25 }
                ],
                nums: [1,2,3,4,4]
            },
        })
    </script>
</body>

</html>

~~~

~~~
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' }
    ]
  }
})

<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>

~~~
~~~
  <blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post">
</blog-post>

Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `
})

~~~
# 6、v-if和v-show
在vue实例的data指定一个bool变量，然后v-show赋值即可。

show里的字符串也可以比较

if是根据表达式的真假，切换元素的显示和隐藏（操作dom元素）

区别：show的标签F12一直都在，if的标签会移除，

if操作dom树对性能消耗大

v-if和v-show.html
~~~
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <!-- 
        v-if，顾名思义，条件判断。当得到结果为true时，所在的元素才会被渲染。
        v-show，当得到结果为true时，所在的元素才会被显示。 
    -->
    <div id="app">
        <button v-on:click="show = !show">点我呀</button>
        <!-- 1、使用v-if显示 -->
        <h1 v-if="show">if=看到我....</h1>
        <!-- 2、使用v-show显示 -->
        <h1 v-show="show">show=看到我</h1>
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>
        
    <script>
        let app = new Vue({
            el: "#app",
            data: {
                show: true
            }
        })
    </script>

</body>

</html>
~~~
### 7、v-else和v-else-if
~~~
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <button v-on:click="random=Math.random()">点我呀</button>
        <span>{{random}}</span>

        <h1 v-if="random>=0.75">
            看到我啦? &gt;= 0.75
        </h1>
        <h1 v-else-if="random>=0.5">
            看到我啦? &gt;= 0.5
        </h1>
        <h1 v-else-if="random>=0.2">
            看到我啦? &gt;= 0.2
        </h1>
        <h1 v-else>
            看到我啦? &lt; 0.2
        </h1>

    </div>


    <script src="../node_modules/vue/dist/vue.js"></script>
        
    <script>         
        let app = new Vue({
            el: "#app",
            data: { random: 1 }
        })     
    </script>
</body>

</html>

~~~
  
缩写@
Vue.js 为两个最为常用的指令提供了特别的缩写：

v-bind 缩写
  
~~~
  <!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
~~~
v-on 缩写
~~~
  <!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>

~~~
  
  
### 2、计算属性和侦听器
  计算属性computed
什么是计算属性：属性不是具体值，而是通过一个函数计算出来的，随时变化
  
  ~~~
  <div id="app">
    <p>原始字符串: {{ message }}</p>
    <p>计算后反转字符串: {{ reversedMessage }}</p>
</div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            message: 'Runoob!'
        },
        computed: {
            // 计算属性的 getter
            reversedMessage: function () {
                // `this` 指向 vm 实例
                return this.message.split('').reverse().join('')
            }
        }
    })
</script>

  ~~~
  
  监听$watch
  监听属性 watch，我们可以通过 watch 来响应数据的变化。

以下实例通过使用 watch 实现计数器
  ~~~
  <div id = "app">
    <p style = "font-size:25px;">计数器: {{ counter }}</p>
    <button @click = "counter++" style = "font-size:25px;">点我</button>
</div>
<script type = "text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            counter: 1
        }
    });
    vm.$watch('counter', function(nval, oval) { // new old
        alert('计数器值的变化 :' + oval + ' 变为 ' + nval + '!');
    });
</script>
~~~
  ~~~
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <!-- 某些结果是基于之前数据实时计算出来的，我们可以利用计算属性。来完成 -->
        <ul>
            <li>西游记； 价格：{{xyjPrice}}，数量：<input type="number" v-model="xyjNum"> </li>
            <li>水浒传； 价格：{{shzPrice}}，数量：<input type="number" v-model="shzNum"> </li>
            <li>总价：{{totalPrice}}</li>
            {{msg}}
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        //watch可以让我们监控一个值的变化。从而做出相应的反应。
        new Vue({
            el: "#app",
            data: {
                xyjPrice: 99.98,
                shzPrice: 98.00,
                xyjNum: 1,
                shzNum: 1,
                msg: ""
            },
            computed: {
                totalPrice(){
                    return this.xyjPrice*this.xyjNum + this.shzPrice*this.shzNum
                }
            },
            watch: {
                xyjNum: function(newVal,oldVal){
                    if(newVal>=3){
                        this.msg = "库存超出限制";
                        this.xyjNum = 3
                    }else{
                        this.msg = "";
                    }
                }
            },
        })
    </script>

</body>

</html>

  ~~~
  #### 过滤器filter
  
  定义filter组件后，管道符后面跟具体过滤器{{user.gender | gFilter}}
  ~~~
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <!-- 过滤器常用来处理文本格式化的操作。过滤器可以用在两个地方：双花括号插值和 v-bind 表达式 -->
    <div id="app">
        <ul>
            <li v-for="user in userList">
                {{user.id}} ==> {{user.name}} ==> {{user.gender == 1?"男":"女"}} ==>
                {{user.gender | genderFilter}} ==> {{user.gender | gFilter}}
            </li>
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>

        // 全局过滤器
        Vue.filter("gFilter", function (val) {
            if (val == 1) {
                return "男~~~";
            } else {
                return "女~~~";
            }
        })

        let vm = new Vue({
            el: "#app",
            data: {
                userList: [
                    { id: 1, name: 'jacky', gender: 1 },
                    { id: 2, name: 'peter', gender: 0 }
                ]
            },
            filters: { // 局部过滤器，只可以在当前vue实例中使用
                genderFilter(val) {
                    if (val == 1) {
                        return "男";
                    } else {
                        return "女";
                    }
                }
            }
        })
    </script>
</body>

</html>
~~~
  
 ### 4、组件化
  在大型应用开发的时候，页面可以划分成很多部分。往往不同的页面，也会有相同的部分。例如可能会有相同的头部导航。

但是如果每个页面都自开发，这无疑增加了我们开发的成本。所以我们会把页面的不同分拆分成立的组件，然后在不同页面就可以共享这些组件，避免重复开发。

在vue里，所有的vue实例都是组件

组件其实也是一个vue实例，因此它在定义时也会接收：data、methods、生命周期函数等
不同的是组件不会与页面的元素绑定（所以不写el），否则就无法复用了，因此没有el属性。
但是组件渲染需要html模板，所以增加了template属性，值就是HTML模板
data必须是一个函数，不再是一个对象。
全局组件定义完毕，任何vue实例都可以直接在HTML中通过组件名称来使用组件了
下面的html内容是在说：

<div id="app">这个块跟下面的new Vue对象绑定
vue对象里指定了components属性，是指：标签里有子标签，'button-counter': buttonCounter代表有个组件叫buttonCounter，可以填充标签<button-counter>
  1) 全局组件
  ~~~
  // 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () { // data是函数
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>' // 渲染的模板
})

// 上面代表组件名为button-counter   
会拿 <button v-on:click="count++">You clicked me {{ count }} times.</button> 替代
~~~
  2) 局部组件
  ~~~
  <div id="app">
    <runoob></runoob>
</div>

<script>
    
    // 声明一个组件
    // 这个东西可以在当前文件中写，也可以import
    var Child = {
        template: '<h1>自定义组件!</h1>'
        // 不定义el
        // 可以定义data函数
    }

    // 创建根实例(它是使用组件的)
    new Vue({
        el: '#app',
        components: {
            // <runoob> 将只在父模板可用  别的组件如果想使用其他局部组件，必须指定
            'runoob': Child //局部组件在div中感应不到，所以要绑定一下，原来是就是说碰到runoob标签就替换为template内容
        }
    })
</script>
~~~
  ### 5、生命周期钩子函数
  每个vue实例在被创建时都要经过一系列的初始化过程：创建实例，装载模板、渲染模板等等。vue为生命周期中的每个状态都设置了钩子函数（监听函）。每当vue实列处于不同的生命周期时，对应的函数就会被触发调用。
  ![image](https://user-images.githubusercontent.com/59955759/121802390-a1ec0300-cc6e-11eb-9435-a7c7a15377b8.png)

  ~~~
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <span id="num">{{num}}</span>
        <button @click="num++">赞！</button>
        <h2>{{name}}，有{{num}}个人点赞</h2>
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>
    
    <script>
        let app = new Vue({
            el: "#app",
            data: {
                name: "张三",
                num: 100
            },
            methods: {
                show() {
                    return this.name;
                },
                add() {
                    this.num++;
                }
            },
            beforeCreate() {
                console.log("=========beforeCreate=============");
                console.log("数据模型未加载：" + this.name, this.num);
                console.log("方法未加载：" + this.show());
                console.log("html模板未加载：" + document.getElementById("num"));
            },
            created: function () {
                console.log("=========created=============");
                console.log("数据模型已加载：" + this.name, this.num);
                console.log("方法已加载：" + this.show());
                console.log("html模板已加载：" + document.getElementById("num"));
                console.log("html模板未渲染：" + document.getElementById("num").innerText);
            },
            beforeMount() {
                console.log("=========beforeMount=============");
                console.log("html模板未渲染：" + document.getElementById("num").innerText);
            },
            mounted() {
                console.log("=========mounted=============");
                console.log("html模板已渲染：" + document.getElementById("num").innerText);
            },
            beforeUpdate() {
                console.log("=========beforeUpdate=============");
                console.log("数据模型已更新：" + this.num);
                console.log("html模板未更新：" + document.getElementById("num").innerText);
            },
            updated() {
                console.log("=========updated=============");
                console.log("数据模型已更新：" + this.num);
                console.log("html模板已更新：" + document.getElementById("num").innerText);
            }
        });
    </script>
</body>

</html>
~~~
 ###  6、路由
  Vue.js 路由允许我们通过不同的 URL 访问不同的内容。

通过 Vue.js 可以实现多视图的单页Web应用（single page web application，SPA）。
  ~~~
  
 cnpm install vue-router
~~~
  Vue.js + vue-router 可以很简单的实现单页应用。

<router-link>是一个组件，该组件用于设置一个导航链接，切换不同 HTML 内容。 to 属性为目标地址， 即要显示的内容。

以下实例中我们将 vue-router 加进来，然后配置组件和路由映射，再告诉 vue-router 在哪里渲染它们
  ~~~
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
 
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
~~~
  ~~~
  // 0. 如果使用模块化机制编程，导入 Vue 和 VueRouter，要调用 Vue.use(VueRouter)

// 1. 定义（路由）组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
    routes // （缩写）相当于 routes: routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
    router
}).$mount('#app')

// 现在，应用已经启动了！
~~~
  # 五、element-ui
  官网： https://element.eleme.cn/#/zh-CN/component/installation

安装
  ~~~
  # 直接npm安装，在项目中执行
npm i element-ui -S

# 或者引入样式

  ~~~
  使用：

在 main.js 中写入以下内容：
  ~~~
  import ElementUI  from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css';

// 让vue使用ElementUI组件
Vue.use(ElementUI);
~~~
  
  
