# JVM이란?

JVM은 자바로 작성한 코드를 해석해서 실행해주는 프로그램이다.

JVM은 각 운영체제에 적합하게 개발되어 있고 자바는 JVM을 매개로 운영체제와 소통하기 때문에 운영체제로부터 독립적이다. (C++과 차이점)

<br>
<br>
<br>

# JVM 작동 과정

![](../assets/Java/JVM%EA%B5%AC%EC%A1%B0.png)
자바로 코드를 작성한 후에 실행하면 먼저 컴파일러가 실행되고 컴파일이 진행된다.

그 결과 .java 파일(자바 코드 파일)은 .class 파일(바이트 코드 파일)로 변환된다.

JVM은 운영체제로부터 실행에 필요한 메모리를 할당받는다. 그림에서 Runtime Data Area가 이를 가리킨다.

그 후, 클래스 로더(Class Loader)가 .class 파일을 JVM으로 불러들여 Runtime Data Area에 로드시킨다.

로드가 완료되면 실행 엔진(Execution Engine)이 Runtime Data Area에 저장된 .class 파일을 실행시킨다.

실행 방식으로는 다음 두 가지가 있다.

- 인터프리터(Interpreter)를 통해 한 줄씩 기계어로 번역하고 실행시킨다.
- JIT Compiler(Just-In-Time Compiler)를 통해 바이트 코드 전체를 번역하고 실행시킨다.

실행 엔진은 기본적으로 1번 방법으로 실행시키다가 특정 코드가 자주 실행되면 해당 코드를 JIT Compiler를 통해 실행시킨다.

<br>
<br>
<br>

# JVM 메모리 구조

![](../assets/Java/JVM%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B5%AC%EC%A1%B0.png)

## Heap Area

JVM이 작동되면 자동으로 생성되며 **객체**나 **인스턴스 변수**, **배열**이 저장된다.

```java
Beer blanc = new Beer();
```

위 코드에서 `new Beer()` 가 실행되면 Heap Area에 인스턴스가 생성되고 생성된 위치의 주소 값을 `blanc`에게 할당해준다. 이 `blanc` 은 Stack Area에 선언된 변수이다.

즉, 자바에서 객체를 다룰 때 Stack Area에 있는 참조 변수를 통해 Heap Area의 실제 객체에 접근하게 된다.

<br>

## Stack Area

메서드가 호출되면 할당되는 영역이다. Heap Area에 생성되는 객체의 **참조변수 값**과 int, long, boolean 등의 **기본 자료형**이 저장된다. 메서드 실행이 종료되면 pop되어 사라진다.

<br>

## Method Area(Static Area, Class Area)

JVM이 작동되면 생성되며 종료 시까지 유지되는 공통 영역으로 .class 파일의 바이트 코드가 로드되는 곳이다. **static 변수**와 **클래스 정보, 메서드 정보** 등이 저장된다.

<br>
<br>
<br>

# Garbage Collection

가비지 컬렉션은 프로그램에서 **더 이상 사용하지 않는 객체를 찾아 삭제하여** 메모리를 확보하는 것을 의미한다.

아무에게도 참조되고 있지 않은 객체나 변수들을 자동으로 검색하여 삭제하며 **효율적인 메모리 사용**에 도움을 준다.

![](../assets/Java/%EA%B0%80%EB%B9%84%EC%A7%80%EC%BB%AC%EB%A0%89%EC%85%98.png)
<br>

Heap Area는 객체가 얼마나 오래 존재하냐에 따라 Young Area, Old Area로 나누게 된다.

Young Area에서는 새롭게 만들어진 객체가 할당되며 많은 객체가 생성되고 소멸되는 것을 반복한다. 여기서 활동하는 가비지 컬렉터를 Minor GC라고 한다.

Old Area에서는 Young Area에서 상태를 유지하고 살아남은 객체들이 복사되고 크기가 크게 할당된다. 크기가 클수록 가비지 발생률은 적다. 여기서 활동하는 가비지 컬렉터를 Major GC라고 한다.

 <br>
 <br>

## 동작 방식

Young Area와 Old Area의 메모리 구조에 차이가 있어서 세부적인 동작 방식은 다르지만 기본적으로 다음 두 가지 단계를 가진다.

<br>

1. Stop The World

가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. 가비지 컬렉션을 실행하면 해당 스레드를 제외한 모든 스레드들의 작업이 중단되고 정리가 완료되면 재개된다.

 <br>

2. Mark and Sweep

Mark는 사용되는 메모리와 그렇지 않은 메모리를 구별하는 작업이며, Sweep은 Mark에서 사용되지 않음으로 구별된 메모리를 해제하는 작업이다.
