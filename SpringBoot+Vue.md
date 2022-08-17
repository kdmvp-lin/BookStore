# SpringBoot+Vue

## 1.技术栈

### 1.1前端技术栈

1. Vue.js

2. ElementUI

3. axios

### 1.2后端技术栈

1. SpringBoot

2. Apache Shiro

3. Apache Log4j2

4. Spring Data JPA

5. Spring Data Redis

### 1.3数据库

1. MySQL

2. Redis（docker）

## 2.使用cli脚手架搭建Vue项目

### 2.1安装Vue CLI

在安装Vue CLI之前需要用到npm命令，因此需要先安装Node.js访问官网 https://nodejs.org/en/，首页即可下载。

安装完成后` node -v`查看node.js版本号` npm -v` 查看npm版本号 `npm -g install npm` ，将 npm 更新至最新版本

完成准备工作后

使用 `npm install -g vue-cli` 安装脚手架。

之后进入问价` vue init webpack wj-vue`

根据需求进行下一步

` npm run dev`运行前端

### 2.2Vue项目目录

![](C:\Users\kdmvp\AppData\Roaming\marktext\images\2022-08-10-17-22-05-image.png)

```
|--dist                         打包之后的发布目录      
|   |--css                          存放打包之后的css文件夹
|   |--img                          存放打包之后的img资源文件夹
|   |--js                           存放打包之后存放的js文件夹
|   |--index.html                   存放打包之后的入口页面
|
|--mock                             本地前端工程mock的一些数据  
|
|--node_module                  本地项目的第三方依赖
|
|--public               
|
|--src                          开发目录  
|   |
|   |--assets                       开发时候存放的一些静态资源
|   |
|   |--components                   业务开发的组件   
|   |
|   |--lib                          开发过程中自己写的一些工具库
|   |
|   |-plugins
|   |
|   |-router                    前端工程的路由，将其模块化，单独的拿出来
|   |   |
|   |   |-router.js                 工程的路由配置
|   |
|   |-vuex                      vuex转态管理
|   |   |
|   |   |-store.js
|   |
|   |-App.vue                   工程的入口文件，也是整个工程的根组件  
|   |-main.js                   工程的入口文件，用以引入一些全局变量  
|   |-.gitignore                提交git仓库忽略的文件
|   |-babel.config.js           babel的一些配置
|   |-package.json              依赖的第三方配置文件
|   |-readMe.md                    项目简单介绍
```

## 3.创建后端项目

在 IDEA 中新建项目，选择 Spring Initializr，点击 Next

选择 Web -> Web，Next

运行 Application.java

## 4.登录页面开发

### 4.1前端页面开发

首先我们开发登录页面组件，右键 `src\components` 文件夹，New -> Vue Component，命名为 `Login`，如果没有 Vue Component 这个选项，可以选择新建一个 File，命名为 `Login.vue` 即可。

<template> 标签中随便写了一个登录的界面，

 methods 中定义了登录按钮的点击方法，即向后端` /login` 接口发送数据，获得成功的响应后，页面跳转到` /index`。因为之前我们设置了默认的 URL，所以请求实际上发到了 `http://localhost:8443/api/login。`

```html
<template>
  <div>
      用户名:<input type="text" v-model="loginForm.username" placeholder="请输入用户名"/>
      <br><br>
      密码： <input type="password" v-model="loginForm.password" placeholder="请输入密码"/>
      <br><br>
      <button v-on:click="login">登录</button>
  </div>
</template>

<script>

  export default {
    name: 'Login',
    data () {
      return {
        loginForm: {
          username: '',
          password: ''
        },
        responseResult: []
      }
    },
    methods: {
      login () {
        this.$axios
          .post('/login', {
            username: this.loginForm.username,
            password: this.loginForm.password
          })
          .then(successResponse => {
            if (successResponse.data.code === 200) {
              this.$router.replace({path: '/index'})
            }
          })
          .catch(failResponse => {
          })
      }
    }
  }
</script>
```

右键 `src\components` 文件夹，新建一个 directory，命名为 `home`，再在 `home` 下新建一个 `Appindex.vue` ，即首页组件，这里暂时不做过多开发，先随便写个 Hello World。

```html
<template>
    <div>
      Hello World!
    </div>
</template>

<script>
  export default {
    name: 'AppIndex'
  }
</script>

<style scoped>

</style>
```

### 4.2前后端结合

注意我们的项目是前后端分离的，这里的结合意思不是就不分离了，是如何把这俩分离的项目串起来用。

前面提到过前后端分离的意思是前后端之间通过 RESTful API 传递 JSON 数据进行交流。不同于 JSP 之类，后端是不涉及页面本身的内容的。

在开发的时候，前端用前端的服务器（Nginx），后端用后端的服务器（Tomcat），当我开发前端内容的时候，可以把前端的请求通过前端服务器转发给后端（称为反向代理），这样就能实时观察结果，并且不需要知道后端怎么实现，而只需要知道接口提供的功能，两边的开发人员（两个我）就可以各司其职啦。

艾玛做一个完整的教程真不容易，遇到的每个知识点感觉都能讲一堆。上次的文章被一位老哥反问是不是太着急了，也不知道是什么意思，我自己反思可能是讲的不够细吧，这里我就再啰嗦一下讲两句 正向代理 和 反向代理。

正向代理就是，你要访问一个网站，比如“谷弟弟”，然后发现访问不到，于是你访问了一个能访问到“谷弟弟”的代理服务器，让它帮你拿到你想浏览的页面。

反向代理就是，你访问了一个网站，你以为它是“谷弟弟”，但其实它是“谷姐”，“谷姐”知道你其实是想找她弟，就取回“谷弟弟”的内容给你看。作为用户的你，是不知道有这个过程的，这么做是为了保护服务器，不暴露服务器的真实地址。

### 4.3前端相关配置

#### 设置反向代理

修改 `src\main.js` 代码如下：

因为使用了新的模块 `axios`，所以需要进入到项目文件夹中，执行 `npm install --save axios`，以安装这个模块。

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'
// 设置反向代理，前端请求默认发送到 http://localhost:8443/api
var axios = require('axios')
axios.defaults.baseURL = 'http://localhost:8443/api'
// 全局注册，之后可在其他组件中通过 this.$axios 发送数据
Vue.prototype.$axios = axios
Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

#### 配置页面路由

修改 `src\router\index.js` 代码如下

```javascript
import Vue from 'vue'
import Router from 'vue-router'
// 导入刚才编写的组件
import AppIndex from '@/components/home/AppIndex'
import Login from '@/components/Login'

Vue.use(Router)

export default new Router({
  routes: [
  // 下面都是固定的写法
    {
      path: '/login',
      name: 'Login',
      component: Login
    },
    {
      path: '/index',
      name: 'AppIndex',
      component: AppIndex
    }
  ]
})
```

#### 跨域支持

为了让后端能够访问到前端的资源，需要配置跨域支持。

在 `config\index.js` 中，找到 proxyTable 位置，修改为以下内容

```javascript
    proxyTable: {
      '/api': {
        target: 'http://localhost:8443',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
```

执行 `npm run dev`，或双击 dev（start 也一样）脚本，查看登录页面效果。

注意地址是 `localhost:8080/#/login`

### 4.4后端开发

#### User类

在 `Login.vue` 中，前端发送数据的代码段

```html
.post('/login', {
            username: this.loginForm.username,
            password: this.loginForm.password
          })
```

后端如何接收这个 JS 对象呢？我们很自然地想到在需要创建一个形式上一致的 Java 类。

打开我们的后端项目 wj，首先在 src\main\java\com\evan\wj 文件夹（就是你自己的 web 项目的包）下，新建一个 pojo 包（package），然后新建 User类，代码如下

```java
package com.evan.wj.pojo;

public class User {
    int id;
    String username;
    String password;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

#### Result 类

Result 类是为了构造 response，主要是响应码。新建 `result` 包，创建 `Result` 类，代码如下

```java
package com.evan.wj.result;

public class Result {
    //响应码
    private int code;

    public Result(int code) {
        this.code = code;
    }

    public int getCode() {
        return code;
    }

    public void setCode(int code) {
        this.code = code;
    }

}
```

实际上由于响应码是固定的，`code` 属性应该是一个枚举值，这里作了一些简化。

状态码    英文名    中文描述
200    OK    请求成功。一般用于GET与POST请求
201    Created    已创建。成功请求并创建了新的资源
202    Accepted    已接受。已经接受请求，但未处理完成
400    Bad Request    客户端错误，请求包含语法错误或无法完成请求
401    Unauthorized    请求要求用户的身份认证
403    Forbidden    服务器理解请求客户端的请求，但是拒绝执行此请求
404    Not Found    服务器无法根据客户端的请求找到资源（网页）
500    Internal Server Error    服务器内部错误，无法完成请求
501    Not Implemented    服务器不支持请求的功能，无法完成请求
503    Service Unavailable    由于超载或系统维护，服务器暂时的无法处理客户端的请求

#### LoginController

Controller 是对响应进行处理的部分。这里我们设定账号是 admin，密码是 123456，分别与接收到的 User 类的 username 和 password 进行比较，根据结果返回不同的 Result，即不同的响应码。前端如果接收到成功的响应码（200），则跳转到 /index 页面。

在 wj 下新建 controller 包，新建 LoginController 类，代码如下

```java
package com.evan.wj.controller;

import com.evan.wj.result.Result;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.util.HtmlUtils;

import com.evan.wj.pojo.User;

import java.util.Objects;

@Controller
public class LoginController {

    @CrossOrigin
    @PostMapping(value = "api/login")
    @ResponseBody
    public Result login(@RequestBody User requestUser) {
    // 对 html 标签进行转义，防止 XSS 攻击
        String username = requestUser.getUsername();
        username = HtmlUtils.htmlEscape(username);

        if (!Objects.equals("admin", username) || !Objects.equals("123456", requestUser.getPassword())) {
            String message = "账号密码错误";
            System.out.println("test");
            return new Result(400);
        } else {
            return new Result(200);
        }
    }
}
```

这里只是为了演示前后端的交互过程，真正的登录验证要考虑更多因素，后面的文章会有详细介绍。另外教程初期对项目结构做了一些简化，实际上在 controller 里写这么多逻辑是不合理的，要尽量封装到 service 里面去。

最后，在 src\main\resources 文件夹下找到 application.properties 文件配置端口，即加上 `server.port=8443`（初始应该是空白的，后期还要配置数据库等）

> 注意这里端口号的设置要和前端vue项目中`config\index.js` 中， proxyTabl下的`target: http://localhost:8443`

### 4.5数据库

#### 4.5.1相关配置

打开我们的后端项目 `wj`，首先修改 `pom.xml`，配置我们需要的依赖。为了方便以后的开发，我直接把我的配置粘贴上来，大家可以把原来的所有配置删除掉，再粘贴我的。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.evan</groupId>
    <artifactId>wj</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>wj</name>
    <description>White Jotter - Your Mind Palace</description>
    <packaging>war</packaging>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <dependencies>
        <!-- springboot web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- springboot tomcat 支持 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
        <!-- 热部署 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
        <!-- jpa-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <!-- redis -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <!-- springboot test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- thymeleaf -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <!-- elastic search -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
        </dependency>
        <!-- 用了 elasticsearch 就要加这么一个，不然要com.sun.jna.Native 错误 -->
        <dependency>
            <groupId>com.sun.jna</groupId>
            <artifactId>jna</artifactId>
            <version>3.0.9</version>
        </dependency>

        <!-- thymeleaf legacyhtml5 模式支持 -->
        <dependency>
            <groupId>net.sourceforge.nekohtml</groupId>
            <artifactId>nekohtml</artifactId>
            <version>1.9.22</version>
        </dependency>
        <!-- 测试支持 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- tomcat的支持.-->
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <version>8.5.23</version>
        </dependency>
        <!-- mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.21</version>
        </dependency>

        <!-- junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version> 4.12</version>
        </dependency>
        <!-- commons-lang -->
        <dependency>
            <groupId>commons-lang</groupId>
            <artifactId>commons-lang</artifactId>
            <version>2.6</version>
        </dependency>
        <!-- shiro -->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring</artifactId>
            <version>1.3.2</version>
        </dependency>
        <!-- hsqldb -->
        <dependency>
            <groupId>org.hsqldb</groupId>
            <artifactId>hsqldb</artifactId>
        </dependency>
    </dependencies>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

接下来就等待依赖的自动安装。过程可能比较长。如果自动安装的过程没有执行，可以在 `pom.xml` 上右键，选择 Maven -> Reimport 。

配置完依赖后，还需要配置数据库。打开 `src\main\resources\application.properties`

```properties
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/white_jotter?characterEncoding=UTF-8&serverTimezone=GMT
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto = none
```

### 4.6登录控制器

配置完成后，我们就可以完善登录控制器了。

#### User类

首先，我们修改 User 类代码如下，以建立对数据库的映射。

```java
package com.evan.wj.pojo;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

import javax.persistence.*;

@Entity
@Table(name = "user")
@JsonIgnoreProperties({"handler","hibernateLazyInitializer"})

public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    int id;

    String username;
    String password;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

上面使用了一些注解，其中

@Entity 表示这是一个实体类
@Table(name=“user”) 表示对应的表名是 user

为了简化对数据库的操作，我们使用了 Java Persistence API（JPA），对于 @JsonIgnoreProperties({ “handler”,“hibernateLazyInitializer” })，解释起来比较复杂，下面的话看不懂可以忽略：

> 因为是做前后端分离，而前后端数据交互用的是 json 格式。 那么 User 对象就会被转换为 json 数据。 而本项目使用 jpa 来做实体类的持久化，jpa 默认会使用 hibernate, 在 jpa 工作过程中，就会创造代理类来继承 User ，并添加 handler 和 hibernateLazyInitializer 这两个无须 json 化的属性，所以这里需要用 JsonIgnoreProperties 把这两个属性忽略掉。

#### UserDAO

Data Access Object（数据访问对象，DAO）即用来操作数据库的对象，这个对象可以是我们自己开发的，也可以是框架提供的。这里我们通过继承 JpaRepository 的方式构建 DAO。

首先新建一个 package，命名为 dao，然后创建 Java Class，命名为 UserDAO，选择种类为 Interface

```java
package com.evan.wj.dao;

import com.evan.wj.pojo.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserDAO extends JpaRepository<User,Integer> {
    User findByUsername(String username);

    User getByUsernameAndPassword(String username,String password);
}
```

这里关键的地方在于方法的名字。由于使用了 JPA，无需手动构建 SQL 语句，而只需要按照规范提供方法的名字即可实现对数据库的增删改查。

如 `findByUsername`，就是通过 `username` 字段查询到对应的行，并返回给 User 类。

这里我们构建了两个方法，一个是通过用户名查询，一个是通过用户名及密码查询。

#### UserService

新建 package，命名为 `service`，新建 Java Class，命名为 `UserService`，代码如下

```java
package com.evan.wj.service;

import com.evan.wj.dao.UserDAO;
import com.evan.wj.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
@Service
public class UserService {
    @Autowired
    UserDAO userDAO;

    public boolean isExist(String username) {
        User user = getByName(username);
        return null!=user;
    }

    public User getByName(String username) {
        return userDAO.findByUsername(username);
    }

    public User get(String username, String password){
        return userDAO.getByUsernameAndPassword(username, password);
    }

    public void add(User user) {
        userDAO.save(user);
    }
}
```

这里实际上是对 UserDAO 进行了二次封装，一般来讲，我们在 DAO 中只定义基础的增删改查操作，而具体的操作，需要由 Service 来完成。当然，由于我们做的操作原本就比较简单，所以这里看起来只是简单地重命名了一下，比如把 “通过用户名及密码查询并获得对象” 这种方法命名为 get。

#### LoginController

登录控制器是我们功能的核心部分，尽管它十分简单。逻辑上面已经讲过了，具体的实现，就是通过 `UserService` 提供的 `get` 方法查询数据库，如果返回的对象为空，则验证失败，否则就验证成功。代码如下

```java
package com.evan.wj.controller;

import com.evan.wj.pojo.User;
import com.evan.wj.result.Result;
import com.evan.wj.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.util.HtmlUtils;

@Controller
public class LoginController {

    @Autowired
    UserService userService;

    @CrossOrigin
    @PostMapping(value = "/api/login")
    @ResponseBody
    public Result login(@RequestBody User requestUser) {
        String username = requestUser.getUsername();
        username = HtmlUtils.htmlEscape(username);

        User user = userService.get(username, requestUser.getPassword());
        if (null == user) {
            return new Result(400);
        } else {
            return new Result(200);
        }
    }
}
```

> - DAO 用于与数据库的直接交互，定义增删改查等操作
> - Service 负责业务逻辑，跟功能相关的代码一般写在这里，编写、调用各种方法对 DAO 取得的数据进行操作
> - Controller 负责数据交互，即接收前端发送的数据，通过调用 Service 获得处理后的数据并返回

#### 测试

同时运行前端项目 `wj-vue` 与后端项目 `wj`，访问 `http://localhost:8080/#/login`

重新输入密码 `123`，成功进入 localhost:8080/#/index

## 5. Element 辅助前端开发

### 5.1安装Element

根据官方文档的描述，在项目文件夹下，执行 `npm i element-ui -S` 即可

### 5.2引入Element

引入分为完整引入和按需引入两种模式，按需引入可以缩小项目的体积，这里我们选择完整引入。

根据文档，我们需要修改 main.js 为如下内容

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

var axios = require('axios')
axios.defaults.baseURL = 'http://localhost:8443/api'
Vue.prototype.$axios = axios
Vue.config.productionTip = false

Vue.use(ElementUI)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  render: h => h(App),
  router,
  components: { App },
  template: '<App/>'
})
```

这样便完成了 Element 的引入。为了测试一下，我们打开 `Login.vue`，把最外层的 `<div>` 标签改为 `<el-card>`（下面是 `<template>` 内的完整代码）

```html
  <el-card>
      用户名:<input type="text" v-model="loginForm.username" placeholder="请输入用户名"/>
      <br><br>
      密码： <input type="password" v-model="loginForm.password" placeholder="请输入密码"/>
      <br><br>
      <button v-on:click="login">登录</button>
  </el-card>
```

### 5.3优化界面

首先，让我们去掉这个清奇的 V ，打开 App.vue，把 `<img src="./assets/logo.png">` 删掉即可。不过我一般喜欢先注释掉，确定没有影响了再删除。

#### 使用Form组件

为了设计界面，我们需要关注的地方是 <template> 标签内的 html 和 <style> 标签内的 css。登录框我们一般会用 Form 来做，打开 Element 的组件文档（http://element-cn.eleme.io/#/zh-CN/component/），发现它为我们提供了丰富的 Form 组件，我们可以点击“显示代码”，复制我们需要的部分。
首先，我们修改 `<template>` 里的代码如下

```html
<template>
  <el-form class="login-container" label-position="left"
           label-width="0px">
    <h3 class="login_title">系统登录</h3>
    <el-form-item>
      <el-input type="text" v-model="loginForm.username"
                auto-complete="off" placeholder="账号"></el-input>
    </el-form-item>
    <el-form-item>
      <el-input type="password" v-model="loginForm.password"
                auto-complete="off" placeholder="密码"></el-input>
    </el-form-item>
    <el-form-item style="width: 100%">
      <el-button type="primary" style="width: 100%;background: #505458;border: none" v-on:click="login">登录</el-button>
    </el-form-item>
  </el-form>
</template>
```

#### 添加样式

为了进一步优化界面，我们为组件再添加一些样式，即在 `Login.vue` 的最后添加如下代码

```css
<style>
  .login-container {
    border-radius: 15px;
    background-clip: padding-box;
    margin: 90px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
  }

  .login_title {
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }

</style>
```

#### 设置背景

放在 src\assets 文件夹下，命名为 eva.jpg 在 build 项目时，这个文件夹里的图片会被自动转成 base64。也可以在这个文件夹里再新建文件夹，方便管理。

为了使用背景图片，我在 <el-form> 标签的外又添加了一个父标签 <body>，id 设置为 poster，并在 <style> 中添加如下内容

```css
  #poster {
    background:url("../assets/eva.jpg") no-repeat;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
  }
  body{
    margin: 0px;
  }
```

之所以还要再写一个 body 的样式，是为了覆盖掉浏览器（用户代理）的默认样式

这时候发现界面上方有一片空白，经过查找，发现问题出在 `App.vue` 里，把下面这句代码删除即可`margon-top:60px;`

#### 完整代码

```html
<template>
  <body id="poster">
    <el-form class="login-container" label-position="left"
             label-width="0px">
      <h3 class="login_title">系统登录</h3>
      <el-form-item>
        <el-input type="text" v-model="loginForm.username"
                  auto-complete="off" placeholder="账号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input type="password" v-model="loginForm.password"
                  auto-complete="off" placeholder="密码"></el-input>
      </el-form-item>
      <el-form-item style="width: 100%">
        <el-button type="primary" style="width: 100%;background: #505458;border: none" v-on:click="login">登录</el-button>
      </el-form-item>
    </el-form>
  </body>
</template>

<script>

  export default {
    name: 'Login',
    data () {
      return {
        loginForm: {
          username: 'admin',
          password: '123'
        },
        responseResult: []
      }
    },
    methods: {
      login () {
        this.$axios
          .post('/login', {
            username: this.loginForm.username,
            password: this.loginForm.password
          })
          .then(successResponse => {
            if (successResponse.data.code === 200) {
              this.$router.replace({path: '/index'})
            }
          })
          .catch(failResponse => {
          })
      }
    }
  }
</script>

<style>
  #poster {
    background:url("../assets/eva.jpg") no-repeat;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
  }
  body{
    margin: 0px;
  }
  .login-container {
    border-radius: 15px;
    background-clip: padding-box;
    margin: 90px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
  }
  .login_title {
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }

</style>
```

至此，登录页面的开发似乎已经较为完善了，但其实还没有完，因为这个登录页面其实没有用，别人直接输入首页的网址，就可以绕过登录页面。为了让它发挥作用，我们还需要开发一个**拦截器**。

## 6.前端路由与登录拦截器

### 6.1前端路由

大家如果留心观察就会发现，之前我们做的页面的 URL 里有一个 # 号，这个 # 号有什么含义呢？

假设在 html 中有这么一段代码：`This is a test`，如果我们想让页面定位到这个 div 所在的位置，可以加一个超链接 `Jump to test`，这里的 # 被称为“锚点”，点击超链接，可以发现网页的 URL 发生了变化，但页面并不会跳转。

在互联网流量如此庞大的今天，我们需要想办法后端服务器的压力，利用 AJAX，我们可以不重载页面就刷新数据，如果再加上 `#` 号的特性（即改变 URL 却不请求后端），我们就可以在前端实现页面的整体变化，而不用每次都去请求后端。

为了实现前端路由，我们可以监听` #` 号后面内容的变化（hashChange），从而动态改变页面内容。URL 的` # `号后面的地址被称为 hash ，估计是哪个大神拍脑袋想的，不用深究。这种实现方式我们称之为 Hash 模式，是非常典型的前端路由方式。

另一种常用的方式被称为 History 模式，这种方式使用了 `History API`，`History API `顾名思义就是针对历史记录的 API ，这种模式的原理是先把页面的状态保存到一个对象（state）里，当页面的 URL 变化时找到对应的对象，从而还原这个页面。其实原本人家这个功能是为了方便浏览器前进后退的，不得不说程序员们的脑洞真大。使用了这种模式，就可以摆脱 # 号实现前端路由。

Vue 已经为我们提供了两种模式的前端路由，无需我们自己去实现。

### 6.2History模式

首先我们把 Vue 中配置的路由从默认的 `hash` 模式切换为 `histroy` 模式。打开我们的前端项目 `wj-vue`，修改 `router\index.js`，加入 `mode: 'history` 这句话。整体代码如下：

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import AppIndex from '@/components/home/AppIndex'
import Login from '@/components/Login'

Vue.use(Router)

export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/login',
      name: 'Login',
      component: Login
    },
    {
      path: '/index',
      name: 'AppIndex',
      component: AppIndex
    }
  ]
})
```

运行项目，访问不加 # 号的 http://localhost:8080/login ，成功加载页面。

接下来，我们把前端打包后部署在后端。这不是前后端分离项目推荐的做法，之前我们讲过其实应该把前后端分别部署在不同的服务器中，但实际上仍有不少项目会选择把前后端整合在一起，只使用一个服务器，所以这里我们也提及一下这种方式，但在之后的开发中不会这样部署。

先在项目目录执行 `npm run build`，控制台输出如下内容表明执行完毕：

这时在项目的 dist 文件夹下生成了 static 文件夹和 index.html 文件，把这两个文件，拷贝到我们后端项目的 wj\src\main\resources\static 文件夹下，一定要注意这个位置，这时后端配置的静态文件的 path，虽然看起来很诡异，但一般都不作修改。

在后端项目中新建一个 package 名为 `error`，新建实现 `ErrorPageRegistrar` 接口的类 ErrorConfig，把默认的错误页面设置为 `/index.html`，代码如下

```java
package com.evan.wj.error;

import org.springframework.boot.web.server.ErrorPageRegistrar;
import org.springframework.boot.web.server.ErrorPage;
import org.springframework.boot.web.server.ErrorPageRegistry;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Component;

@Component
public class ErrorConfig implements ErrorPageRegistrar {

    @Override
    public void registerErrorPages(ErrorPageRegistry registry) {
        ErrorPage error404Page = new ErrorPage(HttpStatus.NOT_FOUND, "/index.html");
        registry.addErrorPages(error404Page);
    }

}
```

### 6.3后端拦截器

一个简单拦截器的逻辑如下：

1.用户访问 URL，检测是否为登录页面，如果是登录页面则不拦截  
2.如果用户访问的不是登录页面，检测用户是否已登录，如果未登录则跳转到登录页面

#### LoginController

首先我们修改` LoginController` 的内容。之前我们实现了通过查询数据库验证用户名是否正确，但仅此而已。

为了保存登录状态，我们可以把用户信息存在 `Session` 对象中（当用户在应用程序的 Web 页之间跳转时，存储在 `Session` 对象中的变量不会丢失），这样在访问别的页面时，可以通过判断是否存在用户变量来判断用户是否登录。这是一种比较简单的方式，感兴趣的同学可以尝试别的方法。

修改后的代码内容如下：

```java
package com.evan.wj.controller;

import com.evan.wj.pojo.User;
import com.evan.wj.result.Result;
import com.evan.wj.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.util.HtmlUtils;

import javax.servlet.http.HttpSession;

@Controller
public class LoginController {

    @Autowired
    UserService userService;

    @CrossOrigin
    @PostMapping(value = "/api/login")
    @ResponseBody
    public Result login(@RequestBody User requestUser, HttpSession session) {
        String username = requestUser.getUsername();
        username = HtmlUtils.htmlEscape(username);

        User user = userService.get(username, requestUser.getPassword());
        if (null == user) {
            return new Result(400);
        } else {
            session.setAttribute("user", user);
            return new Result(200);
        }
    }
}
```

其实只是添加了一条语句 `session.setAttribute("user", user);`

#### LoginInterceptor

新建 package 名为 interceptor，新建类 LoginInterceptor。

Interceptor 即拦截器，在 Springboot 我们可以直接继承拦截器的接口，然后实现 preHandle 方法。preHandle 方法里的代码会在访问需要拦截的页面时执行。

```java
package com.evan.wj.interceptor;

import com.evan.wj.pojo.User;
import org.apache.commons.lang.StringUtils;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class LoginInterceptor  implements HandlerInterceptor{

    @Override
    public boolean preHandle (HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        HttpSession session = httpServletRequest.getSession();
        String contextPath=session.getServletContext().getContextPath();
        String[] requireAuthPages = new String[]{
                "index",
        };

        String uri = httpServletRequest.getRequestURI();

        uri = StringUtils.remove(uri, contextPath+"/");
        String page = uri;

        if(begingWith(page, requireAuthPages)){
            User user = (User) session.getAttribute("user");
            if(user==null) {
                httpServletResponse.sendRedirect("login");
                return false;
            }
        }
        return true;
    }

    private boolean begingWith(String page, String[] requiredAuthPages) {
        boolean result = false;
        for (String requiredAuthPage : requiredAuthPages) {
            if(StringUtils.startsWith(page, requiredAuthPage)) {
                result = true;
                break;
            }
        }
        return result;
    }
}
```

看起来似乎比较长，其实就是判断 session 中是否存在 user 属性，如果存在就放行，如果不存在就跳转到 login 页面。这里使用了一个路径列表（requireAuthPages），可以在里面写下需要拦截的路径。当然我们也可以拦截所有路径，那样就不用写这么多了，但会有逻辑上的问题，就是你访问了 \login 页面，仍然会需要跳转，这样就会引发多次重定向问题。

#### WebConfigurer

我们写完了拦截器，但是它却并不会生效，因为我们还没有把它配置到项目中。

新建 package 名为 `config`，新建类 `MyWebConfigurer`，代码如下：

```java
package com.evan.wj.config;

import com.evan.wj.interceptor.LoginInterceptor;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.context.annotation.Bean;
import org.springframework.web.servlet.config.annotation.*;

@SpringBootConfiguration
public class MyWebConfigurer implements WebMvcConfigurer {

    @Bean
    public LoginInterceptor getLoginIntercepter() {
        return new LoginInterceptor();
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry){
        registry.addInterceptor(getLoginIntercepter()).addPathPatterns("/**").excludePathPatterns("/index.html");
    }
}
```

通过这个配置类，我们添加了之前写好的拦截器。这里有一句非常重要的语句，即

```java
registry.addInterceptor(getLoginIntercepter()).addPathPatterns("/**").excludePathPatterns("/index.html");
```

这条语句的作用是对所有路径应用拦截器，除了 /index.html。

之前我们在拦截器 LoginInterceptor 中配置的路径，即 index，触发的时机是在拦截器生效之后。也就是说，我们访问一个 URL，会首先通过 Configurer 判断是否需要拦截，如果需要，才会触发拦截器 LoginInterceptor，根据我们自定义的规则进行再次判断。

/index 与 /index.html 是不同的，也就是说 /index 会触发拦截器而 /index.html 不会，但根据拦截器 LoginInterceptor 中我们定义的判断条件，以 /index 开头的路径都会被转发，包括 index.html。

因为我们做的是单页面应用，之前通过配置 ErrorPage，实际上访问所有路径都会重定向到 /index.html 。我们直接在浏览器地址栏输入 /index 会触发拦截器，经过拦截器重定向到 /login，然后 /login 再经过 Configurer 的判断，再次触发拦截器，由于不在需要拦截的路径中，所以被放行，页面则重新定向到了 /index.html，如果没有再 Configurer 中取消对 /index.html 的拦截，则会再次触发拦截器，再次重定向到 /login，引发如下错误。

### 6.4 Vuex 与前端登录拦截器

前面我们使用了后端拦截器，但这种拦截器只有在将前后端项目整合在一起时才能生效，而前后端分离的项目实际上不推荐这么做，接下来我们尝试用前端实现相似的功能。

实现前端登录器，需要在前端判断用户的登录状态。我们可以像之前那样在组件的 data 中设置一个状态标志，但登录状态应该被视为一个全局属性，而不应该只写在某一组件中。所以我们需要引入一个新的工具——Vuex，它是专门为 Vue 开发的状态管理方案，我们可以把需要在各个组件中传递使用的变量、方法定义在这里。之前我一直没有使用它，所以在不同组件传值的问题上十分头疼，要写很多多余的代码来调用不同组件的值，所以推荐大家从一开始就去熟悉这种管理方式。

#### 6.4.1引入vuex

在我们的项目文件夹中，运行`npm install vuex --save --legacy-peer-deps`之后，在 src 目录下新建一个文件夹 store，并在该目录下新建 index.js 文件，在该文件中引入 vue 和 vuex，代码如下：

> 本来安装vux只需要执行`npm install vuex --save`即可但是有些环境因为版本问题需要加上后面的内容

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

之后，我们在 index.js 里设置我们需要的状态变量和方法。为了实现登录拦截器，我们需要一个记录用户信息的变量。为了方便日后的扩展（权限认证等），我们使用一个用户对象而不是仅仅使用一个布尔变量。同时，设置一个方法，触发这个方法时可以为我们的用户对象赋值。完整的代码如下：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    user: {
      username: window.localStorage.getItem('user' || '[]') == null ? '' : JSON.parse(window.localStorage.getItem('user' || '[]')).username
    }
  },
  mutations: {
    login (state, user) {
      state.user = user
      window.localStorage.setItem('user', JSON.stringify(user))
    }
  }
})
```

这里我们还用到了 `localStorage`，即本地存储，在项目打开的时候会判断本地存储中是否有 user 这个对象存在，如果存在就取出来并获得 `username` 的值，否则则把 `username` 设置为空。这样我们只要不清除缓存，登录的状态就会一直保存。

#### 6.4.2修改路由配置

为了区分页面是否需要拦截，我们需要修改一下 `src\router\index.js`，在需要拦截的路由中加一条元数据，设置一个 `requireAuth` 字段如下：

```javascript
    {
      path: '/index',
      name: 'AppIndex',
      component: AppIndex,
      meta: {
        requireAuth: true
      }
    }
```

完整的index.js代码

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import AppIndex from '@/components/home/AppIndex'
import Login from '@/components/Login'

Vue.use(Router)

export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/login',
      name: 'Login',
      component: Login
    },
    {
      path: '/index',
      name: 'AppIndex',
      component: AppIndex,
      meta: {
        requireAuth: true
      }
    }
  ]
})
```

#### 6.4.3钩子函数判断是否拦截

钩子函数及在某些时机会被调用的函数。这里我们使用 `router.beforeEach()`，意思是在访问每一个路由前调用。

打开 `src\main.js` ，首先添加对 `store` 的引用

```javascript
import store from './store'
```

并修改 Vue 对象里的内容

```javascript
new Vue({
  el: '#app',
  render: h => h(App),
  router,
  // 注意这里
  store,
  components: { App },
  template: '<App/>'
})
```

接着写 `beforeEach()` 函数

```javascript
router.beforeEach((to, from, next) => {
    if (to.meta.requireAuth) {
      if (store.state.user.username) {
        next()
      } else {
        next({
          path: 'login',
          query: {redirect: to.fullPath}
        })
      }
    } else {
      next()
    }
  }
)
```

这个的逻辑很简单，首先判断访问的路径是否需要登录，如果需要，判断 `store` 里有没有存储 `user` 的信息，如果存在，则放行，否则跳转到登录页面，并存储访问的页面路径（以便在登录后跳转到访问页）。

完整的 main.js 代码如下：

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'
import store from './store'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

var axios = require('axios')
axios.defaults.baseURL = 'http://localhost:8443/api'
Vue.prototype.$axios = axios
Vue.config.productionTip = false

Vue.use(ElementUI)

router.beforeEach((to, from, next) => {
    if (to.meta.requireAuth) {
      if (store.state.user.username) {
        next()
      } else {
        next({
          path: 'login',
          query: {redirect: to.fullPath}
        })
      }
    } else {
      next()
    }
  }
)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  render: h => h(App),
  router,
  store,
  components: { App },
  template: '<App/>'
})
```

#### 6.4.4修改Login.vue

之前的登录组件中，我们只是判断后端返回的状态码，如果是 200，就重定向到首页。在经过前面的配置后，我们需要修改一下登录逻辑，以最终实现登录拦截。

修改后的逻辑如下：

1.点击登录按钮，向后端发送数据
2.受到后端返回的成功代码时，触发 store 中的 login() 方法，把 loginForm 对象传递给 store 中的 user 对象
（*这里只是简单的实现，在后端我们可以通过用户名和密码查询数据库，获得 user 表的完整信息，比如用户昵称、用户级别等，返回前端，并传递给 user 对象，以实现更复杂的功能）
3.获取登录前页面的路径并跳转，如果该路径不存在，则跳转到首页

修改后的 login() 方法如下：

```javascript
login () {
  var _this = this
  console.log(this.$store.state)
  this.$axios
    .post('/login', {
      username: this.loginForm.username,
      password: this.loginForm.password
    })
    .then(successResponse => {
      if (successResponse.data.code === 200) {
        // var data = this.loginForm
        _this.$store.commit('login', _this.loginForm)
        var path = this.$route.query.redirect
        this.$router.replace({path: path === '/' || path === undefined ? '/index' : path})
      }
    })
    .catch(failResponse => {
    })
}
```

完整的 `Login.vue` 代码如下：

```html
<template>
  <body id="poster">
    <el-form class="login-container" label-position="left"
             label-width="0px">
      <h3 class="login_title">系统登录</h3>
      <el-form-item>
        <el-input type="text" v-model="loginForm.username"
                  auto-complete="off" placeholder="账号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input type="password" v-model="loginForm.password"
                  auto-complete="off" placeholder="密码"></el-input>
      </el-form-item>
      <el-form-item style="width: 100%">
        <el-button type="primary" style="width: 100%;background: #505458;border: none" v-on:click="login">登录</el-button>
      </el-form-item>
    </el-form>
  </body>
</template>

<script>

  export default {
    name: 'Login',
    data () {
      return {
        loginForm: {
          username: 'admin',
          password: '123'
        },
        responseResult: []
      }
    },
    methods: {
      login () {
        var _this = this
        console.log(this.$store.state)
        this.$axios
          .post('/login', {
            username: this.loginForm.username,
            password: this.loginForm.password
          })
          .then(successResponse => {
            if (successResponse.data.code === 200) {
              // var data = this.loginForm
              _this.$store.commit('login', _this.loginForm)
              var path = this.$route.query.redirect
              this.$router.replace({path: path === '/' || path === undefined ? '/index' : path})
            }
          })
          .catch(failResponse => {
          })
      }
    }
  }
</script>

<style>
  #poster {
    background:url("../assets/eva.jpg") no-repeat;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
  }
  body{
    margin: 0px;
  }
  .login-container {
    border-radius: 15px;
    background-clip: padding-box;
    margin: 90px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
  }
  .login_title {
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }

</style>
```



## 7.导航栏与图书页面

### 7.1路由配置

为了实现第一个要求，我们需要把导航栏放在其它页面的父页面中（对 Vue 来说就是父组件），之前我们讲过，`App.vue `是所有组件的父组件，但把导航栏放进去不合适，因为我们的登录页面中不应该显示导航栏。

为了解决这个问题，我们在 `component`s 目录下直接新建一个组件，命名为 `Home.vue`，原始代码如下：
