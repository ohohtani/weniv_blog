# 그리디 알고리즘

백준 기본 코스 한 바퀴 돌았기 때문에 다시 백준하기 싫어서 아직 손도 안 댄 프로그래머스 시작하겠습니다

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
