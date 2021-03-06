## 涉及到知识点：
* vue.js
* 首页底部Tab
* banner广告
* 沉浸式状态栏
* 分类顶部切换的slider
* 图片预览并保存到相册
* 拍照、扫码、选择本地文件
* 侧滑栏
* 搜索列表页两层侧滑栏筛选
* 上拉刷新下拉加载
* 登录、退出逻辑处理
<br><br>主要功能点：<br>
首页商品展示，商品分类，搜索列表，商品详情，购物车，下订单，个人中心，我的订单/退单，收货地址管理

另外：[获取手机设备信息、app版本信息、ip地址](http://wblog.work/H5%E8%8E%B7%E5%8F%96%E6%89%8B%E6%9C%BA%E8%AE%BE%E5%A4%87%E4%BF%A1%E6%81%AF%E5%92%8Capp%E7%89%88%E6%9C%AC%E4%BF%A1%E6%81%AF/#more)


## 示例图片
<br>部分示例图片<br>
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/1.jpg" width="375" alt="示例1" />
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/2.jpg" width="375" alt="示例2" />
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/3.jpg" width="375" alt="示例3" />
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/4.jpg" width="375" alt="示例4" />
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/5.jpg" width="375" alt="示例5" />
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/6.jpg" width="375" alt="示例6" />
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/7.jpg" width="375" alt="示例7" />
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/8.jpg" width="375" alt="示例8" />
<img src="https://github.com/gs-wenbing/mui-mall/blob/master/img/show/9.jpg" width="375" alt="示例9" />

## 说明
如果对您有帮助，您可以点右上角 "Star" 支持一下 谢谢！ ^_^<br>
如有问题请直接在 Issues 中提，谢谢！ ^_^

该项目的数据和图片来自网络，均只用于案例展示，请勿用于其他用途。
## 常见问题
* 首页底部的Tab
html页面<br>
```
<nav class="mui-bar mui-bar-tab footerBar">
	<a id="defaultTab" class="mui-tab-item ft-tab-item mui-active" href="home.html">
		<span class=" mui-icon iconfont icon-m-ao"></span>
		<span class="mui-tab-label">首页</span>
	</a>
	<a id="tab1" class="mui-tab-item ft-tab-item" href="classify.html">
		<span class="mui-icon iconfont icon-m-am"></span>
		<span class="mui-tab-label">分类</span>
	</a>
	<a id="tab2" class="mui-tab-item ft-tab-item" href="cart.html">
		<span class="mui-icon iconfont icon-m-y"></span>
		<span class="mui-tab-label">购物车</span>
	</a>
	<a id="tab3" class="mui-tab-item ft-tab-item" href="my.html">
		<span class="mui-icon iconfont icon-m-aq"></span>
		<span class="mui-tab-label">我的</span>
	</a>
</nav>
```
js代码
```
//底部选项卡切换跳转
(function jumpPage() {
	//跳转页面
	var subpages = ['view/home/home.html', 'view/home/classify.html', 'view/home/cart.html', 'view/home/my.html'];
	var ids = ['home.html', 'classify.html', 'cart.html', 'my.html'];
	var aniShow = {};
	//创建子页面，首个选项卡页面显示，其它均隐藏；
	mui.plusReady(function() {
		plus.screen.lockOrientation("portrait-primary"); 
		var subpage_style = {
			top: '0px',
			bottom: '51px'
		};
		//设置bottom绝对位置
		//iphoneX中出现遮挡底部tab现象,采用js判断屏幕大小方式改变bottom值
		//isIPhoneX() 要在plusReady后调用
		if (isIPhoneX()) {
			subpage_style = {
				top: '0px',
				bottom: '88px', //34px
				styles: {
					"render": "always",
				}
			};
		}
		var self = plus.webview.currentWebview();
		for (var i = 0; i < 4; i++) {
			var temp = {};
			var sub = plus.webview.create(subpages[i], ids[i], subpage_style);
			if (i > 0) {
				sub.hide();
			} else {
				temp[ids[i]] = "true";
				mui.extend(aniShow, temp);
			}
			self.append(sub);
		}
	});
	//当前激活选项
	var activeTab = ids[0];

	//选项卡点击事件
	mui('.mui-bar-tab').on('tap', 'a', function(e) {
		e.preventDefault();
		var targetTab = this.getAttribute('href');
		if (targetTab == activeTab) {
			return;
		}
		//显示目标选项卡
		//若为iOS平台或非首次显示，则直接显示
		if (mui.os.ios || aniShow[targetTab]) {
			plus.webview.show(targetTab);
		} else {
			//否则，使用fade-in动画，且保存变量
			var temp = {};
			temp[targetTab] = "true";
			mui.extend(aniShow, temp);
			plus.webview.show(targetTab, "fade-in", 300);
		}
		//隐藏当前;
		plus.webview.hide(activeTab);
		//更改当前活跃的选项卡
		activeTab = targetTab;
	});
})()

```
注意iphoneX中出现遮挡底部tab现象,采用js判断屏幕大小方式改变bottom值，isIPhoneX()，isIPhoneX() 要在plusReady后调用。


* 沉浸式状态栏

1.在manifest.json文件，切换到代码视图，在plus -> statusbar 下添加immersed节点并设置值为true
```
"statusbar" : {
	"immersed" : true
},
```

2.__设置了沉浸式状态栏后，状态栏的高度变为0，如图所示<br>
![Image text](https://github.com/gs-wenbing/mui-mall/blob/master/img/show/status1.jpg)
<br>输入框把状态挡住了，这时候需要重写mui.css或者mui.min.css样式表，在样式表底部添加如下一段样式__： 
```
*解决沉寖式状态栏导致导航栏高度少20px的问题*/
.mui-bar-nav {
    height: 64px;
    padding-top: 22px;
}

.mui-bar-nav ~ .mui-content
{
    padding-top: 64px;
}

.mui-bar-nav ~ .mui-content .mui-pull-top-pocket
{
    top: 64px;
}

.mui-bar-nav ~ .mui-content.mui-scroll-wrapper .mui-scrollbar-vertical
{
    top: 64px;
}

.mui-bar-nav ~ .mui-content .mui-slider.mui-fullscreen
{
    top: 64px;
}
```
显示效果如图<br>
![Image text](https://github.com/gs-wenbing/mui-mall/blob/master/img/show/status2.jpg)
<br><br>注意：以上操作后Android沉浸式状态就完成了，但是IOS还需在distribute节点下的apple节点下添加
```
"UIReserveStatusbarOffset" : false
```
__本以为沉浸式状态栏就完成了，结果老板iPhoneX手机显示有问题，于是又单独适配iPhoneX，具体操作：<br>
在mui.js或者mui.min.js中底部添加如下一段代码__：
```
/**
 * 适配iPhone X 系列手机的导航栏(包括状态栏)
 */
mui.plusReady(function(){
    if(plus.navigator.isImmersedStatusbar() && isIPhoneX()){
        //.mui-bar-nav
        var nav = document.querySelector(".mui-bar-nav");
        if(nav){
            nav.style.cssText="height:88px; padding-top: 44px;";
        } else {
            return;
        }
        //.mui-bar-nav ~ .mui-content
        var content = document.querySelector(".mui-content");
        if (content) {
            content.style.paddingTop = "88px";
        } else {
            return;
        }
        //.mui-bar-nav ~ .mui-content .mui-pull-top-pocket
        var pullTopPocket_Arr = content.querySelectorAll(".mui-pull-top-pocket");
        if (pullTopPocket_Arr) {
            pullTopPocket_Arr.forEach(function(value){
                value.style.top = "88px";
            });
        }
        //.mui-bar-nav ~ .mui-content.mui-scroll-wrapper .mui-scrollbar-vertical
        var scrollbarVertical = document.querySelector(".mui-content.mui-scroll-wrapper .mui-scrollbar-vertical");
        if (scrollbarVertical) {
            scrollbarVertical.style.top = "88px";
        }
        //.mui-bar-nav ~ .mui-content .mui-slider.mui-fullscreen
        var slider_fullscreen_Arr = content.querySelectorAll(".mui-content .mui-slider.mui-fullscreen");
        if (slider_fullscreen_Arr) {
            slider_fullscreen_Arr.forEach(function(value){
                value.style.top = "88px";
            });
        }
    }
});

/**
 * 判断是否为iPhone X 系列机型
 */
function isIPhoneX(){
    if(plus.device.model.indexOf('iPhone') > -1 && screen.height >= 812){
        return true;
    }else{
        return false;
    }
}
```

*  标题栏在IOS上存在的问题
原生标题栏

```
var styles = { // 窗口参数 参考5+规范中的WebviewStyle,也就是说WebviewStyle下的参数都可以在此设置
	titleNView: { // 窗口的标题栏控件
		titleText: title, // 标题栏文字,当不设置此属性时，默认加载当前页面的标题，并自动更新页面的标题
		titleColor: "#FFFFFF", // 字体颜色,颜色值格式为"#RRGGBB",默认值为"#000000"
		backgroundColor: "#E60012", // 控件背景颜色,颜色值格式为"#RRGGBB",默认值为"#F7F7F7"
		autoBackButton: true
	}
}
plus.webview.create(url, id, styles, extras);
```
__1.mui原生标题栏，假如titleColor的值为小写（#ffffff）的话，在IOS上不显示标题，必须要大写（#FFFFFF）才显示，亲测<br>__
__2.非原生标题栏，假如页面中有输入框的话，软键盘弹出，IOS上会把标题栏顶上去，因为ios弹出软键盘的时候，webview的高度没有变化导致超出屏幕范围，
而plus这时候又会自动把header的 position：fixed 属性设置为 position：relative，header就跟着滚动了。在mui社区找到一个的解决方案：__
http://ask.dcloud.net.cn/question/10629
```
plus.webview.currentWebview().setStyle({  
    softinputMode: "adjustResize"  // 弹出软键盘时自动改变webview的高度  
}); 

html, body {  
    height: 100%;  
    margin: 0px;  
    padding: 0px;  
    overflow: hidden;  
    -webkit-touch-callout: none;  
    -webkit-user-select: none;  
}  

.mui-content {  
    height: 100%;  
    overflow: auto;   
}   
```
这样会解决IOS标题栏顶上去的问题，但是这样处理后，页面打开标题栏会有抖动，虽然很短暂，但是看着不爽,有更好的方案可以联系我（QQ742330561）

* 支付问题
mui的支付还是比较好配置的，详细的参照http://ask.dcloud.net.cn/article/71

* 侧滑导航主体内容设置下拉刷新和上拉加载后不能滚动的问题
使用div侧滑栏的时候，给侧滑栏主题设置了下拉刷新和上拉加载
```
mui.init({
	pullRefresh: {
		container: '#pullrefresh',
		down: {
			style: 'circle',
			callback: pulldownRefresh
		},
		up: {
			auto: false,
			contentrefresh: '正在加载...',
			callback: pullupRefresh
		}
	}
});
```
，页面不能滚动，把这段代码注释掉后就能滚动了，不知道是不是mui的bug，还是我没找到合适的方法。<br>
下面推荐webview模式侧滑菜单
```
//plusReady事件后，自动创建menu窗口；
main = plus.webview.currentWebview();
//setTimeout的目的是等待窗体动画结束后，再执行create webview操作，避免资源竞争，导致窗口动画不流畅；
setTimeout(function() {
	menu = mui.preload({
		id: 'class-menu',
		url: 'class-menu.html',
		styles: {
			left: "20%",
			width: '80%',
			zindex: 10000
		}
	});
}, 500);
```
详细查看demo的view/search/search.html和view/search/class-menu.html两个文件。

* 下拉刷新和其他控件的冲突

可以在控件某个事件触发的时候，禁止滚动下拉刷新，事件结束后在开启
```
mui('#pullrefresh').pullRefresh().setStopped(true);//暂时禁止滚动
mui('#pullrefresh').pullRefresh().setStopped(false);//开启禁止滚动
```
注意判断mui('#pullrefresh').pullRefresh()是否为空，否则会报错

