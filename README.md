# AOP面向切面编程
- 通知（advice）
- 切点（pointcut）
- 连接点（joinpoint）


##### 通知 Advice
```java
在AOP术语中，切面的工作被称之为通知。通知定义了切面是什么以及何时使用。除了描述切面要完成的工作，
通知还解决了何时执行这个工作的问题。
```
###### 通知类型
```
Before         （前置通知）：在目标方法被调用之前调用通知功能
After          （后置通知）：在目标方法完成之后调用通知，此时不会关心方法的输出是什么
After-returning（返回通知）：在目标方法成功执行之后调用通知
After-throwing （异常通知）：在目标方法抛出异常后调用通知
Around         （环绕通知）：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为
```

##### 切点 Pointcut
```
如果说通知定义了切面‘何时做’和‘做什么’，那么切点就定义了‘何处’。切点的定义会匹配通知所要织入的一个
或多个连接点。我们通常使用注解或者正则表达式以及全类名来告诉切点在何处执行
```

##### 连接点 Join point
```
连接点是在应用执行过程中能够插入的一个点，这个点可以是调用方法时，抛出异常时...。切面可以利用这些点插入到
正常的流程之中，并添加新的行为。
```

##### 切面 Aspect
```
切面是通知和切点的结合。通知和切点共同定义了切面的全部内容，它是什么、在何时何处去做什么。
```

##### 引入 Introduction
```
引入允许我们向现有类添加新的方法和属性
```

##### 织入 Weaving
```
织入是把切面应用到目标对象并创建新的代理对象的过程。切面在指定的连接点被织入到目标对象中，
在目标对象的生命周期里有多个点可以进行织入

编译期：切面在目标类编译时被织入。这种方式需要特殊的编译器。Aspect的织入编译器就是以这种方式织入的。

类加载期：切面在目标类加载到JVM时被织入，这种方式需要特殊的类加载器，它可以在目标类被引入应用之前增强该
        目标类的字节码。
        
运行期：切面在运行的某个时刻被织入，一般情况下，在织入切面时，AOP容器会为目标动态地创建一个代理对象，Spring AOP就是这种方式

```

##### Spring支持的 AOP

```
基于代理的Spring AOP   [比较笨重，使用ProxyFactoryBean]
纯POJO切面
@AspectJ注解驱动的切面  [本人更倾向于使用注解]
注入式AspectJ切面      [适用于Spring各个版本]
```

### 小试牛刀

```java
表达式

execution 在执行方法时触发
* 表示返回任意类型
com.example.TestClass 方法所属类
say  方法
(..) 使用任意参数，无参数限定

execution(* com.example.TestClass.say(..))

如果我们想限制在某个包目录下可以如下操作
execution(* com.example.TestClass.say(..)&&within(com.*))

匹配特定的 Bean 此处 testBean 是 Bean id
execution(* com.example.TestClass.say()) and bean('testBean')

使用非操作限定特定 Id 以外的其它 Bean
execution(* com.example.TestClass.say()) and !bean('testBean')

```
```java

public interface WeakUp{

  void weakUp();

}



@Aspect
public class AspectConfig{

 @Before("execution(* com.example.WeakUp.weakUp(..))")
 public void openEye(){
   System.out.println("睁开眼睛");
 }

 @AfterReturning("execution(* com.example.WeakUp.weakUp(..))")
 public void washFace(){
  Sytem.out.println("洗脸");
 }

 @AfterThrowing("execution(* com.example.WeakUp.weakUp(..))")
 public void noWater(){
  System.out.println("没水了");
 }

}
```

> 多次定义表达式有点繁琐。我们可以通过pointcut来实现

```java
@Aspect
public class AspectConfig{

 @Pointcut("execution(* com.example.weakUp.weakUp(..))")
 public void point(){}


 @Before("point()")
 public void openEye(){
  System.out.println("睁开眼睛");
 }

 @AfterReturning("point()")
 public void washFace(){
  Sytem.out.println("洗脸");
 }

 @AfterThrowing("point()")
 public void noWater(){
  System.out.println("没水了");
 }
}
```
