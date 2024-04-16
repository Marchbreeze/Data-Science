## 1. 공공데이터의 뜻과 관리 지침

### (1) 공공데이터법
 
- 공공데이터의 제공 및 이용 활성화에 관한 법률 | 제2조 제2호
    
    > 데이터베이스, 전자화된 파일 등 공공기관이 법령 등에서 정하는 목적을 위하여 생성 또는 취득하여 관리하고 있는 광(光) 또는 전자적 방식으로 처리된 자료 또는 정보
    > 

- 개별 공공기관이 일상적 업무수행의 결과물로 생성 · 수집 · 취득한 텍스트, 수치, 이미지, 동영상, 오디오 등 다양한 형태의 모든 자료 또는 정보가 대상
    - ex) DB, 전자화된 파일
- 기계 판독이 가능한 형태 (Machine Readable) 로 정비하고자 공공기관장은 노력해야 함

### (2) Open Data 5 Star

- World Wide Web의 창시자인 Tim Berners-Lee가 주창 (1~5스타)
- 기계판독 정도 : 스타 1 / 2 / 3~5

1. 미충족 포맷 
    1. 스타 1
        - 특정 SW에서 읽기만 가능, 자유로운 수정 · 변환 불가
        - ex. PDF, 표 캡쳐
    - 미충족 포맷은 공공데이터포털에 등록이 불가능
    
2. 최소충족 포맷 
    1. 스타 2
        - 특정 SW에서 읽기 · 수정 · 변환 가능
        - ex. HWP, XLS, JPG, PNG, MP3, WMV, …
        
3. 오픈 포맷 (스타 3,4,5)
    1. 스타 3 
        - 적어도 하나의 비독점적 SW에서 읽기 · 수정 · 변환 가능
        - ex. CSV, JSON, XML
    2. 스타 4 
        - URI에 기초하여 데이터의 **속성 및 관계에 대해 기술**
        - ex. RDF (Resource Description Framework)
    3. 스타 5 
        - 웹상의 다른 데이터와 **연결되어 제공**되는 데이터
        - ex. LOD (Linked Open Data) - 확장자 X, 데이터가 서로의 연결 관계로 이루어진 웹
    - 공공기관은 공공데이터를 오픈 포맷의 형태로 제공하는 것이 원칙
    

### (3) 공공데이터 진행 과정

1. 생성 · 수집
    - 기관이 생성하는 데이터를 기계 판독이 가능한 꼴(Machine Readable)로 정비
    - 데이터 개방을 위해 공공데이터의 제공대상을 정의

1. 처리 (검토)
    - 제공하고자 하는 데이터가 법적으로 공공데이터에 해당하는지 검토
    - 제공제외대상이 포함되어 있는지(ex. 개인정보), 저작권 확보 여부 등을 확인
    
2. 등록
    - 공공데이터목록등록관리시스템에 제공대상 목록 및 데이터를 등록

1. 제공
    - 기관의 데이터를 국민과 기업 등을 대상으로 개방 및 제공

1. 사후 관리
    - 제공 단계 이후 공공데이터 변경사항 및 갱신주기 등을 반영
    - 데이터 이용에 있어서의 불편사항 접수 및 분쟁 조정 처리
    

### (4) 공공데이터 제공 신청

- 공공데이터 목록에 없는 데이터일지라도 개인이 제공 신청할 수 있음
- 공공기관장은 저작권 등의 요소를 검토한 후 10일 이내에 제공 여부를 결정해야 함
    
    (공공데이터법 제27조 제3항)
    
    ![2024-04-16_00-21-39.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/99835488-d93d-4ec6-accf-e0a0008c8492/2024-04-16_00-21-39.jpg)
    

 

---

## 2. 공공데이터 수집 및 활용

### (1) 공공데이터 수집 경로

- 공공데이터포털 ([https://www.data.go.kr](https://www.data.go.kr/))
    - 행정안전부장관은 공공데이터의 효율적 제공을 위하여 통합제공시스템을 구축해야 함 (공공데이터법 제21조 제1항)
    - 다양한 정보가 제공됨
        
        ![2024-04-16_00-26-10.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/af2f4135-a47b-42fd-91ea-6816c7de21a8/2024-04-16_00-26-10.jpg)
        
        - 유형 분류 (파일데이터, 오픈API, 표준데이터셋, …)
        - 확장자 형식

- 이외 경로 (각 분야에 전문적이고 특화된 데이터 제공)
    
    ![2024-04-16_00-24-39.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/4992fe33-e690-42e0-bf45-997a73344cbe/2024-04-16_00-24-39.jpg)
    

### (2) 웹 크롤링과 OpenAPI

- 웹 크롤링
    - 웹상의 데이터를 가공해서 수집해오는 것
    - ~~BeautifulSoup을 이용한 정적 크롤링, Selenium을 이용한 동적 크롤링~~
    - OpenAPI를 이용한 데이터 접근 및 수정 - 역시 웹 크롤링의 종류로 볼 수 있음

- OpenAPI
    - 파일 형태가 아닌 OpenAPI 방식으로 공개된 데이터도 공공데이터포털에 존재
    - API 활용을 위한 인증 키 신청 후, Python을 이용하여 데이터 수집 가능
    
    ![2024-04-16_00-36-36.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/20066460-0daa-422a-81a7-ed255c20c7b4/2024-04-16_00-36-36.jpg)
    
1. 인증 키 요청
    
    ![2024-04-16_00-39-10.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/f71c788e-f844-4118-a1bb-fbbb0be6c63f/2024-04-16_00-39-10.jpg)
    
    - 제공 기관마다 절차가 다를 수 있음
    - 활용 신청 - 발급 요청 후 1시간 내외의 시간이 소요될 수 있음
    - 참고문서
        - 데이터를 활용 가능한 가이드라인을 제공함
        - 데이터 표준 - XML, JSON 등 제공 확장자 표시됨
        - 요청-응답 메시지 & 에러코드 제공
            - 요청 메시지 명세서 제공 - query, path의 종류 표시
            - 예시 API
                
                ```html
                http://apis.data.go.kr/B553530/BEEC/BEEC_01_LIST?serviceKey=서비스키&apiType=XML&q1=2022&q2=2022&q3=1&q4=1&q5=9
                ```
                
                - 발급받은 인증키를 “서비스키” 부분에 넣어서 사용
                

---

1. XML 형식 데이터의 수집 & 처리
    
    (1) 인증키를 활용한 서버통신 진행
    
    ```python
    import requests
    from bs4 import BeautifulSoup
    
    url1 = 'http://apis.data.go.kr/B553530/BEEC/BEEC_01_LIST?serviceKey='
    key = '인증키 넣는 칸'
    url2 = '&apiType=XML&q1=2022&q2=2022&q3=1&q4=1&q5=9'
    
    url = url1 + key + url2
    
    result = requests.get(url)
    result = result.content
    ```
    
    (2) BeautifulSoup을 이용한 XML 데이터 처리
    
    ```python
    soup = BeautifulSoup(result, 'lxml-xml')
    ```
    
    - 'lxml-xml' : xml의 Parser
    - 받은 request의 결과값을 가공해줌
        
        ![2024-04-16_00-53-45.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/63b65191-e72b-43e1-bc10-8b63ce0dcb77/2024-04-16_00-53-45.jpg)
        
    
    (3) XML 형식 데이터의 pandas DataFrame 변환
    
    ```python
    import pandas as pd
    
    df = pd.read_xml(result, xpath = './/item')
    ```
    
    - `xpath = './/item'` : item 별로 데이터프레임 만들도록 설정
        
        ![2024-04-16_00-56-04.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/54c99308-cc0d-46f8-b15e-835fcca97ac6/2024-04-16_00-56-04.jpg)
        
    

---

1. JSON 형식 데이터의 수집 & 처리

```python
import requests
from bs4 import BeautifulSoup

url1 = 'http://apis.data.go.kr/B553530/BEEC/BEEC_01_LIST?serviceKey='
key = '인증키 넣는 칸'
url2 = '&apiType=JSON&q1=2022&q2=2022&q3=1&q4=1&q5=9'

url = url1 + key + url2

result = requests.get(url)

# result = result.content 대신 사용
result = result.json()
```

- JSON : BeautifulSoup으로 가공하지 않아도 해석 가능한 자료로 받아올 수 있음
    
    ![2024-04-16_00-59-17.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/410225bb-7028-4735-818b-75e446b530e7/2024-04-16_00-59-17.jpg)
