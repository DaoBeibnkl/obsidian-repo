>不依赖于任何的框架


## 1. @Inject和@Resource
>由官方提供的依赖注入注解。IDEA不建议在字段上用@autowired。。。

@Inject和@Resource都不能使用required=false。
	@inject需要额外导入jdk依赖
	@Resource跟@Autowired相反，先按名字（ByName），再按类型（ByClass）匹配查找。