# 特此说明~~注意看：
## 1) 本文主要分析vue作为一个MVVM框架的基本实现原理
         数据代理
         模板解析
		 数据绑定
## 2) 不直接看vue.js的源码
## 3) 剖析github上某基友仿vue实现的mvvm库
## 4) 地址: [https://github.com/DMQ/mvvm](https://github.com/DMQ/mvvm)
# 准备知识
## 1) [].slice.call(lis): 将伪数组转换为真数组(新数组)
* 案例：

```
// 1. [].slice.call(lis): 将伪数组转换为真数组(或者用from)
//伪数组（对象，length和数组下标属性）
const lis=document.getElementsByTagName("li")
//查看是否为伪数组，并且查看是否有foreach
console.log(lis instanceof Array,lis.forEach)
//call 调用执行这个函数的同时更改this的指向，返回一个真的数组
const lis2=Array.prototype.slice.call(lis)
//查看是否为真数组，并且查看是否有foreach
console.log(lis2 instanceof Array,lis2.forEach,lis2)
```
## 2) node.nodeType: 得到节点类型

```
2. node.nodeType: 得到节点类型
//元素---1    属性----2   文本(标签中的value)----3  注释---8  文档----9
const elementNode=document.getElementById('test')
const attrNode=elementNode.getAttributeNode("id")
const textNode=elementNode.firstChild
```

## 3) Object.defineProperty(obj, propName, {}): 给对象添加/修改属性(指定描述符)

* configurable: true/false  是否可以重新define
* enumerable: true/false 是否可以枚举(for..in / keys())
* value: 指定初始值
* true/false value是否可以修改
* get: 回调函数, 用来得到当前属性值
* set: 回调函数, 用来监视当前属性值的变化

```
const p = {
	firstName: 'A',
	lastName: 'B'
	}
	Object.defineProperty(p, 'fullName', { // 属性描述符
// 数据描述符
configurable: false,  // 是否可以重新define
enumerable: false, // 是否可以枚举(for..in / keys())
//    value: 'A-B', // 指定初始值
//    writable: true, // value是否可以修改
// 存取描述符
get () {
  return this.firstName + '-' + this.lastName
},
set (value) {
  const names = value.split('-')
  this.firstName = names[0]
  this.lastName = names[1]
}
})
console.log(p.fullName) // A-B
p.firstName = 'C'
p.lastName = 'D'
console.log(p.fullName) // C-D
p.fullName = 'E-F'
console.log(p.firstName, p.lastName) // E F
```
## 4)  Object.keys(obj): 得到对象自身可枚举的属性名的数组
* Object.keys(obj): 得到对象自身可枚举属性组成的数组
* Object.values(obj):得到对象自身可枚举属性值组成的数组  

`const names=Object.keys(p)`
`console.log(names)`

## 5) obj.hasOwnProperty(prop): 判断prop是否是obj自身的属性

```
obj.hasOwnProperty(prop): 判断prop是否是obj自身的属性
console.log(p.hasOwnProperty("fullName")) //true
console.log(p.hasOwnProperty("toString"))  //false
```

## 6) DocumentFragment: 文档碎片(高效批量更新多个节点)，内存节点的容器对象

```
//1.在内存中创建一个空容器
const fragment=document.createDocumentFragment()
//2.取出ul下的所有节点并转移到fragment中
const ul=document.getElementById('fragment_test')
let child
while (child =ul.firstChild){
//一个节点只能有一个父节点，appendChild将child节点从ul转除，添加到fragment容器中，成为fragment中的子节点
  fragment.appendChild(child)
}
//3.更新fragment中所有的li的文本
[].slice.call(fragment.childNodes).forEach(node=>{
  if(node.nodeType===1){
    node.textContent="我爱中国" //更新节点时不会更新界面！！！
  }
})
//4.将fragment中的子节点添加到ul中
ul.appendChild(fragment)
```

# 一、数据代理
## 1) 数据代理: 通过一个对象代理对另一个对象(在前一个对象内部)中属性的操作(读/写)
## 2) vue数据代理: 通过vm对象来代理data对象中所有属性的操作
## 3) 好处: 更方便的操作data中的数据
## 4) 基本实现流程
* 通过Object.defineProperty()给vm添加与data对象的属性对应的属性描述符
* 所有添加的属性都包含getter/setter
* getter/setter内部去操作data中对应的属性数据
	
```
<body>
	<!--
	1. vue数据代理: data对象的所有属性的操作(读/写)由vm对象来代理操作
	2. 好处: 通过vm对象就可以方便的操作data中的数据
	3. 实现:
	  1). 通过Object.defineProperty(vm, key, {})给vm添加与data对象的属性对应的属性
	  2). 所有添加的属性都包含get/set方法
	  3). 在get/set方法中去操作data中对应的属性
	-->
	<!--编译-->
	<script type="text/javascript" src="./js/mvvm/compile.js"></script>
	<!--监视-->
	<script type="text/javascript" src="./js/mvvm/watcher.js"></script>
	<!--观察-->
	<script type="text/javascript" src="./js/mvvm/observer.js"></script>
	<script type="text/javascript" src="./js/mvvm/mvvm.js"></script>
	<script type="text/javascript">
	  const vm=new MVVM({
	    el:"#test",
	    data:{
	      name:"DAMU"
	    }
	  })
	  console.log(vm)
	  console.log(vm.name) //读取vm中data的name属性值---代理读
	  vm.name="SADAMU"  //数据保存到vm中data的name属性上---代理写
	  console.log(vm._data.name)
	  /*综上所述：我们操作vm，实际操作的是_data中的数据，称之为数据代理
	  * vm称为代理对象，data称为被代理对象，利用数据代理是为了编码简单*/
	</script>
</body>
```

# 二、模板解析
## 1. 模板解析的基本流程
* 1)将el的所有子节点取出, 添加到一个新建的文档 fragment 对象中
* 2)对 fragment 中的所有层次子节点递归进行编译解析处理
	* 对大括号表达式文本节点进行解析
	* 对元素节点的指令属性进行解析
	* 事件指令解析
	* 一般指令解析
* 3)         将解析后的 fragment 添加到 el 中显示
## 2. 模板解析(1): 大括号表达式解析
* 1)根据正则对象得到匹配出的表达式字符串: 子匹配/RegExp.$1  name
* 2)从data中取出表达式对应的属性值
* 3)将属性值设置为文本节点的textContent
## 3. 模板解析(2): 事件指令解析
* 1)从指令名中取出事件名
* 2)根据指令的值(表达式)从methods中得到对应的事件处理函数对象
* 3)给当前元素节点绑定指定事件名和回调函数的dom事件监听
* 4)指令解析完后, 移除此指令属性
## 4. 模板解析(3): 一般指令解析
* 1)得到指令名和指令值(表达式)   text/html/class  msg/myClass
* 2)从data中根据表达式得到对应的值
* 3)根据指令名确定需要操作元素节点的什么属性
	* v-text---textContent属性
	* v-html---innerHTML属性
	* v-class--className属性
* 4)将得到的表达式的值设置到对应的属性上
* 5)移除元素的指令属性

## #mvvm.js vm源码解析：

```
function MVVM(options) {//相当于Vue的构造函数
    //将配置对象保存在vm上
    this.$options = options;
    //将data数据对象保存在vm与data变量上
    var data = this._data = this.$options.data;
    //将vm保存到me变量上
    var me = this;
    //遍历data中所有属性并实现对其代理
    Object.keys(data).forEach(function(key) {//属性名：name
        //对指定属性名的属性实现代理： vm.xx--->vm._data.xx
        me._proxy(key);
    });
    //对data中所有层次的属性进行监视
    observe(data, this);
    //创建一个编译对象
    this.$compile = new Compile(options.el || document.body, this)
};

MVVM.prototype = {
    $watch: function(key, cb, options) {
        new Watcher(this, key, cb);
    },
    /*对指定属性名的属性实现代理*/
    _proxy: function(key) {
        //保存vm
        var me = this;
        //给vm添加属性（使用属性描述符）
        Object.defineProperty(me, key, {
            configurable: false,//不可以重新在定义
            enumerable: true,   //可以枚举
            //当通过vm.name读取属性值时，自动调用，去data中获取对应的属性值返回（作为vm的属性值）
            get: function proxyGetter() {
                return me._data[key];
            },
            //当通过vm.name=value改变了属性值时自动调用，将最新的值保存到data中对应的属性上
            set: function proxySetter(newVal) {
                me._data[key] = newVal;
            }
        });
    }
};
```

## compile.js 编译源码解析：

```
function Compile(el, vm) {
    //保存vm
    this.$vm = vm;
    //保存el元素
    this.$el = this.isElementNode(el) ? el : document.querySelector(el);
    //只有当el元素存在
    if (this.$el) {
        //1.取出el中所有子节点并保存到内存的fragment容器中
        this.$fragment = this.node2Fragment(this.$el);
        //2.初始化编译（对fragment中所有层次的子节点）
        this.init();
        //3.将fragment添加到el中显示
        this.$el.appendChild(this.$fragment);
    }
}

Compile.prototype = {
    node2Fragment: function(el) {
        //创建内存中的fragment容器
        var fragment = document.createDocumentFragment(),
            child;

        // 遍历el中所有子节点将其转移到fragment容器中
        while (child = el.firstChild) {
            fragment.appendChild(child);
        }
        //将fragment中的子节点插入到el中
        return fragment;
    },

    init: function() {
        //编译fragment中所有子节点
        this.compileElement(this.$fragment);
    },
    /*
    * 编译指定元素的所有子节点，同时利用递归实现所有层次子节点的编译
    * 1.element中的指令属性
    * 2.大括号表达式格式的文本节点*/
    compileElement: function(el) {
        //得到所有外层的子节点
        var childNodes = el.childNodes,
          //保存编译对象
            me = this;
        //遍历子节点
        [].slice.call(childNodes).forEach(function(node) {
            //得到节点的文本内容
            var text = node.textContent;
            //定义正则表达式（匹配大括号表达式的，并且自带了一个（）子匹配，匹配大括号中的表达式）
            var reg = /\{\{(.*)\}\}/;
            //如果是元素节点
            if (me.isElementNode(node)) {
                //编译元素节点的指令属性
                me.compile(node);
              //给如果是大括号表达式格式的文本节点
            } else if (me.isTextNode(node) && reg.test(text)) {
                //编译大括号表达式格式的文本节点
                me.compileText(node, RegExp.$1);//表达式：name
            }
            //如果当前子节点还有子节点，进行递归调用，实现对所有层次子节点进行编译
            if (node.childNodes && node.childNodes.length) {
                me.compileElement(node);
            }
        });
    },

    compile: function(node) {
        //得到所有的属性节点
        var nodeAttrs = node.attributes,
            //保存编译对象
            me = this;
        //遍历所有属性
        [].slice.call(nodeAttrs).forEach(function(attr) {
            //得到属性名：v-on:click
            var attrName = attr.name;
            //得到指令属性
            if (me.isDirective(attrName)) {
                //得到属性值（表达式）：show
                var exp = attr.value;
                //得到指令名：on：click
                var dir = attrName.substring(2);
                // 如果是事件指令
                if (me.isEventDirective(dir)) {
                    //进行事件指令编译
                    compileUtil.eventHandler(node, me.$vm, exp, dir);
                } else {
                  // 如果是普通指令，得到指令对应的编译工具函数执行编译（当前dir的值的可能：text/html/class）
                    compileUtil[dir] && compileUtil[dir](node, me.$vm, exp);
                }
                //移除指令属性
                node.removeAttribute(attrName);
            }
        });
    },
    //编译文本节点
    compileText: function(node, exp) {//exp：表达式（name）
      //调用编译工具对象的text（）进行编译
        compileUtil.text(node, this.$vm, exp);
    },

    isDirective: function(attr) {
        return attr.indexOf('v-') == 0;
    },

    isEventDirective: function(dir) {
        return dir.indexOf('on') === 0;
    },

    isElementNode: function(node) {
        return node.nodeType == 1;
    },

    isTextNode: function(node) {
        return node.nodeType == 3;
    }
};

// 用于解析指令/大括号表达式的工具对象
var compileUtil = {
    //编译v-text/大括号表达式的工具函数
    text: function(node, vm, exp) {
        this.bind(node, vm, exp, 'text');
    },
    //编译v-html的工具函数
    html: function(node, vm, exp) {
        this.bind(node, vm, exp, 'html');
    },
  //编译v-model的工具函数
    model: function(node, vm, exp) {
        //实现初始化显示
        this.bind(node, vm, exp, 'model');

        var me = this,
          //得到表达式的值
            val = this._getVMVal(vm, exp);
        //给节点绑定input事件监听
        node.addEventListener('input', function(e) {
          //当输入改变时，自动调用
          //得到输入框最新的值
            var newValue = e.target.value;
            if (val === newValue) {
                return;
            }
            //将最新的值保存到表达式对应的data的某个属性上---》触发数据绑定的流程
            me._setVMVal(vm, exp, newValue);
            //保存最新的值，
            val = newValue;
        });
    },
    //编译v-class的工具函数
    class: function(node, vm, exp) {
        this.bind(node, vm, exp, 'class');
    },
    //以上函数都会调用bind，真正编译指令的工具函数
    /* node：节点
    * vm：mvvm的实例
    * exp：表达式（当前为name）
    * dir：指令名（当前为text）*/
    bind: function(node, vm, exp, dir) {
        //根据指令名来得到对应的节点更新函数
        var updaterFn = updater[dir + 'Updater'];
        //执行更新函数去更新节点，实现初始化显示
        updaterFn && updaterFn(node, this._getVMVal(vm, exp));

        //创建与表达式对应的watcher对象，，用于更新界面节点
        new Watcher(vm, exp, function(value, oldValue) {
            //当data中数据发生变化时，应该自动调用去更新
            updaterFn && updaterFn(node, value, oldValue);
        });
    },

    // 事件处理
    /*
    * exp:表达式（show）
    * dir：指令名：（on：click）
    */
    eventHandler: function(node, vm, exp, dir) {
        //得到事件名/类型： click
        var eventType = dir.split(':')[1],
          //根据表达式从methods配置中取出对应的函数
            fn = vm.$options.methods && vm.$options.methods[exp];
      //如果都存在
        if (eventType && fn) {
            //给节点绑定指定事件名和回调函数（强制绑定this为vm）的dom事件监听
            node.addEventListener(eventType, fn.bind(vm), false);
        }
    },
    //得到指定表达式所对应的值
    _getVMVal: function(vm, exp) {
        var val = vm._data;
        exp = exp.split('.');
        exp.forEach(function(k) {
            val = val[k];
        });
        return val;
    },

    _setVMVal: function(vm, exp, value) {
        var val = vm._data;
        exp = exp.split('.');
        exp.forEach(function(k, i) {
            // 非最后一个key，更新val的值
            if (i < exp.length - 1) {
                val = val[k];
            } else {
                val[k] = value;
            }
        });
    }
};

//包含了n个用于更新节点的方法
var updater = {
  //更新节点的textContent属性
    textUpdater: function(node, value) {
        node.textContent = typeof value == 'undefined' ? '' : value;
    },
  //更新节点的innerhtml属性
    htmlUpdater: function(node, value) {
        node.innerHTML = typeof value == 'undefined' ? '' : value;
    },
  //更新节点的className属性
    classUpdater: function(node, value, oldValue) {
        var className = node.className;
        className = className.replace(oldValue, '').replace(/\s$/, '');
        var space = className && String(value) ? ' ' : '';
        node.className = className + space + value;
    },
  //更新节点的value属性
    modelUpdater: function(node, value, oldValue) {
        node.value = typeof value == 'undefined' ? '' : value;
    }
};
```

## observer.js 编译源码解析：

```
function Observer(data) {
    //保存data数据对象
    this.data = data;
    //开启对data中数据的劫持工作
    this.walk(data);
}

Observer.prototype = {
    walk: function(data) {
        var me = this;
        //遍历data中所有属性，key为属性名，第一次为name，wife。第二次为name与age
        Object.keys(data).forEach(function(key) {
            me.convert(key, data[key]);
        });
    },
    convert: function(key, val) {
        //给data重新定义响应式属性
        this.defineReactive(this.data, key, val);
    },

    defineReactive: function(data, key, val) {
        //为当前属性创建对应的dep对象
        var dep = new Dep();
        //通过隐式递归调用实现对所有层次属性的劫持
        var childObj = observe(val);
        //给data重新定义key属性（添加getter，setter）
        Object.defineProperty(data, key, {
            enumerable: true, // 可枚举
            configurable: false, // 不能再define
            //返回属性值---建立dep与watcher之间的关系
            get: function() {
                if (Dep.target) {
                    dep.depend();
                }
                return val;
            },
            //一旦属性值发生改变，通知dep/watcher去更新界面
            set: function(newVal) {
                if (newVal === val) {
                    return;
                }
                val = newVal;
                // 新的值是object的话，进行监听
                childObj = observe(newVal);
                // 通知订阅者
                dep.notify();
            }
        });
    }
};

function observe(value, vm) {
    if (!value || typeof value !== 'object') {
        return;
    }
    //创建一个观察者对象（一个观察者监视一层的属性）
    return new Observer(value);
};


var uid = 0;

function Dep() {
    this.id = uid++;
    this.subs = [];
}

Dep.prototype = {
    addSub: function(sub) {
        this.subs.push(sub);
    },

    depend: function() {
        Dep.target.addDep(this);
    },

    removeSub: function(sub) {
        var index = this.subs.indexOf(sub);
        if (index != -1) {
            this.subs.splice(index, 1);
        }
    },
    //通知所有相关的watcher
    notify: function() {
        this.subs.forEach(function(sub) {
            sub.update();
        });
    }
};

Dep.target = null;
```

## watcher.js 编译源码解析：

```
function Watcher(vm, exp, cb) {
    this.cb = cb; //用与更新界面节点的回调函数
    this.vm = vm;   //
    this.exp = exp; //节点包含的表达式
    this.depIds = {};//保存n个相关的dep的对象容器（属性名是dep的id，属性值是dep）
    this.value = this.get();//得到表达式所对应的value
}

Watcher.prototype = {
    update: function() {
        this.run();
    },
    run: function() {
        //重新获取表达式最新的值
        var value = this.get();
        //得到表达式原来的值
        var oldVal = this.value;
        //如果变化了
        if (value !== oldVal) {
          //保存最新的值
            this.value = value;
            //调用更新节点的回调函数
            this.cb.call(this.vm, value, oldVal);
        }
    },
    addDep: function(dep) {
        //如果关系还没有建立
        if (!this.depIds.hasOwnProperty(dep.id)) {
            //将当前watcher保存到dep的subs中
            dep.addSub(this); //dep到watcher的关系
            //将dep保存到当前watcher的depIds上
            this.depIds[dep.id] = dep; //watcher到dep的关系
        }
    },
    get: function() {
        //给dep指定watcher
        Dep.target = this;
        //读取表达式在data中对应的值，会导致相关属性的getter调用（建立对应的dep与当前的watcher之间的关系）
        var value = this.getVMVal();
        //去除Dep上的已经建立完关系的watcher
        Dep.target = null;
        return value;
    },

    getVMVal: function() {
        var exp = this.exp.split('.');
        var val = this.vm._data;
        exp.forEach(function(k) {
            val = val[k];
        });
        return val;
    }
};
```
# 三、数据绑定

## 数据绑定
	一旦更新了data中的某个属性数据, 所有界面上直接使用或间接使用了此属性的节点都会更新
## 数据劫持
* 1)数据劫持是vue中用来实现数据绑定的一种技术
* 2)基本思想: 通过defineProperty()（给data中的属性添加 get 与 set 方法）来监视data中所有属性(任意层次)数据的变化, 一旦变化就去更新界面
## 四个重要对象
* 1)Observer
	* a.用来对data所有属性数据进行劫持的构造函数
	* b.给data中所有属性重新定义属性描述(get/set)
	* c.为data中的每个属性创建对应的dep对象
* 2)Dep(Depend)
	* a.data中的每个属性(所有层次)都对应一个dep对象
	* b.创建的时机:
      * 在初始化define data中各个属性时创建对应的dep对象
      * 在data中的某个属性值被设置为新的对象时
	* c.对象的结构

             `{
                 id, // 每个dep都有一个唯一的id
                 subs //包含n个对应watcher的数组(subscribes的简写)
             }`

	* d.subs属性说明
   		* 当watcher被创建时, 内部将当前watcher对象添加到对应的dep对象的subs中
        * 当此data属性的值发生改变时, subs中所有的watcher都会收到更新的通知,
从而最终更新对应的界面
* 3)Compile
	* a. 用来解析模板页面的对象的构造函数(一个实例)
	* b. 利用compile对象解析模板页面
	* c. 每解析一个表达式(非事件指令)都会创建一个对应的watcher对象, 并建立watcher与dep的关系
	* d. complie与watcher关系: 一对多的关系
* 4)Watcher
	* a. 模板中每个非事件指令或表达式都对应一个watcher对象
	* b. 监视当前表达式数据的变化
	* c. 创建的时机: 在初始化编译模板时
	* d. 对象的组成

        `{
               vm,  //vm对象
              exp, //对应指令的表达式
              cb, //当表达式所对应的数据发生改变的回调函数
              value, //表达式当前的值
              depIds //表达式中各级属性所对应的dep对象的集合对象
                             //属性名为dep的id, 属性值为dep
         }`
                           
* 5)总结: dep与watcher的关系: 多对多
	* a. data中的一个属性对应一个dep, 一个dep中可能包含多个watcher(模板中有几个表达式使用到了同一个属性)
	* b. 模板中一个非事件表达式对应一个watcher, 一个watcher中可能包含多个dep(表达式是多层: a.b)
	* c. 数据绑定使用到2个核心技术
    	* defineProperty()
        * 消息订阅与发布
# dep与watcher分析
## watcher对象
* 什么时候创建?: 解析完一个指令(非事件)/大括号表达式
* 与谁对应?: 与模板中的表达式(指令/大括号表达式)一一对应
* 创建多少个? 模板中表达式的个数
## dep对象
* 什么时候创建? 在对data中的属性进行劫持前创建
* 与谁对应?  与data中的属性一一对应
* 创建多少个?  data中所有层次属性的个数
##dep与watcher之间的关系
* dep(1): watcher(n): 
	多个表达式用到同一个属性
	subs: [] 用来保存所有相关的watcher
* watcher(1): dep(n): 
	一个表达式用到了多个属性(多层表达式)
	depIds: {} 用来保存所有相关的dep(用dep的id作为属性名, dep作为属性值)
# MVVM原理图分析
![mvvm原理图](https://i.imgur.com/5z0QpWi.jpg)
# 双向数据绑定
* 1)双向数据绑定是建立在单向数据绑定(model==>View)的基础之上的
* 2)双向数据绑定的实现流程:
	* a.在解析v-model指令时, 给当前元素添加input监听
	* b.当input的value发生改变时, 将最新的值赋值给当前表达式所对应的data属性
