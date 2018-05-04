## NamespaceHandler 接口
```
public interface NamespaceHandler {
	void init();
	BeanDefinition parse(Element element, ParserContext parserContext);
	BeanDefinitionHolder decorate(Node source, BeanDefinitionHolder definition, ParserContext parserContext);
}
```

#### 作用:
> 在Spring中bean作为基础，当要对Spring进行扩展的时候，我们可能会需要添加Spring的自定义标签，例如Spring-JDBC，Spring-MVC等等，
这些标签不属于Spring的默认标签。我们需要解析我们自己的标签。NamespaceHandler正是为此而生

**NamespaceHandler是用处理XML标签的NameSpace 然后找到对应的beanDefintionParser用来解析自定义标签**

**init()**  在NamespaceHandler 的实现类实例化后执行一些初始化操作，Spring中例如MVC的标签的NameSpaceHandler 的实例类
org.springframework.web.servlet.config.MvcNamespaceHandler,用来注册对应标签的解析器，
```
registerBeanDefinitionParser("annotation-driven", new AnnotationDrivenBeanDefinitionParser());
		registerBeanDefinitionParser("default-servlet-handler", new DefaultServletHandlerBeanDefinitionParser());
		registerBeanDefinitionParser("interceptors", new InterceptorsBeanDefinitionParser());
		registerBeanDefinitionParser("resources", new ResourcesBeanDefinitionParser());
		registerBeanDefinitionParser("view-controller", new ViewControllerBeanDefinitionParser());
		registerBeanDefinitionParser("redirect-view-controller", new ViewControllerBeanDefinitionParser());
		registerBeanDefinitionParser("status-controller", new ViewControllerBeanDefinitionParser());
		registerBeanDefinitionParser("view-resolvers", new ViewResolversBeanDefinitionParser());
		registerBeanDefinitionParser("tiles-configurer", new TilesConfigurerBeanDefinitionParser());
		registerBeanDefinitionParser("freemarker-configurer", new FreeMarkerConfigurerBeanDefinitionParser());
		registerBeanDefinitionParser("velocity-configurer", new VelocityConfigurerBeanDefinitionParser());
		registerBeanDefinitionParser("groovy-configurer", new GroovyMarkupConfigurerBeanDefinitionParser());
		registerBeanDefinitionParser("script-template-configurer", new ScriptTemplateConfigurerBeanDefinitionParser());
		registerBeanDefinitionParser("cors", new CorsBeanDefinitionParser());
```
**parse(Element element, ParserContext parserContext)**  则是解析标签所使用的,
**decorate(Node source, BeanDefinitionHolder definition, ParserContext parserContext)**  和parse方法类似，不过是用来
获取BeanDefinetion的装饰类实例


#### NamespaceHandlerSupport 为我们实现一个标准的解析器，提供了一个标准的支持，一般NamespaceHandler实例继承自此接口。
NamespaceHandlerSupport 主要提供了提供三个容器，用来保存解析器和NameSpace的对应关系，而parse则是通过容器查找Parser




