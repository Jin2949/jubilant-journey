DFS(깊이우선탐색) - Depth First Search

BFS(너비우선탐색) - Breadth First Search

DFS : 반복,재귀로 구현 (스택사용)

- 그림으로 잘 이해해야함. DFS 탐색

계산기를 스택으로 표기하기

백트래킹은 해보지 않아도 되는 것에 대해서는 제거함.

부분집합 재귀로 만들기!!

```python
def f(i, N, K): #i 부분집합에 포함될지 결정할 원소의 인덱스, N전체 원소개수, K 찾는 합
    if i==N:
		print(bit)
        s= 0
    	for j in range(N):
            if bit[j]:
                s += a[j]
                print(a[j], end = ' ')
        print(s)
        if s== K:
            for j in range(N):
                if bit[j]:
                    print(a[j], end=' ')
			print()
    else:
        bit[i] = 1
        f(i+1, N, K)
        bit[i] = 0
        f(i+1, N)
	return

a =[1,2,3,4,5,6,7,8]
N = len(a)
bit = [0] *N 
f(0, N, 10)
```

```python
def f(i, N):
    if i==N:
        print(p)
    else:
        for j in range(i, N):
            p[i], p[j] = p[j], p[i]
            f(i+1, N)
            p[i], p[j] = p[j], p[i]

p = [1,2,3]
N = 3
f(0, N)
```

그래프를 저장하는 법

- 인접행렬
  전체 행렬적고 연결되어있으면 1로 적음

- 인접리스트
  1연결 2,4
  2연결 1,5
  처럼 되어 있는 것





