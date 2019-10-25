### 목차

1. [파일 시스템, File System](#1-파일-시스템-File-System)
2. [파일 할당, File Allocation](#2-파일-할당-File-Allocation)
   1. 연속 할당, Continuous Alloaction
   2. 연결 할당, Linked Allocation
   3. 색인 할당, Indexed Allocation
3. [디스크 스케쥴링, Disk Scheduling](#3-디스크-스케쥴링-Disk-Scheduling)
   1. FCFS Scheduling
   2. SSTF Scheduling
   3. SCAN Scheduling
   4. SCAN Scheduling's Variants

<br>

<a href="https://github.com/jarvis08/Reminders">메인으로</a>

<br>

# 1. 파일 시스템, File System

컴퓨터 시스템의 자원 관리, 운영체제의 관리 대상은 다음 세 가지 입니다.

- CPU 프로세스 관리, Process Managemant

  - CPU 스케쥴링, CPU Scheduling
  - 프로세스 동기화, Process Synchronization

- 주기억장치, Main Memory Unit

  - 페이징, Paging
  - 가상 메모리, Virtual Memory

- 보조기억장치, Auxiliary Memory(Storage) Unit

  파일 시스템, File System

<br>

### 하드 디스크 HDD, Hard Disk Drive

그 중에서 이번 6장에서 다룰 내용은 보조기억장치 및 하드 디스크(HDD)입니다. 하드 디스크의 구성은 다음과 같습니다.

![05_Disk_Structure](../assets/05_Disk_Structure.jpg)

- **트랙, Track**

  - **실린더, Cylinder**: 하드 디스크는 여러 층의 트랙들로 되어 있으며, 실린더는 다른 층들이지만 같은 위치인 섹터들(기둥 형태)의 집합

- **섹터, Sector**

  주로 512 Bytes로 사용된다.

  - **블록, Block**: Sector들의 모임이며, 파일 시스템에서는 디스크 공간을 블록 단위로 관리

    i.g., 메모장에 1 Byte의 내용만 작성하여도 4 KB의 공간이 사용된다.

- **헤더, Header**

  디스크의 헤더에는 코일이 감겨 있으며, **회전하는 디스크 트랙 위의 목표하는 블록 및 섹터로 위치를 이동하여 전자기장을 형성하고, 데이터를 읽거나 씁니다**.

- **디스크, Disk**

  : Pool of free blocks

블록 단위를 사용하는 이유는, **섹터처럼 작은 단위로 사용할 경우 디스크의 읽기/쓰기 작업을 너무 빈번하게 진행**해야 하기 때문입니다.

각각의 파일에 대해 free block을 어떻게 할당할 것인가를 결정하는 것이 **파일 할당(File Allocation)**입니다.

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 2. 파일 할당, File Allocation

파일을 블록에 할당하는 방법으로는 세 가지 방법이 있습니다.

- 연속 할당, Continuous Allocation
- 연결 할당, Linked Allocation
- 색인 할당, Indexed Allocation

<br>

<br>

## 2-1. 연속 할당, Continuous Allocation

연속 할당은 IBM(VM/CMS)에서 60-70년대에 사용하던 할당 방법입니다. 연속 할당은 파일을 할당하는 가장 단순한 방법으로, **연속된 복수개의 블록들에 파일을 순서대로 저장**합니다.

<br>

### 연속 할당의 장점

연속 할당은 **디스크 헤더(Header)의 이동을 최소화** 할 수 있습니다. 특정 파일을 읽어내고 싶다면 시작 블록으로 이동하여 순차적으로 읽어나가기만 하면 됩니다.

- **순차 접근, Sequential Access**

  파일 하나를 읽기 위해 순차적인 블록 공간을 읽어내기만 하면 된다.

- **직접 접근, Direct Access**

  파일의 중간을 탐색해야 할 때, 특정 지점을 지정하여 그 부분만 읽어낼 수 있다.

<br>

### 연속 할당의 단점

연속 할당은 분명 읽기 속도 면에서 가장 빠른 속도를 낼 수 있습니다. 하지만 파일을 삭제하거나 갱싱할 경우 치명적인 문제가 발생합니다.

파일 삭제 시 파일이 기록되어 있던 블록은 빈 공간(Hole)이 되며, 파일의 생성과 삭제가 반복되면 빈 공간들이 곳곳에 흩어지게 됩니다. 이로 인해 **외부 단편화**라는 큰 문제가 발생합니다. 이를 Compaction 하는 것에도 긴 시간이 걸리며, 파일 삭제가 아니더라도 log file과 같이 **파일에 필요한 공간이 늘어나게 될 시** 이를 처리하는 것도 곤란할 수 있습니다.

<br>

<br>

## 2-2. 연결 할당, Linked Allocation

파일을 **연결된 리스트의 블록들로 관리**하며, **파일 디렉토리(File Directory)**는 파일의 **맨 처음 블록**을 가리킵니다. 첫 블록 이후의 블록들은 4 Byte 이상의 (**다음 블록의 포인터**)를 보유합니다.

### 연결 할당의 장점

**외부 단편화가 발생하지 않습니다**. 블록 마다 링크만 연결된다면, 어느 위치이든 파일을 할당할 수 있습니다.

<br>

<br>

### 연결 할당의 단점

**순차 접근(Sequential Access)** 방식입니다. 계속해서 다음 블록의 포인터를 쫒아 파일을 읽어냅니다. 그런데 연결 할당은 다음의 단점들이 있습니다.

- 직접 접근(Direct Access) 불가

- 블록마다 포인터 저장을 위한 4 Byte 공간이 필요

- 낮은 신뢰성

  포인터가 끊어지면, 그 다음 순차의 데이터들에는 접근이 불가

- 느린 속도

  잦은 헤더 이동

<br>

<br>

### 연결 할당의 개선, FAT 파일 시스템

FAT(File Allocation Table) 파일 시스템은 연결 할당의 문제점을 해결 하며, MS-DOS, OS/2, Windows, 대부분의 USB 등에서 다양하게 사용됩니다.

FAT 파일 시스템의 핵심은 **포인터들을 모두 FAT 블록에 모으는 것**입니다. 포인터들을 따로 모아주는 것으로 공간과 속도 모두를 개선할 수 있습니다.

- 신뢰성 보장

  - 링크가 모두 FAT에 모여있기 때문에 끊길 일이 없다.
  - FAT 손실을 대비하여 복구용 FAT를 하나 더 만든다.

- 직접 접근(Direct Access) 가능

- 일반적으로 메모리 캐싱이다.

  거의 메모리에 상주하는 것과 같다. 운영체제 부팅하듯, 로드하여 유지한다.

<br>

<br>

## 2-3. 색인 할당, Indexed Allocation

파일 당 하나의 인덱스 블록을 보유합니다. 즉 파일에 여러 데이터 블록(데이터가 기록된 블록)들이 있을 때, 이 블록들을 연결할 수 있도록 데이터 블록들의 인덱스가 기록된 **인덱스 블록**을 따로 생성하여 관리합니다. 즉, 파일 마다 포인터들의 모음인 인덱스 블록을 보유하고 있으며, **파일 디렉토리는 인덱스 블록을 가리킵니다**. Linux/Unix 등에서 사용합니다.

<br>

### 색인 할당의 장점

- 직접 접근(Direct Access) 가능
- 외부 단편화 없음

<br>

### 색인 할당의 단점

인덱스 블록을 할당해야 하기 때문에 저장 공간이 손실됩니다.

하지만 이러한 문제 보다, 하나의 인덱스 공간이 하나의 블록을 사용한다 했을 때, 인덱스 블록이 **많은 내용을 가리킬 수 없는 것이 더 큰 문제**입니다. 이는 다음의 예시를 보고 확인할 수 있습니다.

```
1 블록 = 512 byte = 4 byte * (128개의 인덱스)
:: 128개의 인덱스 * 512 byte = 64 KB
```

즉, 하나의 블록이 512 byte 크기를 갖는다 했을 때, 하나의 인덱스 블록은 최대 64 KB 크기의 파일만을 가리킬 수 있습니다. 만약 블록 크기가 1 KB일 지라도 최대 256 KB의 파일 크기로 제한됩니다.

<br>

### 색인 할당의 보완

- Linked

  인덱스 블록의 포인터들 중 하나가 다른 인덱스 블록을 가리킴

- Multilevel Index

  **인덱스 블록 하나**가 다른 인덱스 블록들을 가리키는, **인덱스 블록들의 인덱스 블록 역할을 수행**

- Combined

  (**Linked + Multilevel Index**)이며, 실제 Linux에서 사용하는 방법

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 3. 디스크 스케쥴링, Disk Scheduling

디스크가 데이터를 읽어오는 시간인 **디스크 접근 시간**은 다음과 같습니다.

`Seek Time + Rotational Delay + Transfer Time`

- Seek Time

  헤더가 데이터가 위치한 섹터 및 블록을 찾고, 이동하는데 걸리는 시간

- Rotational Delay

  디스크가 회전하는데 걸리는 시간

- Transfer Time

  전자기장을 형성하여 데이터를 읽어내는데 걸리는 시간

**디스크 스케쥴링 알고리즘(Disk Scheduling Algorithm)**은 디스크 접근 시간을 최소화 하는 방법에 대한 알고리즘입니다.

<br>

<br>

## 3-1. FCFS Scheduling

**First-Come, First-Served Scheduling**입니다. 가장 간단하며 공정한 방법으로, **디스크 큐(Disk Queue)**에 쌓이는 요청들을 시간 순서대로 처리합니다.

<br>

### 예시

200의 cylinder disk가 있을 때,

- Disk_Queue = [98, 183, 37, 122, 14, 124, 65, 67]

- headers'_cylinder = 53

위의 Disk Queue 내용으로 탐색 대기열이 있다고 해 봅시다. FCFS 스케쥴링 방법은 현재 헤더의 위치인 53과 무관하게, 0번 인덱스 부터 차례로 방문합니다. 따라서 헤더의 이동 거리는 640 Cylinders가 됩니다.

<br>

<br>

## 3-2. SSTF Scheduling

**Shortest-Seek-Time-First Scheduling**입니다. SSTF 스케쥴링은 현재 헤더 위치를 기준으로, 가장 가까운 거리에 위치한, seek time을 최소로 하는 요청부터 처리합니다.

<br>

### 예시

200의 cylinder disk가 있을 때,

- Disk_Queue = [98, 183, 37, 122, 14, 124, 65, 67]

- headers'_cylinder = 53

헤더의 위치에서 가장 가까운 곳을 탐색하여 이동하므로, [65, 67, 37, 14, 98, ...]의 순서로 이동하며, 헤더의 총 이동거리는 236 Cylinders입니다.

<br>

### 문제점

현재 예시만을 봤을때, 헤더의 총 이동 거리는 FCFS에 비해 월등히 낮습니다. 하지만 현실의 대부분의 경우에는 지속적으로 Disk_Queue에 요청이 유입되며, 만약 특정 위치의 실린더들에 대한 요청들이 지속적으로 들어올 경우 **Starvation**이 발생할 수 있습니다. Starvation은 특정 처리가 계속해서 처리되지 않는 상황을 말합니다. 또한, 모든 경우에 FCFS보다 좋은 성능을 보인다고도 할 수 없습니다.

<br>

<br>

## 3-3. SCAN Scheduling

SCAN Scheduling은 이름처럼, 디스크를 계속해서 앞뒤로 스캔하는 방식입니다.

<br>

### 예시

200의 cylinder disk가 있을 때,

- Disk_Queue = [98, 183, 37, 122, 14, 124, 65, 67]

- headers'_cylinder = 53

헤더의 총 이동 거리는  236(=53+183) Cylinders이며, SSTF Scheduling 방식보다 적은 시간을 소요합니다.

<br>

<br>

## 3-4. SCAN Scheduling's Variants

SCAN Scheduling을 변형한 방법이 총 세 가지 있습니다.

- C-SCAN
- LOOK
- C-LOOK

<br>

### C-SCAN

만약 계속되는 실린더의 요청이 **연속균등분포(Uniform Distribution)**를 따른다고 가정해 봅시다. 연속균등분포를 따를 경우 실린더 공간에 고르게 요청되며, scheduling 방식에 의해 scan하는 헤더 주변이 가장 요청이 적은 지역일 것입니다. 헤더의 위치가 한쪽 끝일 경우, 요청이 가장 많은 지역은 그 반대쪽 끝입니다. 

이러한 가정에 입각하여 제안된 것이 **Circular SCAN**입니다. Circular SCAN은 **circular list**가 순환하듯, 헤더의 이동 방향이 하나입니다. **한쪽 끝에 도달**한 후 반대쪽 방향으로 다시 탐색을 하는 것이 아니라, **바로 반대 쪽 끝으로 이동하여 다시 같은 방향으로 탐색**합니다.

<br>

### LOOK

**SCAN Scheduling** 방식을 따르지만, 헤더가 **진행 방향의 마지막 요청 위치 까지**만 스캔합니다. 그러기 위해 움직이기 직전의 순간마다, 지속적으로 진행 방향으로의 요청이 있는가를 확인합니다.

<br>

### C-LOOK

**C-SCAN**의 방식을 따르지만, LOOK과 같이 **마지막 요청 위치까지만 스캔**합니다.

<br>

### Elevator Algorithm

**C-LOOK**의 움직임이 엘리베이터의 움직임과 동일하다 하여 **Elevator Algorithm**이라고도 불립니다.

