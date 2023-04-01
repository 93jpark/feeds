# 모던~한, 자바스러운 자바? - 1

## Introduction

<img src="/kenneth/images/google_search_modern.png">


이 주제에 대해 생각해보게 된 것은 다른 언어 진영의 아티클 및 유명 도서 제목에서 시작했다.<br>
파이썬은 파이썬다운(pythonic) 코드에 대해, 자바스크립트는 모던 자바스크립트에 대해 이야기하는 걸 몇 번 본적이 있다.<br><br>
여기서 의문이 생겼다.<br>
그렇다면 자바는? 코틀린이 있어서 그런 얘기가 안나오나? 자바스러운(javaic) 코드는 무엇일까. 자바에서 모던함은 무엇을 말할까? 무엇이 자바스럽게 만드는지, 무엇이 모던함인지 알아보도록 하자.<br>
<br>

## Philosophy of programming Language 

먼저, 각 언어를 해당 언어스럽게 사용하기 위해서는 해당 언어가 소개된 배경 또는 철학을 알아야한다고 생각했다. 각 언어의 철학을 간략하게 알아보았다.<br>

- Python

> _high-level, general-purpose programming language. Simple is better than complex._

- JavaScript
> _Usability in various environment from website to server-side SW. JS focus on lightweight and easy-to-learn._

- Java
> _Write one, run everywhere._

요즘 많이 접하게 되는 언어들의 각 철학은 다음과 같았다. JS는 한줄로 표현하고 있는 레퍼런스가 없어서 개인적으로 요약해보았다.<br>
<br>
위의 각 철학을 살리기위해 언어마다 고유한 특징을 한 눈에 알아볼 수 있다.<br>
파이썬의 경우, 문법이 타 언어에 비해 직관적이고 인덴테이션으로 구분하도록 한다.<br>
<br>
JS진영이 아니지만, C++/Java에 비해 간결한 문법과 다양한 환경에서 구동시킬 수 있다는 점이 떠오른다.<br>
<br>
Java는 JVM을 기반으로 한번 작성된 프로그램을 다른 OS환경에서 구동이 가능하다. Java의 등장배경을 보았을 때, 추가적으로 OOP라는 패러다임을 따르면서 기존의 절차지향에서 불편했던 유지보수를 더 편리하게 했음을 예상해본다.<br>
<br><br>


### Java's core feature
자바의 철학이 _Write once, Run everywhere_ 인 것은 이제 알겠다. 그렇다면, 자바의 특징들은 뭐가 있을까?<br>
<br>
> Simple, Object-oriented, Protable, Platform independent, Secured, Robust, Architecture netrual, Multithreaded ... 

특징들이 너무 많다. 여러 레퍼런스들 중에서 자바의 특징을 한개로 꼽으라면 역시 `객체지향`이라고 할 수 있을 것이다. 현대에서 이러한 객체지향 패러다임만으로 충분하지 않으니 `모던함`이 추가되었을 것으로 추측해보았다. 여기서 말하는 `모던함`은 자바 뿐 만 아니라 다른 언어에서도 공통적으로 겪고 있는 불편함이나 문제점 등에서 출발해 해결하기 위해 언급되는 주제가 아닐까.
<br>
<br>


## Then, what about 'Modern'?
그렇다면 모던하다는 것은 무엇을 의미할까? 기존의 어떤점들이 다양한 언어들에서 모던함을 추구하게 만들었을까?<br>
<br>
`모던 프로그래밍`을 정의하는 것은 매우 모호하며, 시대에 따라 다르게 정의될 수 있다. 근대의 프로그래밍 언어, 프레임워크를 사용하는 것을 말할 수 있고, IDE에 종속되지 않게 개발하는 것, 애자일하게 개발하여 빠르게 프로덕트를 만드는 것 등 다양한 맥락에서 사용될 수 있다. 그래서 다음의 것들을 모던한~ 프로그래밍이라고 이야기한다.<br>

- Object-oriented programming
- Functional Programming
- Agile development
- Cloud computing
- Machine learning and artificial intelligence

<br>
이 중에서 프로그래밍 패러다임 측면에서, 객체지향 & 함수형 프로그래밍을 꼽을 수 있을 것이다. 둘 모두 확장과 유지보수에 용이하게 위한 공통 목표를 지닌다. OOP는 친숙하지만 Functional Programming(함수형 프로그래밍)은 많이 들어봤지만 낯선 개념이다.<br>
<br>
<br>

## Key feature of functional programming

`함수형 프로그래밍(FP)`은 객체지향과 마찬가지로 프로그래밍의 패러다임으로, 데이터조작과 결과를 도출하기 위해 사용되는 함수를 프로그램의 기본 단위로 여긴다. OOP에서는 객체가 중심이라면, FP에서는 함수 또는 메소드가 중심이다.<br>
<br>
> _The main idea begind functional programming is to write programs that are declarative rather than imperative. ~ declare what the result should be ~._

FP에 대한 아티클을 번역하면, 함수형 프로그래밍은 프로그램을 명령형으로 작성하는 것보다 선언적으로 개발하는 것을 말한다. 직역하면 와닿지 않아 다음과 같이 번역해 보았다. 개발하는데에 있어 '어떻게 하는지' 보다 '무엇'에 집중한다. 어떤 것이 결과적으로 나오는지, 어떤 것이 필요로 하는지에 초점을 맞춘다는 것이다.
<br><br>
이러한 설명만으로는 어떤게 명령형이고, 함수형인지 와닿지 않았다. 코드를 예시로 직접 확인해보자.<br> 예시는 [여기](https://mangkyu.tistory.com/111)를 참조했다.
```Java
for(int i = 1; i < 10; i++) {
    System.out.println(i);
}
```
FP에서는 대입문을 사용하지 않으며, 로직 처리를 위해 작은 단위의 함수를 만들어서 사용한다고 한다. for문을 돌리기위해서 i의 값을 대입하여 동작시키도록 하고 있다. 이를 함수형으로 바꾸면 다음과 같은 함수를 사용할 수 있다.<br>
```Java
iterateIndex(10, print(num));
```
`iterateIndex()`는 첫번째 파라미터로 얼만큼 로직을 수행할지, 두번째 파라미터에서 어떤 로직을 수행할지 함수를 받고 있다.<br>
즉, 위의 메소드를 통해서 '무엇'을 할것인지 함수를 기반으로 프로그램을 작성한 것이다.<br>
<br>
이런 느낌이 함수형 프로그래밍이라는 것은 이제 알겠다. 이를 포함한 함수형 프로그래밍의 특징을 정리하면 다음과 같다.<br>

- Immutability(불변성)
    - 한 번 값이 할당되면 변경되지 않는 것.
- Side effect(부수효과)
    - 외부 상태를 변경하거나, 예외/오류등이 발생하는 것.
- Pure function(순수함수)
    - 어떠한 부수효과 없고, 외부의 상태를 변경하지 않으며, input으로 받은 파라미터에 대해서만 의존하는 함수.
- First-class Object(1급 객체)
    - 파라미터나 리턴 값으로 사용될 수 있으며, 변수나 데이터 구조에 할당될 수 있는 객체.
- Referential Transparency(참조 투명성)
    - 동일한 input에 대해 동일한 output을 반환하며, 기존의 값이 변경되지 않고 유지된다.

<br>

JS에서는 ECMA6 이후, 자바에서는 8이후를 '모던'이라고 이야기한다. 즉, 해당 버전에서 `모던함`을 위한 특징들이 추가되었다고 할 수 있다. 다음 장에서는 자바에서 모던한 특징들을 다루어보도록 하겠다.
<br>
<br>

---

<br>







- 참조 레퍼런스
    - https://medium.com/javascript-scene/the-forgotten-history-of-oop-88d71b9b2d9f
    - https://www.google.com/search?q=what+does+mean+modern+in+programming+language%3F&rlz=1C5CHFA_enJP1037JP1037&oq=what+does+mean+modern+in+programming+language%3F&aqs=chrome..69i57j33i160l5.9284j0j7&sourceid=chrome&ie=UTF-8
    - https://violetboralee.medium.com/%EB%AA%A8%EB%8D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-f7e115f03533
    - https://mangkyu.tistory.com/111
