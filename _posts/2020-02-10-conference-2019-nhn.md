---
title:  "2019 NHN Forward 경남/창원"
excerpt: "NHN에서 주최하는 컨퍼런스 in 경남/창원"
toc: true
toc_sticky: true
toc_label: "Index"

categories:
  - conference
tags:
  - conference
  - NHN
  - NHNFORWARD창원
  - NHNFORWARD경남
last_modified_at: 2020-02-10T09:17:00-0:10:00
---

> Small Steps make a Big Difference

종강하고 대구와 가까운 창원에서 컨퍼런스를 한다고하여 바로 참가했습니다!!

## 1. 참가확인
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/participate01.jpg){: .align-center}
성탄절 다음날이여서 아마 준비하시는 분들이 휴일날 고생하셨을 것 같네요ㅜㅜ<br/>
점심식사가 400분에게 무료로 제공된다고하여 아침에 불이 나게 달려갔지만! 창원이여서 그런가 총 인원이 400명 안되서 전부 제공받았습니다.<br/>
접수한 후 받은 팔찌
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/participate02.jpg){: .align-center}
인사말
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/participate03.jpg){: .align-center}
지방에서 컨퍼런스를 개최하는 건 처음이라고 하네요!! 이례적인 일이기는 하나 처음이라고해서 놀랐던...
<br/>

## 2. 세션
이 컨퍼런스가 열리기 약 한달 전에 서울에서도 컨퍼런스가 열렸었는데 경남/창원과 같은 트랙도 있지만 다른 것도 있다고 한다. 특히 스몰섹션부분이 다르다고 한다.
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/step01.jpg){: .align-center}
등록 후 인사말을 듣고 너무 배가 고파 아침을 먹고와서 키노트를 못 들어서 아쉬운 마음을 가지고 다음 섹션에 갔다.<br/>

### 2-1. HTTP API 설계, 후회, 고민
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/step02.jpg){: .align-center}
처음으로 내가 들은 트랙은 <HTTP API 설계, 후회, 고민>이였다.

* Dooray! 서비스의 HTTP API를 개발하면서 경험했던 일
- Dooray API는 협업 서비스이다. 
- <REST API 디자인 규칙>이라는 책을 참고.
- 책을 끝까지 읽지 않고(메타 데이터 디자인, 표현 디자인부분은 간과) 개발을 하여 아쉬움이 남는다.
- 아쉬운 점 및 고민하고 있는 부분 : <br/>
      ㄱ) (지금 생각해본다면) 업무와 프로젝트를 같은 레벨에 두었다면 어땠을까라는 생각을 한다.<br/>
      둘 중에 어떤 representation이 더 좋은 방법일까?<br/>
      현재 : 조직 - 프로젝트 - 업무<br/>
      if  : 조직 - 프로젝트 | 업무<br/>
      ㄴ) 임시저장기능<br/>
      현재 : 조직 - 프로젝트 - 업무 | 초안<br/>
      if  : 조직 - 프로젝트 | 초안 - 업무<br/>
      ㄷ) 파일첨부기능<br/>
      ㄹ) 업무목록에서 파일 첨부의 여부를 알고싶다<br/>
      ㅁ) 태그관리 <br/>
      ㅂ) 여러 업무에 한번에 태그 달기<br/>
      ㅅ) 업무를 다른 프로젝트로 옮기기(PUT ; 뭐가 바뀌었는 지 알 수 X, POST ; POST 사용)<br/>
      ㅇ) 메일 읽음 표시(JSPATCH, JSMERGEPATCH를 살펴보지 않은 것에 대한 아쉬움)<br/>
      ㅈ) HTTP cache 적용을 했으면 더 쉬웠을까?<br/>
      ㅊ) 다른 값으로부터 계산된 속성을 표현해야하는 경우<br/>
      등에 대해서 이야기했다.<br/>
- 교훈 : <br/>
  미리 잘 정해두면 좋은 것<br/>
  1) 리소스에 어울리는 단어들 - 비슷한 뜻을 가진 단어들이 혼용되는 것을 방지<br/>


    ```
    create, register

    update, edit, modify

    delete, remove

    use, enable, activate
    ```


   2) flag성 이름 - 규칙을 정하자

   ```
   _flag, _able

   is_, has_, can_ 
   ```

   3) 날짜, 날짜 시간 필드명
    ![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/img01.jpg){: .align-center}<br/><br/>

  ### cf. 점심시간
  ![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/step03.jpg){: .align-center}<br/>
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/lunch.jpg){: .align-center}<br/><br/>
갈비탕이였다!!! 진짜 맛있었다.<br/>
든든하게 한끼를 마치고 디저트로 과일도 나누어주었다.<br/>
힘을 내서 그 다음 내용을 들으러 고고!<br/>

### 2-2. NHN 베이스캠프 : 신입사원들은 무엇을 배우나요?
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/step04.jpg){: .align-center}<br/>
 사실 다른 내용은 들어도 잘 모를 것 같기도 하고 진짜 나에게 맞춤형 내용이지 않을까? 싶어서 이 트랙에 왔다.<br/>        
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/img02.jpg){: .align-center}<br/>
우선 제목에서 언급되어있는 베이스캠프란 높은 산에 오르는 준비를 말하는 것이다.<br/>
신입사원이 처음으로 회사에 와서 어떻게 일하는 지 등을 준비하는 과정을 빗대어 말했다.<br/>
(NHN 회사에서는 신입사원을 루키라고 부른다)
베이스캠프에서는 코딩 뿐만아니라 결재방법, 책읽기 등도 배운다고 한다.<br/><br/>

베이스캠프에서..<br/>
  - 전개 : 만든 코드를 개선, 코드리뷰
  - 위기 : 서버확장을 하면서 section관리, DB확장
  - 절정 : 미션을 똭 주신다!(공개X)
  - 결말 : 안정적으로 운영되는 지, 서버가 죽었을 때 작동하는 지 등을 확인한 후 선배들이 피드백을 해주신다고한다.<br/><br/>

​
1. 기술스택

2. 책 읽는 습관 만들기
    - 기술의 구성, 전체적인 그림을 파악할 수 있어야한다.    
    - 기술 내용 외에 작가의 개발 철학을 알수 있다.
    - 기술의 배경이나 이유를 설명해준다.
   ex) 토비의 스프링3.1, 인프라 엔지니어의 교과서, 자바스크립트마스터북, 모던 자바스크립트 등

3. 세 가지를 정리하는 습관을 들이자

    ㄱ) 문제를 명확하게 한다.

    ㄴ) 내가 이해하지 못했던 부분, 궁금한 점을 명확하게 한다.

    ㄷ) 그 궁금한 점을 알기 위해 어떤 노력을 했고, 어떤 것을 알게 되었는지 정리한다.

  3-1. 질문하는 법

    "리버덕 디버깅" 방법 : 인형에게 먼저 질문하기

      ex) 로그인기능작동을 하고 있는데 oo기능에서 문제가 발생, 찾아봐서 ooo을 했는데 ooo오류가 발생합니다.

4. 블로그 운영하기 

     어떤 블로그를 사용해도 상관없으나 꾸준히 하는 것이 가장 중요하다.

5. 같이 일하는 법 

     코드 스타일 통일하기 : 같은 사람이 개발한 것처럼 만들기(작성하는 것보다 읽는 게 더 중요하기 때문에)<br/>
<br/>

### 2-3. NHN 루키의 성장담
​![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/step05.jpg){: .align-center}<br/>

루키 : NHN 신입사원<br/>

1. Git-Flow 
주요 브랜치 소개




| 브랜치 | 설명 |
|--------|------|
| master | 실서버로 배포가 나가는 브랜치 |
| hotfix | 실서버 버전에서 발생한 버그를 수정하는 브랜치 |
| release | 실서버 버전 배포를 준비하는 브랜치 |
| develop | 개발 브랜치 |
| feature | 기능 개발 브랜치 |




develop하기 전에 테스트한다. 기능별로 나누어서 feature 브랜치를 사용한다.<br/>
release 브랜치에서 QnA와 피드백 과정을 거친 후 master를 해준다.<br/>
기능별로(작은 기능별로) 천천히 올려야한다. 이유 : commit 하나가 하나의 기능이다.<br/>

​![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/img03.jpg){: .align-center}<br/>
2. 코드 리뷰

- 제 3자의 입장에서 보기(서로 연관되어있기때문에 같은 코드 즉, 중복 코드를 방지하기 위해)

- git hub에 어떤 것이 변경되었는지 설명적어두기

- 줄 밑에 댓글을 달 수도 있다
<br/>

### 2-4. 리액트 첫걸음을 위한 속성 가이드 투어
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/step06.jpg){: .align-center}<br/>
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/img04.jpg){: .align-center}<br/>
리액트를 모르는 상태에서 들어도 괜찮다고 설명해주셨고 처음 들었음에서도 어떤 것이 리액트이고 실제 코드와 구현 내용을 그 자리에서 보여주셔서 좋았지만 직접 실습을 하지 못한 점이 아쉽다.

1. 리액트의 특징

    - 선언적(<---> 명령적)
    - 컨포넌트 기반
    - 한번 배워서 여러 개 사용가능


2. 실습
(내용은 생략하고 사진으로 보여드릴께요.)
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/img05.jpg){: .align-center}<br/>

### 2-5. Spring JPA의 사실과 오해
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/step07.jpg){: .align-center}<br/>
"소프트웨어공학의 사실과 오해" 라는 책에서 트랙 제목을 정했다고 한다.

(데이터베이스와 연관되어있어서 이해하기 쉬웠다)

1. 연관관계 맵핑

    오해 : 양방향 매핑보다 단방향 매핑이 좋다.

    - 대개의 경우 단방향 매핑이면 충분

    - 일대다(1:N) 단방향 연관관계 매핑에서 영속성 전이(cascade)를 통한 insert 시 양방향 매핑

2. N+1 문제

    - Entity에 대해 하나의 쿼리로 N개의 레코드를 가져왔을 때, 연관관계 Entity를 가져오기 위해 쿼리를 N번 추가적으로 수행하는 문제

    오해1 : EAGER Fetch 전략때문에 발생한다?!

    - Fetch 전략을 LAZY로 설정했더라도 연관 Entity를 참조하면 그 순간 추가적인 쿼리가 수행됨

    오해2 : findAll() 메서드는 N+1 문제를 발생시키지 않는다?!  

3. JPA Repository 메서드와 JOIN

    오해 : JPA Repository 메서드로는 JOIN 쿼리를 실행할 수 없다?!

4. 잘 알려지지않은 사실 : Page vs Slice 

5. JPA Repository 메서드와 DTO Projection

    오해 : JPA Repository 메서드로는 DTO Projection을 할 수 없다?!

(내용을 다 적으면 정리 요약본이 아닐 것 같아서 오해들만 적어두고 나중에 제대로 공부할 때 관련 내용들을 다시 한번 보도록 하려고 한다.)
<br/>

### 2-6. 딥러닝, 야 너도 할 수 있어(feat. PyTorch) 
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/step08.jpg){: .align-center}<br/>
딥러닝. 요즘에는 유튜브를 자주보다보면 유튜브 알고리즘이라는 용어를 듣곤 한다.<br/>

알고리즘을 알아서 내 관심분야를 찾아줘서 추천해주는 것인데 그것이 딥러닝에 해당하는 게 아닌가 싶다.<br/>

딥러닝을 이용해 사진을 주면 성별, 나이, 닮은 연예인도 판별해주는 것들이 있다.<br/>
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/img06.jpg){: .align-center}<br/>
해당 트랙에서는 그 딥러닝에 대해서 알아보는 시간이였다.<br/>
서울에서 할 때는 직접 실습이 가능하고 4시간으로 강의를 했다고 하셨지만 여기서는 한시간으로 하셔야만 했다.<br/>
그래서 리액트 때와 마찬가지로 실습을 하지 못한 아쉬움이 들었다.<br/>
딥러닝은 수학계산이 많다고 하셨다. 그래서 공식(?)을 알려주고 설명해주셨다. <br/>
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/img07.jpg){: .align-center}<br/>


## 3. 기념품
![](https://kimmy100b.github.io/assets/images/conference/2019_12_NHN/gift.jpg){: .align-center}<br/>

## 4. 마무리
 처음으로 회사에서 여는 컨퍼런스에 참가를 하게 되어 기대가 엄청 컸다. 그러나 기대하는 만큼 두려움도 컸다. 특강을 들으면 무엇을 하라고는 하는데 정확히 어떻게 해야하는 지 감이 안오게 말씀을 해주시거나 뜬구름잡는 느낌의 말씀이 많았기 때문이다. 그래서 이 컨퍼런스도 그러면 어쩌지하는 생각으로 인해 두려움이 있었다. <br/>
 사실 웹을 하기로 마음먹은 것도 얼마되지 않았고 나는 많이 부족하고 들어도 못 알아듣는 것이 많을 것이기 때문에 스몰 스탭이 나에게 너무 좋았던 것 같다. 가장 좋았던 트랙도 스몰 스탭에서 했던 "NHN 베이스캠프 : 신입사원들은 무엇을 배우나요?"였다. 신입사원을 배우는 것들인데 내가 지금 실천하고 공부하는데 필요한 방법들을 알려주셨고 어떻게 해야하는 지 자신의 사례를 들어 설명을 해주니 이해 및 적용하기가 쉬웠다.(그 덕에 이렇게 블로그를 꾸준히 하려고 노력합니다!!) 아직 책은 읽어가지 못하고 있지만 조만간 책도 읽으면서 가르쳐주신 것을 잘 실천하며 발전하는 내가 되었으면 좋겠다라는 생각을 한다.<br/>
 이번 컨퍼런스에서 말한 "Small Steps make a Big Difference"처럼 오늘의 소소한 이 발걸음들이 모여서 나중에는 좋은 코드를 짜는 멋진 개발자가 되고 싶다! Make my Web Delicious!<br/>