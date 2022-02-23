# TomcatServlet


#注解
@Scope("prototype|singleton")
@Component("beanId")
@Repository("beanId") //Dao层
@Service("beanId") //Service层
@Controller("beanId") //web层
@Autowired //自动注入，根据成员类型
@Qualifier(name="beanId") //指定beanId注入（需要跟Autowired一起用）
@Resources(name="beanId") //指定beanId注入（可单独使用，但需要导库：javax.annotation-api）
@POSTConstruction //在函数上用，用于指定bean的初始方法
@PreDestroy //指定bean的销毁方法

@Configuration //指定配置类
@ComponentScan("package") //指定扫描的包
@PropertySource("classpath:fileName") //指定加载的property
@Bean("beanId") //用于函数，返回的对象，作为容器中的bean
@Value("@{propertyKey}") //用于注入


#Spring框架
##spring-context
spring的主框架，主要是实现spring容器，配置`applicationContext.xml`

ClassPathXMLApplicationContext
SystemFileApplicationContext
AnnotationApplicationContext
##spring-web
是spring对web的封装
ContextLoaderListener，是spring对web的服务启动的监听器。只要做好web.xml相关配置，在web服务启动的时候，就会创建好spring容器
配置：指定applicationContext.xml
```xml
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>
```
##spring-webmvc
DispatcherServlet: 前端控制器
主要用于转发请求到指定的Servlet。需要给他配置spring容器，来让他知道他转发到哪个容器的哪个方法。

@Controller: 在指定的类，表明这里是web的控制器，在spring-context库里
@RequestMapping("/path"): 用来匹配请求路径。在函数上注解。在spring-web库里
