[교착상태(DeadLock)]

- 일련의 프로세스들이 서로가 가진 자원을 기다리면 block된 상태
- Resource(자원) : 하드웨어, 소프트웨어 등을 포함하는 개념

[DeadLock 발생 조건4가지]

1. Mutual Exclusion(상호 배제) : 매 순간 하나의 프로세스만이 자원을 사용할 수 있다.
2. No Preemption(비선점) : 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음
3. Hold and wait(보유 대기) : 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음
4. Circular wait(원형 대기) : 자원을 기다리는 프로세스 간에 사이클이 형성되어야 함.
   => 이 4가지 조건이 모두 만족해야만 데드락이 발생한다.

[DeadLock 그림1,2]

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/4.%20DeadLock/%EC%82%AC%EC%A7%84/deadlocks/deadLock.png"
    width="600"
    height="400"
  />

- 네모는 자원이고 그 안에 동그라미는 자원의 갯수, 원은 프로세스를 의미한다.
- 그림1)

1. Mutual Exclusion => 각각의 자원에 하나의 프로세스만 할당되어 있다 (0)
2. No Preemption => 각 프로세스가 어떤 자원도 강제로 빼앗기지 않았다. (0)
3. Hold and wait => P1은 P2가 가진 자원 R1을 기다리고 있고 P2는 P3가 가진 R3자원을 기다리고 있고 P3는 R2자원을 기다리고 있다. (0)
4. Circular wait => 원형 모형을 그리고 있다. (0)  
    => 따라서 그림1은 DeadLock 상태이다.  
   Q. 만약 R2의 자원이 3개라면? 프로세스P3는 자원을 획득할테니깐 DeadLock 상태가 아니다. **DeadLock조건을 모두 만족한다고해서 꼭 DeadLock이 되는 것은 아니다!**

- 그림2)

1. Mutual Exclusion => 각각의 자원에 하나의 프로세스만 할당되어 있다 (0)
2. No Preemption => 각 프로세스가 어떤 자원도 강제로 빼앗기지 않았다. (0)
3. Hold and wait => P1은 P2가 가진 자원 R1을 기다리고 있고 P3는 R2자원을 기다리고 있다. (0)
4. Circular wait => P2와 P4를 보면 사이클을 돌고 있지 않다. (X)  
   => 따라서 그림2는 DeadLock 상태가 아니다.

[DeadLock 처리 방법]

1. DeadLock이 안생기게 애초에 방지 2가지 방법  
   (1) DeadLock Prevention : DeadLock 발생 조건4가지 중 하나를 무산시켜서 방지함.  
   a. Mutual Exclusion : 이걸 막는 건 말이 안됨. 애초에 공유 문제로 인해서 DeadLock이 나온거니(X)  
   b. No Preemption : 자원을 빼앗아서 사용한다.  
   c. Hold and wait : 내가 자원을 기다려야하는 경우면, 내가 가지고 있던 자원도 그냥 갖지 않는 방법 => 그러면 여러 자원을 취해야하는 다른 프로세스가 먼저 다 자원을 갖을테니까  
   d. Circular wait : 모든 자원에 할당 순서를 정한다. 예를 들어 프로세스1은 자원1을 먼저 할당 받아야만 자원5를 할당받을 수 있게 한다.  
   => 자원의 효율을 감소 시키거나, Starvation시킬 수 있다.

   (2) DeadLock Avoidance : 점선(프로세스가 자원을 요청할 수도 있는 상황(?))을 포함했을 때, Circular wait가 되지 않으면 프로세스에게 자원을 할당한다.

a. DeadLock Avoidance는 프로세스들이 평생에 쓸 자원을 미리 알고 있다고 가정하고 사용.

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/4.%20DeadLock/%EC%82%AC%EC%A7%84/deadlocks/DeadLock%20Avoidance.png"
    width="600"
    height="300"
  />

- 점선은 프로세스들이 자원을 요청할 수 있음을 뜻한다.
- 실선은 이미 자원을 점유하거나 요청하는 것임을 뜻한다.
- 1번 그림  
  P1과 P2는 이 자원R2을 요청했다.  
  자원R2을 점유하고 있는 P1과 자원을 요청하고 있는 P2이다.
- 2번 그림  
  P2가 자원R2를 요청했다. => 그림3번 시, Circular wait문제가 있다.  
  그러므로 DeadLock Avoidance는 자원을 P2에게 주지 않고 기다린다. => 기다리다보면, P1이 자원R2를 먼저 얻을테고, 그러면 P2는 P1이 끝나면 자원을 사용할 수 있게 된다.
- 3번 그림  
  P2가 자원 R2를 점유하면 Circular wait가 될 문제를 가진다.

b. 자원 안에 여러 자원을 가지고 있을 경우, Banker's Algorithm을 사용.

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/4.%20DeadLock/%EC%82%AC%EC%A7%84/deadlocks/Banker's%20Algorithm.png"
    width="600"
    height="400"
  />

- Allocation은 각 프로세스가 취한 자원의 갯수
- Max는 프로세스가 최대로 요청 할 수 있는 자원의 갯수
- Avaliable은 각 자원에 남은 자원의 갯수
- Need는 필요한 자원의 갯수  
  예1) P0은 C가 3개 필요한데 남아있는 자원은 2개이다. 받을 수 있을까?  
  => 받을 수 없다. Avaliable에 비록 여유가 있더라도 P0이 최대로 요청했을 때, 만족이 되지 않으므로 DeadLock이 발생할 수 있기 때문이다.  
  예2) P1의 요청은 받아준다.  
  예3) P2의 요청은 받아주지 않는다.  
  예3) P3의 요청은 받아준다.  
  예3) P4의 요청은 받아주지 않는다.  
  예4) P1과 P3의 요청을 받아주면 이 두 프로세스가 나가게 되면 P0의 요청을 처리할 수 있게 된다.

2. DeadLock이 그냥 지금 있는지 없는 지를 보는 방법 2가지

(1) DeadLock Detection and Recovery

A. DeadLock Detection방법

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/4.%20DeadLock/%EC%82%AC%EC%A7%84/deadlocks/DeadLock%20Detection%20and%20recovery1.png"
    width="600"
    height="400"
  />
(그림1)

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/4.%20DeadLock/%EC%82%AC%EC%A7%84/deadlocks/DeadLock%20Detection%20and%20recovery2.png"
    width="600"
    height="400"
  />
(그림2)  
- Allocation은 각 프로세스가 취한 자원의 갯수  
- Request는 프로세스가 요청한 자원의 갯수  
- Avaliable은 각 자원에 남은 자원의 갯수 => 여유자원이 있으면 그냥 무조건 줌  
  Request를 하지않은 프로세스들의 Allocation된 자원 갯수들을 계산을 해서 Request를 하는 프로세스들에게 줄 수 있는지를 가정을 한다. 그래서 만약 이 가정에 자원의 부족함이 없다면 DeadLock아니라고 하는 것이다.  
  DeadLock Detection and recovery그림1은 DeadLock이 아니지만,  
  DeadLock Detection and recovery그림2는 DeadLock이다.  
  => 그림2같은 경우 Request가 없는 프로세스는 P0뿐이다. 그런데 B자원을 하나 반납한다고 해도, 다른 프로세스들을 충족해 줄 수가 없기 때문이다.

Q) DeadLock Detection방법을 사용했을 때, DeadLock이라면 어떻게 해야할까?  
=> DeadLock Recovery를 해야한다.

B. DeadLock Recovery 2가지 방법

a. DeadLock과 연루된 프로세스를 운영체제가 죽이는 방법

- 모든 프로세스를 다 죽이는 방법.
- 프로세스를 하나씩 죽이면서 DeadLock인지를 확인하면서 프로세스를 죽이는 방법.

b. 자원을 빼앗는 방법

- 프로세스의 자원을 빼앗음
- 비용이 적게 드는 프로세스의 자원부터 빼앗음(자원을 적게 뺏으면 되는 프로세스) => Starvation의 문제가 생길 수 있다. 희생의 횟수를 고려해서 처리 해야한다.

(2) DeadLock Ignorance

- DeadLock에 대해 아무런 조치를 취하지 않는다. => DeadLock은 매우 드물게 일어나기 때문이다.
- **그러나 현재에는 대부분 이 방법을 사용한다. => 그렇다면 DeadLock이 발생한다면? 사람이 컴퓨터를 강제종료하거나 프로세스를 죽이는 방법으로 처리한다.**

---

출처: http://www.kocw.net/home/cview.do?lid=387bf8c00f64248e
