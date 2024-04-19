## 1. 공공데이터 품질 관리와 오류율

### (1) 데이터 품질 지표 DQI

- 데이터의 품질을 측정하는 기준 : DQI 활용

1. 일관성 
    - 표준 오류율과 직결
    - 세부 지표 : 속성 / 표준 / 중복값 / 연계값
    - 개체의 속성이 표준을 준수하고 있으며 중복값은 없는가?
    - 데이터 연계에 있어 일관성이 유지되고 있는가?
    - ex) 01012345678 ↔ 010-1234-5678
    
2. 완전성
    - 구조 오류율과 직결
    - 세부 지표 : 논리모델 / 물리구조 / 식별자 / 속성의미
    - DB 구조 구축에 있어 논리적 모델 설계와 물리적 구조가 올바르게 구축됐는가?
    
3. 정확성
    - 값 오류율과 직결
    - 세부 지표 : 입력값 / 업무규칙 / 범위·형식 / 참조관계 / 계산식
    - 데이터가 유효한 범위 및 형식으로 구성되어 있는가?
    - ex) MMDD ↔ 1232 (12월 32일?)
    
4. 준비성
    - 데이터 품질 관리 정책 및 지침이 기관에 맞게끔 잘 정의되어 있는가?
    - 경영자 또는 의사결정권자가 데이터 품질 관리 필요성을 이해하고 있는가?
5. 보안성
    - DB 관리 시스템이 정전이나 재해 등에 대비되어 있는가?
    - 저작권, 개인정보 등 비공개 대상 정보를 파악하여 관리하고 있는가?
6. 적시성 
    - 데이터 갱신 주기와 방법이 명시적으로 정의되어 있는가?
    - 주요 데이터 별 응답시간 기준이 마련되어 있는가?
7. 유용성
    - 사용자가 만족할 정도의 충분한 정보가 제공되고 있는가?
    - 정보 접근을 위한 사용자의 편의성이 충분히 확보되었는가?
    

### (2) 데이터 품질 오류율

- 데이터 품질의 오류율의 정량적인 산출 : 3항목의 가중평균
    
    ![2024-04-17_01-47-17.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/b0b2fcc3-b8ef-430e-b260-bc0ef96cc419/2024-04-17_01-47-17.jpg)
    
1. 값 오류율
    - 진단 항목별 전체 데이터건수 오류 대비 데이터 건수가 가지는 비율
        
        (값 자체가 잘못 명시된 비율)
        
    - DQI 중 정확성과 직결

1. 표준 오류율
    - 표준 오류율 측정을 위한 각 진단 항목별 오류율의 산술평균
        - 표준 오류율 측정을 위한 진단 항목이 A,B 2개인 경우 : A 항목의 오류율 4%, B 항목의 오류율 6% → 표준 요류율 5%
    - DQI 중 일관성과 직결
    
2. 구조 오류율
    - 구조 오류율 측정을 위한 각 진단 항목별 오류율의 산술평균
        - = 표준 오류율 방법
    - DQI 중 완전성과 직결
    

---

## 2. 이상치와 결측치에 대한 대응

### (1) 이상치 구분 방법

- Box-and-Whisker Plot
    
    ![2024-04-17_01-56-39.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/8283271e-8fb2-436d-b357-b77aca437d9b/2024-04-17_01-56-39.jpg)
    
    - 분위 : Q3 (상위 25%) & Q1 (하위 25%)
    - IQR (Interquartile Range) = Q3 - Q1
        - 데이터 셋의 절반이 IQR 범위 내에 분포함
    - Median(중앙값) → 평균, 최빈값을 Box Plot에서 도출해낼 수는 없음
        - 정규분포 : Median이 박스 중앙에 위치
    - Max & Min : Outlier(이상치)를 제외한 최대 최소

- IQR을 활용한 이상치 구분 방법
    1. Tukey’s Fences
        - Q3보다 1.5 IQR 보다 더 큰 값
        - Q1보다 1.5 IQR 보다 더 작은 값
        
    2. Carling’s Modification
        - Median과 2.3 IQR 이상의 차이가 나는 값
    
- IQR을 활용하지 않는 이상치 구분 방법
    - 3-Sigma Rule
        - 정규분포 하에서 평균과 3𝜎 이상의 차이가 나는 값
            
            (99.7%의 데이터는 3𝜎 거리 내에 위치)
            
            ![2024-04-17_02-57-39.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/a5237108-f98f-45f2-b6bf-9161d5d04487/2024-04-17_02-57-39.jpg)
            

### (2) 결측치의 종류

1. MCAR
    - Missing Completely At Random
    - 결측치가 특정 변수와 관계없이 무작위로 발생 (완전 무작위 결측)

1. MAR
    - Missing At Random
    - 결측치가 다른 변수와 연관되어 발생

1. MNAR
    - Missing Not At Random
    - 다른 변수와 관계없이, 변수 스스로와 연관되어 결측치가 발생

- 예시
    
    ![2024-04-17_03-01-02.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/7a012980-b879-4961-b8b6-6f8b89b242f0/2024-04-17_03-01-02.jpg)
    
    - Age : MCAR (다른 변수와 관계없이 발생)
    - Lane : MAR (Gender == F인 경우 발생)
    - Tier : MNAR (5,6이 Bronze인 경우, 밝히지 않아 발생)
    

### (3) 결측치 처리 방법

1) 삭제

1. Column Drop 
    - 결측치가 일정 비율 이상인 열 제거
    - ex) 25%으로 설정하면 Age, Tier, Lane 제거
    
2. Listwise Deletion 
    - 결측치 하나라도 존재하는 행 제거
    - ex) 2,3,5,6,7 행 제거
    
3. Pairwise Deletion 
    - 분석 모델에 투입되는 열에 대해, Listwise Deletion 진행
    - ex) Age, Tier 분석 시 → 2,5,6,7 행 제거

2) 대체

![2024-04-17_03-09-15.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/3b24001e-9510-44ad-ab22-77358a291213/2024-04-17_03-09-15.jpg)

1. 평균, 최빈값으로 대체 
    
    → but, X2, X3, X4를 활용하지 않는 방법 = 권장 X
    

1. k-NN 방법
    - 유클리드 거리 - 다른 변수들을 통해 각 행 간 차이 파악
        
        ![2024-04-17_03-13-19.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/58da6e8e-5f9c-481c-941e-38e1260d613d/2024-04-17_03-13-19.jpg)
        
    - 가장 가까운 행들의 값으로 결측치 대체
        
        ![2024-04-17_03-17-42.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/f2dc3b4c-b399-4e35-8e97-c32f8a348626/2024-04-17_03-17-42.jpg)
        
    
2. 가중치를 부여한 k-NN 방법
    
    ![2024-04-17_03-18-49.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/5fe225af-d507-45e3-b328-04ac2f20da2b/2024-04-17_03-18-49.jpg)
   