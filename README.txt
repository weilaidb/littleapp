小程序属于前端开发
二、小程序框架介绍和运行机制
1.我们建立了“普通快速启动模板“，然后整个项目目录如下：
pages
index 
index.js
index.wxml
index.wxss
logs
utils
util.js
app.js
app.json
app.wxss
project.config.json 

2.app.js 
整个项目的启动文件，如注释写的 onlaunch 方法有三大功能，浏览器缓存进行存和取数据；用登陆成功的回调；获取用户信息。
globalData是定义整个项目的全局变量或者常量。
=== eg app.js ====
App({
    onlaunch:function() {
        console.log('App Launch')
    },
    onShow: function() {
        console.log('App Show')
    },
    onHide: function() {
        console.log('App Hide')
    },
    globalData: {
        hasLogin:false,
        openid:null
    },
    //lazy loading openid
    getUserOpenId: function(callback) {
        var self = this 
        
        if(self.globalData.openid) {
            callback(null, self.globalData.openid)
        } else {
            wx.login({
                success.function(data){
                    wx.request({
                        url:openIdUrl,
                        data:{
                            code:data.code
                        },
                        success:function(res) {
                            console.log('拉取openid成功', res)
                            self.globalData.openid = res.data.openid
                            callback(null, self.globalData.openid)
                        },
                        fail:function(res) {
                            console.log('拉取openid失败，将无法正常使用开放接口等服务', res)
                            callback(res)
                        }
                    })
                },
                fail: function(err) {
                    console.log('wx.login 接口调用失败，将无法正常使用开放接口等服务', err)
                    callback(err)
                }
            })
        }
    }
})

3.app.json
整个项目的配置文件，比如注册页面，配置tab页，设置整个项目的样式，页面标题等；
！注意：小程序启动默认的第一个页面，就是app.json的pages中的第一个页面。

== eg app.json ==== 
{
    "pages": [
        "page/component/index",
        "page/component/pages/view/view",
        "page/component/pages/scroll-view/scrool-view",
        "page/component/pages/swiper/swiper",
        "page/component/pages/text/text",
        "page/component/pages/icon/icon",
        "page/component/pages/progress/progress",
        
        "page/component/pages/button/button",
        "page/component/pages/checkbox/checkbox",
        "page/component/pages/form/form",
        "page/component/pages/input/input",
        "page/component/pages/label/label",
        "page/component/pages/picker/picker",
        "page/component/pages/radio/radio",
        "page/component/pages/slider/slider",
        "page/component/pages/switch/switch",
        "page/component/pages/textarea/textarea",
        
        "page/component/pages/navigator/navigator",
        "page/component/pages/navigator/navigate",
        "page/component/pages/navigator/redirect",
        
        "page/component/pages/image/image",
        "page/component/pages/audio/audio",
        "page/component/pages/video/video",
        
        "page/component/pages/map/map",
        
        "page/component/pages/canvas/canvas",
        
        "page/API/index",
        
        "page/API/pages/login/login",
        "page/API/pages/get-user-info/get-user-info",
        "page/API/pages/request-payment/request-payment",
        "page/API/pages/share/share",
        "page/API/pages/custom-message/custom-message",
        "page/API/pages/template-message/template-message",
        
        "page/API/pages/set-navigation-bar-title/set-navigation-bar-title",
        "page/API/pages/navigation-bar-loading/navigation-bar-loading",
        "page/API/pages/navigator/navigator",
        "page/API/pages/pull-down-refresh/pull-down-refresh",
        "page/API/pages/animation/animation",
        
        "page/API/pages/action-sheet/action-sheet",
        "page/API/pages/modal/modal",
        "page/API/pages/toast/toast",
        
        "page/API/pages/get-network-type/get-network-type",
        "page/API/pages/get-system-info/get-system-info",
        "page/API/pages/on-compass-change/on-compass-change",
        "page/API/pages/make-phone-call/make-phone-call",
        "page/API/pages/scan-code/scan-code",
        
        
        "page/API/pages/request/request",
        "page/API/pages/web-socket/web-socket",
        "page/API/pages/upload-file/upload-file",
        "page/API/pages/download-file/download-file",
        
        "page/API/pages/image/image",
        "page/API/pages/voice/voice",
        "page/API/pages/file/file",
        "page/API/pages/on-accelerometer-change/on-accelerometer-change",
        "page/API/pages/canvas/canvas",
        "page/API/pages/background-audio/background-audio",
        "page/API/pages/video/video",
        
        "page/API/pages/get-location/get-location",
        "page/API/pages/open-location/open-location",
        "page/API/pages/choose-locaton/choose-location",
        
        "page/API/pages/storage/storage",
    ],
    "window": {
        "navigationBarTextStyle": "black",
        "navigationBarTitleText:"演示",
        "navigationBarBackgroundColor": "#F8F8F8",
        "backgroundColor":"#FFFFFF",
        "enablePullDownRefresh":true
    },
    "tabBar": {
        "color":"#7A7E83",
        "selectedColor":"#3cc51f",
        "borderStyle":"black"
        "backgroundColor":"#ffffff",
        "list": [{
            "pagePath":"page/component/index",
            "iconPath":"image/icon_component.png",
            "selectedIconPath":"image/icon_component_HL.png",
            "text":"组件"
        },
        {
            "pagePath":"page/API/index",
            "iconPath":"image/icon_API.png",
            "selectedIconPath":"image/icon_API_HL.png",
            "text":"接口"
        }]
    },
    "networkTimeout": {
        "request":10000,
        "connectSocket": 10000,
        "uploadFile":10000,
        "downloadFile": 10000
    },
    "debug": true 
}


4.pages 
小程序的页面组件，有几个页面就会有几个子文件夹。比如快速启动模板，就有两个页面，index和logs
5.打开index目录 
可以看到有三个文件，其实和我们web开发的文件是一一对应的。
index.wxml对应index.html;
index.wxss对应 index.css 
index.js就是js文件。
一般我们还会给每个页面组件添加一个.json文件，作为该页面组件的配置文件，设置页面标题等功能。
6.双击index.js文件 
1）var app=getApp();
引入整个项目的app.js文件，用来取其中的公共变量等信息。
如果要使用util.js工具库中的某个方法，在util.js中modele.exports导出，然后在需要的页面中require即可得到。
2)比如，我们要获取豆瓣电影的时候，我们就需要调用豆瓣的api;我们先在app.js中的 globalData 中定义 doubanBase然后在index.js中使用app.globalData.doubanBase即可取到这个值。
当然这些常量你也可以在页面需要的时候，再用写死的值，但是为了整个项目的维护，还是建议把这种公用的参数统一写在配置文件中。
globalData:{
    defaultCity: '',
    defaultCountry:'',
    weatherData:'',
    air:'',
    day:'',
    g_isPlayingMusic:false,
    g_currentMusicPostId:null,
    doubleBase:"https://api.douban.com",
    heWeatherBase:"https://free-api.heweather.com",
    juhetoutiaoBase:"https://v.juhe.cn/toutiao/index",
    tencentMapKey:"afde-1234-",
    curBook:""
}

//获取豆瓣电影正在热映信息
var inTheatersUrl = app.globalData.doubleBase + 
    "/v2/movie/in_theaters" + "?start=0&count=6";
this.getMovieListData(inTheatersUrl, "inTheaters", "正在热映热映");

3)接下来在整个page({})中，第一个data,就是本页面组件的内部数据，会渲染到该页面的wxml文件中，类似于vue/react，通过setData修改data数据，驱动页面渲染。
data: {

},
onLoad: function(options) {

},

4)一些生命周期函数
比如onload(),onready(),onshow(), onhide()等等，监听页面加载、页面初次渲染、页面显示、页面隐藏等
更多的可以查看官网API,其中用到的最多就是onload方法，和onShareAppMessage()方法（设置页面分享的信息）

7.wxml模板的使用
比如本项目的电影页面，就是以最小的星级评价组件wxml当做模板，star到movie到movie-list，一级一级的嵌套使用。
start-template.wxml 页面写好name属性；然后import引入的时候通常name获得即可。
stars-template.wxml
<template name="starsTemplate">...
</template>

movie-template.wxml 
<import src="../stars/stars-template.wxml"/>
<template name="movieTemplate">
    <view class="movie-container" catchtap="onMovieTap" data-movieId=" {
        {movieId}">
    <image class="movie-img" src=""{{coverageUrl}}"></image>
    <text class="movie-text"> {{title}}</text>
    <template is="starsTemplate" data="{{stars:stars, score:average}}"/>
    </view>
</template>

8.常用wxml标签
view,text,icon,swiper,block,scroll-view等等，这些标签直接查官网文档即可

三、小程序框架、各个页面以及发布上线的注意点
1.整个框架中的一些注意点
1）整个wxml页面，最底层的标签是<page></page>
2)每个页面顶部导航的颜色，title在本页面的json中配置，如果没有配置的话， 取app.json中的总配置。
3）json中不能写注释 ，不然会报错。
4）路由相关
对于路由的触发方式及页面生命周期函数如下：







































