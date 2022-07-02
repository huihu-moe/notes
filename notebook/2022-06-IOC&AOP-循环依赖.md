# IOC&AOP 循环依赖

```java
protected Object getSingleton(String beanName, boolean allowEarlyReference) {
  // 从 singletonObjects 获取实例，singletonObjects 是成品 bean
	Object singletonObject = this.singletonObjects.get(beanName);
	// 判断 beanName ，isSingletonCurrentlyInCreation 对应的 bean 是否正在创建中
	if (singletonObject == null && isSingletonCurrentlyInCreation(beanName)) {
		synchronized (this.singletonObjects) {
		  // 从 earlySingletonObjects 中获取提前曝光未成品的 bean
			singletonObject = this.earlySingletonObjects.get(beanName);
			if (singletonObject == null && allowEarlyReference) {
			  // 获取相应的 bean 工厂
				ObjectFactory<?> singletonFactory = this.singletonFactories.get(beanName);
				if (singletonFactory != null) {
				  // 提前曝光 bean 实例，主要用于解决AOP循环依赖
					singletonObject = singletonFactory.getObject();
					
					// 将 singletonObject 放入缓存中，并将 singletonFactory 从缓存中移除
					this.earlySingletonObjects.put(beanName, singletonObject);
					this.singletonFactories.remove(beanName);
				}
			}
		}
	}
	return (singletonObject != NULL_OBJECT ? singletonObject : null);
}
```