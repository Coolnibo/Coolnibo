

Spring事务注解
是通过aop切面来做到的，在方法执行前开启一个事务，如果没有报错则提交，如果有报错则回滚

```java
@Transactional()
```
