### 목차

1. [운영체제 개요](#1-운영체제-개요)
2. [운영체제의 역사](#2-운영체제의-역사)
3. [고등 운영체제](#3-고등 운영체제-Advanced-OS)
4. [인터럽트 기반 시스템](#4-인터럽트-기반-시스템-Interrupt-Based-System)
5. [이중 모드](#5-이중-모드-Dual-Mode)
6. [하드웨어 보호](#6-하드웨어-보호-HW-Protection)
7. [운영체제 서비스](#7-운영체제-서비스-OS-Services)
8. [시스템 콜](#8-시스템-콜-System-Call)

<br>

<br>

# 1. 운영체제 개요

### 운영체제의 개념 및 역할

컴퓨터를 조작하는 프로그램이며, 컴퓨터의 자원들을 관리한다.

1. **프로세스 관리, Process Management**
2. **주기억장치 관리, Main Memory Management**
3. **파일 관리, File System Management**

<br>

### 컴퓨터 관리의 목적

- 성능 향상
- 편리성 향상

<br>

## 1-1. 컴퓨터의 자원과 구조

- **`CPU`**

  - **`Process`**: CPU가 조작하는 대상

- **`Main Memory`**

  - **`ROM`**

    - Main Memory의 극히 일부의 공간을 차지

    - **비휘발성 메모리**

    - 컴퓨터를 Power-on 시 POST와 Boot Loader를 실행하는 코드를 저장

      - **`POST`**(Power-On Self-Test)

        어떤 H/W들이 연결되어 있는지 확인

      - **`Boot Loader`**

        Hard Disk에서 OS를 찾아내고, **RAM 영역으로 이동시켜 적재(Booting)**

  - **`Resident`**: Power-On일 경우 OS는 메모리에 상주(Resident)

  - **`RAM`**

    예시) Smart Phone의 Flash Memory
    
    - Main Memory의 대부분의 공간을 차지
    - **휘발성 메모리**이므로 Power-Off 시 저장 내용이 모두 증발
    - **OS의 코드가 부팅되는 메모리**이며, 컴퓨터 실행 후 수행하는 **프로그램 등의 내용을 저장**

- 기타

  - `Disk`(보조기억장치)
  - `Network`
  - `Mouse`, `Keyboard`, `Speaker`
  - GPS

- `Program 내장형 컴퓨터`

  Power-Off일 때에도 Program을 Memory에 내장

<br>

### 컴퓨터의 논리적 구조

1. **Applications**

   사용자가 사용하는 프로그램

2. **Operating System**

   Process Management, Memory Management, File Management, I/O Mgmt, Network Mgmt, Security Mgmt 등의 역할을 수행하는 프로그램

3. **Hardware**

   CPU(Central Processing Unit), Main Memory, Disk, Monitor, Printer 등의 장치

<br>

### Instruction과 프로그램

- **`Instruction`**: **H/W**에게 명령하는 코드(명령어)들
- **`Program`**: Instruction들의 집합

<br>

### 커널과 쉘

- **`커널`**, **`Kernal`**

  Shell이 해석 및 번역한 사용자의 명령을 **수행, H/W를 조작**

- **`쉘`**, **`Shell`**(Command Interpreter)

  **사용자**로부터 받은 명령을 OS의 Kernal이 이해할 수 있도록 **해석 및 번역**

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 2. 운영체제의 역사

### 2-1. No Operating System

컴퓨터가 탄생한 1940년대 말 즈음에는 운영체제가 존재하지 않았으며, 컴퓨터 구조는 다음과 같다.

- Card Reader

  - 연구자가 작성한 코드에 해당하는 카드
  - 코드 실행을 위한 필수 기능을 실행하는 카드
    - Compile
    - Link
    - Load

- Memory

- Processing

- Line Printer

  망치로 종이를 두들겨 찍어냄

<br>

### 2-2. 일괄처리시스템, Batch Processing System

최초의 운영체제이며, Card Reader의 필수 기능(Compile, Llink, Load)들을 Memory에 상주시킴

- ex) MS-DOS

<br>

### 2-3. 다중프로그래밍 시스템, Multiprogramming System

- Main Memory에 여러 개의 Program들을 적재하여 처리

- 한 번에 한 프로그램만을 적재하여 사용하던 일괄처리시스템보다 효율적으로 컴퓨터를 사용

- CPU Scheduling으로 인해 Program의 처리 순서 변경이 가능하며,

  속도가 느린 I/O 장치가 끝날 때까지 CPU가 기다리(Idle)지 않아도 다른 Program을 실행할 수 있음

- Mamory Management로 인해 Memory의 빈 공간에 Program을 할당하기가 용이

- 다른 Program들 끼리 침범하지 않도록 보호

<br>

### 2-4. 시공유 시스템, Time-Sharing System

**다중프로그래밍 시스템인 컴퓨터 1대**를 **단말기(모니터+키보드)**를 이용하여 **여러 사용자가 공유**

- **`강제 절환`**

  컴퓨터의 사용을 N명의 사용자가 번갈아가며 사용

- **`시분할 시스템`**

  **강제 절환**을 하는데, N명의 사용자가 컴퓨터를 짧은 시간 동안 번갈아가며 사용

  ex) 1초를 3명이 나누어 쓸 때, 1인 당 약 30회의 기회를 부여

  **컴퓨터의 빠른 속도로 인해 사용자들은 자기 혼자 컴퓨터를 쓰는 것 처럼 느낄 수 있다**.

- **`대화형 시스템, Interactive System`**

  모니터와 키보드를 사용하여 컴퓨터와 대화하듯 사용

- **`가상 메모리, Virtual Memory`**

  사용자가 많아짐에 따라 Main Memory가 부족하므로,

  Hard Disk를 Main Memory인 것 마냥(실제로는 그렇지 않음) 사용

- **`Process 간 통신`**

  Data를 전송하여 공유

- **`동기화`**

- Windows, Linux, OSX, Android, iOS 모두 TSS 방식

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 3. 고등 운영체제, Advanced OS

고등 컴퓨터 구조(Advanced Computer Architectures)가 등장하며, 고등 운영체제 또한 등장

- 일반적인 컴퓨터 구조, **`폰 노이만의 컴퓨터 구조`**

  : `1 CPU` + `1 Memory`

<br>

### 3-1. 다중 프로세서 시스템, Multiprocessor System

`하나의 Memory`를 `여러 CPU`들이 **병렬적으로, 강결합**하여 사용

- 다음과 같이도 불림

  - **`병렬 시스템, Parallel System`**
  - **`강결합 시스템, Tightly-Coupled System`**

- 장점 3가지

  - **Performance**

  - **Cost**

  - **Reliability**

    고장난 CPU를 다른 CPU가 대체 가능

- **`다중 프로세서 운영체제, Multiprocessor OS`**

<br>

### 3-2. 분산 시스템, Distributed System

분산되어 있는 **여러개의 PC Set(1 CPU + 1 Memory)**들을 **LAN(근거리 통신망)을 통해 연결**

- 다음과 같이도 불림
  - **`다중 컴퓨터 시스템, Multi-Computer System`**
  - **`소결합 시스템, Loosely-Coupled System`**
- 장점 3가지
  - **Performance**
  - **Cost**
  - **Reliability**
- **분산 운영체제, Distributed OS**

<br>

### 3-3. 실시간 시스템, Real-Time System

빠르기만 한 것이 아닌, **Deadline을 준수하도록** 작업을 수행

- 시간 제약 : Deadline

  **CPU Scheduling으로 우선 순위를 적용**

- 공장 자동화(FA), 군사, 항공, 우주 등의 분야에서 사용

- **실시간 운영체제, Real-Time OS(=RTOS)**

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 4. 인터럽트 기반 시스템, Interrupt-Based System

Operating System은 Booting 종료 시, 메모리에 상주 및 사건(event)을 기다리며 대기한다. 그러던 중 마우스, 키보드를 포함한 사용자의 동작에 의해 사건(event) 발생 시 Interrupt에 의해 다시 동작

<br>

### 4-1. 하드웨어 인터럽트, Hardware Interrupt

1. 마우스, 키보드와 같은 H/W를 움직여 전기 신호를 CPU에 전송

2. CPU는 Memory 속 OS의 특정 코드(ISR)를 실행

   마우스의 ISR(Interrupt Service Routine)은 마우스 포인터를 모니터상에서 신호에 따라 움직이게 처리

3. OS는 Interrupt Service Routine 종료 후 다시 대기

<br>

### 4-2. 소프트웨어 인터럽트, Software Interrupt

- 사용자 프로그램이 소프트웨어 인터럽트를 일으킴(운영체제의 기능을 이용)

1. PPT와 같은 사용자 프로그램이 Hard Disk에 기록되어 있는 file을 읽어달라고 요청
2. CPU는 위와 같은 S/W Interrupt를 위한 OS의 특정 코드(ISR)을 실행하여 file을 읽어옴

<br>

**즉, Interrupt는 H/W에서 H/W로, 혹은 S/W에서 H/W로의 요청에 의해 발생**

- ISA(Instruction Set Architecture) 중 명령어 예시

  - add register

  - sub register

  - mov register

  - **swi register**

    Software Interrupt를 발생시키는 명령어

<br>

<br>

## 4-3. 인터럽트 기반 운영체제, Interrupt-Based OS

**현재 대부분의 OS들은 인터럽트 기반 운영체제**

- 운영체제는 메모리에 상주하며, 평소에는 대기 상태를 유지

<br>

### Event 발생 시

- HW Interrupt 발생 시 운영체제 코드(ISR)를 실행

- SW Interrupt 발생 시 운영체제 코드(ISR)를 실행

- **Internal Interrupt(내부 인터럽트)** 발생 시 운영체제 코드(ISR) 실행

  ex) 프로그램이 어떤 값을 0으로 나누라고 명령했을 경우

  1. CPU는 Interrupt가 발생한 것으로 취급

  2. OS의 **Devide by Zero**라는 ISR을 실행

     주로 프로그램을 강제 종료

- **ISR 종료 시 원래의 대기 상태, 또는 사용자 프로그램으로 복귀**

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 5. 이중 모드, Dual Mode

대부분의 운영체제는 이중 모드를 지원하며, OS가 이중 모드를 통해 보호하고자 하는 대상은 다음 세가지이다.

1. 입출력 장치
2. 메모리
3. CPU

서버 컴퓨터는 여러 사람에게 동시에 사용될 수 있는 환경이다. 개인 PC일 지라도 여러 개의 프로그램을 동시에 사용한다. 여러 사용자 혹은 프로그램들이 STOP, HALT, RESET과 같은 명령어를 남용한다면 큰 문제가 발생할 수 있다.

따라서 대부분의 운영체제(스마트폰 포함)는 이중 모드를 지원한다. 이중 모드는 관리자(Supervisor) 모드와 사용자(User) 모드로 구성된다.

- 관리자(=시스템, 모니터, 특권) 모드

  Superviser(=System, Monitor, Previliged) Mode

- 특권 명령, Privileged Instructions

  STOP, HALT, RESET, SET_TIMER, SET_HW, ...

- 권한이 없는 사용자가 위 명령을 시도하면 `Privileged Instructions Violation` 발생

이중 모드는 레지스터(Register)의 플래그(Flag)를 사용하여 조작한다.

<br>

<br>

## 5-1. CPU에서 이중 모드 제어하기

- CPU의 구조
  - 레지스터, Register
  - 산술 논리 장치, ALU(Arithmetic Logic Unit)
  - 제어 유닛, Control Unit
- CPU Register의 다섯 가지 플래그(Flag)
  - `Carry`: 자리수 상승을 표현
  - `Negative`: 계산 결과 값이 음수
  - `Zero`: 계산 결과 값이 0
  - `Overflow`
  - `Dual Mode`
    - `1`: 관리자 모드
    - `0`: 사용자 모드

마우스, 키보드와 같은 I/O 장비 또한 관리자 모드로만 조작할 수 있으며, 사용자 프로그램 실행 시 사용자 모드로 flag가 변환된다. 사용자 프로그램 사용 중에도 마우스 및 키보드 조작, H/W에 접근, 프린터 및 모니터 사용이 필요하다면, OS에 요청하여 관리자 모드로 전환하고, 해당하는 SW Interrupt를 발생시켜 Interrupt Service Routine을 발생시켜야 한다. 만약 사용자 프로그램이 직접 ISR을 조작하게 한다면, 보안 등에서 큰 문제가 발생할 수 있다.

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 6. 하드웨어 보호, HW Protection

운영체제는 이중 모드를 활용하여 하드웨어를 보호한다.

<br>

## 6-1. 입/출력 장치 보호, Input/Output Device Protection

사용자의 잘못된 입출력 명령을 방지

- 다른 사용자의 입출력, 정보 등이 침해하려는 경우
- 예시
  - printer 혼선
  - reset 명령
  - 다른 사용자의 파일 읽고 쓰기

<br>

### 보호 방법

OS는 입출력을 특권 명령(Previleged Instruction)으로만 사용 가능하게 하여 위의 문제들을 방지한다.

- 특권 명령(Previleged Instruction)의 예시
  - `ldr`: 메모리의 data를 register로 가져옴
  - `stop`: CPU 중지
  - `reset`: 전체 시스템 초기화
  - `in`: 키보드 등의 입력 장치의 명령을 가져옴
  - `out`: 프린터, 모니터와 같은 출력 장치를 조작

사용자(, 사용자 프로그램)가 입출력 명령을 직접 내릴 경우 **Privileged Instruction Violation** 발생

<br>

## 6-2. 메모리 보호, Memory Protection

어느 사용자 프로그램이 다른 사용자의 메모리, 혹은 운영체제 메모리 영역으로 접근 시 차단한다.

i.g., 해킹 프로그램이 운영체제 메모리 영역의 ISR을 조작하려 시도

i.g., 다른 프로그램이 어떤 프로그램의 메모리에 접근

<br>

### 보호 방법

CPU는 Main Memory에 **Address Bus**를 통해 특정 주소의 데이터를 달라고 요청하며, 메인 메모리는 **Data Bus**를 통해 요청 받은 주소의 데이터를 반환한다. 운영체제는 Address Bus를 전송하는 시점에 MMU(Memory Management Unit)을 반드시 거치게 하여, 허용되지 않은 주소의 데이터를 요청(**Segment Violation**)하면 CPU에 Interrupt를 발생시킨다.

<br>

## 6-3. CPU 보호, CPU Protection

어느 사용자 혹은 프로그램이 CPU를 독점하여 사용하려 하는 경우를 방지한다.

프로그래밍 언어의 `while True:`와 같은 표현은 CPU 독점을 야기할 수 있다. 따라서 **Timer Interrupt**를 적용하여 일정 시간 경과 시 강제로 다른 프로그램으로 전환한다.

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 7. 운영체제 서비스, OS Services

## 7-1. 프로세스 관리, Process Management

프로세스, Process: **메모리에서 실행 중인 프로그램**, Program in execution

<br>

### 주요 기능

- 프로세스의 생성(**Creation**), 소멸(**Deletion**)
- 프로세스 활동 일시 중지(**Suspend**), 활동 재개(**Resume**)
- 프로세스 간 통신(**IPC**, Inter-Process Communication)
- 프로세스 동기화(**Synchronization**)
- 교착 상태 처리(**Deadlock Handling**)
  - Deadlock: 다수의 프로세스가 실행되던 중 충돌이 발생한 상태

<br>

<br>

## 7-2. 주기억장치 관리, Main Memory Management

### 주요 기능

- 프로세스에게 메모리 공간을 할당(**Allocation**)

- **메모리의 어느 부분이 어느 프로세스에게 활당되었는지 추적 및 감시**

- 프로세스종료 시 메모리 회수(**Dallocation**)

- 메모리의 **효율적인 공간 사용** 유도

  i.g., 사용 가능하지만 사용되지 않는 메모리 공간을 관리

- **가상 메모리** 관리

  실제 물리적 메모리보다 큰 용량을 사용하도록 조치

<br>

<br>

## 7-3. 파일 관리, File Management

**Track**/**Sector**로 구성된 **디스크**를 **파일**이라는 논리적 관점으로 관리

Hard Disk는 자성을 띈 판 위에 **Track**이 깔려 있으며, Track은 여러 **Sector**들로 구분된다. **Coil**이 감긴 **Header**를 통해 Sector들을 N/S극으로 **자화**시켜 데이터를 저장 및 복사한다. 자극하는 Sector들을 묶어 **Block** 단위로 관리한다.

<br>

### 주요 기능

- 파일의 생성(**Creation**)과 삭제(**Deletion**)

- 디렉토리(**Directory**)(or Folder)의 생성과 삭제

- 파일에 대한 **기본 동작** 지원

  ```open, close, read, write, create, delete```

- Track/Sector와 파일간의 매핑(**Mapping**)

- 백업(**Backup**)

<br>

<br>

## 7-4. 보조 기억 장치 관리, Secondary Storage Management

- 보조 기억 장치(Secondary Storage)의 예시

  Hard Disk, Flash Memory(in Smart Phone)

<br>

### 주요 기능

- **빈 공간 관리, Free Space Management**

  (빈 Block들)

- 저장 공간 할당, **Storage Allocation**

  어느 Block에 할당할 것인가?

- 디스크 스케쥴링, **Disk Scheduling**

  디스크 공간에 Block들이 흩어져 있을때, Block들의 Head들을 어떻게 하면 효율적으로 찾아다니며 읽을 수 있겠는가?

<br>

<br>

## 7-5. 입출력 장치 관리, I/O Device Management

### 주요 기능

- 장치 드라이브, Device Drivers

- 입출력 장치 성능 향상

  - Buffering

    입출력 장치에서 읽었던 내용을 메모리에 유지하여, 같은 내용이 다시 호출됐을 때 빠른 속도로 반환

  - Caching

    Buffering과 유사

  - Spooling

    Memory 대신 Hard Disk를 중간 매체로 사용하여 효율을 증가시킨다.

    i.g., 프린터에서 작업할 내용을 계속해서 Memory에 두는 것이 아니라, 비교적 덜 귀한 Hard Disk에 저장

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

# 8. 시스템 콜, System Call

## 8-1. 시스템 콜의 정의

Application이 운영체제 서비스(OS Service)를 받기 위해 호출하는 행위

<br>

<br>

## 8-2. 주요 시스템 콜

### Process

- end

  프로세스를 종료

- abort

  프로세스를 강제 종료

- load

  H/W 프로그램을 메모리로 로드

- execute

- create

- terminate

- get/set attributes

  프로세스의 속성(ID, 메모리 사용량 등)을 읽음/설정

- wait events

- signal event

<br>

### Memory

- allocate
- free

<br>

### File

- create, delete
- open, close
- read, write
- get/set attributes

<br>

### Device

- request
- release
- read, write
- get/set attributes
- attach/detach devices

<br>

### Information

- get/set time
- get/set system data

<br>

### Communication

- socket
- send, receive

<br>

<br>

## 8-3. 시스템 콜 만들어 보기

파일을 생성하는 과정으로 예를 들어 보겠습니다.

1. Assembly 언어로 파일 속성을 정의
2. OS System Call 요청

코드들은 각 운영체제의 System Call Library에서 제공하는 형태로 작성합니다.

<br>

### MS-DOS

```shell
AH = 3CH
CX = file_attributes
DS:DX = file_name
INT = 80H
```

- `INT`: Interrupt

<br>

### Linux

```shell
# 여기서의 8은 파일을 생성할 때 사용
EAX = 8
ECX = file_attributes
EBX = file_name
INT = 80H
```

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>



