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

* ClassPathXMLApplicationContext
* SystemFileApplicationContext 
* AnnotationApplicationContext
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

###DispatcherServlet 工作原理
![](media/16456664485162.jpg)
1. 浏览器发送请求给Tomcat服务器，Tomcat引擎把请求封装，转发给DispatcherServlet处理
2. DispatcherServlet收到请求后，转发给`HandlerMapping`
3. HandlerMapping收到请求，生成HandlerExecutionChain（执行链），返回给DispatcherServlet
4. DispatcherServlet把HandlerExecutionChain转发给HandlerAdapter
5. HandlerAdapter负责根据HandlerExecutionChain来执行，使用处理器Handler来执行。
6. 处理器Handler处理，结果返回给HandlerAdapter，这里就是我们编写的@Controller逻辑
7. 根据Handler处理的结果，生成ModelAndView，返回给DispatcherServlet
8. DispatcherServlet转发ModelAndView给ViewResolver（视图解析器）
9. ViewResolver处理，返回View对象给DispatcherServlet
10. DispatcherServlet拿到view对象，进行渲染视图，得到jsp
11. 然后返回给浏览器

@Controller: 在指定的类，表明这里是web的控制器，在spring-context库里


@RequestMapping("/path"): 用来匹配请求路径。在函数上注解。在spring-web库里

#ModelAndView使用
在通过`modelAndView.addObject("key","value")`给request域设置入参的时候，
如果在jsp使用`el表达式`没效果的话，在%>前面加`isELIgnored="false"`

@RequestMapping("/path"): 
1. 用来匹配请求路径。在函数上注解。在spring-web库里
2. 也可以用在类上，表示路径前缀。（"/user/quick")
3. 入参
    1. value： 字符串，缺省值，表示路径
    2. params： 数组，要求入参一定要有指定入参
    3. method：枚举（RequestMethod.GET), 指定请求方法
    4. @RequestMapping(value="/path", method=ReqeustMethod.POST, params={"username", "age!20"}h
    5. 表示路径/path, 一定要用POST请求，入参一定要有username，age，age不能是20

```java
@RequestMapping("/user")
class controller {

@RequestMapping("/quick")
public void func1() {
    return "success.jsp" //如果前面不加/，则表示jsp在/user路径下
    return "/success.jsp" //前面加/, 则表示在类加载路径下的jsp
}

}

```

###ViewResolver 源码
在spring-mvc的源码库里。寻找DispatcherServlet.properties文件，里面有告知你哪个类是ViewResolver。
![](media/16456747858410.jpg)
找到父类
![](media/16456748473115.jpg)
默认的是foward前缀，可以换成redirect前缀
![](media/16456748957815.jpg)

还有这个set方法
![](media/16456749297415.jpg)
用来表示前缀和后缀
我们可以用spring注入的方式，给这个类成员变量进行赋值
![](media/16456750324822.jpg)
这样，你方法返回值就可以省略前缀和后缀了。

##RequestMapping使用
获取HttpServletRequest和HttpServletResponse都可以通过设置形参方式获取
```
@RequestMapping("/asd")
private void func(HttpServletRequest request, HttpServletResponse response)
```
request可以设置Request域的参数：`request.setAttribute(key, value)`
response可以设置回写数据:
`response.getWriter().print("jsonString")`

##@ResponseBody
用在方法上，表示return的字符串是用于数据回写

###对象转json字符串
导包
```
 <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.9.0</version>
    </dependency>
```
目标类不需要实现什么特殊接口
直接如下使用
```java
@RequestMapping("/quick5")
    @ResponseBody
    private String func5() throws JsonProcessingException {
        User user = new User();
        user.setUsername("lisi");
        user.setAge(123);
        ObjectMapper objectMapper = new ObjectMapper();
        String json = objectMapper.writeValueAsString(user);
        return json;
    }
```

###通过SpringMVC 框架，集成Jackson
通过注入，配置HandlerAdapter
![](media/16460180593880.jpg)
![](media/16460181256897.jpg)

```xml
<bean id="handlerAdapter" class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="messageConverters">
            <list>
                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
            </list>
        </property>
    </bean>
```

PS： org.springframework.http.converter.json.MappingJackson2HttpMessageConverter
这个类依赖jackson框架
![](media/16460183693363.jpg)

###\<mvc:annotation-driven/>
使用这个标签，springMVC就会自动加载RequestMappingHandlerMapping，RequestMappingHandlerAdapter。并且会自动集成Jackson框架到HandlerAdapter上。

PS：所以上面对HandlerAdapter配置的xml代码可以不用写

##Get请求获取入参
在RequestMapping方法里，直接声明同名的形参，即可获得相应的入参
http://localhost:8080/quick?username=asd
```java
@RequestMapping("/quick")
private void func1(String username){
 
}
```
###入参对象
或者直接传入一个对象，对象成员变量与入参同名
```java
private void func(User user){
}
```
###入参数组
需要请求地址http://localhost/quick?asd=123&asd=321&asd=231
```java
private void func(String[] asd){
}
```

###入参集合(List\<T>)
集合只能封装到对象里，作为成员变量。
```java
class VO {
    private List<User> userList
}

@RequestMapping("")
private void func(VO vo) {
}
```
还只能用POST，不能用GET=。=
![-w628](media/16460401063148.jpg)

在获取集合类型入参时，如果不想作为成员变量封装在对象里，则需要引用注解`@RequestBody`
```java
private void func(@RequestBody List<User> userList){ 
}
```
![-w877](media/16460421785684.jpg)
入参名不重要，主要是要数组