# 프로세스 간 통신

프로세스에는 2가지 종류가 있다.

* independent process
    * 다른 프로세스와 데이터 공유X
* cooperating process
    * 다른 프로세스와 데이터를 공유

## IPC란?
* IPC는 inter process communication으로써 Cooperating process는 IPC mechanism이 필요하다. 서로 데이터를 공유하기 위한 방식이다.

IPC 방법에는 2가지가 있는데 
* Shared Memomry
* Message Passing

~image

## Producer-Consumer Problem
쉽게 생각해서 한 프로세스는 생산해서 넣고 다른 프로세스는 그걸 가져다 쓰는 구조를 말한다.

### shared memory 사용 방법
producer와 consumer가 동시에 동작하는 것을 allow 한다.
item을 넣을 수 있는 buffer를 제공하고, producer는 채우고, consumer는 비운다.

위의 동작들은 os가 제공하는 message passing 을 통해 가능하다.
os가 제공하는 message passing 방법으로는 또 두가지가 있는데
Send, receive 가 바로 그것이다.

Communication Link
P와 Q라는 프로세스가 통신을 하고 싶으면, send receive를 무조건 사용해야하는데, 이 communication link는 여러가지 방법을 통해 구현할 수 있다. 

* direct or indirect communication
    * direct의 경우 sender와 receiver가 명시적으로 정해져있다.
    * Send(P,Message), Receive(Q,Message)
    * indirect의 경우 sender는 특정 mailbox로 보내고, 그 mailbox를 구독하는 프로세스도 있다. 
    * mailbox를 같이 공유하기 시작해야 link가 생긴다. 또 여러개의 process와 공유할 수 있다.복잡한 communi

* synchronous and asynchronous communication
    * 보내면 바로 받게 되는것이 sync, 보내는 것과 받는것이 자유로운 모델이 async
    * blocking은 sender가 receive동작이 끝날때까지 기다리고 있는것이고, non blocking 은 기다리지않고 시작하는 것이다.
    
* automatic or explicit buffering
    * 


