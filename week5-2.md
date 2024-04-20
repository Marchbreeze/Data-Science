## 1. 의사결정 트리와 분류

### (1) 불순도

- `분류`의 원리
    
    ![2024-04-01_16-38-35.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/c92d8189-00c6-4e1a-a6d0-cc5e85efc579/2024-04-01_16-38-35.jpg)
    
    - 나무의 Root Node에서 출발하여 Leaf Node에 이르기까지 분기를 수행
    - 데이터를 활용해 결과 예측해 분류 (이탈하는 경향을 가짐, but 모두 이탈은 x)
    - 분류 성능은 분기의 기준에 영향 → `불순도` 활용

- 불순도
    1. `Gini Index`
        
        ![2024-04-01_16-39-30.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/71ffac57-f7e3-408d-b102-c0099842efc3/2024-04-01_16-39-30.jpg)
        
        - ex. 강아지 2, 고양이 1 : Gini = 1 - 4/9 - 1/9 = 4/9
    2. `Entropy`
        
        ![2024-04-01_16-39-38.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/4877d533-af44-4605-a8c4-cebae7ca7ba9/2024-04-01_16-39-38.jpg)
        

---

### (2) 의사결정 트리 생성

- `불순도가 낮아지는 방향`으로 주어진 데이터를 분류하는 분석 방법
- 어떤 불순도 지표를 택할지, 어떻게 분류할지에 따라 생성 방식이 상이

- `CART` (Classification And Regression Tree)
    - 지니 계수에 기초하여 트리 생성
    - ex.
        
        ![2024-04-01_17-09-19.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/b05a3429-722d-4fcc-8150-932adc314003/2024-04-01_17-09-19.jpg)
        
        - case1 사용 = 1 - (4/25) - (9/25) = 12/25
        - case1 비사용 = 0 (몰빵)
        - case1 가중 평균 → Gini = 5*(12/25) + 2*0 / 7 = 12/35
        - …
        - 3가지 case를 비교해서 가장 불순도 낮은 case를 분류 기준으로 선택

- 코드 구현 :
    
    ```python
    # 트리 시각화 위한 라이브러리 설치
    !pip install graphviz
    ```
    
    ```python
    # 주요 라이브러리 불러오기
    import pandas as pd
    from sklearn.tree import DecisionTreeClassifier
    from sklearn.tree import export_graphviz
    import graphviz
    ```
    
    ```python
    # DataFrame 제어
    df = pd.read_excel('telecom.xlsx')
    df = pd.get_dummies(df, columns = ['Technology'])
    df_x = df.iloc[:, [0, 1, 2, 4, 5, 6, 7]]
    df_y = df['Leave']
    ```
    
    ```python
    # 의사결정 트리 생성
    tree_model = DecisionTreeClassifier(criterion = 'gini', 
                                        max_depth = 3, 
                                        min_samples_leaf = 5)
    tree_model.fit(df_x, df_y)
    ```
    
    - 지니 계수 활용, 깊이 3, 리프 노드 최소 5개로 트리 생성
    
    ```python
    # 의사결정 트리 시각화
    dot_data = export_graphviz(tree_model, 
    													 out_file = None,
                               feature_names = df_x.columns,
                               class_names = tree_model.classes_,
                               filled = True, 
                               rounded = True,
                               special_characters = True)
    graph = graphviz.Source(dot_data)
    graph
    ```
    
    - feature_names = df_x.columns
    - class_names = tree_model.classes_ 로 설정
    - filled : class 별로 색칠, 불순도 낮을 수록 진한 색상
    
    ![2024-04-01_17-19-52.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/d04d2162-28a5-444e-a7a4-af28ee3f5871/2024-04-01_17-19-52.jpg)
    
    - sample : 분류에 해당하는 데이터 수 (분류 보려면 하위 노드 sample 확인)
    - value : 해당 시점에서 최종 목표에 해당하는 데이터 수
    - ex)
        - 루트 노드에서 4000개를 분류할 때
            - 분류 : True 2883개 & False 1117개
            - 클래스 : NO 3365개 & YES 635개
            
    
    ```python
    # 생성된 의사결정 트리를 활용해 임의 데이터를 분류
    df_predict = pd.read_excel('telecom_new.xlsx')
    df_predict = pd.get_dummies(df_predict, columns = ['Technology'])
    
    tree_prediction = tree_model.predict(df_predict)
    tree_prediction
    ```
    
    ![2024-04-01_17-36-53.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/894bf4a4-5969-4bd5-aad3-57f43a34ab38/2024-04-01_17-36-53.jpg)
    

---

### (3) 의사결정 트리의 회귀

- 분류 후, `해당 리프에 존재하는 모든 원소들의 y의 평균으로 예측`
- 회귀에서의 불순도로는 Gini 대신 `표준편차`를 활용함
- ex)
    - CASE 1:
        - 5G 사용 집단의 표준편차
        - 5G 미사용 집단의 표준편차
            
            → 가중표준편차 도출
            
    - …

---

## 2. 분류를 평가하는 방법

### (1) 정확도

- `정확도` (Accuracy)
    
    ![2024-04-08_15-35-33.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/f447fa46-81e4-40d3-b421-851f7fe6d42c/2024-04-08_15-35-33.jpg)
    

- 혼동 행렬 (confusion matrix)
    
    ![2024-04-08_15-36-25.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/6336cb4a-f69f-4b6c-a22d-9b8207202d64/2024-04-08_15-36-25.jpg)
    
    - `Accuracy` : 350/400 = 87.5%
    - `Error rate` : 50/400 = 12.5%

- 정확도의 단점
    - 이탈을 예측한 것과 유지를 예측한 것이 모두 합쳐져서 표시되어 있음
    - 이탈 예측과, 유지 예측에 대해서 중요도에 따라 전략을 설정할 수 없음
    

---

### (2) 정확도 지표의 확장

`Recall`

- 실제 positive 데이터 중, 맞게 예측한 비율
- (실제 이탈한 사람 중, 이탈이라고 예측한 비율 : 270/290 = 93.1%)

`Precision`

- 예측 positive 데이터 중, 맞게 예측한 비율
- (이탈할 것으로 예측한 사람 중, 실제 이탈한 비율 : 270/300 = 90.0%)

---

`민감도` (Sensitivity)

- 실제 positive 데이터 중, 맞게 예측한 비율 (= Recall)
- (실제 이탈한 사람 중, 이탈이라고 예측한 비율 : 270/290 = 93.1%)

`특이도` (Specificity)

- 실제 negative 데이터 중, 맞게 예측한 비율
- (실제 유지한 사람 중, 유지라고 예측한 비율 : 80/110 = 72.7%)
    
    ![2024-04-21_04-36-53.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/e70fcfe3-440c-465f-b179-a19edf8e883a/2024-04-21_04-36-53.jpg)

특이도 (Specificity)

- 실제 negative 데이터 중, 맞게 예측한 비율
- (실제 유지한 사람 중, 유지라고 예측한 비율 : 80/110 = 72.7%)
