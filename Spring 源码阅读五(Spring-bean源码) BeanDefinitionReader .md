## Spring 源码阅读(Spring-bean源码)
#### BeanDefinitionReader 源码理解
> 使用过spring的同学都知道，spring最重要的就是IOC容器,和AOP,其中IOC更是Spring的基础和重点，但是任何框架都是一点点在基础上搭建起来的。SPring也不例外.**BeanDefinitionReader**:是一个接口,为什么要从它说起呢?Spring的容器依赖于XML配置，所以构建容器的基础是构建在解析XML功能的基础上的,BeanDefinitionReader就是这个基础。BeanDefinitionReader只是一个标准的bean definition读取器接口,他提供了几个标准方法:
1. BeanDefinitionRegistry getRegistry();
2. ResourceLoader getResourceLoader();
3. ClassLoader getBeanClassLoader();
4. BeanNameGenerator getBeanNameGenerator();
5. int loadBeanDefinitions(Resource resource)loadBeanDefinitions有多个重载方法，但是功能都是一样，用于加载bean的定义
> 首先是getRegistry用于获取一个bean注册的容器类,BeanDefinitionRegistry在这里也是一个接口，具体到类就要提到DefaultListableBeanFactory，这里不做介绍，后面介绍，有了容器类,那么解析的bean信息就可以存储了,BeanDefinitionRegistry定义了基础的注册bean，移除bean，获取bean数量等基础方法
> getResourceLoader 这里用于获取资源加载器,这里不做详细介绍
> getBeanClassLoader 这里是获取用来加载bean的classloader
> getBeanNameGenerator 顾名思义,这里就是用来获取bean名称生成器，当配置bean是没有指定ID或name时，spring则使用默认的 BeanNameGenerator去生成一个名字作为标识符
> loadBeanDefinitions 有多个重载方法,主要体现在参数的大同小异，这里的参数是一个Resource类型的参数,阅读源码可以发现该类继承自**InputStreamSource**只是在操作资源做了进一步的封装.方便读取XML**BeanDefinitionReader**中最重要的方法应该就是loadBeanDefinitions，通过该方法去加载BeanDefinition
## **说了这么多，也许都不是很明白，通过源码才能体会到其中的奥妙**说到这里，就要具体看一下他的实现类**AbstractBeanDefinitionReader**和**XmlBeanDefinitionReader**
```
public abstract class AbstractBeanDefinitionReader implements EnvironmentCapable, BeanDefinitionReader {
	protected final Log logger = LogFactory.getLog(getClass());
	private final BeanDefinitionRegistry registry;
	private ResourceLoader resourceLoader;
	private ClassLoader beanClassLoader;
	private Environment environment;
	private BeanNameGenerator beanNameGenerator = new DefaultBeanNameGenerator();
```
**AbstractBeanDefinitionReader** 一个抽象类，它提供一些公共属性和方法，例如BeanNameGenerator，这里就给出了默认实现类DefaultBeanNameGenerator，有兴趣的小伙伴可以研读一下源码，这里不做详细说明。
在AbstractBeanDefinitionReader中，重写了loadBeanDefinitions的三个方法，但是峰回路转,
```
public int loadBeanDefinitions(String location, Set<Resource> actualResources) throws BeanDefinitionStoreException {}
```
最终会调用此方法,由于是面向继承的,此方法又回调了
```
int loadBeanDefinitions(Resource resource) throws BeanDefinitionStoreException;
```
此方法，这里没有做实现,这里就不得不说到**XmlBeanDefinitionReader**，前面都是公共属性和方法抽象化，关键方法一般留给不同类型的子类做不同的实现，由于Spring配置是基于XML配置的，所有就有了**XmlBeanDefinitionReader**这个类
```
/**
	 * Load bean definitions from the specified XML file.
	 * @param resource the resource descriptor for the XML file
	 * @return the number of bean definitions found
	 * @throws BeanDefinitionStoreException in case of loading or parsing errors
	 */
	@Override
	public int loadBeanDefinitions(Resource resource) throws BeanDefinitionStoreException {
		return loadBeanDefinitions(new EncodedResource(resource));
	}

	/**
	 * Load bean definitions from the specified XML file.
	 * @param encodedResource the resource descriptor for the XML file,
	 * allowing to specify an encoding to use for parsing the file
	 * @return the number of bean definitions found
	 * @throws BeanDefinitionStoreException in case of loading or parsing errors
	 */
	public int loadBeanDefinitions(EncodedResource encodedResource) throws BeanDefinitionStoreException {
		Assert.notNull(encodedResource, "EncodedResource must not be null");
		if (logger.isInfoEnabled()) {
			logger.info("Loading XML bean definitions from " + encodedResource.getResource());
		}

		Set<EncodedResource> currentResources = this.resourcesCurrentlyBeingLoaded.get();
		if (currentResources == null) {
			currentResources = new HashSet<EncodedResource>(4);
			this.resourcesCurrentlyBeingLoaded.set(currentResources);
		}
		if (!currentResources.add(encodedResource)) {
			throw new BeanDefinitionStoreException(
					"Detected cyclic loading of " + encodedResource + " - check your import definitions!");
		}
		try {
			InputStream inputStream = encodedResource.getResource().getInputStream();
			try {
				InputSource inputSource = new InputSource(inputStream);
				if (encodedResource.getEncoding() != null) {
					inputSource.setEncoding(encodedResource.getEncoding());
				}
				return doLoadBeanDefinitions(inputSource, encodedResource.getResource());
			}
			finally {
				inputStream.close();
			}
		}
		catch (IOException ex) {
			throw new BeanDefinitionStoreException(
					"IOException parsing XML document from " + encodedResource.getResource(), ex);
		}
		finally {
			currentResources.remove(encodedResource);
			if (currentResources.isEmpty()) {
				this.resourcesCurrentlyBeingLoaded.remove();
			}
		}
	}
```
仔细查看着两个方法的源码，最终传入的参数的是EncodedResource的类型，（EncodedResource主要是为了编码不一致使用的，这里就用了经典的**装饰者模式**,以达到增强功能的作用，不同于Resource的区别就在于编码设置）看到最后我们看到了**doLoadBeanDefinitions**方法，doLoadBeanDefinitions方法做了两件事情：
1. 就是把从EncodedResource中获取的InputSource转化为Document，这里用到JAXP来解析XML。
2. 然后把Document作为参数，继续解析**说明:**spring设计的经典就在于充分的说明了一个类只做一件事的单一职责模式的好处,这里解析XML，是委托给**DocumentLoader**去解析,具体实现类是DefaultDocumentLoader中做了实现,而解析Document又委托给了BeanDefinitionDocumentReader去解析，逻辑非常清晰，同样的实现类DefaultBeanDefinitionDocumentReader中做了实现。
> 说了这么多，却才说了一点，这让我感觉spring考虑了许多事情,这其中还有许多细节没有说到，让我感觉到spring的庞大，也看到了这么优秀的设计,细节下一篇中继续说明
