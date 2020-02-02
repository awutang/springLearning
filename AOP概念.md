
##### AOP:Aspect Oriented Programming 面向切面编程
1. 作用：在程序运行期间，不修改源码对已有方法进行增强 --本质是是动态代理的作用
2. 术语
    1. 连接点
    2. 切入点：连接点中进行了增强的方法

##### Spring中的AOP通过配置的方式实现，之前所讲的生成代理类部分Spring已经帮我们做了。
* 配置：
  1. xml
      1. aop:config aop:aspect aop:before(method、pointcut)
      2. 切入点表达式的写法：包括修饰符、包名、类名、方法名、参数列表，可以用通配符 *.
          * 通常写法：* com.itheima.service.impl.*.*(..)
          * aspectjweaver jar包负责解析切入点表达式
          * aop:pointcut可以在aspect内部或外部（前面）
      3. 通知类型
          * 前置
          * 后置
          * 异常
          * 最终
          * 环绕通知：
              1. 问题：当配置了环绕通知后，切入点方法未执行但通知执行了
              2. 分析：对比动态代理的环绕通知，发现是有明确调用切入点方法的
              3. 解决：Spring提供了ProceedingJoinPoint接口，接口中有一个proceed(),即明确调用切入点方法。该接口可以作为环绕通知方法的参数，程序执行时Spring会传入实现类供通知方法使用
              4. 是Spring提供的一种可以控制增强语句是在切入点方法之前或之后的方式          
  2. 注解
      1. 需要导入xmlns:context的schema
      2. @Aspect：表示是一个切面类 @Before("pt()")：前置通知方法  @Pointcut:切面表达式
      3. @After等最终与异常、最终与后置，顺序是有问题的，但是@Around环绕通知由于是自己定义的切入点的调用，没有问题
      4. @EnableAspectJAutoProxy：开启切面 等价<aop:aspectJ-autoProcy></aop:aspectJ-autoProcy>
* Spring会根据目标类是否实现了接口从而决定使用何种代理
  * 实现了接口采用的代理是基于接口的动态代理--jdk动态代理
  * 未实现接口采用的是基于子类的动态代理，要求被代理类即目标类不能是最终类即没有被final修饰--cglib动态代理
  * 因此基于子类的动态代理也可以用于基于接口的，比如某个被代理类即实现了接口，又不是最终类，那么Spring就可以指定选用何种方式的代理
  
* 总结：转账需要实现事务-》抽取公共代码(beginTransaction，rollback,commit) -》利用代理将公共代码增强到目标代码 -》利用Spring AOP实现增强


  
