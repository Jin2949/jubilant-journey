## 큐의 연산과정



#### 공백 큐 생성

front = rear = -1 (front는 삭제된 위치, rear는 저장된 위치)

enQueue는 rear를 증가시키고 저장

deQueue는 front를 증가시키고 삭제

front ==rear이면 공백상태

```python
def enQueue(item): #enQueue함수 정의
    global rear
    if isFull() : print("Queue_Full")	#Queue가 꽉찼을때 디버깅목적
        #큐가 작게 설계 혹은 값이 너무 많이 들어감.
	else:
        rear += 1				#enQueue 인큐
        Q[rear] = item
```

```python
def deQueue()	#deQueue함수 정의
	if(isEmpty()) then Queue_Empty(); #front ==rear일 경우도 같다.
    else:
		front += 1
        return Q[front]
```



#### 원형큐

front = rear = 0 에서 공백상태로 시작한다.

front와 rear의 위치가 배열의 마지막 인덱스 n-1가리킨후 다시 처음인덱스 0으로 간다.

(앞으로 이동하기 위해서 modulo연산자 사용한다)

ex) rear = (rear+1) % Q.size

원형큐 연산은 0,1,2,3으로 돌면서 값들을 채움 

공백상태는 front == rear일때

포화상태는 rear가 돌다가 front를 잡았을때(꼬리잡기 연상)

- (rear+1) mod n == front

FIFO 파이포

LIFO 라이포



#### DEQ(덱)

덱은 앞뒤로 데이터를 넣을 수 있는 구조이다.



#### 우선순위Queue	#예외

heap을 알아야한다.



BFS구현

```python
def BFS(G, v) #그래프 G, 탐색 시작점 v
	visited = [0]*(n+1)	#n:정점의 개수
    queue = []			#큐 생성
    queue.append(v)		#시작점 v를 큐에 삽입 enque
    while queue:		#큐가 비어있지 않으면
        t = queue.pop(0)	#t는 큐의 첫번째 들어간 값	deque
        if not visited[t]:	#t 값이 방문하지 않은 값이면
            visited[t] =True	#visited는 방문했다고 바꿔주고
            visit(t)			#정점 t에서 할 일
		for i in  G[t]:			#t와 연결된 모든 정점에 대해
            if not visited[i]:	#방문되지 않았으면
                queue.append(i)	#큐에 넣기
```

```python
아래는 위에서 visited[t]의 if문을 줄일 수 있는 코드이다.
이렇게 하면 장점이 queue에 중복이 생기지 않는다. 내가 처리하는 정점만큼 큐만듦.
def BFS(G, v) #그래프 G, 탐색 시작점 v
	visited = [0]*(n+1)	#n:정점의 개수
    queue = []			#큐 생성
    queue.append(v)		#시작점 v를 큐에 삽입 enque
    visited[v] = 1		#시작점 v를 방문에도 삽입
    while queue:		#큐가 비어있지 않으면
        t = queue.pop(0)	#t는 큐의 첫번째 들어간 값 deque
        visit(t)			#정점 t에서 할 일
		for i in  G[t]:			#t와 연결된 모든 정점에 대해
            if not visited[i]:	#방문되지 않았으면
                queue.append(i)	#큐에 넣기
                visited[i] = visited[t]+1
                #다음 visited의 값은 1에서 파생되었으면 2로표시 다음애에서 파생되면 3으로 표시 점점 늘려간다.
```

DFS와 BFS는 상황에 따라 다르게 사용한다.



교수님 수업

Queue구현

```python
queue = []

queue.append[1]
queue.append[2]
queue.append[3]

queue.pop(0) --- #비효율적

#해결법
import queue

my_Q = queue.Queue(3)	#새로운 queue 생성 사이즈3
my_Q.put(1)
my_Q.put(2)
my_Q.put(3)

elem = my_Q.get()
print(elem)		#1이나온다 fifo

print(my_Q[1])	#Queue는 인덱스 접근이 안된다.

#위에 사이즈 3을 넣고 my_Q.put(4)를 넣으면 하나가 빠질때까지 계속 코드가 돌아간다. 멀티스레드에서 사용하기위해서 만든 기능이다.
```



미로찾기

```python
def bfs(i, j, N)
	visited = [[0] * N for _ in range(N)]	#미로의 크기만큼 생성
    queue = [] #큐생성
    queue.append((i,j))	#시작점 인큐
    visited[i][j] = 1	#시작점 방문표시
    while queue:		#큐가 비어있지 않으면 반복
        i, j = queue.pop(0)
        if maze[i][j] == 3:	#목적지도착
            return visited[i][j] -2 #출발 도착 칸 제외
        for di, dj in [[0,1],[1,0],[0,-1],[-1,0]]
        	ni, nj = i+di, j+dj
            if 0<=ni<N and 0<=nj<N and maze[ni][nj]!=1 and visited[ni][nj]==0:
				queue.append((ni,nj))
                  visited[ni][nj] = visited[i][j] + 1
	return 0
                
```













