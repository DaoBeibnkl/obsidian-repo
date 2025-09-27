在dao中设置init()和destory()方法，并且在配置文件对应的bean中指明方法。

在service中继承InitializingBean,DisposableBean方法


容器关闭，调用ApplicationContext儿子接口中的close()方法，或者registerShutdownHook()。