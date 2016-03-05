---
layout: post
title: 如何实现spring环境下的自定义annotation
category: 技术
tags: spring,annotation
description: 实现spring环境下的自定义annotation
---
## 背景
  spring的注解使用为大家的开发带来了极大的方便，那么如何在spring架构中实现自定义的annotation相信大家都带有这样的疑问。笔者也在项目中
遇到一样的情况。我们在针对APP的后台开发时，有时会出现需要登录才能进行相关操作，而其他的页面则不需要登录。通常比较笨的方法就是在每个接口
写上相关登录的校验。这样做无疑增加了开发量和维护量，如果有一个统一的方式来进行管理而且不需要进行大量的开发来解决这种情况就好了？
相信大家第一时间想到的方法就是自定义的annotation。那么spring环境下如何实现自定义的annotation呢？
          *注意中的内容仅针对@Target(ElementType.METHOD)*
  
## 实现
### 思考？
 实现annotation，必须先捕获annotation。需要捕获annotation必须先获取具体的方法对象及Method，获取Method？反射？！！！！
 然后Spring中的反射？针对特定的场景？AOP？？！！！！
 
### annotation定义   

```java
 @Target(ElementType.TYPE.METHOD)
 @Retention(RetentionPolicy.RUNTIME)
 @Documented
 public @interface LoginRequired {
     public boolean value() default true;
 }
```
 
### 利用AOP实现自定义annotation

```java
public class LoginRequiredInterceptor extends HandlerInterceptorAdapter {


    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        HandlerMethod handlerMethod = (HandlerMethod)handler;
        LoginRequired annotation = handlerMethod.getMethodAnnotation(LoginRequired.class);
        if(annotation == null){//无登录需求
            return true;
        }
        Boolean val = annotation.value();
        if(!val){//不需要进行登录
            return true;
        }else {
            //进行相关的登录校验。。。
            if("已登录"){
                return true;
            }else{
                response.sendRedirect("登录页面");//或者返回其他信息
                return false;
            }
        }
    }
```

```xml 
<mvc:interceptor>
   <mvc:mapping path="/**"/>
   <bean class="xxx.xxx.xxx.LoginRequiredInterceptor"/>
</mvc:interceptor> 
```
    
## 最后
  当然除了上述方法外，根据不同的业务场景，还有其他的实现方式。
  针对普通的需求，可以使用反射实现；针对 *@Target(ElementType.PARAMETER)* 可以借助于 **spring的HandlerMethodArgumentResolver** 接口


