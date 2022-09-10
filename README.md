## AOP: Ordering Aspects

**Problem**
+ How to control the order of advices being applied?

<img src="![img.png](img.png)" width=500 />


**To Control Order**
+ **Refactor:** Place advices in separate Aspects
+ Control order on Aspects using the @Order annotation
+ Guarantees order of when Aspects are applied

**Development Process**
1. Refactor: Place advices in separate Aspects
2. Add @Order annotation to Aspects

_Step 1: Refactor:Place advices in separate Aspects_

<img src="https://user-images.githubusercontent.com/80107049/189486849-a2202e40-f0ac-4aa2-a228-ebf0814fe74b.png" width=500 />

_Step 2: Add @Order annotation_
+ Control order on Aspects using the @Order annotation

```JAVA
@Order(1)
public class MyCloudLogAspect {
 ... 
}
```

+ Guarantees order of when Aspects are applied
+ Lower numbers have higher precedence

**@Order**
+ We want the following order:
    1. MyCloudLogAspect
    2. MyLoggingDemoAspect
    3. MyApiAnalyticsAspect

<img src="https://user-images.githubusercontent.com/80107049/189486832-448604a0-06fa-4c88-8f7d-baeb077e3fdd.png" width=500 />


```Java
@Aspect
@Component
@Order(1)
public class MyCloudLogAspect {
  ...
}
```

```Java
@Aspect
@Component
@Order(2)
public class MyLoggingDemoAspect {
  ...
}
```

```Java
@Aspect
@Component
@Order(3)
public class MyApiAnalyticsAspect {
  ...
}
```

+ Lower number have higher precedence
    + Range: Integer.MIN_VALUE to Integer.MAX_VALUE
    + Negative numbers are allowed
    + Does not have to be consecutive

<img src="https://user-images.githubusercontent.com/80107049/189486814-2408e803-3bf0-46a5-b94b-2916ea3b8a81.png" width= 500 />


+ Example with negative numbers

<img src="https://user-images.githubusercontent.com/80107049/189486796-bde5e36c-acb5-46bd-b6b8-82efdc0974c9.png" width=500 />


+ FAQ: What if aspects have the exact same @Order annotation?

```JAVA
@Order(1)
public class MyCloudLogAspect {...}

@Order(6)
public class MyShowAspect {...}

@Order(6)
public class MyFunnyAspect {...}

@Order(6)
public class MyLoggingDemoAspect {...}
```
+ The both **@Order(6)** at this point is undefined
+ Will still run AFTER MyCloudLogAspect and BEFORE MyLogingDemoAspect

