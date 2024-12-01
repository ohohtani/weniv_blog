백준 기본 코스 한 바퀴 돌았기 때문에 다시 백준하기 싫어서 아직 손도 안 댄 프로그래머스 시작하겠습니다

# 그리디 알고리즘

### [그리디 알고리즘 1/6]
![문제 설명](img/체육복_문제_설명.jpg)

**문제 요약** : 체육복 하나 더 있는 사람은 앞 or 뒤 번호한테만 빌려줄 수 있다.
           최소 한 명 이상 여벌의 체육복이 있고(여벌 포함 2개), 최소 한 명 이상 체육복이 없다(도난 당함).
           체육복을 입을 수 있는 최대 학생 수를 구해라.

**주의 사항** : 앞 번호 빌려주면 뒷 번호한테 못 빌려주는 거 생각 -> 빌려주면 리스트에서 제거하기
              
1. 도난 당한 놈 리스트 정리( 도난 당했지만 여벌의 체육복이 있는 경우 빼기 )
2. 여벌 있는 놈 리스트 정리( 여벌의 체육복이 있지만 도난 당한 경우 빼기 )

체육복 입을 수 있는 사람 수 -> answer에 저장

이제 정리 된 도난 당한 놈(체육복 없음) 리스트를 순회하면서 앞이나 뒷번호 친구가 여벌의 체육복이 있으면
체육복 입을 수 있는 사람 수(answer)를 1 증가 시키고 더 이상 빌려준 친구는 여벌의 옷이 없는 것이니
여벌 있는 놈 리스트에서 제거 시켜주면 된다.

``` python
def solution(n, lost, reserve):
    real_lost = sorted(set(lost) - set(reserve))         # 둘다 포함 되어 있으면 빼줘야댐
    real_reserve = sorted(set(reserve) - set(lost))      # 정렬을 해야 쉽게 순회 가능  

    answer = n - len(real_lost)                          # 체육복 있는 사람 => 전체 학생 - 없는 사람
    
    for i in real_lost:
        if i-1 in real_reserve:
            real_reserve.remove(i-1)
            answer += 1

        elif i+1 in real_reserve:
            real_reserve.remove(i+1)
            answer += 1

    return answer
```



# 스택, 큐

### [스택, 큐 알고리즘 1/6]
![문제 설명](img/스택큐문제설명1.png)

**문제 요약** : 연속된 숫자는 하나만 출력 시키는 배열로 만들어라

**주의 사항** : 딱히 없음

어차피 첫 번째 원소는 정답 배열에 무조건 추가되므로 박아 놓고 시작.

그리고 두 번째 원소부터 반복문을 시작해서 이전의 원소와 다르면 정답 배열에 추가해주면 끝.

```python
def solution(arr):
    answer = [arr[0]]  # 첫 번째 요소는 박아 놓고 시작

    for i in range(1, len(arr)):  # 두 번째 요소부터 시작
        if arr[i] != arr[i - 1]:  # 이전 요소와 다를 때만 추가하기
            answer.append(arr[i])

    return answer
```


# 해시

### [해시 알고리즘 1/6]
![문제 설명](img/해시1_문제설명.png)

**문제 요약** : N/2 마리만 가져가는데 최대한 다른 종류로 가져갔을 때 종류 수의 최댓값(?) 한국어는 확실히 어렵다

**주의 사항** : 문제를 이해할 능력 요구

근데 진짜 문제 설명만 길고 코딩테스트 kit 중에 제일 간단한 문제인 것 같다.
[3,3,3,2,2,2] 같은 예시로, N/2 마리를 가져가면 3마리긴 한데 그래봤자 2종류 뿐이니까 2를 출력해야 하는 이 상황을 써주기만 하면 된다.

```python
def solution(nums):
     return min(len(nums)/2, len(set(nums)))
```

너무 짧게 나와서 뭔가 잘못된 줄 알고 다른 사람들 풀이 찾아봤는데 다들 똑같이 푸셨다.


### [해시 알고리즘 2/6]
![문제 설명](img/해시2_문제설명.png)

**문제 요약** : 참가자 배열에서 완주자 배열 빼고 완주 못 한 한명을 출력해라 (완주자 배열은 참가자 배열보다 딱 1명 적게 세팅됨)

**주의 사항** : 동명이인 있음 주의, Counter 함수를 쓸 줄 아는가

단순히 배열에서 빼는 접근으로 가면 동명이인의 경우 같이 지워지기 때문에
Counter 함수 이용해서 참가자에서 완주자 빼고 안에 있는 요소(key)만 출력시키면 됨 (Counter는 key와 개수를 같이 저장함)
그리고 "Mike" 와 같은 요소 '하나'의 출력을 요구하기 때문에 리스트 형태로 반환하면 안 됨. so, 이터레이터로 만들고 next를 통해 출력

```python
from collections import Counter        # 리스트의 요소와 개수를 저장시킬 수 있는 Counter 를 import

def solution(participant, completion):
    result = Counter(participant) - Counter(completion)    
    return next(iter(result))          # Counter에서 이터레이터를 만들면 키(key)에 대해서만 순회함. ★★
```


# 완전 탐색
### [완전 탐색 알고리즘 1/6]
![문제 설명](img/최소_직사각형.png)

**문제 요약** : 지갑에 명함을 센스있게 쑤셔 넣어라

**주의 사항** : 이걸 얼마나 간단하게 나타낼 수 있을까

명함을 눕혀서 넣는 경우도 고려해야 하니, 가장 작은 지갑으로 만들려면 배열 한쪽라인에 큰값을 몰아넣고
남은 한쪽라인에는 작은값을 몰아넣어서 각 라인에서 최댓값을 추출한 후 곱해주면 될 것 같다.

```python
def solution(sizes):
    return max(max(x) for x in sizes) * max(min(x) for x in sizes)
```


# 깊이/너비 우선 탐색(DFS/BFS)
### [깊이/너비 우선 탐색(DFS/BFS) 알고리즘 1/6 : 게임 맵 최단거리]

문제 설명은 프로그래머스 사이트 참조

**문제 요약** : 목표 지점까지 가는 최단 경로를 구하라

**주의 사항** : BFS 사전 지식 없으면 그냥 풀기는 너무 어려울 듯

BFS에 대한 깨달음만 있다면 원리를 이해하고 풀 수 있겠지만, 그렇다고 해서 머릿 속 지식만으로 술술 풀어내는 건 쉽지 않다ㅠㅠ

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

나는 이거 이해하는 데에도 꽤나 시간이 걸렸는데, 2차원 배열에서의 좌표 체계가 좀 다르기 때문이다

![좌표 체계](img/2차원배열좌표체계.png)

이런 식이라서 그냥 수학에서 쓰던 좌표 평면 상으로만 대입하면 어지러워진다.
이 체계를 알고 나면 상하좌우 개념이 잡혀서 코드를 이어나갈 수 있다.

```python
from collections import deque   # 큐를 사용하기 위해 import
def solution(maps):
    answer = 0

    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]

    def bfs(x, y):
        queue = deque()
        queue.append((x,y))

        while queue:                # 큐가 빌 때까지
            x, y = queue.popleft()  # 먼저 들어온 놈 탐색하기

            
            for i in range(4):      # 상하좌우를 탐색
                nx = x + dx[i]      # 저장된 좌표에서 dx dy 배열 돌면서
                ny = y + dy[i]      # 탐색된 상하좌우 값으로 nx, ny를 업데이트

                if nx < 0 or nx >= len(maps) or ny < 0 or ny >= len(maps[0]) : continue  # 맵을 벗어나면 무시

                if maps[nx][ny] == 0 : continue # 벽면이면 무시합니다 (BFS에서, 방문하지 않은 길은 항상 1로 저장되어 있습니다.)

                if maps[nx][ny] == 1:
                    maps[nx][ny] = maps[x][y] + 1
                    queue.append((nx, ny))

        return maps[len(maps)-1][len(maps[0])-1]
    
    answer = bfs(0,0)
    
    if answer == 1:
        return -1
    else:
        return answer
```

시작지점부터 도착지점까지 도달하는 과정을 글로 써보자면,
시작지점에서 상하좌우를 탐색한다. 탐색한 곳이 벽면이거나, 맵을 벗어나거나, 이미 방문한 길이라면 무시하고
방문하지 않은 길이라면 그 좌표를 큐에 집어 넣는다. 
큐에 가장 먼저 들어온 놈의 좌표로 이동한 뒤, 그 지점의 값을 +1 시켜준다. (특정 지역의 거리를 갱신)
이 과정을 큐가 비어있을 때까지 반복해준 뒤, 목표 지점의 거리 값을 출력 시킨다.

아마 지금 당장 안 보고 다시 풀라고 해도 못 풀 것 같아서, BFS/DFS 유형 완벽해질 때까지 매일 풀어야 할 것 같다.