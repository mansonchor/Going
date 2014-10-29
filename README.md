Going
=====

轻量的SPA 页面逻辑控制组件

单纯处理页面控制，做最简单的业务抽象

##主要API说明：

###Going.mount_container( container_id_or_obj , [options] )

@**return** {obj} page_controler ： page_controler控制对象，这个对象提供页面控制的所有方法

@**param** {string | dom obj} container_id_or_obj ： 承载页面的容器

@**param** {json} options

>{bool}  **use_routing** ： 是否连接路由使用

>{routing obj}  **routing_obj** ： (use_routing为true时生效)绑定的Routing实例，绑定后可自动映射路由和页面的关系

>{bool}  **listen_scroll** ： 是否监听页面滚动（若为true，可在后续每次页面滚动时触发page的window_scroll事件）

```javascript
Routing.initialize()

var page_controler = Going.mount_container('page-container' , { use_routing : true , routing_obj : Routing , listen_scroll : true })
```

**Routing为另外一个独立路由控制模块，可[参考这里](https://github.com/mansonchor/Routing)**


##page_controler对象

###page_controler.container     

容器dom obj

###page_controler.page_identify     

page的唯一标识，是用来判断页面是否已经存在的关键

组织规则是：page_id + params

###page_controler.page_arr      

所有的page对象

###page_controler.add_page(page_id , options)

往控制器添加一个page，page可以设定各种参数和监听事件

@**return** {null} 

@**param** {string} page_id ： 页面的ID，作为一个独立页面的唯一标识

@**param** {json} options

>{string}  **route** ： 页面匹配的路由规则

```javascript
options.route = "index"
```
也可以组建带参数的url hash
```javascript
options.route = "last/:art_id"    //能匹配 #last/1000 等url hash

options.route = "cat/:cat_id/:art_id"    //能匹配 #cat/2/1000 等url hash
```

也可通过()设置非强制参数
```javascript
options.route = "hot/(:is_big_img)"    //能匹配 #hot 和 #hot/true

options.route = "cat/:cat_id/(:art_id)"    //能匹配 #cat/2/1000 和 #cat/2
```

url参数可在页面的 `page_init` `page_show` 等事件监听下接收

```
//页面路由配置
options.route = "cat/:cat_id/(:art_id)"

options.page_init = function()
{
	//params是一个数组，依次是url传递的参数
	var cat_id = this.params[0]    //url的第一个参数，即cat_id
	var art_id = this.params[1]    //url的第二个参数，即art_id
}
```

>{bool}  **dom_not_cache** [defalut false]： 当前页面返回上一页面后，是否把该页从dom remove掉

>{bool}  **ignore_exist** [defalut false]： 当路由到该页面已经存在时，是否忽略存在再另外新建一个页面

这里几个情况要说明一下：

1.一些静态的页面，创建了之后建议保留下来；需要每次动态获取数据的页面，可以设置成true

2.当路由到一个页面时，Going会自动判断是否已经存在，假如存在将不会重新创建一个新页面，除非 **ignore_exist为true**

3.路由到一个存在的页面时，**不会触发 page_init 监听**

~~>{emum} **transition_type** [ none , slide  , slide reverse , slideup , fade ] ：页面的转场效果~~  **由于性能和体验问题，已弃用**

>{function} **initialize** ： 如果定义了该函数，页面view创建时会调用

一般用作page框架render

>{function} **page_init** ： 如果定义了该函数，页面view创建时会调用

它的调用在initialize之后，一般用作异步获取数据等操作

>{function} **page_before_show** ： 如果定义了该函数，页面在进入可视区域前会调用

>{function} **page_show** ： 如果定义了该函数，页面在完全进入可视区域后会调用

它和 page_before_show 的调用时机差别，在于转场效果会占用一定的动画时间
假如没有转场效果，理论上 page_before_show 和 page_show 的调用时间一致


>{function} **page_before_hide** ： 如果定义了该函数，页面在退出可视区域前会调用

>{function} **page_hide** ： 如果定义了该函数，页面在完全退出可视区域后会调用

两个页面之间的切换，进场页面的 page_show 是呼应着退场页面的 page_hide 的

>{function} **window_change** ： 如果定义了该函数，在屏幕大小变化时会触发当前激活页面该函数，其它隐藏页面不会触发

例如：手机横竖屏变化、某些浏览器导航条的折叠等，都会引发

>{function} **window_scroll** ： 如果定义了该函数，在浏览器滚动时会触发当前激活页面该函数，其它隐藏页面不会触发

window_scroll会返回一些参数

```javascript
options.window_scroll : function(judge)
{
	if(judge.nearBottom)
	{
		//滚动接近底部
	}
}
```

