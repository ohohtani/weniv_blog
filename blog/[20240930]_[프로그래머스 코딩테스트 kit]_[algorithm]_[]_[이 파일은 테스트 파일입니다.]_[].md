# 코딩테스트 kit 문제 정복하기

백준 기본 코스 한 바퀴 돌았기 때문에 다시 백준하기 싫어서 아직 손도 안 댄 프로그래머스 시작하겠습니다

첫 번째 문제 [그리디 알고리즘 1/6]
![문제 설명](img/체육복_문제_설명.jpg)

문제 요약 : 체육복 하나 더 있는 사람은 앞 or 뒤 번호한테만 빌려줄 수 있다.
           최소 한 명 이상 여벌의 체육복이 있고(여벌 포함 2개), 최소 한 명 이상 체육복이 없다(도난 당함).
           체육복을 입을 수 있는 최대 학생 수를 구해라.

주의 사항 : 앞 번호 빌려주면 뒷 번호한테 못 빌려주는 거 생각 -> 빌려주면 리스트에서 제거하기
              
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

