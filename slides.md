---
theme: apple-basic
layout: statement
highlighter: shiki
fonts:
  mono: Input Mono
---

# What is Rest?


---

# 定义

Rest是一种用于设计网络应用程序的架构风格,最早由Roy Thomas Fielding在2000年提出：将一个功能强、性能好、适宜通信的架构命名为Rest。
<br/>
<br/>

<div v-click="1">
Representational State Transfer的缩写，即:表现层状态转化
</div>

<v-clicks at="2">

- 表现层：资源具体呈现出来的形式，在http请求头中用accept和content-type来描述。eg:文本可以txt格式表现出来，也可以按照html、json等格式
- 状态转换：客户端与服务端通信过程势必会发生数据和状态的转变。（get|post|put|delete）

</v-clicks>

<img v-click="3" border="rounded" src="http://127.0.0.1:5500/imgs/rest/cs.png" />

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
img{
  width:100%;
  height:270px;
}
</style>
---

# restful架构的原则

<div class="grid grid-cols-2 gap-x-4 gap-y-4">

<v-clicks at="1">

- 统一接口
  - 一切 RESTful Web 服务设计的基础
  - 通过资源标识：接口必须唯一标识客户端和服务器之间涉及的资源
  - 通过操作标识资源：get|post|put|delete
  - 自描述信息:每个资源标识应携带足够的信息来描述如何处理消息
- 客户端-服务器设计模式
- 无状态
  - 要求客户端到服务器的每个请求必须包含理解和完成请求所需的所有信息。

</v-clicks>

<v-clicks at="4">

- 可缓存
  - http缓存策略，响应可以隐性或显式的将自身标记为可缓存或不可缓存
- 分层系统
  - 允许通过约束组件行为来由分层组成架构
  - 通过分层和解耦来降低系统的复杂度
- 按需编码
  - 允许通过已小程序或脚本的形式下载和执行代码来扩展客户端功能

</v-clicks>

</div>

---

# Restful api

### 遵循rest架构风格的api称为restful api

<br>
<br>
<br>

<v-clicks at="1">

<img  border="rounded" src= "http://127.0.0.1:5500/imgs/rest/rest_api.svg" />

</v-clicks>

<v-clicks at="2">

帮助我们与服务器之间通信的“语言”。

<br>

rest api 是一套api设计的准则，规范了api设计的框架，使得服务器、客户端之间有一个通用的沟通语言

</v-clicks>

---

# 如何设计restful api

<v-clicks at="1">

restful api 规范了api设计的两大核心原则：
1. API应该作用于 Resource（资源）上
2. 对资源的操作应使用对应语义的几种操作，包括： GET, POST, PUT, PATCH, DELETE

在设计时选择正确的资源并以正确的粒度对资源进行建模非常重要，以便 API 使用者从 API 获得所需的功能，API 行为正确且 API 可维护
<br>

什么是资源？？
<br>
资源是不同的应用程序向其客户端提供的信息。资源可以是图像、视频、文本、数字或任何类型的数据。
<br>
任何的信息在REST架构里都被抽象为资源：图像、文档、集合、用户，等，REST通过资源标识符来和特定资源进行交互.对于资源的命名，通常使用名词的复数形式

<br>

资源是对一组实体的概念映射，而不是在任何特定时间点对应于映射的实体，
选择资源的起点是分析业务领域并提取与业务需求相关的名词

</v-clicks>


---


<div class="grid grid-cols-2 gap-x-4 gap-y-4">

<div v-click="1">

通常path组成如下:

```javascript
/{version}/{resources}/{resource_id}
```
version：API版本号，有些版本号放置在头信息中也可以，通过控制版本号有利于应用迭代。
<br>
resources：资源，RESTful API推荐用小写英文单词的复数形式。
<br>
resource_id：资源的id，访问或操作该资源。

若有的时候资源的级别较大，下面可以分为很多子资源,最多两层资源，太多导致path太长，尽量保证简洁：
```javascript
/{version}/{resources}/{resource_id}/{subresources}/{subresource_id}
```

若简单的增删改查不满足需求，可以增加action,命名同样遵循http方法一样,get就是get而不是getById,例如batch：
```javascript
/{version}/{resources}/{resource_id}?action=batch
```

</div>

<div v-click="2">

具体设计规则如下：
1. 不用大写字母，所有单词使用英文且小写。
2. 连字符用中杠"-"而不用下杠"_"
3. 正确使用 "/"表示层级关系,URL的层级不要过深，并且越靠前的层级应该相对越稳定
4. 结尾不要包含正斜杠分隔符"/"
5. URL中不出现动词，用请求方式表示动作
6. 资源表示用复数不要用单数
7. 不要使用文件扩展名

</div>

</div>

---

## 防止将业务逻辑迁移到api使用者

<v-clicks at="1">

如果你的api设计细粒度程度到对每个文档进行crud，面向 CRUD 的低级方法将业务逻辑放在客户端代码中，在客户端（API 使用者）和它不应该关心的服务（API）之间创建紧密耦合,每当业务逻辑发生变化时，所有 API 使用者都必须更改代码并重新部署系统.

<br>

eg: 流程A:需要对a,b文档进行操作。api使用者则需要调用ab文档的更新接口，使得api使用者也要去关心流程A里的业务操作。若果流程A变更了，增加了对c文档的更新。则api使用者需要修改代码和重新部署。

<br>

解决方法:对于跨越多个资源的复杂业务流程，可以将业务流程视为资源本身，api使用者只需要对这个资源进行操作，则不需要关心背后到底操作了多少文档。

</v-clicks>

---

## 摆脱低级的CRUD

<v-clicks at="1">

如果你的api设计全是单纯的curd,那你的服务就允许外部随便来put和delete你的资源状态，那你的服务感觉上就变成了一个低级的数据库。

<br>

随着业务越来越复杂，我们系统的api已经不单单是对文档的curd了，涉及了很多复杂的业务操作。

eg: 更新站点api，不仅仅是更新站点model这么简单，背后有很多其他的逻辑，那我们就可以将更新站点视为一个业务流程或者领域事件，将这一操作视为资源本身。将里面其他的逻辑也抽象为某个业务流程资源，这样就使更新站点里的代码逻辑更加清晰。

<br>

解决方法：围绕基于业务流程和领域事件的资源设计 API。

<br>
<br>

</v-clicks>

---

# 总结

<v-clicks at="1">

<br>
<br>
<br>

在 REST 架构风格中，数据和功能被视为资源，并使用 统一资源标识符 (URI) 进行访问

api的设计不能仅仅局限于低级crud建立资源，在复杂业务流程面前应该围绕具体事件进行资源的抽象。

restful api使一种风格，而不是规则或约束，过于理想的restful API会付出比较大的成本。

过分强调资源，而实际业务API可能有各种需求比较复杂，单单使用资源的增删改查可能并不能有效满足使用需求，强行使用RESTful风格API只会增加开发难度和成本。

</v-clicks>


---
layout: center
class: text-center
---

# Learn More

[rest-api-design-resource-modeling](https://www.thoughtworks.com/insights/blog/rest-api-design-resource-modeling) · [GitHub API](https://docs.github.com/cn/rest)
