# Spring的装配

Spring具有非常强大的灵活性，提供了三种主要的装配机制。

## 在XML中显式的配置
1. 创建需要装配的Bean

	```java
		public class Test {
	    	public Test() {
	        	System.out.println("Create Class MyTest");
	    	}
		}
	```
 

1. 配置xml文件

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
		<beans xmlns="http://www.springframework.org/schema/beans"
		       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    		<bean id="bean1" class="com.zl.Test"></bean>
		</beans>	
	```
2. 读取配置

	```java
		class SpringTest{
		    public static void main( String[] args )
		
		    {
		        ApplicationContext ctx = new ClassPathXmlApplicationContext("spring_bean.xml");
		        Test myTest = ctx.getBean("bean1",Test.class);
		    }
		}
	```
	
## 在Java中进行显式配置（使用注解）
1. 创建需要装配的Bean

	```java
		public class Test {
	    	public Test() {
	        	System.out.println("Create Class MyTest");
	    	}
		}
	```
 

1. 创建配置类

	```java
	@Configuration
	public class AppConfiguration {
	    @Bean
	    public Test bean1(){
	        return new Test();
	    }
	}
	
	```
2. 读取配置

	```java
	class SpringTest
    {

        public static void main( String[] args )

        {
            ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfiguration.class);
            Test myTest = ctx.getBean("bean1",Test.class);
        }
    }
	```
## 自动化装配Bean
自动装配bean是spring提供的最便利的一种方式，但<font color=red size=3 face="黑体">不一定是最好的</font>实现方式，具体还是要看使用场景。
spring从两个角度实现自动化装配：
	+ 组件扫描：Spring会自动发现应用上下文创建的Bean
	+ 自动装配：Spring自动满足Bean直接的依赖

	1. 使用@Component注解创建可被发现的Bean：

		```java
		@Component
		public class Test {
	    	public Test() {
	        	System.out.println("Create Class MyTest");
	    	}
	    	
	    	public void test(){
	    		System.out.println("test Method");
	    	}
		}
		```
	
	2. 使用@ComponentScan注解启动组件扫描：
   
	   ```java
	   	@Configuration
		@ComponentScan
		//这里ComponentScan未设置任何属性，会扫描配置类所在的包下面的类
		public class ComponentScanTest {
		}
	
	   ```
   
   4. 测试  
   
	   ```java
	   @RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration(classes = ComponentScanTest.class)
	public class SpringTest {
	    @Autowired
	    Test myTest;
	
	    @org.junit.Test
	    public void test() {
	       myTest.test();
	    }
	}
	```	
	
		<font color=red>注:步骤2中启动扫描组件也可以使用xml配置</font>
	
		```java
		<beans xmlns="http://www.springframework.org/schema/beans"
	       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	       xmlns:context="http://www.springframework.org/schema/context"
	       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
	    <context:component-scan base-package="com.spirngtest"></context:component-scan>
	</beans>
		
		```
 	3. 其他说明
 	   + 如果想扫描其他包的或者多个包，可以通过配置@ComponentScan注解
 	     1. @ComponentScan("packageName")或者@ComponentScan(basePackages="packageName")，
 	     2. 多个包下的@ComponentScan(basePackages={"packageName1","packageName2"})
 	     3. @ComponentScan(basePackages={Test1.class,Test2.class})
 	     4. 
 	     

     