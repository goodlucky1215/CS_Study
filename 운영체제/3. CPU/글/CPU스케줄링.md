[CPU bound job & I/O bound job]

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/3.%20CPU/%EC%82%AC%EC%A7%84/CPU%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81/bound%20job.png"
    width="600"
    height="400"
  />

- 이 그림은 x축이 CPU사용이고 y축이 I/O사용을 나타낸다. 즉 y축이 높은 것은 I/O를 길게 쓰고 CPU를 잘 사용하지 않는 프로그램이라는 것이고, x축으로 길게 뻗고 y축이 낮은 것으로 보아 CPU를 길게 쓰는 프로그램이라는 것이다.
- 프로세스는 CPU와 I/O 작업 둘 중의 작업을 하면서 생을 마감한다.  
  CPU bound job : CPU를 길게 쓰는 프로그램을 부르는 말. => 연산이 많은 작업 => CPU burst가 길게 나타남  
  I/O bound job : I/O를 길게 쓰는 프로그램을 부르는 말. => 대게 사람과의 interation이 많은 경우이다. => I/O burst와 CPU burst가 돌아가면서 나타남. ex) 사람이 타자를 치는 동안은 I/O burst, 그게 화면에 나타나도록 하는 건 CPU burst라고 생각하면 됨.

[CPU Scheduler & Dispatcher]

- CPU Scheduler란? Ready 상태의 프로세스 중에서 CPU를 줄 프로세스를 고른다.
- CPU의 제어권을 CPU Scheduler에 의해 선택된 프로세스에게 넘긴다. 이 과정이 context switch(문맥 교환)이고, Dipatcher가 context switch를 하는 역할을 해준다.

[CPU 스케줄링이 필요한 경우 4가지]

1. Running -> Blocked( ex) I/O 요청하는 시스템 콜)
2. Running -> Ready( ex) timer interrupt)
3. Blocked -> Ready( ex) I/O완료후 인터럽트 => 어떤 스케줄링인지에 따라 CPU를 제일 먼저 할당 받을 수도 있으니까)
4. Terminate(프로세스 종료)  
   *nonpreemptive(비선점, 강제로 빼앗지 않고 자진 반납) : 1,4  
   *preemptive(선점, 강제로 빼앗음) : 2,3

[Scheduling 성능 척도]  
CPU utilization(이용률) : CPU를 시간 안에 얼마나 이용했는지 => 퍼센트가 높을수록 좋음  
Throughput(처리량) : CPU가 일정 시간 안에 프로세스를 얼마나 완료했는지 => 많이 처리할수록 좋음  
Turnaround time(소요시간, 반환시간) : 프로세스가 CPU를 사용하고 기다린 모든 시간의 합 => 짧을수록 좋음  
Waiting time(대기시간) : 프로세스가 들어오고 끝날때까지 기다린 모든 시간의 합 => 짧을수록 좋음  
Response time(응답시간) : 프로세스가 최초로 CPU를 얻기까지 걸린 시간 => 짧을수록 좋음

[Scheduling Algorithm]

1. FCFS(First-Come First-Served)

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/3.%20CPU/%EC%82%AC%EC%A7%84/CPU%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81/FCFS.png"
    width="600"
    height="400"
  />

- nonpreemptive
- 프로세스가 도착한 순서대로 받음.
- 그림과 다르게 P2, P3, P1 순서대로 처리했다면?  
  Waiting time은 P1= 6, P2 = 0, P3 = 3  
  Average Waiting time은 (6+0+3)/3 = 3
- 단점 : Convoy effect가 발생  
  \*Convoy effect란? 오래걸리는 프로세스가 먼저 실행될 경우 짧은 프로세스들이 오래동안 기다려야하는 효과를 말함.

2. SJF(Shortest Job First)

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/3.%20CPU/%EC%82%AC%EC%A7%84/CPU%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81/SJF.png"
    width="600"
    height="400"
  />  
(SJF그림)

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/3.%20CPU/%EC%82%AC%EC%A7%84/CPU%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81/SRTF.png"
    width="600"
    height="400"
  />  
(SRTF그림)

- CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄
- Average Waiting time이 모든 Scheduling Algorithm 중에서 가장 짧다. 특히 preemptive한 SRTF가 제일 짧다.
- 2가지가 다 가능  
  만약 CPU를 사용중이 프로세스의 CPU burst가 2초 남은 상황에서
  CPU burst가 1초인 새로운 프로세스가 들어왔다면?

1. nonpreemptive : 일단 CPU를 사용중인 프로세스가 있으면 끝까지 사용.
2. preemptive : 새로운 프로세스가 CPU를 뺏어감. 이를 **Shortest-Remaining-Time-First(SRTF)**라고 부른다.

- 단점

1. Starvation => CPU burst가 긴 프로세스는 영원히 CPU를 사용하지 못할 수가 있다.
2. 프로세스들의 CPU burst time을 알 수가 없다. => 추정(estimate)만이 가능  
   과거의 CPU burst time으로 예측함.  
   예측하는 식 => (4-3강의의 30분~다시보면 됨)

3) Priority Scheduling

- 우선순위가 높은 프로세스부터 CPU할당.
- 2가지가 다 가능  
  만약 우선순위가 5인 프로세스가 CPU를 할당받아서 사용하고 있는데 우선순위가 3인 새로운 프로세스가 들어온다면?

1. nonpreemptive : 일단 CPU를 사용중인 프로세스가 있으면 끝까지 사용.
2. preemptive : 새로운 프로세스가 CPU를 뺏어감.

- 단점  
  Starvation => 우선순위가 낮은 프로세스는 영원히 CPU를 사용하지 못할 수가 있다.
- Starvation 해결방안  
  Aging : 프로세스가 기다린 시간이 많을 수록 우선순위를 높여주는 방법

4. Round Robin

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/3.%20CPU/%EC%82%AC%EC%A7%84/CPU%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81/RR.png"
    width="600"
    height="400"
  />

- 각 프로세스에게 동일한 크기의 시간을 할당에서 CPU를 사용
- 이 할당 시간이 끝나면 프로세스는 선점(preemptive) 당하고 다시 ready queue의 제일 뒤에 가서 다시 줄을 선다.
- Response time이 짧은게 장점. => 기다리는 시간이 제한적이니까
- 짧은 job과 긴 job을 가진 프로세스들이 섞여있을 때 사용해야 효과적임. => 비슷비슷한 job을 가진 프로세스들이 섞여있을 때 사용하면 Average Waiting time이 길어짐.
- 문제  
  할당 시간이 너무 길면? FCFS  
  할당 시간이 너무 짧으면? Context switch 오버헤드가 커진다.

Q. RR스케줄링을 쓴다고 가정할때 기다리는 시간과 평균 기다리는 시간은?

- 프로세스 p1, p2, p3의 순서로 들어왔고, 각각의 CPU사용시간 p1은 24, p2는 3, p3은 3 (진입시간은 p1은 0, p2는 1, p3은 2, CPU할당 시간을 2이라고 가정)  
  => 기다린 시간 : p1은 2+2 = 4, p2는 (2-1)+4 = 5, p3은 (2-2)+3 = 3 이다. (기다린 시간에서 진입시간은 빼줘야한다.)  
  => 평균 기다린 시간은 : (4+5+3)/3 = 4 이다.

5. Multilevel Queue

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/3.%20CPU/%EC%82%AC%EC%A7%84/CPU%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81/Multilevel%20Queue.png"
    width="600"
    height="400"
  />

- Ready queue를 여러 개로 분할  
  foreground(interactive) / background(batch - no human interactive)
- 각 큐는 독립적인 스케줄링 알고리즘을 가짐  
  foreground - RR/ background - FCFS  
  \*long job은 Context switch가 자주 일어나는게 더 비효율적이라서 FCFS
- 2가지 방법

1. 모든 foreground를 마치고, background를 실행 => background Stavation가능성이 있음.
2. CPU time을 적절한 비율로 할당 ex) foreground에는 80%, background에는 20%

6) Multilevel Feedback Queue

<p align="center">
  <img
    src="https://github.com/goodlucky1215/CS_Study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/3.%20CPU/%EC%82%AC%EC%A7%84/CPU%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81/Multilevel%20Feedback%20Queue.png"
    width="600"
    height="400"
  />

- 프로세스가 다른 큐로 이동이 가능(위의 Multilevel Queue의 그림과 같이 여러개의 큐가 존재할 때 프로세스들이 이동이 가능하다는 거임.)
- Aging과 같이 방식으로 구현 가능
  ex) 그림에서와 같이 프로세스가 quantum이 8인 큐일 때, quantum이 8인 큐 안에 끝나지 않으면 quantum이 16인 아래 큐로 강등이 된다. quantum이 16인 아래 큐로 강등 되고도 처리되지 못하면 FCFS로 강등이 된다.

7. Thread Scheduling

- Local Scheduling
  User level thread의 경우 프로그램이 내부적으로 어떤 thread를 Scheduling할지를 결정(운영체제가 thread의 존재를 모르니까)
- Global Scheduling
  Kernel level thread의 경우, 운영체제가 어떤 thread를 Scheduling할지를 결정(운영체제가 thread의 존재를 아니까)

[Algorithm Evaluation] - 어떤 알고리즘이 좋은지 확인하는 3가지 방법

1. Queueing models : queue에 도착한 시간과 처리 시간 등의 수식을 통해서 성능을 이론적으로 측정 => 옛날 방식
2. Implementation(구현)&Measurement(성능 측정) : 실제 구현을 해서 측정
3. Simulation(모의 실험) : 알고리즘을 모의 프로그램으로 작성 후에,후에 후에 trace(데이터들)를 입력하여 돌려보는 방법
