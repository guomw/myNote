# springMvc + thymeleaf模板引擎 配置

* 开发工具：IDEA
* 项目创建：使用maven，指定项目类型maven-archetype-webapp

### 一、springMvc 添加依赖

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.2.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>4.3.2.RELEASE</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>4.3.2.RELEASE</version>
        </dependency>
                
        <!-- https://mvnrepository.com/artifact/org.thymeleaf/thymeleaf-spring4 -->
        <!--thymeleaf 模板引擎插件-->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring4</artifactId>
            <version>3.0.1.RELEASE</version>
        </dependency>

### 二、springMvc 配置
   > 在web.xml里面配置DispatcherServlet

        <servlet>
            <servlet-name>mvc-dispatcher</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/mvc-dispatcher-servlet.xml</param-value>
            </init-param>
        </servlet>
        <servlet-mapping>
            <servlet-name>mvc-dispatcher</servlet-name>
            <url-pattern>/</url-pattern>
        </servlet-mapping>


### 三、springmvc + thymeleaf 模板引擎配置
    > 下面是整个bean文件

        <?xml version="1.0" encoding="UTF-8"?>
            <beans xmlns="http://www.springframework.org/schema/beans"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xmlns:context="http://www.springframework.org/schema/context"
                xmlns:mvc="http://www.springframework.org/schema/mvc"
                xsi:schemaLocation="http://www.springframework.org/schema/beans
                    http://www.springframework.org/schema/beans/spring-beans.xsd
                    http://www.springframework.org/schema/context
                    http://www.springframework.org/schema/context/spring-context.xsd
                    http://www.springframework.org/schema/mvc
                    http://www.springframework.org/schema/mvc/spring-mvc.xsd">

                <!--激活@Required @Autowired,JSR 250's @PostConstruct @PreDestroy and @Resource 等标注-->
                <context:annotation-config/>
                <mvc:resources mapping="/resources/**" location="/resources/"/>

                <!--DispatcherServlet上下文，只搜索@Controller标注的类，不搜索其他标注的类-->
                <context:component-scan base-package="com.guo.mvc">
                    <context:include-filter type="annotation"
                                            expression="org.springframework.stereotype.Controller"/>
                </context:component-scan>

                <mvc:annotation-driven/>

                <!--JSP + JSTL的视图配置-->
                <!--<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">-->
                    <!--<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>-->
                    <!--<property name="prefix" value="/WEB-INF/jsps/"/>-->
                    <!--<property name="suffix" value=".jsp"/>-->
                <!--</bean>-->

                <!--要实例化一个 org.thymeleaf.spring4.SpringTemplateEngine的模板引擎-->
                <bean  id="templateResolver" class="org.thymeleaf.templateresolver.ServletContextTemplateResolver">
                    <property name="prefix" value="/WEB-INF/template/"/>
                    <property name="suffix" value=".html"/>
                    <property name="templateMode" value="HTML5"/>
                </bean>
                <bean id="templateEngine"
                    class="org.thymeleaf.spring4.SpringTemplateEngine">
                    <property name="templateResolver" ref="templateResolver" />
                </bean>
                <bean class="org.thymeleaf.spring4.view.ThymeleafViewResolver">
                    <property name="templateEngine" ref="templateEngine" />
                </bean>

            </beans>