#### SPRNG 源码解读 （一）
###### SPRING-MVC源码阅读记录和理解
> 在阅读源码前,我们先理解SPRING-MVC中几个关键概念:
- multipartResolver    文件解析器
- localeResolver        语言解析器，环境解析器
- themeResolver         主题解析器
- handlerExceptionResolver   异常处理器
- viewNameTranslator        请求解析视图器
> 了解完几个概念之后,我们再了解几个接口: 
- org.springframework.web.servlet.HandlerAdapter       真正的处理业务逻辑的接口
- org.springframework.web.servlet.HandlerMapping     HandlerExecuteChain 执行链映射接口
- org.springframework.web.servlet.ViewResolver     视图解析接口
- org.springframework.web.servlet.View         视图接口
> 再了解几个关键类:
- org.springframework.web.servlet.DispatcherServlet   请求分发
- org.springframework.web.context.ContextLoaderListener  SPRINGContext上下文加载监听器
> 通过前面的了解后，我们再回过头来看原理这里有一幅SPRING-MVC的官方原理图:
![原理图](https://www.btsearch.site/img/5/501.png)**透过这幅图，我们可以对SPRING有个大概的轮廓:**
1. 用户在浏览器发起请求
2. 请求到达服务器，并到达Spring的前端控制器
3. 前端控制器经过转化找到对应的处理器
4. 处理器返回modelandview也就是视图和业务数据
5. 前端控制器通过viewresourver也就是视图解析器找到对应的view(视图)
6. 视图处理好数据后返回给浏览器，用户看到页面,一次请求就完成了
**用SPRING的处理方式去思考也就是:**
1. 请求到达DispatcherServlet（前端控制器）
2. DispatcherServlet 通过HandlerMapping（处理器映射表）找到HandlerAdapter（处理器，也就是真正做业务逻辑的地方）
3. HandlerAdapter处理完成后,会返回ModelandView也就是视图和业务数据,回到DispatcherServlet
4. DispatcherServlet会翻译视图教给ViewResourver去解析出VIEW(视图)
5. View(视图)处理数据格式和添加一些必须的响应头(Header),返回给浏览器
到这里就对SPRING有了大概的了解了.