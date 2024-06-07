## 1. K-평균 군집화

- 유사한 데이터끼리 같은 그룹으로 묶기

### (1) K-means Clustering

- 사전에 군집의 개수 K 를 결정
- 각 군집에는 중심이 존재 → 중심과 군집 내 데이터 거리 차의 제곱 합을 최소로 하는 최적 군집을 찾음
    
    ![2024-06-08_02-11-34.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/e47d4c1f-f249-4929-bc5e-74edac7ce02b/2024-06-08_02-11-34.jpg)
    

- 방법
    
    ![2024-06-08_02-15-07.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/d6d6f294-bacc-4106-8a4a-e5abe845b73e/2024-06-08_02-15-07.jpg)
    
    1. 랜덤으로 중심 K개를 설정하기
    2. 각각의 데이터에 대해 가까운 중심으로 군집 설정
    3. 각 군집에 대해서 중심의 위치를 업데이트 
    4. 각각의 데이터에 대해서 새로 생긴 중심점에 대해서 군집 재설정 
        
        → 변경 사항이 없을 때까지 3~4 iteration 반복
        

### (2) 실습 _ K-평균 군집화

1. 알고리즘 수행을 위해 필요한 라이브러리
    
    ```
    import matplotlib.pyplot as plt
    import pandas as pd
    from sklearn.cluster import KMeans
    
    plt.figure(figsize = (10, 5))
    ```
    
    - K-means 모듈 호출

1. DataFrame 불러오기
    
    ```python
    df = pd.read_csv('menu.csv', engine = 'python')
    df
    ```
    
    ![2024-06-08_02-25-08.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/f6b3a396-7157-4311-b8ab-0fb93c364d4c/2024-06-08_02-25-08.jpg)
    
2. K-means Clustering의 실제
    
    ```python
    df_data = df[['가격', '판매량']]
    
    km = KMeans(n_clusters = 4, random_state = 0) 
    km.fit(df_data)
    
    result = km.predict(df_data)
    result
    ```
    
    ![2024-06-08_02-26-35.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/ce465252-4714-4723-bc7d-8c3aa0219a59/2024-06-08_02-26-35.jpg)
    
    - 군집화 과정에서 사용되는 숫자 자료형 변수(가격, 판매량)만 가져옴
    - `n_clusters` 로 K 개수 설정

1. 군집의 중심 위치를 출력
    
    ```python
    km.cluster_centers_
    ```
    
    ![2024-06-08_02-22-18.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/5d6d4a9e-23f6-488a-b2b9-769f9d214bad/2024-06-08_02-22-18.jpg)
    
2. 할당 결과를 DataFrame에 하나의 열로 추가
    
    ```python
    df['CLUSTER'] = result
    ```
    
3. 산점도로 시각화
    
    ```python
    plt.scatter(df['가격'], df['판매량'], c = df['CLUSTER']) 
    plt.show()
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/0f4891de-849f-4120-a4de-63ec1c64ca12/Untitled.png)
    
    - x, y축 레이블 설정 후 `c`로 4가지의 색별 군집 분류
    
- 결과 분석
    - 두개의 변수를 넣었음에도 불구하고, 가격보다는 판매량을 중심으로 군집이 형성된 모양을 보임
    - 두 변수의 scale이 다르기 때문 (거리의 제곱합에 큰 영향)
        
        → 필요 시 스케일링 (재조정)을 수행한 후 실행 시 다른 결과값을 확인할 수 있을 것
        

- Exercise 1)
    
    
    ![2024-06-08_02-31-05.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/8d0688e2-25d0-4797-a053-bb135054b747/2024-06-08_02-31-05.jpg)
    
    문제 :
    
    - C, E를 최초 중심점으로 설정
    - 거리 기준: 맨해튼 거리
        
        ![2024-06-08_02-31-56.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/40ac6b78-f9d8-4e6d-9253-5ab4bb9b5da8/2024-06-08_02-31-56.jpg)
        
    
    풀이 :
    
    ![2024-06-08_02-34-48.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/809d66d6-f70f-40d2-b138-f6659d09c228/2024-06-08_02-34-48.jpg)
    
    ![2024-06-08_02-35-34.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/cf1fe6e0-496f-47e5-95b6-71b752bbf7e2/2024-06-08_02-35-34.jpg)
    

### (3) 군집 개수 K의 최적의 값 - Elbow Method

- Elbow Method
    - 가로축을 K, 세로축을 오차(SSE)로 하는 그래프에서 기울기가 완만해지는 지점을 최적의 K로 선택
        
        ![2024-06-08_02-37-54.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/86f0d8a9-a9b8-411f-b974-4bd89c5e5593/2024-06-08_02-37-54.jpg)
        

---

## 2. 퍼지 군집화

### (1) Fuzzy Clustering

- 기존 군집화의 문제점
    - 다수의 군집화 방법론은 단일 데이터에 대해 반드시 하나의 군집만 할당한다는 특성을 지님
        - ex. 월요일 날씨는 무조건 비가 옵니다 (100%)

- 퍼지 군집화 = 각각의 데이터마다 특정 군집에 속하게 될 가중치를 배정하는 방식
    - 대표적인 알고리즘 : FCM 알고리즘

### (2) FCM 알고리즘 (Fuzzy C-Means)

- 사전에 정의된 퍼지 승수 p와 군집수 K에 대해 다음과 같은 절차로 군집화를 수행
    - 군집수 K : K-means와 유사
    - 퍼지 승수 p :
        - 1 ~ 무한대 사이의 값
        - p값이 1에 가까울수록 K-means와 유사해짐
        - p값이 올라갈수록 각 중심이 전체 평균과 유사해짐 (fuzzier 해진다)
    
    1. 개별 데이터에 대해 임의로 각 군집에 속하게 될 가중치를 부여 (1번 데이터는 A 0.6, B 0.4, …)
    2. 부여된 가중치를 이용하여 각 군집의 중심이 어디인지 계산
    3. 군집 별 중심에 부합하게끔 개별 데이터에 대한 가중치 재계산
    4. 재계산된 가중치에 기초하여 새로운 군집 별 중심 계산
    5. 가중치가 일정한 값에 수렴할 때까지 가중치 재계산과 군집 중심 재계산 반복

- 개별 데이터들의 가중치를 알 때, 군집의 중심을 계산하는 방법 = 가중평균 & p의 승수
    - ex. 군집 A의 중심 x1 = $\frac{(-300*0.2^p-200*0.3^p+200*0.5^p+100*0.4^p)}{ (0.2^p + 0.3^p + 0.5^p + 0.4^p)}$
        
        ![2024-06-08_03-17-50.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/f5368a15-471a-4271-b15a-83751907eb1c/2024-06-08_03-17-50.jpg)
        

### (3) K-means의 한계 보완 가능한 군집화 방법론

- K-means 군집화
    - 군집의 수를 정해놓고, 각 군집의 중심에 데이터가 최대한 몰리게끔 군집화
    - 원형으로 몰려 있는 데이터가 아닌 경우 부적절하다는 단점
        
        ![2024-06-08_03-27-14.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/9db2fdaa-810a-4439-836b-c29f9d794baa/2024-06-08_03-27-14.jpg)
        

- 해결 가능한 다른 군집화
    1. 밀도 기반 군집화
        - 같은 군집에 속한 데이터들은 끼리끼리 밀도 있게 모여있을 것이라 가정
        - DBSCAN (Density-Based Spatial Clustering of Applications with Noise)
    2. 계층적 군집화
        - 유사도가 높은 개체끼리 묶고, 그 군집을 다시 다른 개체와 묶는 과정을 반복
        - Dendrogram을 통해 군집화 결과를 시각화할 수 있음

---

## 3. 밀도 기반 군집화

### (1) DBSCAN

- DBSCAN (Density-Based Spatial Clustering of Applications with Noise)
    - `원의 반지름`(epsilon)과 `원 안에 포함될 최소 점 개수`(min point)를 사전에 지정 (수정 불가능)
    - 데이터를 중심으로 원을 그리고 → 최소 개수보다 더 많은 점이 포함되면, 원으로 점을 묶음
    - 만들어진 원들을 하나로 이어 최종 군집을 결정
    - 군집 개수 지정 불가능

- 예시 (최소 점 3개)
    
    ![2024-06-08_03-33-09.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/9ab8cb20-7f75-45a3-bc39-176a0518f802/2024-06-08_03-33-09.jpg)
    
    - 구성 성공한 원판들의 합집합 (5개 데이터)가 하나의 군집을 이루게 됨
    - 나머지 두 데이터 : Noise = 아무 군집에도 할당되지 못한 노이즈 (이상치)

- DBSCAN으로 해결 가능한 문제
    - ex. 위치가 가까운 주택끼리 재개발 구역 분류
    - 모든 구역이 무조건 재개발 구역으로 분류할 필요는 없음 (노이즈로 설정, K-means는 불가능)

### (2) 실습 _ DBSCAN

1. 군집화 및 시각화를 위한 라이브러리
    
    ```python
    import pandas as pd
    from sklearn.cluster import DBSCAN
    import matplotlib.pyplot as plt
    ```
    
2. 데이터프레임 불러오기
    
    ```python
    df = pd.read_csv('housing.csv')
    house_position = df[['x', 'y']]
    house_position
    ```
    
    ![2024-06-08_03-43-35.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/aaea1902-f887-4bbd-a659-feed345edec9/2024-06-08_03-43-35.jpg)
    
3. 모수 결정 및 DBSCAN 군집화 수행
    
    ```python
    db = DBSCAN(eps = 10, min_samples = 3)
    cluster_pred = db.fit_predict(house_position)
    ```
    
    - `eps`로 원의 반지름, `min_samples`로 최소 점 개수 설정
    
4. 원본 데이터가 어떤 군집에 속하게 되었는지 확인
    
    ```python
    cluster_pred
    ```
    
    ![2024-06-08_03-44-48.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/0124e5d2-9a83-493a-8436-3bd1a925ae5b/2024-06-08_03-44-48.jpg)
    
    - 노이즈는 `-1` 군집으로 부여
    
5. 할당 결과를 DataFrame에 하나의 열로 추가
    
    ```python
    df['CLUSTER'] = cluster_pred
    ```
    
6. 군집화 결과 시각화
    
    ```python
    x = house_position['x']
    y = house_position['y']
    plt.scatter(x, y, c = cluster_pred)
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/e4ff09dc-cc66-4dbf-9a53-cba703ce6c8c/Untitled.png)
    
    - x, y축 레이블 설정 후 `c`로 색별 군집 분류
    - 군집 개수는 3개로 결정됨 & 노이즈 6개 걸러내기

### (3) Hierarchical DBSCAN

- 기존 DBSCAN의 가능한 문제점
    - epsilon값을 정하는 방식의 문제점 (군집 별로 밀도가 다른 경우)

- HDBSCAN
    - 계층적 차원에서 DBSCAN을 수행함으로써 종전 방법론의 문제점을 개선
    - 반경에 해당하는 epsilon값을 설정하지 않아도 군집화가 가능
        
        → 군집간 밀도 차로 인해 군집화가 잘못된 방향으로 이루어지는 문제를 해결
        
        ![2024-06-08_03-50-05.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/742edf3a-5803-4134-8231-78aa690877cc/2024-06-08_03-50-05.jpg)
        

- Exercise 2)
    
    ![2024-06-08_03-51-36.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/0a4cf4a2-1b91-44c3-876e-3853a43a590f/2024-06-08_03-51-36.jpg)
    
    에서 가능한 실험 결과?
    
    - (A)서 같은 군집에 속했던 X와 Y가 (B)서는 다른 군집에 속함 (O)
        
        ![2024-06-08_03-52-54.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/c0610527-a234-458c-bf6e-325251bfd012/2024-06-08_03-52-54.jpg)
        
    - (B)서 다른 군집이었던 X와 Y가 (C)서는 같은 군집에 속함 (O)
        
        ![2024-06-08_03-54-18.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/181f432c-4a32-4058-b1ca-f03894e337e6/2024-06-08_03-54-18.jpg)
        
    - (C)서는 노이즈로 분류되었던 X가 (A)서는 특정 군집에 속함 (X - 더 까다로워진 조건)
        
        ![2024-06-08_03-55-21.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/99b7cc65-b253-4c61-b67f-f5f5c02aa333/2024-06-08_03-55-21.jpg)
        
    

---

## 4. 계층적 군집화

### (1) Hierarchical Clustering + Dendrogram

- Hierarchical Clustering
    - 데이터로 주어진 모든 개체끼리의 유사도를 계산
    - 유사도가 가장 높은 `두 개체`를 하나의 군집으로 묶음
    - 같은 과정을 반복해 Tree 형태로 최종 군집화 수행 → Dendrogram 도출
        
        ![2024-06-08_03-56-48.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/0f38a9bb-3405-44b5-893e-50ebe5a03af0/2024-06-08_03-56-48.jpg)
        
        - A,B가 가장 가까움 → 하나의 점으로 생각 ⇒ AB와 C가 다음으로 가까움 → 하나의 큰 점으로 생각
        
- 군집과 점(or 군집) 사이의 유사도 계산 5가지 방법
    1. Single Linkage : 서로 다른 그룹의 점 사이 거리의 최솟값
    2. Complete Linkage : 서로 다른 그룹의 점 사이 거리의 최댓값
    3. Average Linkage : 서로 다른 그룹의 점 사이 거리의 평균값 (2개, 2개 그룹 → 4개 거리 평균)
    4. Centroid Linkage : 각 그룹의 중심 간 거리
    5. Ward Linkage : 그룹끼리 합칠 시 늘어날 중심에서의 SSE를 최소화하는 방향으로 군집화 수행

### (2) 실습 _ Dendrogram

1. 계층적 군집화 및 시각화를 위한 라이브러리
    
    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import scipy.cluster.hierarchy
    from scipy.cluster.hierarchy import linkage
    from scipy.cluster.hierarchy import dendrogram
    ```
    
2. 데이터 불러오고 계층적 군집화 수행
    
    ```python
    df = pd.read_csv('housing.csv')
    house_position = df[['x', 'y']]
    
    linked = linkage(house_position, 'single')
    dendrogram(linked, orientation = 'top')
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/1764f708-2a5a-4fc6-a5c4-33965cd9db10/Untitled.png)
    
    - `'single'` : 유사도 계산에 Single Linkage 활용
    - 30개 주택들을 계층적으로 군집화한 결과 표시됨 (모든 주택이 연결됨)
    - 색은 중요하지 않음
        - 세로 값에 따른 기존선을 설정해서 군집 분류
            
            ![2024-06-08_04-08-56.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/06a9b54f-62f7-42c0-bf11-411d7616fe93/2024-06-08_04-08-56.jpg)
            
            - 현재 이미지 예시 : 5개의 군집 생성됨
            - 기준선에 따라서 군집 개수를 맘대로 설정할 수 있음
            

---

## 5. 군집화 결과 평가 방법

### (1) Silhouette Analysis

- Silhouette Analysis
    - 군집화를 모두 마친 이후, `모든 데이터에 대하여` 실루엣 계수를 계산 후 도식화
    - 실루엣 계수의 범위는 -1 이상 1 이하이나 음수인 경우는 드묾
    - 실루엣 계수의 값이 클수록 군집화가 더 잘 이루어진 것임
        
        ![2024-06-08_04-11-40.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/c4a945c6-a115-4483-a28f-508a80d28088/2024-06-08_04-11-40.jpg)
        
        - 모든 데이터들의 실루엣 계수를 막대그래프로 나열해둔 Plot
        
- 실루엣 계수 계산 방법
    
    ![2024-06-08_04-12-05.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/955f0365-bcdd-429e-8959-73117e4efd6f/2024-06-08_04-12-05.jpg)
    
    ![2024-06-08_04-17-02.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/2d54afd4-3d2d-4f81-a792-eb08f3b88b9f/2024-06-08_04-17-02.jpg)
    
    - 군집 내 다른 모든 데이터들과의 거리를 평균내서 활용함

### (2) Hopkins Statistic

- 군집화 필요성이 존재하는 데이터셋인지 확인하는 방법

- Hopkins Statistic
    - 군집화 수행 이전, 해당 데이터셋이 군집화하기에 적절한지 판단하기 위한 지표
    - 값의 범위는 0 이상 1 이하 (1에 가까울수록 좋음)

- 계산 방법
    1. 주어진 데이터셋에서 N개의 표본을 추출
    2. 균등분포에서 무작위 N개의 표본을 추출
    3. 모든 표본에 대해 데이터셋 내 최근접 데이터와 거리 계산
    4. 아래 식의 값이 홉킨스 통계량
        
        ![2024-06-08_04-21-12.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/b8ff8021-9825-4c88-af4f-7ee739f1a225/2024-06-08_04-21-12.jpg)
        

- 활용 예시
    
    
    ![2024-06-08_04-21-36.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/9333d0e8-66d1-4d3f-a668-795ceba97681/2024-06-08_04-21-36.jpg)
    
    ![2024-06-08_04-22-02.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/091f4e73-8972-49c7-81a3-7d41748b68f4/2024-06-08_04-22-02.jpg)
    
    - 데이터셋 표본 거리 합이 작을수록 (아주 가까운 데이터가 있으므로) 1에 가까워짐 → 군집화가 필요함
