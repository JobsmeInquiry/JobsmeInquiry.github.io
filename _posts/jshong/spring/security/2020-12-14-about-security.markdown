---
layout: post
title: Spring Security - 인증, 인가
date: 2020-12-14 11:10:00 +0900
tags: spring, security
categories: jshong
---

#### Spring Security - 인증

- Spring Security란?
  - 어플리케이션에 보안 기능을 구현할 때 사용되는 프레임워크
  

- 보안 관련 용어 정의
  - 접근 주체(Principal) : 보호된 대상에 접근하는 유저(ex. 클라이언트)
  - 인증(Authenticate) : 현재 유저가 누구인지 확인(ex. 로그인)
  - 인가(Authorize) : 현재 유저가 어떤 서비스, 페이지에 접근할 수 있는 권한이 있는지 검사
  - 권한 : 인증된 주체가 애플리케이션의 동작을 수행할 수 있도록 허락되었는지  결정
    - 권한 승인이 필요한 부분으로 접근하려면 인증 과정을 통해 주체가 증명 되어야만 한다

- SecurityFilterChain
  - 브라우저가 서버에 데이터를 요청하면 DispatcherServlet에 전달되기 이전에 여러 ServletFilter를 거친다.
  - 이때 Spring Security에서 등록했던 Filter를 이용해 사용자 보안 관련된 처리를 진행한다.
  - Spring Security와 관련된 Filter들은 연결된 여러 Filter들로 구성되어있다. 이 때문에 Chain이라는 표현을 사용

  ![]({{site.baseurl}}/images/jshong/spring/security/filterchain.png)


- Filter 역할

![]({{site.baseurl}}/images/jshong/spring/security/filter_des.png)

- Spring Security 인증 처리

![]({{site.baseurl}}/images/jshong/spring/security/authprocess.png)

1. AuthenticationFilter (UsernamePasswordAuthenticationFilter)는 사용자의 요청을 가로챈다. 그리고 인증이 필요한 요청이라면 사용자의JSESSIONID가 Security Context에 있는지 판단한다. 없으면 로그인 페이지로 이동시킨다
로그인 페이지에서 요청이 온 경우라면 로그인 페이지에서 입력받은 username과 password를 이용해 UsernamePasswordAuthenticationToken을 만든다. 그리고 UsernamePasswordAuthenticationToken 정보가 유효한 계정인지 판단하기 위해 AuthenticationManager로 전달한다.

2.AuthenticationManager 인터페이스의 구현체는 ProviderManger이고 AuthencationProvider에게 비밀번호 인증 로직 책임을 넘긴다. (AuthencationProvider는 개발자가 직접 커스텀해서 비밀번호 인증로직을 직접 구현할 수 있다.)

3. AuthencationProvider는 UserDetailsService를 실행해 비밀번호 인증 로직을 처리한다.
UserDetailsService는 DB에 저장된 회원의 비밀번호와 비교해 일치하면 UserDetails 인터페이스를 구현한 객체를 반환하는데, UserDetailsService는 인터페이스이며 UserDetailsService를 구현한 서비스를 직접 개발해야한다.

4. 인증 로직이 완료되면 AuthenticationManager는 Authentication를 반환하며, 결과적으로 SecurityContext에 사용자 인증 정보가 저장된다.

5. 인증 과정이 끝났으면 AuthenticationFilter에게 인증에 대한 성공 유무를 전달하고성공하면 AuthenticationSuccessHandler를 호출하고 실패하면 AuthenticationFailureHandler를 호출한다.

