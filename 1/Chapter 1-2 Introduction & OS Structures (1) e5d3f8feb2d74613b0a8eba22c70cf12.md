# Chapter 1-2. Introduction & OS Structures (1)

# 01. 운영체제

## 운영체제(Operating system)란?

- 컴퓨터(computer) 시스템을 운영하는 소프트웨어

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(1)%20e5d3f8feb2d74613b0a8eba22c70cf12/Untitled.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(1)%20e5d3f8feb2d74613b0a8eba22c70cf12/Untitled.png)

## 컴퓨터(Computer)란?

- 정보(information)를 처리하는 기계

## 정보(information)란?

- 불확실(uncertainty)한 상황을 측정해서 수치적(quantitative)으로 표현한 것

## 컴퓨터가 정보를 처리한다

- 정보의 최소 단위 : bit (binary digit)
- 정보의 처리 : 0↔1 상태변환
- 부울 대수(Boolean Algebra) 를 사용하여 정보의 처리를 표현
- 논리 게이트 : NOT, AND, OR, XOR, NAND, NOR
- 논리 회로 : IC, LSI, VLSI, ULSI, Soc ...

    → 집적도가 점점 높아진다. → 하드웨어의 발전은 거의 한계

    ⇒ 무어의 법칙(chip) : 반도체 칩의 트랜지스터 집적도가 매년 2배씩 증가한다.

    ⇒ 황의 법칙(memory) : 메모리 집적도는 매년 2배씩 증가한다.

- 정보의 저장 : 플립-플롭
- 정보의 전송 : 데이터 버스, RF

### 정보 처리?

- 덧셈 ⇒ 반가산기, 전가산기
- 뺄셈⇒ 2의 보수 표현법
- 곱셈과 나눗셈 ⇒ 덧셈과 뺄셈의 반복
- 실수 연산 ⇒ 부동 소수점 표현법
- 함수 ⇒ GOTO (분기문, 반복문)
- 삼각함수, 미분, 적분, 사진 촬영, 동영상 재생...

## 컴퓨터는 만능인가?

- 범용성 : universality

    ⇒ 여러 분야나 용도로 널리 쓰일 수 있는 특성

    ⇒ NOT, AND, OR 게이트로 모든 계산을 할 수 있다. 

     NOT(a1 AND a2 AND ... AND an) == NAND(a1, a2, ..., an)

    = NAND 게이트만으로 모든 계산을 할 수 있다. → NAND 회로을 얼마나 집적시키느냐 → 하드웨어 기술의 수준을 결정

    ⇒ 범용 컴퓨터 : general-purpose computer

    = 특정한 한 가지 일을 수행하는 것이 아닌, 여러가지 일을 수행

- 계산가능성 : computability

    ⇒ Turing-computable : 튜링 머신으로 계산가능한 것

    = 모든 것을 다 계산 가능한 것이 아니다.

    = 튜링 머신으로 계산 가능한 문제를 처리할 수 있다.

    ⇒ 튜링 머신으로 풀 수 없는 문제 : Halting Problem (정지 문제)

## 컴퓨터는 누가 만들었나?

- 컴퓨터의 할아버지 : Alan Turing
- 컴퓨터의 아버지 : John von Neumann → 실제로 동작하는 컴퓨터를 만듦

## 앨런 튜링이 컴퓨터의 할아버지인 이유

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(1)%20e5d3f8feb2d74613b0a8eba22c70cf12/Untitled%201.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(1)%20e5d3f8feb2d74613b0a8eba22c70cf12/Untitled%201.png)

- 현대적 컴퓨터의 원형을 설계
- 유니버셜 튜링 머신 == 운영체제

## 폰 노이만이 컴퓨터의 아버지인 이유

- 내장형 프로그램 방식을 도입한 사람
- stored-program(내장형 프로그램) : 메모리 저장장치에 프로그램을 저장해 두고 사용하는 방식
    - fetch - execute 사이클을 사용한 컴퓨터
- 폰 노이만 아키텍쳐 == ISA (instruction set architecture)

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(1)%20e5d3f8feb2d74613b0a8eba22c70cf12/Untitled%202.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(1)%20e5d3f8feb2d74613b0a8eba22c70cf12/Untitled%202.png)

## 프로그램(program)이란?

- 명령어의 집합
- 명령어 : 컴퓨터 하드웨어가 어떤 일을 수행하게 함

## 운영체제도 프로그램인가?

- 프로그램이다.
- 컴퓨터에서 항상 실행중(running at all times)인 프로그램
- 응용프로그램(application program)에 시스템 서비스를 제공
    - 응용프로그램은 원하는 것을 OS에 요청하기만 하면됨
- 프로세스(process), 리소스, 유저 인터페이스 등을 관리