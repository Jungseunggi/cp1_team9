# Recommend_clothes_size_and_category

## 1.프로젝트 배경 및 목적

### 1.1 리뷰를 통한 사이즈 추천 
<img src="https://user-images.githubusercontent.com/102225200/198227860-cb3a9f23-279c-4b59-bd7a-6a47a7eb813d.png" width="400">

<sub> 이미지 출처 : 통계청</sub>

- 온라인 시장이 확장되는 추세로 온라인 구매 시 직접 입어보지 못함으로 사이즈에 대한 고민 해소

### 1.2 이미지 통한 카테고리 상품 추천 

<img src="https://user-images.githubusercontent.com/102225200/198229281-190de2de-93b8-4bea-8ba3-f3f19a5559fe.png" width="400">

- 레퍼런스 이미지와 비슷한 상품을 사고 싶은 소비자들에게 상품 추천

## 2. 프로젝트를 통해 얻는 이익 

- 다른 온라인 쇼핑몰과의 차별점과 자신들의 상품을 판매 가능

## 3. 프로젝트 과정

<img src="https://user-images.githubusercontent.com/102225200/198229760-740a1059-7e31-42e3-9371-1c17658d0180.png" width="400">

<img src="https://user-images.githubusercontent.com/102225200/198230092-470bedad-60d4-49e9-8d6e-7359f86daca1.png" width="400">

- 본인은 데이터 수집 및 모델링 구현

### 3.1 데이터 수집

- Web scraper(chrome 확장프로그램)를 통한 리뷰 데이터 수집

- Selenium을 통해 이미지 데이터 수집

### 3.2 모델링 구현(사이즈 추천)

- 리뷰데이터의 성별, 카테고리, 키, 몸무게 등 데이터 정제

- 다중분류로서 f1-score를 평가지표로 활용

- 사이즈 영어 표기로 통일(ex : S,M,L ...)

<img src="https://user-images.githubusercontent.com/102225200/198232625-3fe0755a-5521-4cda-a94f-fef53fbc0d26.png" width="800">

- Random Forest Classifier, Knn Classifier 비교하여 **성능이 더 좋은 Random Forest Classifier 채택**

### 3.3 모델링 구현(object detection)

<img src="https://user-images.githubusercontent.com/102225200/198233913-87ec22eb-37b3-4a33-9779-7816a60faa17.gif" width="400">

- 데이터를 수집해도 한계가 있고 라벨링 시간이 너무 오래걸림 -> 이미지 증강을 통해 수집과 라벨링 시간을 절약

- 이미 라벨링을 진행해서 회전을 사용하면 라벨링파일을 사용할 수 없음으로 노이즈 효과를 사용(당시에는 라벨링도 같이 바꿔주는 방법을 몰랐음)

<img src="https://user-images.githubusercontent.com/102225200/198235080-88eefa95-a83d-4725-8406-9e012d245ced.png" width="600">

<sub> 이미지 출처 : SSD(Single Shot MultiBox Detector)</sub>

- SSD vs Yolo : 정확도는 SSD가 비교적 우수하나 속도에선 Yolo가 우수, 따라서 사용자의 편의를 생각하여 속도가 우수한 Yolo 선택

<img src="https://user-images.githubusercontent.com/102225200/198236278-66326732-6cbc-4370-8c0d-2d605f317fec.png" width="600">

<sub> 이미지 출처 : https://blog.roboflow.com/yolov4-versus-yolov5/</sub>

- Yolov4 vs Yolov5s : mAP(mean average precision)은 비슷하나 용량은 더 작은 YOLOv5s 선택


## 4. 프로젝트 결과

<img src="https://user-images.githubusercontent.com/102225200/198238085-4dabf3b2-4c98-4da5-b44c-f8cfe336ea15.png" width="400">

<sub>이미지 출처 : https://towardsdatascience.com/a-single-number-metric-for-evaluating-object-detection-models-c97f4a98616d</sub>

<img src="https://user-images.githubusercontent.com/102225200/198237492-be43983a-03fa-43f9-9b54-e769aa51313d.png" width="900">

<sub>초기 object detection 모델</sub>

- 초기 confusion matrix는 제대로 분류 못하는 카테고리들이 많이 보이며 f1_Confidence Curve가 위의 이미지에서 워스트 모델에 가까움 

  - 데이터가 부족한것으로 판단 
  
  - 이미지 증강 및 부족한 데이터 추가 수집
  
<img src="https://user-images.githubusercontent.com/102225200/198239128-0956b0e7-2e24-4397-80f0-d10ede820559.png" width="900">

<sub>최종 object detection 모델</sub>

- confusion matrix를 통해 부족한 데이터 추가 수집 및 이미지 증강을 통해 성능 향상
  
  - 여전히 제대로 분류 못하는 카테고리가 있음(knit, jogger)
  
  - knit의 경우 카테고리 구분에 어려움을 겪음(ex : pk티셔츠ㅡknit, 맨투맨-knit 등)
  
  - jogger의 경우 다른 팀원이 수집을 해줬는데 하반신만 나온 데이터를 수집(다른 데이터들은 전신사진으로 수집)하여 발생된 것으로 보임
  
 
<img src="https://user-images.githubusercontent.com/102225200/198245664-4d6d405b-6db0-4e11-aad4-ca64ed01d6e8.png" width="400">

<sub> 왼쪽 : 초기, 오른쪽 : 최종</sub>

- blouse의 경우 디자인이 매우 다양 -> 다양한 디자인의 데이터를 추가 수집

<img src="https://user-images.githubusercontent.com/102225200/198246196-19a9fc6d-21a4-4558-b146-37d11d8bf8de.png" width="400">

<sub> 왼쪽 : 초기, 오른쪽 : 최종</sub>

- 포즈에 따라 분류가 제대로 안되는 경우도 발생 -> 다양한 포즈의 데이터를 추가 

## 5. 회고 및 느낀점

- 부족한 데이터와 시간절약을 위해 데이터 증강을 사용한 점은 좋았으나 데이터 증강을 쉽게 하는 방법이 있음(roboflow)

- 카테고리 분류 시 기준을 잡고 시작했다면 성능이 더 좋았을 것이라는 아쉬움

- confusion matrix를 보고 부족한 데이터를 찾는 방법은 좋은 해결책이라고 생각함

- 다시 한다면 유튜브를 통해 데이터를 추가 수집
  
  

