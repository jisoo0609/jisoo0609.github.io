---
layout: post
title: "Process VS Thread"
date: 2023-12-05 13:32:20 +0900
description: 'CS: 프로세스 VS 스레드'
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [process, thread]
categories: [cs-study, computer-science]
---

# 프로세스와 스레드 차이

- 컴퓨터 프로그래밍 및 운영체제에서 사용되는 개념
- 다중 작업(multitasking) 환경에서 어떻게 작업을 조작하고 동시에 실행하는지를 설명하는데 사용

## 1. 프로세스와 스레드의 개념

### 프로세스

- 운영체제로부터 자원을 할당받는 작업의 단위
- 독립적인 실행 단위로 메모리 상에서 실행중인 프로그램
  - 프로그램을 실행시켜 정적인 프로그램이 동적으로 변하여 프로그램이 돌아가고 있는 상태
  - 메모리에 적재되고 CPU 자원을 할당받아 프로그램이 실행되고 있는 상태
  - 실행되어 작업 중인 컴퓨터 프로그램
- 최소 하나의 스레드를 보유하고 있음
  - 하나 이상의 스레드 가짐
- 각각의 프로세스는 운영 체제로부터 자체의 메모리 공간을 할당받음
  - 모든 프로그램은 운영체제로부터 메모리 공간을 할당받아야 실행될 수 있음
  - 운영체제가 메모리 공간을 할당해 줘야 실행됨
- 각 프로세스는 서로 완전히 독립되어 있으며, 다른 프로세스의 메모리에 직접 접근할 수 없음
  - 독립성 → 안정성 보장, 하나의 프로세스가 충돌하더라도 다른 프로세스에는 영향을 미치지 않도록 함
- 프로세스가 생성될 때마다 고유의 PCB 생성되고, 프로세스가 완료되면 PCB 제거됨
- 한 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신(IPC; Inter-Process Communication)을 사용해야 한다

### 스레드

- 프로세스가 할당받은 자원을 이용하는 실제 흐름의 단위
- 한 프로세스 내에서 동작되는 여러 실행의 흐름
  - 프로세스 내의 주소 공간이나 자원들을 같은 프로세스 내의 스레드끼리 공유하면서 실행
  - 하나의 프로세스 안에서 여러가지 작업들 흐름이 동시에 진행
  - 이러한 작업의 흐름들을 스레드 → 여러개 존재한다면 멀티(다중) 스레드(Multi Thread)

### 결론

- 프로세스는 독립적인 실행 환경 제공
- 스레드는 하나의 프로세스 내에서 실행되는 작은 실행 단위로 메모리 공유
- 프로세스 간 통신이 필요한 경우에는 프로세스 사용
- 자원 공유가 주요 고려 사항일 때는 스레드 사용

## 2. 프로세스와 스레드의 자원 구조

- 프로세스의 자원 구조
  - 각각 독립된 메모리 영역을 할당해 주기 때문에 프로세스는 다른 프로세스의 변수나 자료에 접근할 수 없다.

  ![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled.png)

  <details>
    <summary>✅ 자원 구조</summary>

    <ul>
      <li><strong>코드 영역</strong><br>
        프로그래머가 작성한 프로그램 함수들의 코드가 CPU가 해석 가능한 기계어 형태로 저장되어 있음
      </li>
      <li><strong>데이터 영역</strong><br>
        코드가 실행되면서 사용하는 전역 변수나 각종 데이터들이 모여있음
      </li>
      <li><strong>데이터 영역 세분화</strong>
        <ul>
          <li><code>.data</code>: 전역 변수 또는 static 변수 등 프로그램이 사용하는 데이터를 저장</li>
          <li><code>.bss</code>: 초기값 없는 전역 변수, static 변수가 저장</li>
          <li><code>.rodata</code>: <code>const</code> 같은 상수 키워드로 선언된 변수나 문자열 상수가 저장</li>
        </ul>
      </li>
      <li><strong>스택 영역</strong><br>
        지역 변수와 같은, 호출한 함수가 종료되면 되돌아올 임시적인 자료를 저장하는 독립적인 공간<br>
        함수의 호출과 함께 할당되고, 함수 호출이 완료되면 소멸함<br>
        스택 영역을 초과하는 경우 → <strong>stack overflow 에러</strong> 발생
      </li>
      <li><strong>힙 영역</strong><br>
        생성자, 인스턴스와 같은 <strong>동적으로 할당되는 데이터들</strong>을 위해 존재하는 공간<br>
        사용자에 의해 메모리 공간이 <strong>동적으로 할당되고 해제됨</strong>
      </li>
    </ul>

    <blockquote>
      💡 코드 영역과 데이터 영역은 <strong>정적 영역</strong>이지만, 스택 영역과 힙 영역은 프로세스 실행 중 크기가 바뀔 수 있는 <strong>동적 영역</strong>입니다.
    </blockquote>
  </details>

- 스레드의 자원 구조
  - 스레드는 프로세스 내에서 각각 Stack만 따로 할당받고 Code, Data, Heap 영역은 공유
  - 별도의 stack을 가지고 있지만 heap 메모리는 고유하기 때문에 서로 다른 스레드에서 가져와 읽고 쓸 수 있음

  ![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%201.png)

## 3. 자원 공유

### 프로세스의 자원 공유

- 프로세스는 메모리 별도의 주소 공간에서 실행되기 때문에, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없음 → 특정 방법을 통해 다른 프로세스 정보에 접근해야함
1. IPC(Inter-Process Communication) 사용
2. LPC (Local inter-Process Communication) 사용
3. 별도로 공유 메모리를 만들어 정보를 주고받도록 설
- 프로세스 자원 공유는 자원 부담이 크다는 단점 존재
  - CPU 레지스터 교체 뿐만 아니라 RAM과 CPU 사이의 캐시 메모리까지 초기화
  - 다중 작업이 필요한 경우 스레드 이용

## 4. 프로세스와 스레드의 동시 실행 원리

- 컴퓨터 내부적으로 프로세스와 스레드를 동시에 처리하는 멀티태스킹 기술

### 멀티 태스킹 (multi tasking)

- 여러 작업을 동시에 실행하는 능력
- 프로그램의 작업을 아주 작은 단위로 번갈아 가며 처리하면서, 작업 응답 시간 최소화
- 하나의 CPU에서 여러개의 작업을 수행
- 문맥 교환 (Context Switching)
  - 현재 진행하고 있는 Task(Process, Thread)의 상태를 저장하고 다음 진행할 Task의 상태 값을 읽어 적용하는 과정
  - 잘게 나눠진 프로세스끼리 작업이 스위칭
  - 프로세스 작업이 잘게 나뉘어져 교대로 실행

  <details>
    <summary> 문맥 교환의 장점</summary>

    <ul>
      <li><strong>1. 빠른 반응성 제공</strong><br>
        → 여러 프로세스를 번갈아 실행하여 사용자에게 빠른 응답을 제공함
      </li>
      <li><strong>2. 문제 해결 용이성</strong><br>
        → 하나의 프로세스에 문제가 생겨도, 다른 프로세스에 영향을 주지 않고 독립적으로 운영 가능
      </li>
      <li><strong>3. 멀티 프로세서의 효율적인 활용</strong><br>
        → 여러 프로세스가 병렬적으로 실행되며, 시스템 자원의 활용도를 높임
      </li>
      <li><strong>4. 프로세스의 우선순위 조절</strong><br>
        → 우선순위가 높은 프로세스가 더 많은 CPU 시간을 할당받을 수 있어, 중요한 작업이 먼저 처리됨
      </li>
    </ul>
  </details>

<details>
  <summary> CPU의 작업 처리 방식</summary>

  <ul>
    <li><strong>병렬성 (Parallelism)</strong><br>
      → 여러 개의 코어를 활용하여 <strong>여러 개의 프로세스 또는 스레드를 동시에 실행</strong>함으로써 병렬로 작업 수행
    </li>
    <li><strong>동시성 (Concurrency)</strong><br>
      → 둘 이상의 작업을 <strong>번갈아 빠르게 처리</strong>하여 마치 동시에 실행되는 것처럼 보이게 함<br>
      → 실제로는 하나의 CPU에서 <strong>작업을 빠르게 전환(context switching)</strong> 하며 처리
    </li>
  </ul>
</details>

### 멀티 프로세싱

- 여러개의 CPU 코어가 동시에 작업을 처리하는 것
- 여러개의 프로세서가 병렬로 작업 수행하므로 단일 프로세스보다 빠른 처리 속도 보장
- 여러개의 CPU가 각각의 작업을 처리

### 멀티 태스킹과 멀티 프로세싱의 차이

<details>
  <summary> 멀티 태스킹과 멀티 프로세싱의 차이</summary>

  <ul>
    <li><strong>멀티 태스킹 (Multitasking)</strong><br>
      → 하나의 CPU(코어)에서 <strong>여러 작업(프로세스 또는 스레드)을 번갈아가며 수행</strong>하는 방식<br>
      → 실제로 동시에 실행되는 것은 아니지만, <strong>빠른 전환</strong>을 통해 사용자는 동시에 실행되는 것처럼 느낄 수 있음<br>
      → 예시: 한 컴퓨터에서 음악을 들으며 웹 서핑하는 경우
    </li>
    <li><strong>멀티 프로세싱 (Multiprocessing)</strong><br>
      → <strong>둘 이상의 CPU(코어)</strong>를 사용해 <strong>여러 작업을 실제로 동시에 처리</strong>하는 방식<br>
      → 각 프로세스가 별도의 CPU에서 동시에 실행되므로 <strong>진정한 병렬 처리</strong>가 가능<br>
      → 예시: 멀티 코어 CPU가 각각 다른 애플리케이션을 동시에 처리
    </li>
  </ul>

  <blockquote>
    💡 멀티 태스킹은 <strong>CPU 하나에서 시간 분할</strong>,<br>
    멀티 프로세싱은 <strong>여러 CPU를 통한 병렬 실행</strong>입니다.
  </blockquote>

  <br/>

  <table>
    <thead>
      <tr>
        <th>항목</th>
        <th>멀티 태스킹 (Multitasking)</th>
        <th>멀티 프로세싱 (Multiprocessing)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>정의</td>
        <td>하나의 CPU로 여러 작업을 빠르게 전환하여 실행</td>
        <td>둘 이상의 CPU로 여러 작업을 병렬로 실행</td>
      </tr>
      <tr>
        <td>실행 방식</td>
        <td>시분할 방식 (Context Switching)</td>
        <td>병렬 실행 (동시에 여러 작업 수행)</td>
      </tr>
      <tr>
        <td>CPU 필요 수</td>
        <td>1개</td>
        <td>2개 이상</td>
      </tr>
      <tr>
        <td>장점</td>
        <td>자원을 효율적으로 활용 가능</td>
        <td>처리 속도 향상, 안정성 높음</td>
      </tr>
      <tr>
        <td>예시</td>
        <td>음악 재생 + 웹 서핑</td>
        <td>워드 작업 + 영상 렌더링 (CPU 각각 담당)</td>
      </tr>
    </tbody>
  </table>
</details>

## 5. 프로세스 & 스레드의 생명 주기

### 프로세스 스케줄링

- 운영체제에서 CPU를 사용할 수 있는 프로세스를 선택하고, CPU를 할당하는 작업
- 프로세스의 우선순위, 작업량 등을 고려해 효율적으로 배치
- 운영체제는 CPU를 효율적으로 사용하며 시스템 전반적인 성능 향상
- FCFS(First-Come, First-Served), SJF(Shortest-Job-First), Priority, RR(Round-Robin), Multilevel Queue 등의 알고리즘으로 동작

### 프로세스 상태

- 프로세스가 실행되는 동안 변경되는 고유 상태
- 프로세스 상태 전이
  - 프로세스가 실행되는 동안 OS에 의해 상태가 변경되는 것

  ![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%202.png)

### 프로세스 컨텍스트 스위칭 (Process Context Switching)

- CPU가 한 프로세스에서 다른 프로세스로 전환할 때 발생하는 일련의 과정
- 여러개의 프로세스를 번갈아 실행하며 CPU 활용률을 높이기 위해 필요
- 동작 중인 프로세스가 대기하며 해당 프로세스의 상태(Context)를 보관하고, 대기하고 있던 다음 프로세스가 동작하면서 이전에 보관했던 프로세스의 상태를 복구하는 작업
- 컨텍스트 스위칭이 일어날때 다음번 프로세스는 스케줄러가 결정

### PCB(Process Control Block)

- 운영체제에서 프로세스를 관리하기 위해 해당 프로세스의 정보 상태를 담고 있는 자료구조
- 프로세스 컨텍스트 스위칭 시 기존 프로세스의 상태를 어딘가 저장해 둬야 다음에 똑같은 작업을 이어서 할 수 있음.
- 프로세스 스케줄링을 위해 프로세스에 대한 모든 정보를 저장하는 임시 저장소

![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%203.png)

<details>
  <summary> PCB 구조 (Process Control Block)</summary>

  <ul>
    <li><strong>포인터 (Pointer)</strong><br>
      → 프로세스의 현재 위치를 저장하는 포인터 정보
    </li>
    <li><strong>프로세스 상태 (Process state)</strong><br>
      → 프로세스의 상태 정보를 저장 (예: 생성 New, 준비 Ready, 실행 Running, 대기 Waiting, 종료 Terminated)
    </li>
    <li><strong>프로세스 아이디 (Process ID, PID)</strong><br>
      → 각 프로세스를 구분하기 위한 고유한 식별자
    </li>
    <li><strong>프로그램 카운터 (Program counter)</strong><br>
      → 프로세스가 다음에 실행할 명령어의 주소를 포함
    </li>
    <li><strong>레지스터 (Register)</strong><br>
      → 누산기, 베이스 레지스터, 범용 레지스터 등 CPU의 현재 상태 정보 저장
    </li>
    <li><strong>메모리 제한 (Memory Limits)</strong><br>
      → 프로세스가 사용할 수 있는 메모리 범위 등의 메모리 관리 정보
    </li>
    <li><strong>열린 파일 목록 (List of open files)</strong><br>
      → 해당 프로세스에서 열린 파일들의 목록
    </li>
  </ul>

</details>

- 프로세스가 생성될 때마다 고유의 PCB 생성되고, 프로세스가 완료되면 PCB 제거됨
- PCB에 담긴 프로세스 고유 정보를 통해 프로세스 관리
- 프로세스의 실행 상태 파악. 우선순위 조정, 스케줄링 실행, 다른 프로세스와의 동기화 제어

### Context Switching 과정

- 두 개의 프로세스 간에 컨텍스트 스위칭 과정

![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%204.png)

### 스레드 스케줄링

- 운영체제에서 다중 스레드를 관리
- CPU를 사용할 수 있는 스레드를 선택하고 CPU를 할당하는 작업
- Round Robin, Priority-based scheduling, Multi-level Queue scheduling 등 알고리즘 사용
- 하나의 프로세스 내에서 다수의 스레드가 동작하는 형태

  → 스레드 간의 상호작용과 동기화 문제를 고려해야함

### 스레드 상태

![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%205.png)

### 스레드 컨텍스트 스위칭

- 멀티 스레딩 환경에서 스레드 간의 실행을 전환하는 기술
- 하나의 프로세스 내의 스레드들을 교환

![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%206.png)

### TCB (Thread Control Block)

- 각 스레드마다 운영체제에서 유지하는 스레드에 대한 정보를 담고 있는 자료구조

![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%207.png)

- 스레드 간의 자원 공유와 동기화도 TCB를 사용해 관리
  - 뮤텍스(mutex)
  - 세마포어(semaphore)

### 프로세스 컨텍스트 스위칭 vs 스레드 컨텍스트 스위칭

<details>
  <summary>프로세스 컨텍스트 스위칭 vs 스레드 컨텍스트 스위칭</summary>

  <p><strong>멀티태스킹 환경에서 여러 프로세스 또는 스레드를 동시에 실행하기 위한 기술</strong></p>

  <ol>
    <li><strong>TCB가 PCB보다 가볍다</strong><br>
      - <strong>속도</strong><br>
      - 스레드 컨텍스트 스위칭 &lt; 프로세스 컨텍스트 스위칭<br>
      - 프로세스의 스레드는 text, data, heap 영역 메모리를 공유하지만,<br>
        TCB는 stack 및 간단한 레지스터 포인터 정보만 저장
    </li>
    <li><strong>캐시 메모리 초기화 여부</strong><br>
      - CPU 캐시 메모리는 CPU와 메인 메모리 사이 위치<br>
      - 이미 읽은 데이터를 저장해 CPU가 다시 요구할 때 빠르게 전달<br><br>
      - <strong>프로세스 컨텍스트 스위칭</strong><br>
        - 다른 프로세스 실행 시 새로운 명령어 및 데이터 로드<br>
        - 캐시 메모리 초기화 → 부담 있음<br><br>
      - <strong>스레드 컨텍스트 스위칭</strong><br>
        - 같은 프로세스 내 스레드 간에 스택, 레지스터 정보만 변경<br>
        - 캐시 메모리 초기화 없음<br>
        - 단, 스레드가 다른 CPU 코어에서 실행될 때는 해당 코어 캐시에 컨텍스트 로드 필요
    </li>
    <li><strong>자원 동기화 문제</strong><br>
      - 공유 자원에 대한 적절한 동기화 메커니즘 필요<br><br>
      - <strong>스레드 컨텍스트 스위칭</strong><br>
        - 여러 스레드가 heap 영역 공유 데이터 접근 시 동기화 문제 발생 가능<br>
        - <strong>경쟁 조건 (Race condition)</strong><br>
        - 두 스레드가 동시에 변수 수정 시 컨텍스트 스위칭이 발생하면 변수 값이 잘못 업데이트될 수 있음<br><br>
      - <strong>프로세스 컨텍스트 스위칭</strong><br>
        - 기본적으로 독립된 공간이나, IPC 등 공유 자원 사용 시 경쟁 조건 발생<br>
        - 예: 여러 프로세스가 동시에 파일 시스템 접근해 파일 수정 시, 컨텍스트 스위칭으로 파일 내용 손상 가능
    </li>
  </ol>

</details>

## 6. 멀티 프로세스와 멀티 스레드

- 한 애플리케이션에 대한 처리 방식

### 멀티 프로세스

- 운영체제에서 하나의 운영 프로그램에 대하 동시에 여러 개의 프로세스를 실행할 수 있게 하는 기술
- 보통 하나의 프로그램에 대해 하나의 프로세스가 메모리에 실행되지만 부가적인 기능을 위해 여러개의 프로세스를 생성하는 것

![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%208.png)

<details>
  <summary>멀티 프로세스 vs 멀티 프로세서</summary>

  <ul>
    <li><strong>프로세스</strong><br>
      - 프로그램의 실행 상태<br>
      - 하나의 프로그램에서 여러 개의 프로세스를 실행하는 것
    </li>
    <li><strong>프로세서 (CPU 코어)</strong><br>
      - CPU의 코어<br>
      - 여러 개의 CPU 코어가 하나의 시스템에서 동시에 실행되는 것
    </li>
  </ul>

</details>

<details>
  <summary>멀티 프로세스의 장 단점</summary>

  <ul>
    <li><strong>장점</strong><br>
      - 여러개의 자식 프로세스 중 하나에 문제가 발생하면 그 자식 프로세스만 죽는 것 이상으로 다른 영향 확산 X
    </li>
    <li><strong>단점</strong><br>
      - Context Switching 과정에서 캐시 메모리 초기화 등 작업이 진행되고 많은 시간이 소모되는 등 오버헤드가 발생<br>
      - 프로세스는 각각의 독립된 메모리 영역을 할당받았기 때문에 프로세스 사이에서 공유하는 메모리가 없어 Context Switching이 발생하면 캐시에 있는 모든 데이터를 리셋하고 다시 캐시 정보 불러와야
    </li>
  </ul>

</details>

### 멀티 스레드

- 하나의 프로세스 안에 여러개의 스레드가 있는 것
- 하나의 프로그램에서 두 가지 이상의 동작을 동시에 처리하도록 하는 행위가 가능
- 웹 서버가 대표적인 멀티 스레드 응용프로그램
  - 사용자가 서버 데이터베이스에 자료를 요청하는 동안 브라우저의 다른 기능을 이용할 수 있음
  - 하나의 스레드가 지연되더라도 다른 스레드는 작업 지속 가능

![Untitled](/assets/img/posts/cs-study/computer-science/process-vs-thread/Untitled%209.png)

<details>
  <summary> 멀티 스레드의 장단점</summary>

<h4>장점</h4>
  <ul>
    <li>프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리할 수 있음</li>
    <li>스레드 사이 작업량이 작아 Context Switching이 빠름</li>
    <li>프로세스 내 메모리를 공유하기 때문에 스레드 간 데이터 주고받기가 간단하여 시스템 자원 소모 감소</li>
  </ul>

<h4>단점</h4>
  <ul>
    <li>주의 깊은 설계가 필요</li>
    <li>까다로운 디버깅</li>
    <li>다른 프로세스에서 스레드 제어 불가 (프로세스 외부에서 스레드 제어 불가)</li>
    <li>멀티 스레드 자원 공유 문제 발생
      <ul>
        <li>동기화 문제</li>
        <li>교착 상태(데드락)</li>
      </ul>
    </li>
    <li>하나의 스레드에 문제가 발생하면 전체 스레드에 영향 가능</li>
  </ul>

</details>

### 멀티 프로세스 대신 멀티 스레드를 사용하는 이유

- 자원의 효율성 증대
  - 멀티 프로세스로 실행되는 작업을 멀티 스레드로 실행할 경우, 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리할 수 있음
  - 프로세스 간 Context Switching 시 단순히 CPU 레지스터 교체 뿐 아니라 RAM 과 CPU 사이 캐시 메모리에 대한 데이터까지 초기화되므로 오버헤드가 크기 때문
  - 스레드는 프로세스 내 메모리를 공유하기 때문에 독립적인 프로세스와 달리 스레드 간 데이터를 주고받는 것이 간단해지고 시스템 자원 소모가 줄어들게 됨
- 처리 비용 감소 및 응답 시간 단축
  - 프로세스 간 통신 (IPC) 보다 스레드 간 통신이 비용이 적어 부담이 감소됨
  - 프로세스 간 전환 속도보다 스레드 간 전환 속도가 빠름

### 멀티 프로세스 VS 멀티 스레드

| 구분         | 멀티 프로세스                                         | 멀티 프로세서                                         |
|--------------|-------------------------------------------------------|-------------------------------------------------------|
| 정의         | 하나의 프로그램에서 여러 개의 프로세스를 실행         | 여러 개의 CPU 코어가 하나의 시스템에서 동시에 실행   |
| 특징         | 프로세스는 프로그램 실행 상태                         | 프로세서는 CPU의 코어                                  |
| 실행 방식    | 시분할 방식 또는 병렬 실행 (코어 수에 따라 다름)       | 여러 CPU 코어에서 병렬로 동시에 작업 수행             |
| 자원 공유    | 각 프로세스는 독립된 메모리 공간을 가짐               | CPU 코어들은 시스템 자원을 공유                         |
| 장점         | 프로세스 간 독립성으로 안정성 높음                    | 진정한 병렬 처리로 성능 향상                            |
| 단점         | 프로세스 간 통신 및 Context Switching 오버헤드 있음  | 하드웨어 비용 증가 및 설계 복잡도                       |
