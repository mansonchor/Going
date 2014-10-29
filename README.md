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

**Routing为另外一个独立路由控制模块，可[参考这里]**(https://github.com/mansonchor/Routing)


##page_controler对象

###page_controler.container     

容器dom obj

###page_controler.page_arr      

所有的page对象

###page_controler.add_page(page_id , options)

往控制器添加一个page，page可以设定各种参数和监听事件

@**return** {null} 

@**param** {string} page_id ： 页面的ID，作为一个独立页面的唯一标识

@**param** {json} options

>{string}  **route** ： 页面匹配的路由规则

```javascript
options.route = { "index": "index"  }
```
也可以组建带参数的url hash
```javascript
options.route = { "last/:art_id": "last"  }    //能匹配 #last/1000 等url hash

options.route = { "cat/:cat_id/:art_id": "cat"  }    //能匹配 #cat/2/1000 等url hash
```

也可通过()设置非强制参数
```javascript
options.route = { "hot/(:is_big_img)": "hot"  }    //能匹配 #hot 和 #hot/true

options.route = { "cat/:cat_id/(:art_id)": "cat"  }    //能匹配 #cat/2/1000 和 #cat/2
```

url参数可在页面的 `page_init` `page_show` 等监听下接收到


