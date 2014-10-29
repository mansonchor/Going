Going
=====

轻量的SPA 页面逻辑控制组件

单纯处理页面控制，做最简单的业务抽象

##主要API说明：

###Going.mount_container( container_id_or_obj , [options] )

@**return** {obj} page_controler ： page_controler控制对象

@**param** {string | dom obj} container_id_or_obj ： 承载页面的容器

@**param** {json} options

>{bool}  **use_routing** ： 是否连接路由使用

>{routing obj}  **routing_obj** ： (use_routing为true时生效)绑定的Routing实例，绑定后可自动映射路由和页面的关系

>{bool}  **listen_scroll** ： 是否监听页面滚动（若为true，可在后续每次页面滚动时触发page的window_scroll事件）
