##### jdbcTemplate:spring对mysql JDBC（Java database connection）的封装
* Spring配置：

```java   
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- congigure JdbcTemplate-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--configure dataSource-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/eesy"></property>
        <property name="username" value="root"></property>
        <property name="password" value="HotteMYSQL"></property>
    </bean>
</beans>
```
* rowMapper:对返回结果集中实体字段的封装  spring提供BeanPropertyRowMapper
* Dao中两种注入jdbcTemplate方式：
    1. 采用Spring提供的JdbcDaoSupport，需要配置Dao类中的dataSource property
        * 若配置的是dataSource,则JdbcDaoSupport中的setJdbcTempate()会提示此方法未被使用（Method 'setDataSource(javax.sql.DataSource)' is never used）
        ，所以idea还可以检查配置从而确定Spring初始化时使用的是哪个set方法，牛--这应该是javac编译提示信息
    2. 采用注解注入，无需配置dataSource property
  
