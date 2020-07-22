---
title: "예외처리(Exception Handling)"
categories:
  - java
tags:
  - java
  - exception handling
last_modified_at: 2020-05-11 T12:38:00-0:05:00
---

## 예외처리가 왜 필요할까?

"엄마가 이만원 줄테니 다이소가서 건전지, 문구점가서 볼펜, 마트가서 계란을 사오렴"<br/><br/>
그러면 아이는 이만원을 들고 다이소가서 건전지, 문구점가서 볼펜, 마트가서 계란을 사올 것이다.<br/>
그런데 아이가 저녁이 됐는데도 안들어오는 것이다.
엄마가 아이를 찾으러 가보니 마트 앞에서 아이가 앉아있었다.<br/><br/>
"왜 집에 안오고 마트에 앉아있니?"<br/><br/>
"마트에 계란이 다 팔려서 마트 앞에 있었어요"
이처럼 마트에 계란이 매진되는 상황은 예기치 못한 상황이다.<br/>
예기치 못한 상황이 발생하면 사람이라면 융통성을 가지고 집으로 돌아오지만 컴퓨터는 융통성이 없어 그렇게 하지 못한다.<br/><br/>
이 외에도 예기치 못한 상황은 언제든지 발생할 수 있다.<br/>예를 들어 물품 가격이 다 합쳐서 이만오천원인 경우, 마트에 아주머니가 안계시는 경우 등이 있다.<br/><br/>
예기치 못한 상황이 예외인 것이다.<br/>
예외가 발생했을 때, 어떻게 해야하는 가를 해주기 위한 것이 예외처리이다.

## 에러와 예외

에러에는 세가지 종류가 있다.

| 에러 종류   | 에러 설명                                                                     |
| ----------- | ----------------------------------------------------------------------------- |
| 컴파일 에러 | 컴파일 시에 발생하는 에러                                                     |
| 런타임 에러 | 실행 시에 발생하는 에러                                                       |
| 논리적 에러 | 실행은 되지만, 의도와 다르게 동작하는 것,<br/>프로그래머의 논리적 결함에 의해 |

예외처리할 때, 예외는 실행 시에 발생하는 에러(런타임 에러)에 속한다.<br/><br/>
에러(error) - 프로그램 코드에 의해서 수습될 수 없는 심각한 오류 (ex. 메모리부족, 스택오버플로우 등)<br/>
예외(exception) - 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

## 예외클래스의 계층구조

![](https://kimmy100b.github.io/assets/images/java/exceptionhandling.jpg){: .align-center}<br/>

​1. Exception 클래스와 그 자손들 (RuntimeException과 그 자손들 제외)<br/>
사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외

2. RuntimeException 클래스와 그 자손들<br/>
   프로그래머의 실수로 발생하는 예외

## 예외처리(exception handling)

정의 - 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것<br/>
목적 - 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것

```
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다
}

catch (Exception e) {
    // Exception이 발생했을 경우, 이를 처리하기 위한 문장을 적는다
}

finally {
    // 예외 발생 여부와 상관없이 무조건 실행되는 문장
    // 반드시 필요하진 않음
}
```

catch 블럭은 하나 이상이 올 수도 있다. 발생한 예외의 종류와 일치하는 단 한 개의 catch 블럭만 수행된다.

(상속의 개념을 포함하고 있기 때문에 불필요한 코드가 생기지 않게 조심해야한다. 예를 들어 RuntimeException클래스와 NullPointerException클래스가 각각 순서대로 catch에 있다면 NullPointerException은 RuntimeException의 자손이기에 RuntimeException에서 이미 실행을 하여 뒤에 있는 NullPointerException은 실행되지 않는다.)
![](https://kimmy100b.github.io/assets/images/java/try-catch.png){: .align-center}<br/>

## finally가 반드시 필요한 이유

```
Public void Method(){
    try {
        // 예외가 발생할 가능성이 있는 문장들을 넣는다
        return;
    }

    catch (Exception e) {
        // 오류 발생 시
        return;
    }

    finally {
        // 예외 발생 여부와 상관없이 무조건 실행되는 문장
    }

    // 여기에 작성해도 되잖아?
}
```

예외 발생 여부와 상관없이 무조건 실행되는 문장이라면 함수 내에 작성해도 상관없지 않을까?

정답은 NO이다.

try와 catch 블럭 내에서는 return을 적는 경우가 많다. return을 하게 해주면 함수를 벗어나게 되어 그 뒤에 있는 소스는 실행되지 못한다.

만약, finally가 있다면 return을 해줘도 finally내의 코드를 실행할 수 있는 차이점이 있다.

```
Database.openConnection(); // DB접속
try {
    Result = database.searchData(“..”);
    return result; // 찾아낸 데이터 반환
}

catch (Exception e) {
    // 데이터 검색 오류
}

finally {
    database.close(); // DB 접속 종료
}
```

## 예외는 언제 발생하는가?

예제1)<br/>
일치하는 파일명이 없는 경우<br/>
![](https://kimmy100b.github.io/assets/images/java/exceptionhandlingEx1-1.png){: .align-center}
![](https://kimmy100b.github.io/assets/images/java/exceptionhandlingEx1-2.png){: .align-center}

<br/>
예제2)<br/>
계산이 불가능한 경우<br/>
![](https://kimmy100b.github.io/assets/images/java/exceptionhandlingEx2-1.png){: .align-center}
![](https://kimmy100b.github.io/assets/images/java/exceptionhandlingEx2-2.png){: .align-center}

<br/>
예제3)<br/>
사용자로부터 정수가 아닌 값을 입력받은 경우<br/>
![](https://kimmy100b.github.io/assets/images/java/exceptionhandlingEx3-1.png){: .align-center}
![](https://kimmy100b.github.io/assets/images/java/exceptionhandlingEx3-2.png){: .align-center}
<br/>
세 예제를 자세히 보면 Console 창에 뜨는 오류가 우측에 catch에 들어가 있는 것을 볼 수 있다.

## 예외처리 그러면 좋은 거네?

예외처리가 프로그래머의 부담을 덜어주어 편리함을 제공하는 장점이 있다. 그러나 그 장점이 장기적으로는 독이 될 수도 있다. 오류들을 철저히 분석하고 예상해서 소프트웨어를 더 안전하게 짜는 것이 아니라 그냥 예외처리로 대충 묻어버리고 가는 나태한 습관들이 이후 큰 문제들을 발생시키기도 하기 때문이다. 예외처리에 대한 입장은 다양하고 논란도 있다. 그렇기 때문에 개발자인 우리는 코딩을 할 때 어떤 상황에서 예외처리를 하는 것이 적절한지, 고민해보고 관련 자료들도 찾아보고 하면 더 탄탄하고 오류로부터 안전한 코드를 짜는 적절한 방식을 익혀나갈 수 있다.
