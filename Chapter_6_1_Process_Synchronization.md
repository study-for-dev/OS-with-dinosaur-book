## Cooperating Process

-   서로 영향을 주거나 받는 관계의 process들

공유된 Concurrent하게 접근할 수 있게 되면 Data Inconsistency 문제가 발생한다.  
Cooperating Process를 사용하는 경우 서로 Process에 접근하고 영향을 줄 수 있기 때문에 Data Inconsistency가 발생할 수 있다.

이것을 해결하는 방식으로 사실 Process들이 있으면 서로의 순서를 침범하지 않고 순서대로 수행되게 만들어 주면 된다.  
이렇게 수행되면 같은 데이터에 동시에 접근하지 않게 되므로 data integrity문제가 발생하지 않을 것이다.

(integrity of data - 데이터의 무결성을 의미한다. 정확하고 일관적인 데이터 값을 유지하는 성질.)

## Data Integrity

### Concurrent Execution시 발생 & parallel Execution 시 발생

이렇게 여러가지 process가 동시에 수행될 수 있는 환경이 되면 위에서 말한것처럼 순서가 지켜지지 않으므로 언제든 context switching이 일어나면서 data integrity를 보장할 수 없는 상황이 된다.

이 강의에서는 Producer - Consumer Problem을 예시로 들었다.

### Producer & Consumer Problem



위의 코드에서는 Buffer에 Producer가 열심히 Buffer를 채워서 넣고 , Consumer는 Buffer값이 0이 아니면 계속해서 빼오는 방식으로 사용하고 있다.

그냥 보기에는 물론 Count값이 count++이 되면 버퍼에 있는 것을 알 것이고 consumer는 Count++을 하고 나면 Count가 0이 아니니까 빠져나가면서 Buffer에 있는 값을 꺼내오는 순서로 동작하면 문제가 없을 것처럼 보인다.

하지만 이런 경우도 Concurrent한 방식을 사용하게 되면 문제가 생길 수 있다. 기계어로 위의 동작들을 나누어보면



위와 같은 동작을 하는 것을 알 수 있다. 우리에게는 1개의 명령어처럼 보이지만 어셈블리로 해석하게 되면 이렇게 3가지의 명령어를 사용하기 때문이다.



위에서 보인 순서대로 수행을 하게 되면 Register에서 가지고 있는 값들이 그 store시점의 count값에 따라 변하고, 그것들을 또 save하는 시점에 그것이 영향을 주기 때문에 정상적인 값이 출력될 수 없는 것이다.

# Race Condition

Race Condition이란 여러개의 Process가 서로 한정적인 CPU자원을 사용하기위해 경쟁하면서 생기는 상태를 말한다.



어떤 Race Condition은 두개 이상의 process가 어떤 data를 공유하고 있을 때, concurrent하게 접근하고 처리해야할 때 발생하고, 실행의 결과는 순서에 따라서 그때 그때 달라질 수 있다.

## Race Condition 해결 방법

특정 시간에 오직 한개의 프로세스만 shared data를 처리 할 수 있다고 하면 별 문제가 발생하지 않을 것이다. 이 방법을 process synchronization이라고 한다.

# Critical Section Problem - 임계 영역 문제

n개의 프로세스가 있을 때, 각각의 코드 영역을 critical section이라고 부른다.

하나의 process가 critical section을 수행하고 있을 때는 다른 프로세스는 해당 critical section에 접근할 수 없도록 만들면 race condition이 방지되지 않을까? 라는것이 해결 방법의 핵심이다.

소스코드 영역을 4가지 분류를 할 수 있는데

1.  Entry-Section ( Critical section에 진입하는 영역 ) - Critical section 진입 권한을 얻는 곳
2.  Critical Section (실제 Process수행되는 코드영역)
3.  Exit Section - Critical Section이 끝나고 수행하게 되는 곳
4.  Remainder Section -> ?

Critical Section 영역은 반드시 한번에 실행 될 수 있도록, 여기에 진입하면 다음 프로세스가 접근할 수 없도록 만들어주는 것이라고 한다.

## Critical Section 문제 해결방법

1.  Mutual Exclusion(상호 배제)

특정 Process가 Critical Section을 사용하고 있을 때는 다른 Process가 해당 Critical Section을 수행할 수 없게 만들어야 한다.

이 Mutual Exclusion(상호배제)를 하게 되면 Deadlock과 starvation이 발생하게 된다.

Critical Section에 접근하려고 하는 프로세스가 여러개 있을 때, 아무도 접근할 수 없는 그런 현상이 Deadlock이 될 것이다. -> Progressive 하게 구현 필요

Startvation발생의 경우에는 특정 Critical Section으로 접근할때 우선순위에 밀려 접근하지 못하는 process가 생기는 것이다. -> Bounded Waiting을 통해 제한적으로만 기다리고 접근할 수 있도록 만들어주어야한다.

이 3가지를 모두 해결해야 Critical Section을 해결하였다고 할 수 있다.

그래서 현실적으로 모든걸 다 해결하기는 어렵다. 따라서 일반적으로는 mutual exclusion 정도만 지키면서 나머지는 발생한 이후에 처리하는 경우가 더 많다.

운영체제 Kernal 에서도 Race Condition 이 발생할 수 있다.

fork() 두개가 있는데, 새로운 fork1에 의해서 생성하려고 했던 pid가 fork2에서 생성하려고 할때 가져간 pid와 같을 수 있다. 그렇게 되면 pid가 같아서 충돌이 발생하게 된다.

Single core에서 해결할 수 있는 가장 간단한 방법은 interrupt가 발생하지 않도록 막아버리는 것이다. count++등을 하는동안은 interrupt를 disable 시켜버리는 것이다. interrupt가 발생하지 않도록 하면 되는것 아닌가.

\-> 하지만 이 방법도 조금만 복잡해지면 사용하기가 어렵다. 멀티프로세스 환경에서는 코어가 많기 때문에 모든 코어에 interrupt를 막는 방식으로 구현해야하기 때문에 사용하기 어렵다. (성능 저하 등) Critical Section이 길어지는 영역등이 많고 그렇게 되면 이 방식으로 구현하는 것이 어려워진다.

### Preemptive vs non-preemptive Kernels

비선점형

-   한번 진입하고 나면 자신이 내려놓을 때까지 계속 CPU를 사용한다. Race Condition 발생할일이 없다.
-   하지만 성능이 느려서 사용하지 않는다.

선점형

-   프로세스가 중간에 언제든지 CPU를 빼앗길 수 있다. 하지만 성능이 좋기 떄문에 사용을 한다.

# Critical Section Problem's Solution

Software로 해결할 수 있는 여러가지 방법이 존재한다.

Dekeer 알고리즘 - 2개의 프로세스

Eisenberg and McGuires 알고리즘 - n개의 프로세스

Bakery 알고리즘 - 램포트? 가 제안한건데 안다룬거라고 한다.

## Peterson's Algorithm

-   Critical Section을 가장 완벽에 가깝게 해결한 알고리즘
-   하지만 정상 동작에 대한 Guarantees가 없다. (load & Store같은 기계어 사용)

Peterson이 해결한 방법은 다음과 같다.

서로 key를 가지고 들어가서 flag를 통해 수행 조건 체크하고 권한을 획득할 시에만 접근하는 방식이다.

그런데 이렇게 해도 정상적으로 동작하지 않는 경우가 많다. 왜냐하면 앞에 Producer Consumer problem에서 잠깐 보았듯, 한개의 Count를 올리는 동작들도 여러개의 기계어로 변경된다.

```
        mov     eax, DWORD PTR [rbp-12]
        cdqe
        mov     eax, DWORD PTR [rbp-20+rax*4]
        test    eax, eax
        je      .L4
```

이 코드는 while(flag\[j\] && turn == j); 를 어셈블리로 변경한 것이다.

따라서 peterson알고리즘은 동작이 완벽하게 검증되어있다기 보다는, 해당 이론을 통해서 Critical Section Problem을 잘 설명할 수 있고, 해결방법 3가지를 peterson알고리즘으로 증명할 수 있다는 것에 더 큰 의미가 있다.

그렇다면 Critical Section Problem을 진짜로 해결할 수 있는 방법은 없는걸까?

## Hardware-based solution

Peterson알고리즘은 CSP 의 문제를 보여주고 증명해주었다. 하지만 이 방법을 사용하더라도 Hardware의 도움이 없이는 CSP를 실재적으로 해결하는 것이 어렵다.

memory barriers or fences를 사용하거나 hardware instructions나 atomic variables를 사용하여 더 완벽하게 만들 수 있다.

Memory Barriers - 말그대로 Hardware를 통해 Memory Barrier를 치는것이다. 따라서 해당 메모리에 접근을 하려고 하는 다른 Process는 해당 메모리에 Barrier가 쳐져있기 때문에 접근할 수 있는 방법이 없게 되면서 Mutual Exclusion이 자연스럽게 해결될 수 있다.

Atomicity : 원자성

atom -> 더이상 쪼갤 수 없는 물리적 성분

atomic operation의 경우는 더이상 쪼갤 수 없는 (uninterruptable) operation unit.

대부분 atomic operation을 말할때 기준을 CPU 1 Clock을 기준으로 한다. 단일 클럭내에서 실행되는 동안에는 interrupt등을 통해서 context swtich같은 것을 일으키는 것이 불가능하기 때문이다.

어떤 값을 확인하고 수정하는 그런 값들을 3개의 instruction으로 하지 말고 하나의 hardware instruction을 만드는 것이다. test\_and\_set()... compare\_and\_swap

위 코드는 hw instruction 중 대표적으로 사용하고 있는 test and set이라는 것이다. 이 hw instruction을 사용하면 쉽게 mutual exclusion을 구현할 수 있는데, 그 이유는 lock과 init이 한번에 이루어지기 떄문에 lock을 동시에 가져가는 일은 발생하지 않는 것이다.

java에서는 atomicBoolean이라는 라이브러리가 있어서 해당 변수를 통해서 수행을 할 수 있다고 한다.

??? : static안에 뭔가를 생성해서 만들어서 했는데 잘 이해는 못했지만 저 Static {} 안에 있는 내용들은 atomic하게 수행할 수 있도록 만들어주는 것 같다.

일반적으로는 위에서 말한 3가지 CSP를 모두 해결하는 것이 현실적으로 어렵기 떄문에 Mutual Exclusion을 막는 것을 우선으로 하고 있다. 그 해결 방법이 바로 뒤에서 다룰 Mutex, semaphore, monitor등이 있다.
