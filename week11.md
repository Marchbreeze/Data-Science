## 1. 지표를 이용한 연관규칙의 분석

### (1) 지표

- 트랜잭션 = 데이터의 하나의 행 (연관분석에서의 표기)

1. 지지도 (Support)
    - 이걸 본 사람이 얼마나 많을까?
    1. `support(A)` = A가 등장하는 트랜잭션의 비율
    2. `support(A → B)` = A와 B가 모두 등장하는 트랜잭션의 비율 (&의 개념)

1. 신뢰도 (Confidence)
    - 이걸 본 사람은 저것도 봤을까?
    - `confidence(A → B)` = A가 등장하는 트랜잭션 중 B까지 등장하는 비율
        
        = support(A → B) / support(A)
        
        = 조건부확률의 개념 (0~1)
        
    - 높은 confidence를 갖는 연관규칙이 반드시 유의미? X
        - ex) confidence(20학번 입학 → 기대수명 300세 이하) = 1
            
            B의 가능성이 너무 큰경우, 무조건적으로 1이 되어버릴 것
            

1. 향상도 (Lift)
    - Confidence에 뒤의 확률 고려
    - `lift(A → B)` = confidence(A → B) / support(B)
        
        = support(A → B) / { support(A) * support(B) }
        
        = `lift(B → A)`
        

### (2) Apriori Algorithm

1. 설치 및 임포트
    
    ```kotlin
    !pip install mlxtend
    ```
    
    ```python
    import pandas as pd
    from mlxtend.frequent_patterns import apriori
    from mlxtend.frequent_patterns import association_rules
    ```
    
2. 데이터셋 확인
    
    ```python
    # 데이터 불러오기
    df = pd.read_csv('kuflix.csv')
    df
    ```
    
    ![2024-05-13_15-37-16.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/100570fe-901d-4369-beeb-dbd4c97b8972/2024-05-13_15-37-16.jpg)
    
3. Apriori Algorithm 적용
    - `min_support` : support의 최소 제한을 두어서 높은 지표의 데이터만 필터링
        - 0은 불가능
    - use_colnames : column명 보이도록 설정
    
    ```python
    result = apriori(df, min_support = 0.5, use_colnames = True)
    result
    ```
    
    ![2024-05-13_15-37-54.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/c0918a4e-0d9a-4dbe-82fa-cbcc741eb895/2024-05-13_15-37-54.jpg)
    
4. 연관분석을 위한 association_rules
    - `metric = 'confidence', min_threshold = 0.5`
        
        = 규칙을 만들 때 confidence가 0.5 이상인 제한을 추가
        
    
    ```python
    result2 = association_rules(result, metric = 'confidence', min_threshold = 0.5)
    result2
    ```
    
    ![2024-05-13_15-44-40.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/afbe92e6-d89c-40b3-af89-b917f2ef0002/2024-05-13_15-44-40.jpg)
    
    - antecedents → consequents
    - 값이 여러개 포함일수도 있음
        - (A→B)→C : (나는 SOLO, 환승연애3) → (최강야구)

### (3) Exercise

- 데이터 입력
    
    ```python
    df2 = pd.read_csv('league.csv')
    df2
    ```
    
    ![2024-05-13_16-09-58.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/ffe08229-e5ac-4bad-bd3d-929f1ec8630d/2024-05-13_16-09-58.jpg)
    

1. 세 가지 캐릭터를 선택하되, 지지도가 40% 이상인 조합을 구성하려 한다. 선택해야 할 캐릭터는?
    
    ```python
    result3 = apriori(df2, min_support = 0.4, use_colnames = True)
    result3
    ```
    
    ![2024-05-13_16-11-19.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/645672a1-6ee7-4c9d-87c6-309878bf45f6/2024-05-13_16-11-19.jpg)
    
    ⇒ support 0.4의 (TEEMO, YUUMI, VAYNE) 조합
    

1. Blitzcrank가 선택된 팀에서 Zed도 함께 선택되었을 확률은?
    
    ```python
    result4 = association_rules(result3, metric = 'confidence', min_threshold = 0.5)
    result4
    ```
    
    ![2024-05-13_16-12-58.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/a155e270-4937-4702-b143-e7f2a7435945/2024-05-13_16-12-58.jpg)
    
    ![2024-05-13_16-13-14.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/26d31f0d-07fc-4d41-b2ba-2aa1fc9c4110/2024-05-13_16-13-14.jpg)
    
    ⇒ (BLITZCRANK) → (ZED) 의 confidence = 0.64
    

---

## 2. 지지도를 이용한 시퀀스 마이닝

### (1) 시퀀스

- 기존의 연관분석 : 시점에 대한 논의 X
- Timestamp를 활용하는 경우 ?
    
    ![2024-05-13_16-17-54.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/6b477f97-eff2-4cb2-83d6-5fa3d45c658d/2024-05-13_16-17-54.jpg)
    
1. Sequence
    - 이벤트의 발생 순서를 나타낸 것
    - ex) A의 시퀀스 = <{1, 2}, {2, 3, 4}, {2, 4, 5}>
    
2. Subsequence
    - 시퀀스에 포함된 부분 시퀀스
        - 이벤트를 빼도, 타임스탬프를 빼도 가능
    - ex) A의 임의의 서브시퀀스 = {2}, {2, 4}, {4, 5}
        - 언제가 될지는 모르겠지만, 2를 겪은 사람은 이후에 2,4 겪고 4,5도 겪음
    - ex) s(<{3}, {1,4}>)
        - 어떤 시점에서 3을 겪은 사람이 ‘이후 시점’에서 1,4를 겪는 경우
    - 서브시퀀스의 confidence를 활용해서 시퀀스 마이닝 실행 가능
    

### (2) 시퀀스 마이닝

- Ex. 붕어빵을 먹은 학생은 60%확률로 언젠가는 호떡을 먹을 것이다.

s( <{2}, {2,3}> )의 confidence ?

- A의 서브 시퀀스에는 <{2}, {2,3}> 존재 → O
- B의 서브 시퀀스에는 <{2}, {2,3}> 존재X → X
- …
    
    ⇒ A~E 중 3개 존재 : confidence = 0.6
