## 1. 상관계수

### Pearson 상관계수 (r)

- 두 변수 사이에 존재하는 선형적인 상관관계를 수치화한 것
- -1 ~ +1 사이로, 0이면 상관관계 X
- 음수는 음의 상관관계( \ ), 양수는 양의 상관관계 ( / )
    
    ![2024-03-25_15-17-14.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/81d4c833-e602-4952-88d2-7caaecbf18a1/2024-03-25_15-17-14.jpg)
    

- 데이터프레임 내에서 상관계수 도출
    
    ```python
    import pandas as pd
    df = pd.read_csv('english.scv')
    
    df
    ```
    
    ```python
    df_corr = df.corr(method = 'pearson')
    df_corr
    ```
    
    - 변수 개수가 3개 이상인 경우, 알아서 2차원 형태의 테이블로 반환해줌
    - 결측치가 존재하는 경우, 해당 행을 무시하고 나머지 행으로만 게산됨
        - A의 3행이 없는경우 :
            - AB 비교 시 A3, B3 제외
            - BC 비교 시 B3, C3 포힘

- 유의점
    1. 피어슨 : `선형적인` 상관관계를 표시함
        - 선형이 아닌 형태의 관계를 알아차릴 수 없음 (ex_ Y = x^2)
    
    1. 큰 상관관계를 무조건 인과관계로 인식 X (보장하지 않음)
        - ex_ 모기 개체수 & 아이스크림 판매량
    
    1. 상관계수와 기울기는 무관함

---

### Spearman 상관계수

- 각 변수들의 순위로 상관관계 비교함
- Y = x^2 예시 :
    
    ![2024-03-25_15-41-18.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/492fe4bd-3346-4ffb-b5ac-e13cca2c7750/2024-03-25_15-41-18.jpg)
    
    - X : 1,2,3,4,5등 & Y : 1,2,3,4,5등 → 순위 일치함 (상관계수 1)
- 구현 :
    
    ```python
    df_corr = df.corr(method = 'spearman')
    df_corr
    ```
    

---

### Kendall 상관계수

- 상관계수 수식 :
    
    ![2024-03-25_16-01-40.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/38ff5b23-9bed-4639-899c-b024eea11e32/2024-03-25_16-01-40.jpg)
    
    - 순서쌍 ?
        
        ![2024-03-25_16-03-30.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/d9a57536-2581-4257-bfb9-be46376108e2/2024-03-25_16-03-30.jpg)
        
        - 각 변수의 6개 값 경우 : (2,3), (2,4), (2,5) … = 6C2 = 15쌍 존재
    - 조화 순서쌍, 비조화 순서쌍의 차이 활용
        - (2,3)의 경우 : (A>B, A>B) → 조화 순서쌍
    - 모든 순서쌍이 (A>B, A>B)
        - ⇒ Kandall = (15-0)/15 = 1
- 구현 :
    
    ```python
    df_corr = df.corr(method = 'kendall')
    df_corr
    ```
    

---

## 2. 선형회귀

### 선형 회귀 모형

- 수식
    
    ![2024-03-25_16-27-51.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/2ed05019-31e4-45db-8058-a6120eaffe3d/2024-03-25_16-27-51.jpg)
    
- 세타 값들을 조정하며 최적의 상수항과 계수를 구해야함
- `MSE (평균제곱오차)`를 최소화 !

### 선형 회귀 구현

1. 변수 분리
    
    ```python
    # 독립변수 x
    df_x = df[['1st', '2nd', '3rd', '6mo', '9mo']]
    # 종속변수 y
    df_y = df['suneung']
    ```
    
    - 수능 점수(y)를 구하기 위해서 독립변수들(x) 투입
    
2. 학습용 데이터와 검증용 데이터 분리
    
    ```python
    # 사이킷런 데이터 분리
    from sklearn.model_selection import train_test_split
    
    # 학습, 검증 데이터 분리
    x_train, x_valid, y_train, y_valid = train_test_split(df_x, df_y, random_state = 0, test_size = 0.20)
    ```
    
    - 20%만큼의 데이터를 검증용으로 분리 (test_size)
    - 정렬된 데이터셋에서 일부를 분리하게 되면, 학습용 데이터와 검증용 데이터에서의 차이가 발생하게 됨
        - → 랜덤으로 뽑아내어야 함 (train_test_split이 자체로 적용해줌)
        - random_state : 무작위 시드
    
3. 모형 생성과 계수 결정
    
    ```python
    import matplotlib.pyplot as plt
    
    # 사이킷런 선형 회귀
    from sklearn.linear_model import LinearRegression
    
    # 선형 회귀 모형 생성
    reg = LinearRegression()
    
    # 학습 데이터로 선형 회귀 식에서의 계수 결정
    reg.fit(x_train, y_train)
    
    # 학습된 상수항과 회귀 계수 출력
    print(reg.intercept_)
    print(reg.coef_)
    ```
    
4. 검증 데이터로 검증
    
    ```python
    # 선형 회귀 모형 생성
    y_pred = reg.predict(x_valid)
    y_pred
    ```
    
- K-fold Validation
    - 적은 수의 데이터가 있을 때 사용
    - 교차 검증 - 검증 데이터를 하나씩 뺴보며 각각 활용해봄 (검증 데이터 재사용)
    
    ```python
    from sklearn.model_selection import KFold
    ```
