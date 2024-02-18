---
layout: post
title: "CS: CPU Scheduling"
date: 2023-12-18 13:32:20 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [CPU Scheduling]
---
---
# CPU 스케쥴링

## 1. CPU 스케줄링

- 운영체제가 CPU를 사용하려고 하는 프로세스들 사이의 우선순위 관리하는 작업
    - 자원을 어떤 프로세스에 얼마나 할당하는지 정책을 만듦
- CPU 처리율, 이용률 증가시키고 오버헤드, 응답시간, 반환시간 최소화하기 위한 기법
- 특정 프로세스가 적합하게 실행되도록 프로세스 스케줄링에 의해 프로세스 사이에서 CPU 교체가 일어남

### CPU(프로세스) 스케줄링 주요 용어

| 용어 | 설명 |
| --- | --- |
| 서비스 시간 | 프로세스가 결과를 산출하기까지 소요되는 시간 |
| 응답 시간 (Response Time) | 프로세스들이 입력되어 서비스를 요청하고, 반응하기 시작할 때까지 소요되는 시간 |
| 반환 시간 (Turnaround Time) | 프로세스들이 입력되어 수행하고 결과를 산출하기까지 소요되는 시간                         반환시간 = 대기시간 + 수행시간        |
| 대기시간 | 프로세스가 프로세서에 할당되기까지 큐에 대기하는 시간                                             프로세스가 도착 즉시 프로세서에 할당되면 대기시간은 ‘0‘ 이 됨 |
| 평균 대기시간 | 프로세스가 대기 큐에서 대기하는 평균 시간                                                                  대기시간이 ‘0’인 프로세스도 평균 대기시간에 합산하여 결과 도출 |
| 종료시간 | 요구되는 Processing Time을 모두 수행하고 종료된 시간 |
| 시간 할당량 | 한 프로세스가 프로세서를 독점하는 것을 방지하기 위해 서비스되는 시간 할당량 |
| 응답률 | (대기시간 + 서비스 시간) / 서비스 시간                                                                          HRN (Highest Responce ratio Next) 스케줄링에서 사용                                                  HRN 스케줄에서 응답률이 높으면 우선순위가 높다고 판단 |

## 2. CPU 스케줄링의 유형

- 규모에 따라 장기, 중기, 단기 스케줄링으로 구분됨

### 장기 스케줄링 (Long-term scheduler)

- 메모리에 저장된 잠재적인 프로세스 중 어떤 것들을 실행할지를 결정
- 장기스케줄링의 결정에 따라 시스템 내의 프로세스 총 개수(degree of multiprogramming)가 결정
- 디스크나 메인 메모리에 있는 프로그램을 선정하여 입출력 큐로 보내거나 메모리에 적재
- 분 단위에서 시간 단위로 측정
- 최근 운영체제에서는 보통 존재 X → 프로그램을 실행하면 곧바로 ready 상태에 돌입

### 중기 스케줄링 **(Medium-Term Scheduling)**

- 이미 활성화가 된 프로세스들에 대한 관리
- 시스템의 과부하를 막기 위해 활성화된 프로세스들의 중지 여부를 결정하여 활성화된 프로세스 수 조절
- 메모리에 있는 프로세스 중 일부를 디스크로 스왑아웃 or 디스크에 있는 프로세스를 메모리로 스왑인하여 프로세스의 수를 제어
- 초 단위에서 분 단위로 측정
- 중기 스케줄링에 의해 중지된 프로세스는 보류상태가 됨

### 단기 스케줄링 (**Short-Term Scheduling)**

- 현재 메모리에 적재된 프로세스(CPU를 할당받은 프로세스) 중 어떤 것을 다음에 실행할지 결정
- 가장 작은 단위의 스케줄링
- 단기 스케줄러가 어떤 기준에 따라 프로세스를 선택하는 과정 → 스케줄링 알고리즘, 어느정도 자원을 배분(Time slice 와 관련)할지에 따라 시스템에 큰 영향
- 밀리초에서 마이크로초 단위로 측정

## 3. CPU 스케줄링 고려사항

### 선점형 스케줄링 (**Preemptive Scheduling**)

- 하나의 프로세스가 CPU를 차지하고 있을 때, 우선순위가 높은 다른 프로세스가 현재 프로세스를 중단시키고  CPU를 점유하는 스케줄링 방식
- 비교적 빠른 응답
- 대화식 시분할 시스템에 적합
    - 시분할 시스템
        - CPU 스케줄링과 다중 프로그래밍을 이용해 각 사용자들에게 컴퓨터 자원을 시간적으로 분할하여 사용할 수 있게 해주는 대화식 시스템
- 높은 우선순위 프로세스들이 들어오는 경우 오버헤드 초래
- 알고리즘
    - Round Robin
    - SRT
    - 다단계 큐
    - 다단계 피드백 큐
- 실시간 응답 환경, Deadline 응답 환경에서 활용

### 비선점형 스케줄링 (**(Non-preemptive Scheduling**)

- 한 프로세스가 CPU를 할당받으면 작업 종료 후 CPU 반환 시까지 다른 프로세스는 CPU 점유가 불가능한 스케줄링 방식
- 응답시간 예상이 용이
- 모든 프로세스에 대한 요구를 공정하게 처리
- 짧은 작업을 수행하는 프로세스가 긴 작업 종료시까지 대기
- 알고리즘
    - 우선순위
    - 기한부
    - FCFS
    - HRN
    - SJF

### CPU bound process / IO bound process

- 프로세스 상태 중 실제 작업이 일어나는 것 실행 상태, 대기 상태
    - 실행 상태: CPU 사용
    - 대기 상태: 입출력을 요청하여 완료되기를 기다리는 상태
- CPU 버스트
    - CPU 자원을 많이 사용하는 프로세스
    - CPU 버스트가 많은 프로세스 → CPU bound process
    - 계산이나 연산이 많이 필요한 작업을 수행하는 경우
    - CPU 활용도가 높을 때 성능 최적화
- I/O 버스트
    - 출력 작업에 많은 시간을 소비하는 프로세스
    - I/O 버스트가 많은 프로세스 → IO bound process
    - 파일 읽기/쓰기, 네트워크 통신, 데이터베이스 쿼리 등과 같은 입출력 작업
    - 입출력 대기 시간이 길 때 성능 최적화
- IO bound process를 먼저 실행상태로 옮기는 것이 일반적으로 효율적
- IO bound process의 경우 CPU를 할당받아도 금방 대기 상태로 빠지는 반면 CPU bound process의 경우 할당받은 CPU 시간을 전부 사용하는 경우가 많음
- 스케줄링 시 IO bound process의 우선순위를 CPU bound process보다 높이는 것이 시스템 효율 향상에 도움

### **전면 프로세스 / 후면 프로세스**

- 전면 프로세스는 현재 입출력을 사용하는 프로세스(ex) 브라우저)이며 사용자와 상호작용이 가능하지만 후면 프로세스는 사용자와 상호작용이 없는 프로세스(ex) 압축 프로그램)
- 사용자의 요구를 즉각 반응해 상호작용하는 전면 프로세스의 우선순위를 더 높게 산정
- 전면 프로세스가 후면 프로세스보다 CPU를 할당받을 확률 높음

### 프로세스 우선순위

- 커널 프로세스와 일반 프로세스 중 CPU 스케줄러는 보통 커널 프로세스를 높은 우선순위로 채택

## 4. 스케줄링 알고리즘

### 4 - 1) CPU 스케줄링 성능 척도

- 시스템 입장
    - CPU Utilization (CPU 이용률): 전체 시간 중 CPU가 일한 시간의 비율
    - Throughput (처리량): 단위 시간 당 처리량
- 사용자 입장
    - Waiting Time
    - Response Time
    - Turnaround Time

### 4 - 2) 분류 기준

- 단일 처리기 시스템
- 다중 처리기 시스템
- 작업의 특성에 따른 분류
    - 배치 시스템 (Batch Systems)
        - 대량의 작업을 일괄적으로 처리하는 시스템
        - FCFS와 같은 간단한 스케줄링 알고리즘 사용 가능
    - 대화형 시스템 (Interactive Systems)
        - 사용자와 상호작용이 필요한 시스템
        - 응답 시간이 중요한 경우 Round Robin 등의 선점형 알고리즘이 더 적합할 수 있음

## 단일 처리 시스템 (Single Processor Systems)

## 선점형 스케줄링 **(Preemptive Scheduling)**

### 라운드 로빈 (Round Robin)

- 프로세스는 같은 크기의 CPU 시간을 할당(시간 할당량)
- 프로세스가 할당된 시간 내에 처리 완료를 못하면 준비 큐 리스트의 가장 뒤로 보내지고 CPU는 대기 중인 다음 프로세스로 넘어가는 스케줄링 기법
- 균등한 CPU 점유시간을 가짐
- 시분할 시스템 사용
    - 시분할 시스템
        - CPU 스케줄링과 다중 프로그래밍을 이용해 각 사용자들에게 컴퓨터 자원을 시간적으로 분할하여 사용할 수 있게 해주는 대화식 시스템

### SRT (Shortest Remaining Time Firrst)

- 가장 짧은 시간이 소요되는 프로세스를 먼저 수행
- 남은 처리시간이 더 짧다고 판단되는 프로세스가 준비 큐에 생기면 언제라도 프로세스가 선점되는 스케줄링 기법
- 비 선점 방식의 스케줄링 기법에 선점 방식을 도입한 기법
- 짧은 수행 시간 프로세스를 우선 수행

### 다단계 큐 (Multi Level Queue)

- 작업들을 여러 종류 그룹으로 분할
- 여러 개의 큐를 이용하여 상위단계 작업에 의한 하위단계 작업이 선점 당하는 스케줄링 기법
- 각 큐는 자신만의 독자적인 스케줄링을 가짐
- 독립된 스케줄링 큐

### 다단계 피드백 큐 (Multi Level Feedback Queue)

- FCFS와 라운드 로빈 스케줄링 기법을 혼합한 것
- 새로운 프로세스는 높은 우선순위를 가지고 프로세스의 실행시간이 길어질수록 점점 낮은 우선순위 큐로 이동하고 마지막 단계는 라운드 로빈 방식을 적용하는 스케줄링 기법
- 입출력 위주와 CPU 위주인 프로세스의 특성에 따라 큐마다 서로 다른 CPU 시간 할당량을 부여

## 비선점형 스케줄링 **(Non-preemptive Scheduling)**

### 우선순위 (**Priority**)

- 프로세스별로 우선순위가 주어지고 우선순위에 따라 CPU를 할당하는 스케줄링 기법
- 동일 순위는 FCFS 적용
- 주요 / 긴급 프로세스에 대한 우선 처리
- 설정, 자원 상황 등에 따른 우선순위 지정

### 기한부 (Deadline)

- 작업들이 명시된 시간이나 기한 내에 완료되도록 계획하는 스케줄링 기법
- 요청에 명시된 시간 내 처리를 보장

### FCFS (**First-Come-First-Service**)

- 프로세스가 대기 큐에 도착한 순서에 따라 CPU를 할당하는 스케줄링 기법
- FIFO 알고리즘
- 도착한 순서대로 처리

### SJF (**Shortest Job First)**

- 프로세스가 도착하는 시점에 따라 그 당시 가장 작은 서비스 시간을 갖는 프로세스가 종료 시까지 자원을 점유하는 스케줄링 기법
- 준비 큐 작업 중 가장 짧은 작업부터 수행
- 평균 대기 시간 최소
- CPU 요구 시간이 긴 작업과 짧은 작업 간의 불평등이 심하여 CPU 요구 시간이 긴 프로세스는 기아 현상 발생
- 기아 현상
    - 시스템 부하가 많아서 준비 큐에 있는 낮은 등급의 프로세스가 무한정 기다리는 현상
    - 오랫동안 기다린 프로세스에게 우선순위를 높여주도록 처리하는 기법인 Aging을 활용

### HRN (Highest Response Ratio Next)

- 대기 중인 프로세스 중 현재 응답률이 가장 높은 것을 선택하는 스케줄링 기법
- SJF의 약점인 기아 현상을 보완한 기법
- 긴 작업과 짧은 작업 간의 불평등 완화

## 다중 처리기 시스템 **(Multiprocessor Systems)**

### 로드 밸런싱 **(Load Balancing)**

- 네트워크 또는 서버에 가해지는 부하(Load)를 분산시켜 처리하는 기법

## 클러스터 **(Cluster)**

- 여러 개의 독립된 시스템이 네트워크로 연결되어 하나의 시스템처럼 동작하는 경우
- 클러스터 내부 및 클러스터 간 스케줄링이 필요