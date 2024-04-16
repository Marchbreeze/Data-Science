## 1. 지리적 시각화

### (1) 지리 정보 파일

- Shapefile (필수)
    - shp : 기하학적 공간 자료
    - dbf : 속성 정보가 담긴 DB
    - shx : 공간 자료와 속성을 잇는 색인 (인덱스)

- Others (필수X)
    - prj : 투사 및 좌표계 정보
    - xml : 공간 메타데이터 포맷
    

![2024-04-17_03-42-18.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/473bcd36-b8a9-41e5-a3a3-d9550e7b1195/2024-04-17_03-42-18.jpg)

### (2) Shapefile 활용 DataFrame

- GeoDataFrame 생성
    
    ```python
    import pandas as pd
    import geopandas as gpd
    import matplotlib.pyplot as plt
    
    gdf = gpd.read_file('FILENAME.shp', encoding = 'cp949')
    gdf
    ```
    
    - pandas와 유사한 `geopandas`로 지리 정보 활용
        - 읽기, 수정, 연산 등 DataFrame에서 가능한 대부분의 동작을 공유
    - shp 파일을 read_file 실행하는 경우 :
        - dbf, shx 등 지리 정보와 관련된 모든 파일들도 **함께 가져옴**
            
            ![2024-04-17_03-45-36.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/02db0ed3-e175-4fce-b4bd-d20c46b2bbec/2024-04-17_03-45-36.jpg)
            
            - geometry : 다각형 형태의 Polygon 객체로 받아옴
    
    - 한글이 포함된 shapefile을 불러오는 과정에서 문제가 발생하는 경우
        
        → encoding 방식을 cp949, euc-kr, utf-8 등으로 변경 후 다시 시도
        
- Polygon 객체를 통한 지리 정보의 활용
    
    ```python
    gdf['geometry'].area.sort_values(ascending=True)
    ```
    
    ![2024-04-17_03-53-04.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/4b36834d-23a2-46ea-a321-ff69ee897c67/2024-04-17_03-53-04.jpg)
    
    1. `area` : 다각형 면적 반환
    2. `length` : 다각형 둘레 반환
    3. `bounds` : 다각형 min x, min y, max x, max y 반환
    4. `boundary` : 다각형 외부 경계를 LINESTRING 개체로 반환
    5. `centroid` : 다각형 무게중심 반환
    6. `is_valid` : 다각형 유효성 반환
    
- 지리 정보의 시각화
    
    ```python
    gdf.plot(figsize = (10, 20), 
    				 color = 'lightgray',
             edgecolor = 'black', 
    	       linewidth = 2)
    plt.show()
    ```
    
    ![2024-04-17_03-56-05.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/0116eb24-3cbd-4348-9281-8f4933fcbb70/2024-04-17_03-56-05.jpg)
    
- 데이터 수정
    
    ```python
    gdf.iloc[9, 1] = '도봉구' 
    gdf.iloc[10, 1] = '노원구'
    ```
    
- cmap을 이용한 지도 채색
    1. 다른 데이터와의 merge 진행
    
    ```python
    df = pd.read_csv('dust.csv', encoding = 'cp949')
    gdf = gdf.merge(df, how = 'left', on = 'SGG_NM')
    ```
    
    1. 지도 채색
    
    ```python
    gdf.plot(figsize = (10, 20),
    				 column = '미세먼지 농도(단위:㎍/㎥)_y', 
    				 cmap = 'OrRd')
    ```
    
    ![2024-04-17_04-00-50.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/e9bd77d6-66c1-4299-9f2a-bb727c265464/2024-04-17_04-00-50.jpg)
    
- 다른 지리 정보 데이터를 추가적으로 시각화 (두개의 plot)
    - 첫번째 plot으로 축을 정의하고, 불러와서 두번째 plot을 그려 사용
    - **두 데이터의 geography 좌표계가 동일해서 가능**
    
    ```python
    gdf_child = gpd.read_file('TG_ACDT19.shp', encoding = 'utf-8')
    
    ax1 = gdf.plot(figsize = (10, 20),
                   color = 'lightgray',
                   edgecolor = 'black', 
                   linewidth = 2)
    
    gdf_child.plot(ax = ax1)
    plt.show()
    ```
    
    ![2024-04-17_04-03-59.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/522e6f16-5e3f-406c-a338-fa444733552e/2024-04-17_04-03-59.jpg)
    

---

## 2. 좌표계 변환과 folium의 활용

### (1) 주요 좌표계

1. 지리 좌표계 (위경도 좌표계)
    - 위도와 경도를 활용해 원하는 좌표 표현
    - Degree 표현법 : 127.5
    - DMS 표현법 : 127도 30분 00초 (시간처럼 60 기준 변환)
    
2. 평면직각 좌표계
    - 위경도 좌표계를 3차원이 아닌 평면 위에 투영해 좌표 표현
    - TM 표현법 : 중부, 동부, 서부, 동해 4가지 투영 원점이 가능
    - UTM-K 표현법 : 경도 127.5 ̊ 위도 38 ̊ 위치를 원점으로 삼음 (한국 기준)
    

### (2) 좌표계 구분 및 변환

- EPSG 코드 = 좌표계마다 부여된 숫자 코드
    
    ![2024-04-17_04-55-58.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/8a8c2cf7-bbef-4330-9a09-1e83f936f50a/2024-04-17_04-55-58.jpg)
    
    - 위경도 : 4로 시작 / 평면직각 : 5로 시작
    - `gdf.crs` 로 EPSG 정보 확인 가능
        
        ```python
        print(gdf.crs)
        ```
        
        ![2024-04-17_04-53-35.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/9f9ef3fb-86c4-4ef5-83b5-4e88b9a2c149/2024-04-17_04-53-35.jpg)
        
        - 한국의 Central Belt (중부) = 5181

- 경위도 좌표계로 변환하는 방법
    
    ```python
    new_gdf = gdf.to_crs(epsg = 4326)
    new_gdf
    ```
    
    ![2024-04-17_04-57-17.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/ad44abcf-e75e-4c4e-bac4-6bf8aeaf1419/2024-04-17_04-57-17.jpg)
    
    - geometry의 값이 위경도로 바뀜

### (3) folium 시각화

- folium은 위경도(epsg 4326)를 기준으로 함

- 구글 지도를 가져와서 시각화 진행하기
    - `location` : 지도의 중심부 기준 설정
    - `tiles` : 지도의 스타일 설정
    - `zoom_start` : 지도의 스케일 설정
    
    ```python
    import folium
    from folium import Marker
    
    # 배경 지도 설정
    m = folium.Map(location = [37.57, 126.98],
                   tiles = 'openstreetmap', 
                   zoom_start = 15)
    
    # 마커 설정
    df_subway = pd.read_csv('subway.csv', encoding = 'cp949')
    for i in range(0, len(df_subway)) :
      lati = df_subway.iloc[i, 4]
      longi = df_subway.iloc[i, 3]
      station_name = df_subway.iloc[i, 2]
      new_marker = Marker(location = [lati, longi],
    									    popup = station_name)
      new_marker.add_to(m)
    
    m
    ```
    
    ![2024-04-17_05-07-58.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/edfd69d1-6c01-4d0c-9269-1bae8a4e3915/2115e0dd-7449-4987-a3ec-7b94de94806b/2024-04-17_05-07-58.jpg)
