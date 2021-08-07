# Chapter 3. Processes (Part 1, 2)

#
#

# 프로세스의 이해

#
#

# 프로세스란?
- 실행중인 프로그램 (program in execution)
- 좀 더 자세하게는, HDD상의 프로그램이 아닌 메모리에 로딩되어 CPU가 바로 fetch할  수 있는 프로그램
- OS의 주된 역할은 이 '프로세스'를 관리하는 것.

#

# 메모리에서의 프로세스 구조

#

![memory structure](image/Chapter3.Processes(Part1-2)/memory.JPG)

- text section : 가장 기본적인 명령어 코드들
- data section : global, static 변수들
- heap section : malloc, new 등 동적으로 할당된 공간
- stack section : 함수 호출 관련 정보들 (파라미터, 리턴 주소, 로컬 변수 등)

#

## TMI 1 : Process in JAVA?
- OS와 프로세스의 기본 컨셉 : OS가 프로그램에 직접 접근하여 메모리를 할당하고 프로세스 수행
- JAVA 프로그램은 OS가 직접 접근하지 않는다. JVM 이라는 중간자가 존재하는 특수성.
- 그럼 JAVA에서는 프로세스가 일반적인 경우와는 다른 형태의 공간에 존재하게 되지 않을까?

#

### 자바 프로그램 동작 방식

#

![jvm](image/Chapter3.Processes(Part1-2)/JVM.JPG)

1. JVM이 OS에게 메모리를 할당 받는다.
2. 자바 컴파일러가 자바 소스코드를(.java 파일) 컴파일하여 자바 바이트코드로(.class 파일) 변환한다.
3. 바이트 코드는 클래스 로더에 의해 JVM에 로딩됨
4. Execution Engine(실행엔진)에 의해 바이트코드->기계어 번역 후 실행 (interpreter or JIT (Just In Time) 방식) 
5. 프로세스가 실질적으로 수행되는 영역은 Runtime Data Area. 즉, 이 영역이 일반적인 프로세스 실행에서의 '메모리' 부분 (컴퓨터 메모리란 사실은 어차피 똑같긴 하지만)

그 외 GC 등의 부분은 나중에...

#

### Runtime Data Area
#### 그래서 Runtime Data Area는 어떻게 나눠져 있는데?

#

![runtimedataarea](image/Chapter3.Processes(Part1-2)/RuntimeDataArea.JPG)

#### 1. JVM Stack (= Stack) 
- 함수 호출에 관련된 모든 정보들
- 일반적인 프로세스의 스택 영역과 동일 

#### 2. Class Area (= Method Area, Code Area, Static Area) 
- 클래스, 메서드, 전역 변수, static 변수
- (추정) 일반적인 프로세스의 data + text section과 동일

#### 3. Heap Area
- 동적 할당되는 객체, 배열을 위한 공간
- 일반적인 프로세스의 heap 영역과 동일
- Eden, Survivor, Old, Permanent 등으로 또다시 나뉜다
- GC의 주요 타겟이 됨

#### 4. Native method stack 
- JAVA 코드가 아닌 다른 언어로 짜여진 코드를 실행하기 위한 공간
- (???) 그런 걸 실행할 경우가 있나..?

> JVM으로 인한 OS 호환성이 우수하다는 자바의 장점

> JVM은 OS의 역할을 대신 수행하며 자바 프로세스를 수행

> 하지만 JVM이 OS의 모든 기능을 대체하지는 못 함

> 이러한 단점 보완을 위해 JNI (Java Native Interface) 사용

> JVM이 커버하지 못하는 OS의 기능을 OS를 구현한 언어로 구현한다 (보통 C, C++)

> 이렇게 다른 언어(C, C++ 등)로 만들어진 기능은 JVM에 의해 자바 메서드와 연결됨

> 연결된 자바 메서드 호출 시 해당 C, C++ 코드가 수행됨

> Native method stack은 이러한 코드들을 위한 공간

> (C로 짠 코드를 JNI를 통해 직접 이어붙일 수 있다. 아래 링크 참조)


#### 5. PC Register 
- 일반적인 PC 역할과 똑같다. 

#

※ 자바8부터 static 변수는 Class Area가 아닌 Heap Area에 저장되는 것으로 바뀌었다고 함.

#

#### 참고 링크
> ##### JVM - https://asfirstalways.tistory.com/158 
> ##### JVM memory - https://velog.io/@ditt/JavaJVM-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD
> ##### static variable in java - https://stackoverflow.com/questions/8387989/where-are-static-methods-and-static-variables-stored-in-java
> ##### JNI - https://mommoo.tistory.com/71 
  
#

## TMI 2 : JAVA Reflection API (Real TMI)
(자바의 동작 방식에 대해 얘기하다보니.. 컴파일타임, 런타임  이라는 주제로 아주 조금은 연관성이 있는 거 같아서)

```java
class Person {
    public void live();
}

Object obj = new Person();
obj.live();
```

#

위 코드는 다형성에 의해 컴파일 오류가 나지는 않지만 런타임 에러가 발생한다.
그 이유는 다음과 같다.
- 자바의 인스턴스 변수 타입은 컴파일 타임에 정의된다.
- obj는 컴파일 시에 Object 객체 타입으로 정의되어버렸기 때문에 런타임에서 Person의 정보를 찾을 수 없다.
 
 이러한 문제를 JAVA Reflection API를 통해 해결할 수 있다.
 
 #
 
### What is JAVA Reflection API and How does it work?
- 인스턴스 변수에 실제로 할당된 객체의 구체적인 타입을 모르더라도 해당 객체의 정보들을 알 수 있도록 해주는 API
- 자바 코드에 정의된 클래스, 메서드, 멤버변수, static 변수 등등 대부분의 정보는 Class Area에 저장된다.
- 이 Class Area에서 객체 타입에 대한 정보를 얻어와 실제 할당된 객체의 정보를 자유롭게 사용할 수 있게 해준다.
- 프레임워크 내부적으로 많이 사용되어있다고 함 (아마 Spring MVC에서 디스패쳐 서블릿이 어떤 핸들러 어댑터를 사용할지 모르는 상황에서 핸들러 어댑터를 사용하는 상황을 말하는 듯)
- 다만, 런타임에 직접 타입 분석을 하게 되니 더 많은 부하가 발생

#

![springmvc](image/Chapter3.Processes(Part1-2)/springmvc.JPG)

#

실제로 Reflection API를 사용하게 되는 일은 거의 없는 듯..? 그냥 면접용 지식으로 알아두면 좋을만한 정도라고 생각.

#

#### 참고 링크
> ##### https://woowacourse.github.io/tecoble/post/2020-07-16-reflection-api/

#

## 프로세스 상태 주기

![processdiagram](image/Chapter3.Processes(Part1-2)/process-diagram.JPG)

- new : 막 생성된 상태. 머물러있는 상태라기보다는 그냥 생성 직후의 상태를 말한다.
- running : 실행중
- waiting : I/O 등의 특정한 이벤트를 기다리는 상태
- ready : waiting 상태에서 기다리던 이벤트가 충족됐거나 running 상태에서 인터럽트를 맞아서 ready queue에 들어가 대기중인 상태
- terminated : 프로세스 종료

실질적으로 프로세스가 돌면서 머무르는 상태는 2, 3, 4 셋 뿐이라고 봐도 될 듯.

> 프로세스 수행 중 프로세스가 포함되는 상태는 2, 3, 4가 맞다.

> 하지만 new, terminated state 역시 running, waiting, ready와 마찬가지로 queue에 의해 관리된다.

> ready queue에 빈공간이 없다거나 하는 모종의 이유로 프로세스 생성 직후 바로 ready 상태가 될 수 없을 때 new state를 유지하고 있어야 한다.

> 어떤 프로세스의 종료가 다른 프로세스의 시작or생성 조건이 될 수 있다. 이를 위해 terminated state 또한 별도의 queue를 두어 따로 관리해야 한다.


#

## 프로세스 관리 (PCB)
프로세스의 정보는 PCB라는 구조체에 저장되며 각 PCB는 OS 커널에 의해 생성, 관리된다.
PCB에 저장되는 내용은 다음과 같다.

- process state : new, running, waiting, ready, terminated 등 프로세스의 현재 상태
- PC(Program Counter) : 다음에 가져올 명령어의 주소
- CPU Register : (???) CPU 레지스터 정보를 프로세스가 굳이 왜 갖고있음? 프로세스에서 사용중인 레지스터들에 대한 정보라는 것? 이해 잘 안 됨..
- 스케쥴링 정보 : 스케쥴링을 위한 각 프로세스의 우선순위
- 그 외 : 메모리 정보, 계정 정보, I/O 정보


#


### 이해 안 가는 점?

#### Q1. A tree of processes, 각자의 고유한 역할을 갖는 프로세스 생성을 위해 fork() 한다?
- 최초의 init 프로세스를 기준으로 하여 각각의 역할을 갖는 자식 프로세스들을 fork해나가며 트리 구조의 프로세스들이 형성된다.
- fork는 자기와 똑같은 프로세스를 하나 더 만드는 것 아닌가? 그런데 어떻게 각자의 고유한 역할을 가질 수 있는 것?
- (추측) 같은 로직을 공유하되 그 안에서 pid에 따라 조건 분기하여 각자의 역할을 수행하는 것인가?

#### A1. 분기 후 각자가 수행할 코드를 덮어씌워준다.
- fork 직후에 리턴받는 pid값으로 부모, 자식 프로세스의 수행 영역을 구분한다.
- 자식프로세스의 수행 영역에 새로운 코드를 덮어씌워준다. (execlp 사용)

#### Q2. 고아 프로세스?
- fork 이후 부모가 wait 하지 않고 그대로 자기 로직을 수행하여 종료가 된 경우 자식 프로세스를 '고아 프로세스' 라고 칭한다.
- 부모가 반드시 wait을 해야하는 이유가 따로 있는건가?

#### A2. wait은 의무사항이 아니다.
- 고아라는 부정적인 워딩 때문에 부모가 wait하지 않은 상황을 '문제가 생긴 상황'이라고 착각해버렸다.
- orphan은 단지 각 상태를 구분하기 위한 단어일 뿐 긍정/부정의 의미를 담지 않는다.
- 즉, 부모가 반드시 wait을 해야할 이유는 없다.


