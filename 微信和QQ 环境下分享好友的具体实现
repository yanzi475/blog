##   分享好友的功能

####  一些资料
* [mqq](https://open.mobile.qq.com/api/common/index#api:closeWebViews)
* [example](http://qun.qq.com/jsapi.html#js-mqq-ui-popBack)
* [tips](http://wiki.open.qq.com/wiki/%E5%A6%82%E4%BD%95%E5%AE%9A%E5%88%B6%E6%89%8BQ%E7%9A%84Webview)
* [wexin](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)


####  分享oauth授权登录的问题


####  分享oauth的链接

这里贴出两个链接，分别是微信授权登录和QQ授权登录 

```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx90bfe8ac7aa1338a&redirect_uri=https://personal.webank.com/pmbank-wbac/wechatDispatcher/OAuth?appId=wx90bfe8ac7aa1338a&url=https://personal.webank.com/s/hj/weixinmp/index.html?op=product&response_type=code&scope=snsapi_userinfo&state=STATE#wechat_redirect
```



```
http://open.show.qq.com/cgi-bin/login_state_auth_redirect?appid=101219330&redirect_uri=https://test-personal.webank.com/k/pmbankqbac/dispatcher/OAuth?appId=101219330&url=https://testpersonal.webank.com/s/hj/weixinmp/ "+url,
            
```


####  微信分享
微信网页的授权的可以获取用户的一些基本信息，在微信的开发文档中有具体的步骤。
* 授权的链接中带有redirect_uri
* 用户授权后获取code
* 拿到code值后获取access_token
* 授权登录获取用户的一些基本信息

```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=wxf0e81c3bee622d60&redirect_uri=https%3a%2f%2fapi.weixin.qq.com%2fsns%2foauth2%2faccess_token%3fappid%3dAPPID%26secret%3dSECRET%26code%3dCODE%26grant_type%3dauthorization_code+


https://open.weixin.qq.com/connect/oauth2/authorize?appid=wxf0e81c3bee622d60&redirect_uri=https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code &response_type=code&scope=snsapi_userinfo&state=STATE#wechat_redirect

以上获取access_token, openid 可以通过access_token和openid拉取用户信息了
请求方法：
http：GET（请使用https协议）
https://api.weixin.qq.com/sns/userinfo?access_token=ACCESS_TOKEN&openid=OPENID&lang=zh_CN 


```
`尤其注意：由于公众号的secret和获取到的access_token安全级别都非常高，必须只保存在服务器，不允许传给客户端`

请求以上链接就可以获取网页授权的access_token了，

####  进入分享页面，oauth的时机

公众号中的index..js 中的loginHelper.getLoginStatus();以及查询cookie和后台的交互完成授权登录。然而，在分享页面分享的只是当前页面，微信和QQ 中都是一样的。qq 分享的要在调用的文件中引入
`https://open.mobile.qq.com/sdk/qqapi.js?_bid=152`这个文件，而微信中使用WeixinJSBridge，JS API建立在客户端浏览器内置JS对象WeixinJSBridge上。然而WeixinJSBridge并不是WebView一打开就有了，客户端需要初始化这个对象，当这个对象准备好的时候，客户端会抛出事件"WeixinJSBridgeReady"。因此，在调用WeixinJSBridge相关api时，需要做好WeixinJSBridge存在与否的判断：

* [旧版](https://github.com/Tencent/weui/wiki/%E5%BE%AE%E4%BF%A1JSAPI)
* [新版](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)


```
if (typeof WeixinJSBridge == "object" && typeof WeixinJSBridge.invoke == "function") {
    callback();
} else {
    if (document.addEventListener) {
        document.addEventListener("WeixinJSBridgeReady", callback, false);
    } else if (document.attachEvent) {
        document.attachEvent("WeixinJSBridgeReady", callback);
        document.attachEvent("onWeixinJSBridgeReady", callback);
    }
}



// 当微信内置浏览器完成内部初始化后会触发WeixinJSBridgeReady事件。
document.addEventListener('WeixinJSBridgeReady', function onBridgeReady() {
    // 发送给好友
    WeixinJSBridge.on('menu:share:appmessage', function (argv) {
        shareFriend();
    });
    // 分享到朋友圈
    WeixinJSBridge.on('menu:share:timeline', function (argv) {
        shareTimeline();
    });
   
}, false);
},

```

```
WeixinJSBridge.invoke('sendAppMessage',{
    'appid': 'wx1234567890', // 公众号appID
    'type': 'link', // 非必填，music,vido或link,默认为link。
    'data_url': '', // 非必填，连接地址,如音乐的mp3数据地址,供内置播放器使用
    'img_url': 'http://pnewsapp.tc.qq.com/newsapp_bt/0/9963967/640', // 缩略图地址
    'img_height':370, // 缩略图高度
    'img_width':550, // 缩略图宽度
    'link':'http://view.inews.qq.com/a/WXN2013101101385701', // 链接地址
    'desc':'desc', // 描述
    'title':'title' // 标题
},function(res){
    //alert(res.err_msg);
});
```
* [旧版](https://github.com/Tencent/weui/wiki/%E5%BE%AE%E4%BF%A1JSAPI)
* [新版](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)
旧版不需要引入文件，要判断WeixinJSBridge是否初始化完毕，新版需要引入文件`http://res.wx.qq.com/open/js/jweixin-1.2.0.js`

```
exports.openShareWXPay = function(url) {

    var configShare={
        "appid": "wx90bfe8ac7aa1338a",
        "image_url": ENV.IMAGE_URL,
        "img_width": "200",
        "img_height": "200",
         "link": ENV.URL+"weixinmp/"+ url,

        "desc": "先完成缴费，可以提前选宿舍哦！",
        "title": "武汉学院线上缴学费"

    }


function onBridgeConfigShareReadyPay(url) {
    WeixinJSBridge.on('menu:share:appmessage', function () { shareParent(url); });
    WeixinJSBridge.on('menu:share:timeline', function () { shareTimelinePay(url); });
}
// 分享到好友
function shareParent(url) {
    WeixinJSBridge.invoke('sendAppMessage', {
        "appid": configShare.appid,
        "image_url": configShare.image_url,
        "img_width": "200",
        "img_height": "200",
        "link": configShare.link,
        "desc": configShare.desc,
        "title": configShare.title
    }, function(res) {
        //_report('send_msg', res.err_msg);
    });
}
// 分享到朋友圈
function shareTimelinePay(url) {
    WeixinJSBridge.invoke('shareTimeline', {
        "img_url": configShare.image_url,
        "img_width": "200",
        "img_height": "200",
        "link": configShare.link,
        "desc": configShare.desc,
        "title": configShare.title
    }, function(res) {

    });
}


if (typeof WeixinJSBridge == 'undefined') {
    if (document.addEventListener) {
        document.addEventListener('WeixinJSBridgeReady', function () { onBridgeConfigShareReadyPay(url); }, false);
    } else if (document.attachEvent) {
        document.attachEvent('WeixinJSBridgeReady', function () { onBridgeConfigShareReadyPay(url); });
        document.attachEvent('onWeixinJSBridgeReady', function () { onBridgeConfigShareReadyPay(url); });
    }
} else {
    onBridgeConfigShareReadyPay(url);
}
};
```

#### 微信新版分享api 

```

wx.config({
    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '', // 必填，公众号的唯一标识
    timestamp: timestamp(), // 必填，生成签名的时间戳
    nonceStr: Math.random().toString(16).substr(2), // 必填，生成签名的随机串
    signature: '',// 必填，签名，见附录1
    jsApiList: [] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
});
var timestamp =>(){
 return new Date().getTime();
}



```

JS-SDK使用权限签名算法

* 用第一步拿到的access_token 采用http GET方式请求获得jsapi_ticket（有效期7200秒，开发者必须在自己的服务全局缓存jsapi_ticket）：https://api.weixin.qq.com/cgi-bin/ticket/getticket?access_token=ACCESS_TOKEN&type=jsapi 
* 所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串string1 
```
jsapi_ticket=sM4AOVdWfPE4DxkXGEs8VMCPGGVi4C3VM0P37wVUCFvkVAy_90u5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcHKP7qg&noncestr=Wm3WZYTPz0wzccnW&timestamp=1414587457&url=http://mp.weixin.qq.com?params=value
```
* 对string1进行sha1签名，得到signature：




####  qq分享

```
    exports.openShareQQPay = function(url) {
        var param = {
            "share_type": 0,
            "image_url": ENV.IMAGE_URL,
            "share_url":ENV.URL+"weixinmp/"+url,
            "desc": "先完成缴费，可以提前选宿舍哦！",
            "title": "武汉学院线上缴学费"
        };

        // mqq.data.setShareInfo(param);

        mqq.invoke('data', 'setShareInfo', param);

        // mqq.ui.setOnShareHandler(function(type) {
        //     debugger;
        //     console.log("test qq:"+param);
        //     switch (type + '') {
        //         case '0':
        //             mqq.ui.shareMessage(param);
        //             break;
        //         case '1':
        //             param['share_type'] = 1;
        //             mqq.ui.shareMessage(param);
        //             break
        //     }
        // });
    };
    
```


* qq 定制分享内容   mqq.invoke('data', 'setShareInfo', param); 
注意：share_url必须跟页面url同一个域名，否则设置不生效
* qq 公众号中分享按钮的控制
* 返回按钮的文案的更改，以及返回逻辑的处理

![](file:///users/haiyanSong/Documents/pic/wx_qq/open.png)
![](file:///users/haiyanSong/Documents/pic/wx_qq/close.png)
![](file:///users/haiyanSong/Documents/pic/wx_qq/my.png)

```
 if(wx){
    util.redirect("../school/pay-details.html?school_id=" + encodeURIComponent(school_id)+
        "&student_id=" + encodeURIComponent(student_id)+
        "&trans_token=" + encodeURIComponent(trans_token)+
        '&_wv='+encodeURIComponent(hideParam));

}else{
    //qq 环境重新打开一个webview
    mqq.ui.openUrl({
        url: url,
        target: 1,
        style: 3
    })
    //然后，关闭上一个webview
    setTimeout(function(){
        mqq.ui.popBack();

    },1000);


}

```

[文档](http://h5.qianbao.qq.com/wiki/h5_standard/1019)
[文档2](https://open.mobile.qq.com/api/common/index#api:popBack)


####  分享登录态的问题

* 强制授权 share.js 
* 禁止二次分享 QQ下可以实现，微信下实现？
* 几个不能用的API，QQ下无法关闭应用，返回的问题
*  微信分享给qq 登录态的问题？因为那个是微信的授权，和QQ授权登录，登录态是不一样的。。。
*  iOS和 android的各种兼容问题。。。
