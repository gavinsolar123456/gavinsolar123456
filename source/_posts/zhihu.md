---
title: zhihu
date: 2017-03-11 19:45:00
tags:
---
> Vue.js是我所喜爱的，知乎也是我喜爱的，突发奇想使用vue做了一个知乎日报

## 项目地址：
[Github地址](https://github.com/pomelo-chuan/Zhihu-Daily-Vue.js)
[在线预览demo](http://lovestreet.leanapp.cn/zhihu/#/)

## 设计：
1.设计上没有按照知乎日报客户端的交互及UI设计来做，本渣亲自捏了一个，颜色以黑白灰为主，本来想加入知乎的蓝色，但是本渣设计功力实在太差，故舍弃掉了蓝色，只采用黑白灰三色。

2.日报条目采用无边界设计，只添加淡灰色的分割线来区分。

3.整个网页是一个单页应用(Single Page Web App)，所有的数据使用vuex来进行管理，在store里统一管理。

## 预览：
![预览图片](http://upload-images.jianshu.io/upload_images/3261015-36f5995eca60c77c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 涉及到的技术：
1. 该项目使用[vue-cli](https://github.com/vuejs/vue-cli)构建、打包，配合vue全家桶（[vue](https://github.com/vuejs/vue)、[vuex](https://github.com/vuejs/vuex)、[vue-router](https://github.com/vuejs/vue-router)）进行编码、开发

2. 基础样式使用basscss，basscss对层叠样式表的设计方式简练、高效、可复用性强

3. 网络请求使用[axios](https://github.com/mzabriskie/axios)

4. 后台使用Node.js

## [vuex架构](https://github.com/pomelo-chuan/Zhihu-Daily-Vue.js/blob/master/src/vuex/modules/zhihudata.js)：
1. 将getters、mutations、actions变量名设定一个命名空间，否则store写的大了，命名肯定会乱，示例：
```
// actions types
export const FECTH_NEWS_LATEST = 'FECTH_NEWS_LATEST'                // 最新消息列表
// mutstions types
export const TOGGLE_NEWS_LATEST = 'TOGGLE_NEWS_LATEST'              // 最新消息列表
//  getters types
export const DONE_NEWS_LIST_ROOT = 'DONE_NEWS_LIST_ROOT'            // 最新消息列表梗
```
命名具有意义，并且写好注释。

2. store分模块
其实不用分模块，但还是分一下吧，为后续开发着想。

3. 使用getters将状态（state）、数据（data）发往页面模版（template）上，发数据有两三种方式，根据自己习惯来吧，我是这样做的：
```
[types.DONE_NEWS_LIST_ROOT]: state => {
        return state.NewsListRoot
    }
```
```
computed: {
		...mapGetters(['DONE_NEWS_LATEST', 'DONE_LOADING_ONE', 'DONE_LOADING_TWO', 'DONE_NEWS_LIST_ROOT'])
	}
```
```
<div v-for="list in DONE_NEWS_LIST_ROOT">
		<!-- ===使用v-for来循环渲染数据=== -->
</div>
```
4. mutations与actions：
a、mutations是用来处理同步的事情的，比如给state中的变量赋值；
b、actions是用来处理异步的事情，比如网络请求；
c、但是actions也是可以做同步的事情的，但最好按照vuex的建议来做：在mutations中处理同步操作

5. 
具体怎么处理的看我的github，记得点点赞啥的：
[vuex地址](https://github.com/pomelo-chuan/Zhihu-Daily-Vue.js/tree/master/src/vuex)
[store地址](https://github.com/pomelo-chuan/Zhihu-Daily-Vue.js/blob/master/src/vuex/modules/zhihudata.js)


## 遇到的难题：
1. 跨域。跨域是前端一个老生常谈的问题，我使用node做了一下中转，示例代码如下：
```
router.get('/news/latest', function (req, res, next) {
    var options = {
        method: "GET",
        url: "http://news-at.zhihu.com/api/4/news/latest"
    };
    request(options, function (error, response, body) {
        if (error) throw new Error(error);
        res.json(JSON.parse(body))
    });
});
```

2.日报详情的渲染。日报详细内容知乎是一个html格式的字符串，而数据的请求及渲染是异步的，正常情况下来说，浏览器是无法解析成功的，但是vue提供的一个v-html方法，可以搞定，示例代码如下：
```
<div v-html="DONE_NEWS_DETAIL.body"></div>
```
其中DONE_NEWS_DETAIL是数据

## 后记：
大家多多交流，互相学习啊，写的不好的地方情指正哦！