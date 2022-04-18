* @Before, 前置通知，目标方法之前执行
* @After，后置通知，目标方法之后执行
* @AfterReturning，返回后通知，执行方法结束前执行
* @AfterThrowing，异常通知，出现异常时候执行
* @Around，环绕通知，环绕目标方法执行

spring4：前置 - 后置 - 正常返回/方法异常

spring5：环绕-前置-返回后通知/方法异常-后置-环绕





### 循环依赖

构造器注入方法不支持循环依赖

原型不支持循环依赖

![image-20220210234912568](C:\Users\46305\AppData\Roaming\Typora\typora-user-images\image-20220210234912568.png)

![image-20220210235013066](C:\Users\46305\AppData\Roaming\Typora\typora-user-images\image-20220210235013066.png)

![image-20220210235721801](C:\Users\46305\AppData\Roaming\Typora\typora-user-images\image-20220210235721801.png)