# spring

#### 1、线程安全性

singleton	在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，bean作用域范围的默认值。
prototype	每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行newXxxBean()。
request	每次HTTP请求都会创建一个新的Bean，该作用域仅适用于web的Spring WebApplicationContext环境。
session	同一个HTTP Session共享一个Bean，不同Session使用不同的Bean。该作用域仅适用于web的Spring WebApplicationContext环境。
application	限定一个Bean的作用域为ServletContext的生命周期。该作用域仅适用于web的Spring WebApplicationContext环境。

Spring中的Bean默认是单例模式的，框架并没有对bean进行多线程的封装处理。
　　实际上大部分时间Bean是无状态的（比如Dao） 所以说在某种程度上来说Bean其实是安全的。
　　但是如果Bean是有状态的 那就需要开发人员自己来进行线程安全的保证，最简单的办法就是改变bean的作用域 把 "singleton"改为’‘protopyte’ 这样每次请求Bean就相当于是 new Bean() 这样就可以保证线程的安全了。

　　有状态就是有数据存储功能
　　无状态就是不会保存数据

#### 2、事务

@transaction  利用了aop,在方法上下加事务

##### 事务传播机制：

@Transaction(Propagation=XXX)

如果有别的方法调用这个开事务的方法，如何传播

| **事务传播行为类型**      | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| PROPAGATION_REQUIRED      | 如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。这是最常见的选择。 |
| PROPAGATION_SUPPORTS      | 支持当前事务，如果当前没有事务，就以非事务方式执行。         |
| PROPAGATION_MANDATORY     | 使用当前的事务，如果当前没有事务，就抛出异常。               |
| PROPAGATION_REQUIRES_NEW  | 新建事务，如果当前存在事务，把当前事务挂起。                 |
| PROPAGATION_NOT_SUPPORTED | 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。   |
| PROPAGATION_NEVER         | 以非事务方式执行，如果当前存在事务，则抛出异常。             |
| PROPAGATION_NESTED        | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。 |

比如说，我们现在有一段业务逻辑，方法A调用方法B，我希望的是如果说方法A出错了，此时仅仅回滚方法A，不能回滚方法B，必须得用REQUIRES_NEW，传播机制，让他们俩的事务是不同的

方法A调用方法B，如果出错，方法B只能回滚他自己，方法A可以带着方法日一起回滚，NESTED嵌套事务

### **Bean 的生命周期**





实例化后的对象被封装在BeanWrapper对象中，紧接着，Spring 根据BeanDefinition中的信息以及通过BeanWrapper提供的设置属性的接口完成依赖注入。



(1）实例化Bean :

1. Spring启动，查找并加载需要被Spring管理的bean，进行Bean的实例化 

2. Bean实例化后对将Bean的引入和值注入到Bean的属性中

   

3. 如果Bean实现了BeanNameAware接口的话，Spring将Bean的Id传递给setBeanName()方法

4. 如果Bean实现了BeanFactoryAware接口的话，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入

5. 如果Bean实现了ApplicationContextAware接口的话，Spring将调用Bean的setApplicationContext()方法，将bean所在应用上下文引用传入进来。

   

6. 如果Bean实现了BeanPostProcessor接口，Spring就将调用他们的postProcessBeforeInitialization()方法。

7. 如果Bean 实现了InitializingBean接口，Spring将调用他们的afterPropertiesSet()方法。类似的，如果bean使用init-method声明了初始化方法，该方法也会被调用

8. 如果Bean 实现了BeanPostProcessor接口，Spring就将调用他们的postProcessAfterInitialization()方法。

   

9. 此时，Bean已经准备就绪，可以被应用程序使用了。他们将一直驻留在应用上下文中，直到应用上下文被销毁。

10. 如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用了destory-method 声明销毁方法，该方法也会被调用。

### 设计模式

1、工厂模式

工厂模式, spring ioc核心的设计模式的思想提现,他自己就是一个大的工厂,把所有的bean实例都给放在了spring容器里(大工厂)，如果你要使用bean，就找spring 容器就可以了。

2、单例模式

双空检测

3、代理模式

如果说你要对一些类的方法切入一些增强的代码,会创建一些动态代理的对象，让你对那些目标对象的访问，先经过动态代理对象，动态代理对象先做一些增强的代码，调用你的目标对象

在设计模式里，就是一个代理模式的体现和运用，让动态代理的对象，去代理了你的目标对象，在这个过程中做一些增强的访问

### Spring-MVC

把dispacherServlet注册给tomcat,然后再转发请求给controller,再通过servlet转发给controller。

### Spring-Cloud





