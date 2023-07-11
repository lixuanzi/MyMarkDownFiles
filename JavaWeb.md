# JavaWeb

# 1. JS 介绍

## 1.1 JS 引入方式

- 第一种方式 <script></script>可以放在html任意位置

```js
<script>
    alert('Hello Js');
</script>
```

- 第二种方式 外部引入

```js
<script src="./JS/demo.js"></script>
```

## 1.2 基础语法

### 1.2.1 书写语法

结尾分号可有可无

大括号表示一个代码块

#### 输出语句

window.alert()写入警告框

document.write()写入HTML输出

使用console.log()写入浏览器控制台

#### 变量

使用关键字 var （variable）来声明变量

可以用来存放不同语言

**特点1：**定义的变量是全局变量

**特点2：**是可以重复定义

​			let：关键字是类似一var但是是局部变量，不能重复定义

​			const：定义的变量是常量，不能被改变的

**数据类型、运算法、流程控制语句：**

数据类型：原式数据类型 通过 **typeof**可以获取数据类型：typeof a 

运算符：全等运算符=== 运算时不会进行类型转换

​					==进行运算时会进行类型转换

数据类型转换：parseInt()将其他类型转换成数字，无法转换的转换为NaN

​							其他类型转换为false：除了0和NaN之外其他的全部转换为 true

字符串转化：空字符转为false（空字符串是""，中间什么都不写），其余都为false

null和undefined转换之后都是false

#### js 函数

用来执行特定任务的代码块 

function 函数名{

}

形参不需要数据类型，返回值也不需要数据类型直接return

```js
var resultAdd = function add2(a, b) {
	return a + b;
}
alert(resultAdd(3,4));
```

#### js 对象 （Array String JSON BOM DOM）

**数组**：长度及数据类型都是可变的，类似于Java中的集合，未定义的值为undefined

​	定义：var 变量名=new Array(元素列表); | var arr =new Array(1,2,3,4)

​				var 变量名=[元素列表]; | var arr = [1,2,3,4];

​	访问：arr[索引]=值;|arr[10]="Hello";

​	属性：length 遍历数组长度

​	方法：forEach()：遍历数组中每个有值的元素，并调用一次传入函数

​				push()：将新元素添加到数组的末尾，并返回新的长度

​				splice()：从数组中删除元素

```js
// ES6 箭头函数：(...)=>{...} 简化函数的定义
arr.forEach((e)=>{
	console.log(e);
})
```

**String**

​	定义： var 变量名=new String("..."); | car str = new String = "Hello World!";

​				var 变量名= "..."; | var str = "Hello World!";

​	属性：length：字符串的长度

​	方法：charAt()：返回在指定位置的字符

​				indexOf()：检索字符串

​				trim()：去除字符串两边的空格

​				substring()：提取字符串中两个指定位置之间的字符

![image-20230429000120797](https://img.ixuanzi.com/images/typora/image-20230429000120797.png)

**JSON：**采用“key”:value;

就是上面的自定义格式中的key使用双引号引起来

语法结构简单，多用于做数据载体，进行数据传输。

​	定义：var 变量名 = '{"key1": value1,"key2": value2}'; | 

​				var userStr = '{"name": "Jerry","age": 18, "addr": ["北京","上海","深圳"]}';

​	转换：JSON字符串转化为JS对象

​				var jsObject = JSON.parse(userStr);

​				JS对象转化为JSON字符串

​				var jsonSrt = JSON.stringify(jsObject);

**BOM对象：**

​	组成：Window:浏览器窗口对象；Navigator:浏览器对象；Screen:屏幕对象；History:历史记录对象；Location: 地址栏对象

​	**window对象：**直接用Window，其中Window可以省略；window.alert("Hello Window!");=>

​		alert("Hello Window@");

​	属性：

​		history：对 History 对象的只读引用。请参阅 History 对象。

​		location：用于窗口或框架的 Location 对象。请参阅 Location 对象。

​		navigator：对 Navigator 对象的只读引用。请参阅 Navigator 对象。

​	方法：

​		alert()：显示带有一段消息和一个确认按钮的警告框。

​		confirm()：显示带有一段消息以及确认按钮和取消按钮的对话框。

​		setInterval()：按照指定的周期（以毫秒计）来调用函数或计算表达式。

​		setTimeout()：在指定的毫秒数后调用函数或计算表达式。

​	**location：**window.loacation.属性;=>location.属性

​		属性：href：设置返回完整的URL | loacation.href = "https://baidu.com";

**DOM 对象**

​	获取到指定的元素对象 Document

1.   根据ID属性值获取，返回单个Element对象

```js
var h1 = document.getElementById('h1');
```

2. 根据标签名称获取，返回Element对象数组

```js
var divs = document.getElementsByTagName('div');
```

3. 根据name属性值获取，返回Element对象数组

```js
var hobbys = document.getElementsByName('hobby');
```

4. 根据class属性值获取，返回Element对象数组

```js
var clss = document.getElementsByClassName('cls');
```

## 1.3 JS 事件监听

### 1.3.1 事件绑定

- 通过 HTML标签中的事件属性进行绑定

```js
<input type="button" onclick="on()" value="按钮1">
<script>
    function on(){
        alert('我被点击了!');
    }
</script>

```

- 通过 DOM 元素属性绑定

```js
<input type="button" id="btn" value="按钮2">
<script>
    document.getElementById('btn').onclick=function(){
        alert('我被点击了!');
    }
</script>
```

### 1.3.2 常见事件

| **事件名**  |         **说明**         |
| :---------: | :----------------------: |
|   onclick   |       鼠标单击事件       |
|   onblur    |       元素失去焦点       |
|   onfocus   |       元素获得焦点       |
|   onload    | 某个页面或图像被完成加载 |
|  onsubmit   |  当表单提交时触发该事件  |
|  onkeydown  |    某个键盘的键被按下    |
| onmouseover |   鼠标被移到某元素之上   |
| onmouseout  |     鼠标从某元素移开     |

## 1.4 VUE 简介

### 1.4.1 Vue快速入门

1. 新建HTML页面，引入Vue.js文件

```vue
<script src="js/vue.js"></script>
```

2. l在JS代码区域，创建Vue核心对象，定义数据模型

```vue
<script>
    new Vue({
        el: "#app",
        data: {
            message: "Hello Vue!"
        }
    })
</script>
```

3. 编写视图

```vue
<div id="app">
    <input type="text" v-model="message">
    {{ message }}
</div>
```

**差值表达式：**形式:{{ 表达式 }}

内容可以是：变量、三元运算符、函数调用、算术运算

### 1.4.2 Vue 常用指令

指令 html标签上带有 v- 前缀的特殊属性

| **指令**  | **作用**                                            |
| :-------: | :-------------------------------------------------- |
|  v-bind   | 为HTML标签绑定属性值，如设置 href , css样式等       |
|  v-model  | 在表单元素上创建双向数据绑定                        |
|   v-on    | 为HTML标签绑定事件                                  |
|   v-if    | 条件性的渲染某元素，判定为true时渲染,否则不渲染     |
| v-else-if |                                                     |
|  v-else   |                                                     |
|  v-show   | 根据条件展示某元素，区别在于切换的是display属性的值 |
|   v-for   | 列表渲染，遍历容器的元素或者对象的属性              |

**常用命令：**v-bind、v-model

```vue
<body>
    <div id="app">
        <a :href="url">链接1</a>
        <a v-bind:href="url">链接2</a>

        <input type="text" v-model="url">
    </div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data:{
           url:"https://www.baidu.com"
        }
    })
</script>
```

**v-on**

```vue
<body>
    <div id="app">

        <input type="button" value="点我一下" v-on:click="handle()">

        <input type="button" value="点我一下" @click="handle()">

    </div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data: {

        },
        methods: {
            handle: function () {
                alert("你点击了我一下");
            }
        }
    })
</script>
```

**v-if、v-else-if、v-else、v-show**

```vue
<body>
    <div id="app">

        年龄<input type="text" v-model="age">经判定,为:
        <span v-show="age">年轻人(35及以下)</span>
        <span v-show="age">中年人(35-60)</span>
        <span v-show="age">老年人(60及以上)</span>

        <br><br>

        年龄<input type="text" v-model="age">经判定,为:
        <span v-if="age<=35">年轻人(35及以下)</span>
        <span v-else-if="age>35&&age<=60">中年人(35-60)</span>
        <span v-else="age>60">老年人(60及以上)</span>

    </div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data: {
            age: 20
        },
        methods: {

        }
    })
</script>

```

**v-for**

```vue
<body>
    <div id="app">
        <div v-for="addr in addrs">{{addr}}</div>
        <hr/>
        <div v-for="(addr,index) in addrs">{{index+1}} : {{addr}}</div>
    </div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data: {
            addrs: ["北京", "上海", "西安", "成都", "深圳"]
        },
        methods: {

        }
    })
</script>
```

**案例**

```vue
<body>

    <div id="app">

        <table border="1" cellspacing="0" width="60%">
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>年龄</th>
                <th>性别</th>
                <th>成绩</th>
                <th>等级</th>
            </tr>

            <tr align="center" v-for="(user, index) in users">
                <td>{{index+1}}</td>
                <td>{{user.name}}</td>
                <td>{{user.age}}</td>
                <td>
                    <span v-if="user.gender==1">男</span>
                    <span v-else="user.gender==2">女</span>
                </td>
                <td>{{user.score}}</td>
                <td>
                    <span v-if="user.score>=85">优秀</span>
                    <span v-else-if="user.score>=60">及格</span>
                    <span style="color: red;"v-else>不及格</span>
                </td>
            </tr>
        </table>

    </div>

</body>

<script>
    new Vue({
        el: "#app",
        data: {
            users: [{
                name: "Tom",
                age: 20,
                gender: 1,
                score: 78
            }, {
                name: "Rose",
                age: 18,
                gender: 2,
                score: 86
            }, {
                name: "Jerry",
                age: 26,
                gender: 1,
                score: 90
            }, {
                name: "Tony",
                age: 30,
                gender: 1,
                score: 52
            }]
        },
        methods: {

        },
    })
</script>
```

### 1.4.3 Vue 生命周期

生命周期：指一个对象从创建到销毁的整个过程

生命周期的八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法(钩子)。

|   **状态**    | **阶段周期** |
| :-----------: | :----------: |
| beforeCreate  |    创建前    |
|    created    |    创建后    |
|  beforeMount  |    挂载前    |
|    mounted    |   挂载完成   |
| beforeUpdate  |    更新前    |
|    updated    |    更新后    |
| beforeDestroy |    销毁前    |
|   destroyed   |    销毁后    |

# 2.Vue-Element

## 2.1 Ajax

作用：通过Ajax可以给服务器发送请求，并获取服务器响应的数据。

异步交互：可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术，如：搜索联想、用户名是否可用的校验等等。

## 2.2 Axios

### 2.2.1 Axios入门

1. 引入Axios的js文件

```vue
<script src="js/axios-0.18.0.js"></script>
```

2. 使用Axios发送的请求，并获取响应结果

```vue
<script src="js/axios-0.18.0.js"></script>
<body>
    
    <input type="button" value="获取数据GET" onclick="get()">

    <input type="button" value="删除数据POST" onclick="post()">

</body>
<script>
    function get(){
        //通过axios发送异步请求-get
        axios({
            method: "get",
            url: "http://yapi.smart-xwork.cn/mock/169327/emp/list"
        }).then(result => {
            console.log(result.data);
        })


        axios.get("http://yapi.smart-xwork.cn/mock/169327/emp/list").then(result => {
            console.log(result.data);
        })
    }

    function post(){
        // 通过axios发送异步请求-post
        axios({
            method: "post",
            url: "http://yapi.smart-xwork.cn/mock/169327/emp/deleteById",
            data: "id=1"
        }).then(result => {
            console.log(result.data);
        })

        // axios.post("http://yapi.smart-xwork.cn/mock/169327/emp/deleteById","id=1").then(result => {
        //     console.log(result.data);
        // })
    }
</script>
```

## 2.3 前后端分离的开发模式

### 2.3.1 接口文档管理平台

**YAPI：**http://yapi.smart-xwork.cn/

## 2.4 前端工程化

### 2.4.1 环境准备

vue-cli：Vue-cli 是Vue官方提供的一个脚手架，用于快速生成一个 Vue 的项目模板。

安装NodeJS

安装vue-cli

```vue
npm install -g @vue/cli
```

创建Vue_project

命令方式

```vue
vue create vue-project
```

图形化界面

```vue
vue ui
```

### 2.4.2 Vue项目-项目结构

![](https://img.ixuanzi.com/images/typora/image-20230425162756524.png)

配置项目端口**vue.config.js**

```vue
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  devServer: {
    port: 7000
  }
})
```

### 2.4.3 Vue项目结构

![](https://img.ixuanzi.com/images/typora/image-20230425163037320.png)

```vue
<template>
  <div>
    {{ message }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "Hello Vue"
    }
  },
  methods: {

  }
}
</script>
<style></style>

```

## 2.5 Vue组件库Element

组件：组成网页的部件，例如 超链接、按钮、图片、表格、表单、分页条等等。

官网：[https://element.eleme.cn/#/zh-CNListener](https://element.eleme.cn/)

### 2.5.1 快速入门

安装Element

```vue
npm install element-ui@2.15.3
```

main.js引入Element

```vue
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

Vue.use(ElementUI);

new Vue({
  el: '#app',
  render: h => h(App)
});
```

### 2.5.2 基本组件

#### 按钮

```Vue
<el-row>
      <el-button>默认按钮</el-button>
      <el-button type="primary">主要按钮</el-button>
      <el-button type="success">成功按钮</el-button>
      <el-button type="info">信息按钮</el-button>
      <el-button type="warning">警告按钮</el-button>
      <el-button type="danger">危险按钮</el-button>
    </el-row>
```

#### 表格

```vue
<el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="date" label="日期" width="180">
      </el-table-column>
      <el-table-column prop="name" label="姓名" width="180">
      </el-table-column>
      <el-table-column prop="address" label="地址">
      </el-table-column>
 </el-table>

<script>
export default {
  data() {
    return {
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      }]
    }
  }
</script>
```

#### Pagination 分页

```vue
<el-pagination background layout="total, sizes, prev, pager, next, jumper"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange" :total="100">
    </el-pagination>

<script>
export default {
  methods:{
    handleSizeChange(val) {
        alert("每页记录变化"+val);
      },
      handleCurrentChange(val) {
        alert("当前页码是"+val);
      }
  },
}
</script>
```

| layout | 组件布局，子组件名用逗号分隔 | String | `sizes`, `prev`, `pager`, `next`, `jumper`, `->`, `total`, `slot` | 'prev, pager, next, jumper, ->, total' |
| :----- | ---------------------------- | ------ | ------------------------------------------------------------ | -------------------------------------- |

#### **Event**

| 事件名称       | 说明                               | 回调参数 |
| :------------- | :--------------------------------- | :------- |
| size-change    | pageSize 改变时会触发              | 每页条数 |
| current-change | currentPage 改变时会触发           | 当前页   |
| prev-click     | 用户点击上一页按钮改变当前页后触发 | 当前页   |
| next-click     | 用户点击下一页按钮改变当前页后触发 | 当前页   |

#### **Form表单**





# 3.Maven

1.依赖管理-jar包管理器

2.统一项目结构-提供标准、统一的项目结构

![image-20230605165505116](https://img.ixuanzi.com/images/typora/image-20230605165505116.png)

3.项目构建-标准跨平台的自动化项目构建方式

官网：http://maven.apache.org/

## 3.1 Maven安装

- Maven Release History：https://dlcdn.apache.org/maven/
- 配置本地仓库

```xml
<localRepository>D:\Develop\JavaUtils\apache-maven-3.6.2\mvn_repo</localRepository>
```

- 配置阿里云私服：修改 conf/settings.xml 中的 <mirrors>标签，为其添加如下子标签：

```xml
  <mirrors>
    <mirror>  
	  <id>alimaven</id>  
	  <name>aliyun maven</name>  
	  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
	  <mirrorOf>central</mirrorOf>          
    </mirror>
  </mirrors>
```

- 配置环境变量: MAVEN_HOME 为maven的解压目录，并将其bin目录加入PATH环境变量。

```
变量名：MAVEN_HOME
变量值：maven 安装目录 D:\Develop\JavaUtils\apache-maven-3.6.2
    
变量名：Path
变量值：%MAVEN_HOME%\bin  
```

## 3.2 创建Maven项目

### 3.2.1 配置Maven环境

1. 创建空项目->Project structure->Project 设置SDK、language level 均设置为11
2. File->Settings->Build,Execution,Deployment->Maven Maven home path,User settings file,Local repository

![image-20230605213001676](https://img.ixuanzi.com/images/typora/image-20230605213001676.png)

3. File->Settings->Build,Execution,Deployment->Maven->Runner JRE

![image-20230605213034466](https://img.ixuanzi.com/images/typora/image-20230605213034466.png)

4. File->Settings->Build,Execution,Deployment->Compiler->Java Compiler Project bytexode version

![image-20230605213126941](https://img.ixuanzi.com/images/typora/image-20230605213126941.png)

### 3.2.2 idea全局Maven环境

1. Welcome->Customize->All settings

![image-20230605213645876](https://img.ixuanzi.com/images/typora/image-20230605213645876.png)

### 3.2.3Maven坐标

用来定义坐标资源，使用坐标可以唯一确定资源位置

**Maven 坐标主要组成**

- groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.itheima）
- artifactId：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）
- version：定义当前项目版本号

```xml
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

## 3.2 导入Maven项目

- 方式1

![image-20230606215819537](https://img.ixuanzi.com/images/typora/image-20230606215819537.png)

- 方式二

![image-20230608185309808](https://img.ixuanzi.com/images/typora/image-20230608185309808.png)

## 3.3 依赖的配置

依赖：指当前项目运行所需要的jar包，一个项目中可以引入多个依赖。

配置：

1.在 pom.xml 中编写 <dependencies> 标签

2.在 <dependencies> 标签中 使用 <dependency> 引入坐标

3.定义坐标的 groupId，artifactId，version

4.点击刷新按钮，引入最新加入的坐标

```xml
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
	<version>1.2.10</version>
</dependency>
```

Maven官方库：https://mvnrepository.com/

### 3.3.1 排除依赖

使用<exclusions></exclusions>标签

```xml
	<dependency>
            <groupId>com.itheima</groupId>
            <artifactId>maven-projectB</artifactId>
            <version>1.0-SNAPSHOT</version>
            <!--排除依赖-->
            <exclusions>
                <exclusion>
                    <groupId>junit</groupId>
                    <artifactId>junit</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

### 3.3.2 依赖范围

依赖的jar包，默认情况下，可以在任何地方使用。可以通过 <scope>…</ scope > 设置其作用范围。

- 主程序范围有效。（main文件夹范围内）
- 测试程序范围有效。（test文件夹范围内）
- 是否参与打包运行。（package指令范围内）

| **scope****值** | **主程序** | **测试程序** | **打包（运行）** | **范例**    |
| --------------- | ---------- | ------------ | ---------------- | ----------- |
| compile（默认） | Y          | Y            | Y                | log4j       |
| test            | -          | Y            | -                | junit       |
| provided        | Y          | Y            | -                | servlet-api |
| runtime         | -          | Y            | Y                | jdbc驱动    |

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.10</version>
  <scope>test</scope>
</dependency>
```

### 3.3.3 生命周期

Maven的生命周期就是为了对所有的maven项目构建过程进行抽象和统一。

- clean：移除上一次构建生成的文件
- compile：编译项目源代码
- test：使用合适的单元测试框架运行测试(junit)
- package：将编译后的文件打包，如：jar、war等
- install：安装项目到本地仓库

# 4 Spring

## 4.1 Spring概述

官网：[spring.io](https://spring.io/)

## 4.2 Spring Boot

**Spring Boot** **可以帮助我们非常快速的构建应用程序、简化开发、提高效率**

## 4.3 SpringBootWeb入门

- 添加 Spring 模块 勾选相关依赖
- 创建请求处理类HelloController，添加请求处理方法 hello，并添加注解。

```Java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        System.out.println("Hello World~");
        return  "Hello Spring~";
    }
}
```

# 5 Http 协议

## 5.1 http-请求协议





# 6 Tomcat

官方地址：https://tomcat.apache.org/

## 6.1 环境变量

```
变量名：CATALINA_HOME
变量值：Tomcat 安装目录 D:\Develop\JavaUtils\apache-tomcat-8.5.90
    
变量名：Path
变量值：%CATALINA_HOME%\bin  
```

## 6.2 修改编码

将tomcat控制台日志输出编码格式更改为GBK，修改tomcat根目录下conf/logging.properties文件中的ConsoleHandler.encoding=utf-8，这种方式能解决cmd控制台中文乱码，但`不建议使用`。**因为更改了tomcat默认编码，如果我们使用idea启动tomcat，idea的默认编码不是GBK，就会同样产生idea控制台下tomcat乱码问题**。

