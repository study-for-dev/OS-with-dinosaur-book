# Chapter 1-2. Introduction & OS Structures (2)

# 1.1 What Operating Systems Do

## OS의 정의

- 일반적인 정의 : 컴퓨터에서 항상 실행하고 있는 프로그램
- OS의 핵심은 Kernel

# 1.2 Computer-System Organization

## modern(classical) computer system

- CPU + RAM(memory) + storage + I/O 디바이스 등이 bus로 연결됨.
- + 블루투스 + RF 등 무선 기기

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled.png)

- 현재 modern computer 라고 하면, 양자 컴퓨터, 신경망 컴퓨터, 네트워크 컴퓨터 등을 말한 다. (기존의 폰노이만 아키텍쳐를 따르지않는..)

## bootstrap program

- 컴퓨터를 켰을 때(power-on) 가장 먼저 실행되는 프로그램 → 부팅 프로그램

    ⇒ 운영체제를 메모리에 로드하는 역할

## Interrupts

- 하드웨어가 언제나 interrupt 할 수 있다. (키보드/마우스를 누르는 등)

    → system bus 를 통해 CPU로 전달됨.

## 폰노이만 아키텍쳐

- instruction-execution cycle
    - 명령어를 fetch 하고 → 실행하는 과정을 반복
    - 명령어 레지스터(IR, instruction register)에 명령어를 저장

## Storage

- register → cache → main memory(RAM) → solid-state disk(SSD) → hard disk(HDD) → optical disk → magnetic tapes
- → 저장용량이 커짐
- ← 접근 속도가 빨라짐

## I/O Structure

- 대부분의 OS code는 I/O 디바이스 컨트롤을 한다.
    - kernel 개발은 안정화가 된 상태
    - 대부분 디바이스 컨트롤러를 개발한다.
- ex) S20 → S21 을 만들때, 더 좋은 카메라 디바이스를 추가하는 등 디바이스 컨트롤러를 추가함

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%201.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%201.png)

- 디바이스에 따라 CPU를 거처 처리되는 경우도 있지만, memory에 직접 접근하기도 한다.
    - DMA : Direct Memory Access

# 1.3 Computer System Architecture

## Symmetric multiprocessing (SMP)

- 메모리는 한개 + 여러 개의 CPU를 사용
    - ex) 슈퍼컴퓨터

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%202.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%202.png)

## Multi-core design

- CPU 여러 개를 부착하는 것은 많은 비용이 들어간다.
- 하나의 CPU안에 CPU core가 여러 개 들어가 있는 구조

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%203.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%203.png)

# 1.4 Computer System Operations

## Multiprogramming

- 여러 개의 프로그램을 동시에 메모리에 올려두고, 동시에 수행
- 메모리에 여러개의 프로세스가 올라가 있음
- 장점 : CPU 사용효율을 높일 수 있다

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%204.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%204.png)

## Multitasking (multiprocessing)

- CPU가 여러 개의 작업들(jobs)를 자주 전환하며 수행. → 시분할(time-sharing)
- 유저는 한번에 여러 개의 작업를 할 수 있다. → concurrency

    -parallel 과 구분해야함

- CPU 스케줄링
    - 목표 : CPU효율을 가장 좋게하는 선택 방법은?

## Two separate Mode of operations

- user mode AND kernel mode
- 각 모드가 맡은 역할이 있다.
    - user mode는 직접적으로 하드웨어를 제어할 수 없다.
    - kernel mode 가 직접적으로 하드웨어를 제어한다.
- 각 모드가 잘못된 실행을 하지 않도록 제어해주는 것도 OS의 역할

    ex)  user mode에서 system call을 하면, kernel mode에서 system call을 수행한다.(user mode에서는 system call을 처리할 수 없다.)

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%205.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%205.png)

# 1.7 Virtualization

## Virtualization

- 하드웨어가 여러 개의 OS를 돌릴 수 있도록
- VMM : Virtual Machine Manager
    - VMware, XEN, WSL등

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%206.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%206.png)

# 1.10 Computing Environments

## Computing Environments의 다양화

- Traditionmal Computing
- Moblie Computing
- Client-Server Computing : Web, 1:1 통신

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%207.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%207.png)

- Peer-to-Peer Computing : 음악/영화 파일공유, 비트토렌트, BitCoin(BlockChain), N:N 통신

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%208.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%208.png)

- Cloud Computing : AWS, Azure, GCP..

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%209.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%209.png)

- Real-Time Enbedded System : Vxworks, RTOS

# 2.1 Operating System Service

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%2010.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%2010.png)

# 2.2 User and Operating-System Interface

## 사용자가 OS에 interface 하기위한 방법

1. CLI : 명령어 기반 인터페이스
2. GUL : 마우스로 아이콘을 클릭하는 방식
3. Touch-Screen Interface

# 2.3 System calls

## System calls

- 응용프로그램이 OS에 interface하는 방법
- OS의 API(Application Progrmming Interface) == System call

## standard C library

- printf 를 사용하기만 하면 라이브러리가 System call 하여 처리해줌
    - 콘솔 open, 버퍼 전송, close 등의 과정을 일일히 명령하지 않고, 라이브러리가 처리해줌

![Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%2011.png](Chapter%201-2%20Introduction%20&%20OS%20Structures%20(2)%20a3b739afcf2f4fbfabdef1da22102e35/Untitled%2011.png)