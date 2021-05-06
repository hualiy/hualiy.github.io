---
layout: post
title: 使用spring security 登录获取登录对象,密码也显示返回
date: 2021-05-06
categories: [spring]
tags: [spring security]
excerpt: 登录返回的登录对象返回密码返回不会null
---





一种的是在 实现了 UserDetails的类中加入@JsonIgnore

```java
/**实现类对象变成字符串的时候会忽略这个字段,另一个方面json字符串想要生成实现类对象
 *的话也会把这个字*段忽略掉,但有可能这个时候不想忽略它
 */
@JsonIgnore
public String getPassword() {
   return password;
}
```

另一种是在继承了WebSecurityConfigurerAdapter的类中

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http.  
    ......
         .successHandler(new AuthenticationSuccessHandler() {
              @Override
              public void onAuthenticationSuccess(HttpServletRequest req, 
              HttpServletResponse resp,Authentication auth) 
              throws IOException, ServletException {
                  ......
                  Xx xx = (Xx) auth.getPrincipal().setPassword(null);
                  ......
               }
 })
```

