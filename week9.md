## 1. k-최근접 이웃에 기반한 분류

### (1) k-Nearest Neighbor (k-NN) 분류

- 가장 가까운 이웃 k개를 바탕으로 분류를 수행하는 방법
- 새로운 데이터가 주어지면 그때그때 거리 연산을 통한 분류가 이루어지므로, 엄밀하게는 학습이라고 간주하기 어려움
- x가 여러개인 경우 유클리드 거리를 활용해 거리 계산 후 비교
- k는 홀수가 바람직 (동률일 경우 분류 결정을 위해 다른 방법 필요함)
- 분류 예시
    
    ![2024-04-29_15-15-34.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/8296a02c-5796-457e-bc04-a68a8ae1064b/2024-04-29_15-15-34.jpg)
    

### (2) 실습 - Confusion Matrix 출력

1. 필수 라이브러리
    
    ```python
    import pandas as pd
    from sklearn.neighbors import KNeighborsClassifier 
    from sklearn.model_selection import train_test_split 
    from sklearn.metrics import confusion_matrix
    ```
    
2. 데이터 불러오기 (위치와 분류값 보유)
    
    ```python
    df = pd.read_csv('treasure.csv')
    df
    ```
    
    ![2024-04-29_15-24-38.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/d6c2eb50-b7a9-41e4-9835-849ace490f76/2024-04-29_15-24-38.jpg)
    
3. 주어진 데이터에서 입력 변수와 예측 변수를 분리
    
    ```python
    df_x = df[['horizontal', 'vertical']]
    df_y = df['MineOrTreasure']
    ```
    
4. 데이터를 학습용과 검증용으로 분리
    - 30%를 검증용으로 분리 - 200개 중 60개
    
    ```python
    x_train, x_test, y_train, y_test = train_test_split(df_x, df_y, test_size=0.3, random_state=0)
    ```
    
5. 학습용 데이터를 바탕으로 k-NN 분류기 생성
    - n_neighbors 으로 k 값 설정
    - metric 으로 유클리드 거리로 계산하도록 설정
    
    ```python
    kNN = KNeighborsClassifier(n_neighbors = 3, metric = 'euclidean') 
    kNN.fit(x_train, y_train)
    ```
    
6. 만들어진 k-NN 분류기를 검증용 데이터에 적용
    
    ```python
    y_pred = kNN.predict(x_test)
    y_pred
    ```
    
    ![2024-04-29_15-29-14.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/d9951a4b-fb21-4f95-93b5-b514f26c4dea/2024-04-29_15-29-14.jpg)
    
7. Confusion Matrix 출력
    
    ```python
    confusion_matrix(y_test, y_pred, labels = ['MINE', 'TREASURE'])
    ```
    
    ![2024-04-29_15-30-18.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/054e9f98-5797-4544-a463-d0fb40a4d85c/2024-04-29_15-30-18.jpg)
    
    → 정확도 = 1, error rate = 0
    
    - Confusion Matrix 복습
        - `정확도` (Accuracy)
            
            ![2024-04-08_15-35-33.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/f447fa46-81e4-40d3-b421-851f7fe6d42c/2024-04-08_15-35-33.jpg)
            
        
        - 혼동 행렬 (confusion matrix)
            
            ![2024-04-08_15-36-25.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/6336cb4a-f69f-4b6c-a22d-9b8207202d64/2024-04-08_15-36-25.jpg)
            
            - `Accuracy` : 350/400 = 87.5%
            - `Error rate` : 50/400 = 12.5%
        
        `Recall`
        
        - 실제 positive 데이터 중, 맞게 예측한 비율
        - (실제 이탈한 사람 중, 이탈이라고 예측한 비율 : 270/290 = 93.1%)
        
        `Precision`
        
        - 예측 positive 데이터 중, 맞게 예측한 비율
        - (이탈할 것으로 예측한 사람 중, 실제 이탈한 비율 : 270/300 = 90.0%)
        
        `민감도` (Sensitivity)
        
        - 실제 positive 데이터 중, 맞게 예측한 비율 (= Recall)
        - (실제 이탈한 사람 중, 이탈이라고 예측한 비율 : 270/290 = 93.1%)
        
        `특이도` (Specificity)
        
        - 실제 negative 데이터 중, 맞게 예측한 비율
        - (실제 유지한 사람 중, 유지라고 예측한 비율 : 80/110 = 72.7%)
        

- exercise (test, train 자료를 따로 제공하는 경우)
    
    > 맛집은 맛집끼리 모여 있고, 평범한 음식점은 평범한 음식점끼리 모여 있다?
    > 
    
    ```python
    # 훈련 데이터 작업
    df = pd.read_csv('deli1.csv')
    df_x = df[['horizontal', 'vertical']]
    df_y = df['MatjipOrNot']
    ```
    
    ```python
    # k-NN 작업
    kNN = KNeighborsClassifier(n_neighbors = 5, metric = 'euclidean') 
    kNN.fit(df_x, df_y)
    ```
    
    ```python
    # split 없이 검증 데이터 작업
    df_test = pd.read_csv('deli2.csv')
    x_test = df_test[['horizontal', 'vertical']]
    y_test = df_test['MatjipOrNot']
    ```
    
    ```python
    y_pred = kNN.predict(x_test)
    confusion_matrix(y_test, y_pred, labels = ['MATJIP', 'NOT'])
    ```
    
    ![2024-04-29_15-42-44.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/0c09cd67-b330-4788-bbd3-4d87cc58ae55/2024-04-29_15-42-44.jpg)
    

### (3) k-NN을 활용한 회귀

- 거리에 따른 가중치를 고려할 때와 그렇지 않을 때로 나누어 각각 회귀 수행 가능
    
    ![2024-04-29_15-33-51.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/d877cef7-fb37-42ec-9ae1-780a700cd4e1/2024-04-29_15-33-51.jpg)
    

---

## 2. 사후확률과 Naive Bayes 분류

### (1) Naive Mayes (NB)

- 사후확률을 계산하여 분류를 수행하는 방법
    - 의존변수에 해당하는 각 경우의 확률에 대해서 사후확률을 구한 후, 비교
    - 가장 확률이 높은 경우 선정 & 확률 비교해 비율 고려
- 전제 : 각 독립변수가 확률적으로 `상호 독립`이라는 가정
- 데이터 형태에 따른 NB 종류 선정
    1. BernoulliNB : 입력 변수들이 모두 이진 형태인 경우에 활용
        - 출력 변수의 형태는 이진이 아니어도 상관 없이 활용 가능
    2. CategoricalNB : 입력 변수들이 범주형인 경우에 활용

- 독립변수에 따른 경우 예시 :
    
    ![2024-04-29_15-45-46.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/9d496b41-5156-4574-b3fb-23afdc5395c7/2024-04-29_15-45-46.jpg)
    

### (2) 사후확률 (Posteriori Probability)

- 예시 :
    
    ![2024-04-29_15-48-14.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/29f8940e-37f7-420b-a2ba-563b1686062a/2024-04-29_15-48-14.jpg)
    
    ![2024-04-29_15-48-34.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/adc67b71-273e-4225-8861-c3e016a4f777/2024-04-29_15-48-34.jpg)
    
    1. 승리 사후 확률
        
        ![2024-04-29_15-49-23.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/4e8c8790-3da9-4772-ba13-86b8048b74ea/2024-04-29_15-49-23.jpg)
        
        6/10 * 2/6 * 1/6 * 3/6 * 3/6 = 1/80
        
    
    1. 패배 사후 확률
        
        ![2024-04-29_15-49-44.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/aded3c08-4d9d-48dc-ac18-c7f713cad4d2/2024-04-29_15-49-44.jpg)
        
        4/10 * 1/4 * 1/4 * 2/4 * 3/4 = 3/320
        
    
    1. 사후확률 계산 - 비율로 비교
        - 승패 비율 = 4:3
            
            → 고려대학교가 승리했을 확률 4/7 = 57.1%
            

### (3) NB에서 특수 상황 고려 - 라플라스 보정

1. 조건부확률 중 0이 존재하는 경우?
    - Laplacian Correction (라플라스 보정) 활용
        - 조건부확률의 분모와 분자에 동일한 값을 일괄적으로 더함
        - 사후확률 계산 과정에서 결과값이 0이 되는 것을 방지
        - 약간의 왜곡 발생
        - ex. 6/10 * 2.2/6.2 * 1.2/6.2 * 3.2/6.2 * 3.2/6.2
        
2. 전제에 대한 오류 (한계점)
    - 변수들이 서로 독립이라는 가정 하에 동작하는데, 이러한 가정이 타당한가?
    - NB : 비교적 간편한 식으로 빠르게 분류를 수행하는 대신, 실제로 성립하기 어려운 가정을 채택한다는 단점을 지님
    

### (4) 실습 - NB 분류

1. 라이브러리 호출
    
    ```python
    from sklearn.naive_bayes import BernoulliNB
    ```
    
2. 지난 경기 데이터 불러오기
    
    ```python
    df = pd.read_csv('baseball.csv')
    df
    ```
    
    ![2024-04-29_16-26-17.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/ca1e9157-f6c4-4223-a8fe-9cc53b5c917c/2024-04-29_16-26-17.jpg)
    
3. 변수 분리
    
    ```python
    df_x = df[['error', 'pitcher', 'score', 'homerun']]
    df_y = df['GameResult']
    ```
    
4. NB 분류기 생성
    - 알파 : 라플라스 보정값
    
    ```python
    NB = BernoulliNB(alpha = 0)
    NB.fit(df_x, df_y)
    ```
    
5. 새로운 경기 데이터 제공
    
    ```python
    x_test = pd.read_csv('ku_vs_monsters.csv')
    x_test
    ```
    
    ![2024-04-29_16-27-20.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/29db5aa9-48a7-4179-96b7-4d420456b461/2024-04-29_16-27-20.jpg)
    
6. 새로운 경기 데이터에 NB 분류기 적용
    
    ```python
    y_pred = NB.predict(x_test)
    y_pred
    ```
    
    ![2024-04-29_16-27-48.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/ec71f7c4-1a99-420b-887f-b4ec2df52ed1/2024-04-29_16-27-48.jpg)
    
    = 승리
    

- exercise 2
    
    > 1. 전체 문자 메시지 중 스팸 문자 메시지의 비율은 60%
    2. 문자 메시지가 스팸일 때, '배팅'이라는 낱말을 포함할 확률은 90%
    3. 문자 메시지가 스팸이 아닐 때, '배팅'이라는 낱말을 포함할 확률은 20%
    > 
    
    > (가) 안전한★배팅-지금.가입하면~마일리지지급www.sportslotto.net
    (나) [공지]이중전공 선발관련, KUPID 로그인 후 결과 확인바람
    > 
    - 배팅 포함 여부로 파악
    - (가) 스팸사후확률 : 0.6 * 0.9 = 0.54
    - (가) 정상사후확률 : 0.4 * 0.2 = 0.08 ⇒ (가)는 스팸메시지
    - (나) 스팸사후확률 : 0.6 * 0.1 = 0.06
    - (나) 정상사후확률 : 0.4 * 0.8 = 0.32 ⇒ (나)는 정상메시지
    

---

## 3. 로지스틱 회귀의 원리와 그 실제

### (1) 로지스틱 회귀 + 승산

- 이전 의사결정 트리로 풀었던 예제 :
    
    ![2024-04-29_16-56-44.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/7ff6ba24-ea0a-4795-ba43-a97e06c94cb6/2024-04-29_16-56-44.jpg)
    
- 회귀식으로 범주에 대한 분류 수행?
    
    ![2024-04-29_17-00-04.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/17cc92f9-a8c0-478f-a5a0-1ff89191641d/2024-04-29_17-00-04.jpg)
    

- 승산 (odds)
    - (특정한 사건이 발생할 확률) / (특정한 사건이 발생하지 않을 확률)
        
        ![2024-04-29_16-59-41.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/13946448-9409-46d6-83ae-e7a26238b200/2024-04-29_16-59-41.jpg)
        
    - ex. 주사위 굴려 1 나올 승산 = 1/5

- 승산으로 정의하는 로지스틱 회귀
    
    ![2024-04-29_16-58-01.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/6bc35175-afb8-4ba1-a070-57afcd14825e/2024-04-29_16-58-01.jpg)
    
    ![2024-04-29_16-58-16.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/e66bde37-66e8-4b07-a1db-40ca059fc15a/2024-04-29_16-58-16.jpg)
    
    ![2024-04-29_16-58-32.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/ad1265b1-92e7-44d0-add1-207dc5855d15/2024-04-29_16-58-32.jpg)
    
    - p의 범위 : 0~1
        
        ⇒ p를 통해서 (0.5 기준으로) 결과값 예측
        
    

### (2) 실습 - 로지스틱 회귀

1. 데이터 불러오고 독립, 종속 변수 구분
    
    ```python
    df = pd.read_csv('telecom_logistic.csv')
    df_x = df.iloc[:, 0:7]
    df_y = df.iloc[:, 7]
    ```
    
2. 로지스틱 회귀 모형 생성
    
    ```python
    from sklearn.linear_model import LogisticRegression
    
    LR = LogisticRegression()
    LR.fit(df_x, df_y)
    
    print(LR.coef_)
    print(LR.intercept_)
    ```
    
    ![2024-04-29_17-07-30.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/46972f65-a3dc-4ccd-871a-cca2c7e14169/2024-04-29_17-07-30.jpg)
    

1. 예측 데이터에 적용해보기 (해석하기)
    
    ![2024-04-29_17-08-51.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/7a3924b1-e871-4c39-91e8-9dd6428759cc/2024-04-29_17-08-51.jpg)
    
    ![2024-04-29_17-09-04.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/a263f36d-d3e7-4784-9684-b64d609a85c8/2024-04-29_17-09-04.jpg)
    
    - log(Odds) = -0.8259 + (0.00186 * 50) + (0) + (0) + (0.71585 * 1) + (0) + (0.71529 * 1) + (-0.06541 * 80000) = -4.53
    - p = 1 / (1 + e^4.53) = 0.01
        
        ⇒ p < 0.5 → Leave 결과값이 0에 가까움
