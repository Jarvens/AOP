# AOP面向切面编程
- 通知（advice）
- 切点（pointcut）
- 连接点（joinpoint）


##### 通知
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
