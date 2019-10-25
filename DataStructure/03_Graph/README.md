### 목차

1. [그래프 개요](#1-그래프-개요)
2. [그래프의 개념](#2-그래프의-개념)
   1. 그래프의 유형
   2. 그래프 용어
3. [그래프 순회](#3-그래프-순회)
   1. DFS, Depth-First Search
   2. BFS, Breadth-First Search
4. [신장 트리, Spanning Tree](#4-신장-트리-Spanning-Tree)
5. [최소 신장 트리, Minimum Spanning Tree](#5-최소-신장-트리-Minimum-Spanning-Tree)
   1. Prim의 MST
   2. Kruscal의 MST



<br>

<a href="https://github.com/jarvis08/Reminders">메인으로</a>

<br>

## 1. 그래프 개요

- 그래프 표현 방식 

  - 인접 행렬, Adjacent Matrix
  - 인접 리스트, Adjacent List

- 순회 방법 

  - DFS
  - BFS

- 문제 유형 

  - 신장 트리, Spanning Tree

  - 최소 신장 트리, MST, Minimum Spanning Tree 

    - Prim Algorithm
    - Kruscal Algorithm

  - 최단경로 

    - TSP, Traveling Salesman Problem

    - Dijkstra Algorithm

      플로이드 워셜 알고리즘은 출발점과 도착점이 정해져 있는 경우의 좁은 의미.

    - Floyd-Warshall Algorithm

  - AoE

  - AoV

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

## 2. 그래프의 **개념**

### 2-1. **그래프의 유형**

- 무향 그래프, Undirected Graph 
  - *vertices*와 *edges*로 이루어진 유한 집합이다.
  - *edges*는 방향(direction)을 가지지 않는다.
  - *vertices*와 *edges* 둘 다 이름(Label)을 가진다.
- 유향 그래프, Directed Graph 
  - Undirected Graph와 같이 *vertices*와 *edges*로 이루어진 유한 집합이다.
  - Undirected Graph와 다르게 *edges*는 방향을 가진다.
  - Undirected Graph와 같이 *vertices*와 *edges* 둘 다 이름(Label)을 가진다.
- 가중치 그래프, Weighted Graph
- 사이클이 없는 유향 그래프, DAG(Directed Acyclic Graph)

*사이클이 없는 무향 그래프는 `Tree` 라고 분류한다.*

<br>

### 2-2. **그래프 용어**

- `완전 그래프`

  모든 정점들 끼리 연결되어 있는 그래프

- `부분 그래프`

  원본 그래프에서 일부 정점, 혹은 일부 간선을 제외한 그래프

- `인접, Adjacency`

  - 두 개의 정점에 간선이 존재하면, 서로 인접해 있다고 한다.
  - 완전 그래프에 속한 임의의 두 정점들은 모두 인접해 있다.

- 인접의 표현 방식

  - `인접 행렬, Adjacent Matrix`
  - `인접 리스트, Adjacent List`

- `Loop`

  자기 자신과 연결된 *vertex*의 *edge*.

- `Path`

  *vertex*의 연속

- `단순 경로`

  한 경로상에 있는 모든 정점이 서로 다를 때의 경로

- `사이클`

  처음과 마지막 정점이 같은 단순 경로

- 정점의 `차수`

  그 정점에 부속된 간선들의 수

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

## 3. 그래프의 순회

### 3-1. DFS, Depth-First Search

- 시작 vertex에서 이웃 vertex로 탐색을 진행하고, 또 그 이웃의 이웃으로 진행한다.
- 더 이상 탐색을 진행할 이웃이 없을 때, 뒤로 돌아간다.
- Stack 또는 Recursion으로 수행할 수 있다.

깊이 우선 탐색

막다른 길이면 되돌아옴

Stack을 사용

재귀형식의 경우 DFS 형식으로 호출

Back Tracking이라고도 불리는데, 백트래킹에서는 가지치기(Pruning) 기법을 추가적으로 사용

<br>

### 3-2. BFS, Breadth-First Search

- 시작 vertex의 모든 이웃들을 탐색하고, 이웃의 이웃을 탐색하는 방식으로 진행한다.
- 탐색 가능한 모든 vertex에 방문했을 때 탐색이 종료된다.
- Queue로 수행할 수 있다.

Queue를 사용하며, 가까운 것부터 탐색

<br>

<br>

## 4. 신장 트리, Spanning Tree

사이클을 이루지 않으며, 모든 정점이 연결된 트리이다.

- 하나의 그래프에는 많은 신장 트리가 존재할 수 있다.
- `n`개의 정점을 `n-1`개의 간선으로 연결할 수 있는 모든 트리들이다.

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>

<br>

## 5. 최소 신장 트리, Minimum Spanning Tree

간선의 가중치를 고려했을 때, 최소의 비용을 사용하는 신장 트리이다.

<br>

### 5-1. **Prim**의 MST

정점 선택을 기반으로 하는 알고리즘이며, 이전 단계에서 만들어진 신장 트리를 확장해 나아가는 방법이다.

- 과정 
  1. 시작 단계에서는 시작 정점만이 MST( 최소 비용 신장 트리) 집합에 포함된다.
  2. 앞 단계에서 만들어진 MST 집합에 인접한 정점들 중에서 최소 간선으로 연결된 정점을 선택하여 트리를 확장한다. 즉, 가장 낮은 가중치를 먼저 선택한다.
  3. 위의 과정을 트리가 (N-1)개의 간선을 가질 때까지 반복한다.

<br>

### 5-2. **Kruscal**의 MST

- 탐욕적인 방법(Greedy Method)을 이용하는 방법이며, 탐욕적인 방법을 사용하더라도 최소 신장 트리를 만족함을 수학적으로 증명할 수 있다.

- 과정 

  1. 그래프의 간선들을 가중치의 오름차순으로 정렬한다.

  2. 정렬된 간선 리스트에서 순서대로 사이클을 형성하지 않는 간선을 선택한다.

     즉, 가장 낮은 가중치를 먼저 선택하며, 사이클을 형성하는 간선은 제외한다.

  3. 해당 간선을 현재의 MST(최소 비용 신장 트리)의 집합에 추가한다.

<br>

<a href="#목차" style="text-align: right;">맨 위로</a>