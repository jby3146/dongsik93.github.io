---
layout: post
title: "서버 인증 (1) (Session/Cookie)"
subtitle: "Session / Cookie Authorization"
date: 2020-01-08 19:00:00 +0900
categories: til
tags: session cookie authorization
comments: true
---

# 서버 인증 (1) (Session/Cookie)

>
> 토큰을 이용한 인증만 활용해봤었는데 다른 인증 방법들은 어떤것이 있고, 각각의 장단점을 알아보도록 합시다



## Session/Cookie 인증

### 왜 사용하는가?

- HTTP 프로토콜의 Stateless, Connectionless를 보완하기 위해서 사용
  - `Stateless`이란 이전의 데이터 요청과 다음 데이터 요청이 서로 관련이 없다라는 뜻
  - `Connectionless`이란 request를 하고 response을 보내준 뒤에는 접속을 끊는다는 뜻
- 모든 사용자의 요청마다 연결과 해제의 과정을 거치면서 연결상태를 유지하지 않고 연결해제 후에도 상태 정보를 저장하지 않기 때문에 서버의 자원을 절약할 수 있지만, 사용자를 식별할 수 없어서 요청 때마다 매번 새로운 사용자로 인식한다

### 인증 과정

![auth-1](/img/in-post/auth-1.png)

1. 사용자가 로그인을 한다
2. 서버측에서는 로그인 정보를 통해 사용자를 확인한다.
3. 서버측에서 세션 저장소에 사용자 세션 생성을 요청한다.
4. 사용자의 고유한 ID값을 부여하여 세션 저장소에 저장하고, 이와 연결되는 세션 ID를 발행한다.
5. 사용자는 서버에서 해당 세션ID를 받아 쿠키에 저장한다
6. 인증이 필요한 요청을 할 때 헤더에 쿠키를 실어 요청한다
7. 서버에서는 쿠키를 받아 세션 저장소에 대조를 한다
8. 인증 후 요청된 정보를 가져온다
9. 인증이 완료되고 서버는 사용자에게 맞는 데이터를 보내준다

### Cookie(쿠키)란?

- 쿠키는 서버가 사용자의 웹 브라우저에 저장하는 데이터
- 브라우저마다 저장되는 쿠키는 다르다
  - 서버에서는 브라우저가 다르면 다른 사용자로 인식하기 때문
- `Session Cookie`
  - 웹 브라우저가 종료될 때 제거되는 쿠키
- `Permanent Cookie`
  - 브라우저가 종료되더라도 유지되는 쿠키
- 이외에도 쿠키를 제어하기 위해 Secure, HttpOnly, Domain, Path등의 옵션들이 있다

- 쿠키를 이용해서 ID저장, 로그인 상태를 유지할 수 있고 쇼핑몰 장바구니 기능, 최근 검색한 상품들을 광고에서 추천하는 등의 기능을 사용할 수 있다

### Session(세션)이란?

- 일정시간동안 같은 웹 브라우저로부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 일정하게 유지시키는 기술이다
  - 일정시간이라 함은 사용자가 웹 브라우저를 통해 웹 서버에 접속한 시점으로부터 웹 브라우저를 종료함으로써 연결을 끝내는 시점을 말한다
- 세션은 비밀번호와 같은 인증정보를 쿠키에 저장하지 않고 사용자의 식별자인 `session id`를 저장한다

### Session/Cookie 인증의 장점과 단점

#### 장점

- 쿠키가 담긴 HTTP요청이 도중에 노출되더라고 쿠키 자체(세션 ID)는 유의미한 값을 갖고 있지 않아 계정정보를 직점 담아서 인증을 거치는 방법보다는 안전하다
- 고유의 ID값으로 접근하기 때문에 서버의 자원에 접근하기가 용이하다

#### 단점

- 세션 하이재킹 공격을 당할 우려가 있다
  - HTTPS를 사용하거나 세션에 유효기간을 넣어서 해결
- 세션 저장소라는 추가적인 저장공간을 사용하기 때문에 부하가 높아지게 된다

### Session과 Cookie의 차이점

- 쿠키는 사용자의 정보를 **_사용자 컴퓨터 메모리_**에 저장한다
- 세션은 사용자의 요청에 따른 정보를 사용자 메모리에 저장하는 것이 아닌 웹 서버가 세션ID 파일을 만들어 서비스가 돌아가고 있는 **_서버에 저장_**한다

<br>

참고사이트

- [쿠키(Cookie)와 세션(Session) & 로그인 동작 방법](https://cjh5414.github.io/cookie-and-session/){: class="underlineFill"} 
- [쉽게 알아보는 서버 인증 1편(세션/쿠키, JWT)](https://tansfil.tistory.com/58){: class="underlineFill"} 

- [Session 이란?](https://88240.tistory.com/190){: class="underlineFill"} 