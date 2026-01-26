---
title: "[Python] 해시(hash)"
date: 2026-01-27 00:00:00 +0900
categories: [프로그래머스, 코딩테스트 연습]
tags: [python, algorithm, 해시]
---
## 1. 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

### 제한사항
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

## 1. 나의 풀이
완주한 선수 명단(completion)을 참가자 명단(participant)이랑 비교해서, 참가자 명단에 없는 사람을 찾으면 그게 정답(answer)이겠구나라는 의도로 접근

```python
def solution(participant, completion):
    # 완주한 사람 이름을 하나씩 확인해서 참가자 명단에서 지워버림
    for person in completion:
        if person in participant:
            participant.remove(person) # 참가자 명단에서 그 이름을 삭제

    # 다 지우고 마지막에 남은 한 명이 정답
    return participant[0]
```

```python
def solution(participant, completion):
    hash_dict = {}
    sum_hash = 0

    # 1. 참가자들의 이름으로 해시 값을 만들고 더합니다.
    for part in participant:
        hash_dict[hash(part)] = part
        sum_hash += hash(part)

    # 2. 완주자들의 해시 값을 뺍니다.
    for comp in completion:
        sum_hash -= hash(comp)

    # 3. 남은 해시 값이 완주하지 못한 선수의 키(Key)입니다.
    return hash_dict[sum_hash]
```

```python
import collections

def solution(participant, completion):
    # 1. 참가자와 완주자의 이름 개수를 셉니다.
    # 2. 참가자 개수 - 완주자 개수를 뺍니다 (차집합).
    answer = collections.Counter(participant) - collections.Counter(completion)

    # 3. 남은 하나의 키(이름)를 반환합니다.
    return list(answer.keys())[0]
```

## 1. 💡 이번 문제를 통해 배운 점
참가자 수가 많아지는 경우 속도가 너무 느려서 효율성 테스트를 통과를 못함. 

해시나 정렬을 이용해서 문제를 풀어야 함.

파이썬의 라이브러리를 사용하면 코드를 매우 짧게 줄일 수 있습니다. Counter는 리스트 안의 요소 개수를 자동으로 세어줍니다.