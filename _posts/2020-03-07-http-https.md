---
layout: post
title: 'HTTP와 HTTPS의 차이는 무엇일까?'
categories:
    - HTTP&HTTPS
excerpt: ' '
comments: true
share: true
tags:
    - Http
    - Https
date: 2020-03-07T21:58:00-0:05:00
---

# About

홈페이지 사이트를 입력할 때 맨 앞에 자주보이는 단어가 있다. <br/>
바로 http인데요. 어쩔 때보면 https로 표현되는데 과연 http가 무엇이며 https와의 차이점이 무엇인지에 대해 알아보자.<br/><br/>

# HTTP(HyperTest Transfer Protocol)

HTTP는 HyperTest Transfer Protocol의 약자로 인터넷에서 웹 서버와 사용자의 인터넷 웹 브라우저 사이에 문서를 전송하기 위한 통신규약(protocol)이다. 즉, HTTP는 인터넷에서 하이퍼텍스트(HyperText)를 전송하기 위해 사용되는 통신규약(protocol)이다. 또한 포트번호는 80번을 사용하고 있다.<br/><br/>
HTTP는 정보를 텍스트로 주고 받는다. 단순 텍스트를 주고 받기 때문에 네트워크에서 전송 신호를 인터셉트하는 경우 원하지 않는 데이터 유출이 발생할 수도 있다.<br/>
그래서 HTTP의 취약한 보안성을 강화하기 위해 HTTPS가 나왔다.<br/><br/>

# HTTPS(HyperText Transfer Protocol)

HTTPS는 간단하게 HTTP에 '보안(S)'을 추가한 것이다. HTTPS는 SSL/TLS를 통해 암호화하여 컴퓨터 네트워크를 통한 통신을 보안하도록 설계된 웹 통신 프로토콜이다. 포트번호는 443번을 사용하고 있으며 HyperText Transfer Protocol over Secure Socket Layer, HTTP over TLS, HTTP over SSL, HTTP Secure 등으로 불리고 있다.<br/><br/>
HTTPS는 기본 골격이나 사용목적 등은 HTTP와 거의 동일하지만, 데이터를 주고 받는 과정에서 보안이 추가된 것이 가장 큰 차이점이다. 즉, HTTPS를 사용하면 서버와 클리언트 사이의 모든 통신 내용이 암호화된다는 것이다.<br/><br/>

# HTTP와 HTTPS의 차이점 정리

-   URL​<br/>
    HTTP는 http://로 시작하며, HTTPS는 https://로 시작한다.<br/><br/>

-   보안<br/>
    HTTP : 암호화가 전혀 되어 있는 않은 프로토콜로 보안성이 매우 떨어짐<br/>
    HTTPS : 암호화하여 정보를 전송하기 때문에 보안성이 강조됨<br/><br/>

-   속도<br/>
    암호화과정으로 인해 HTTP보다 HTTPS의 속도가 더 느리지만 아주 미세한 속도차이므로 차이를 별로 느낄 수 없음

​
