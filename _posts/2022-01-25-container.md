---
layout: post
title: Container 파손 분류 프로젝트
subtitle: CNN을 활용한 이미지 분류 프로젝트
cover-img: /assets/img/container2.png
thumbnail-img: /assets/img/container2.png
share-img: /assets/img/container2.png
tags: [object detection, custom dataset, centernet]
---

# Container 파손 분류 프로젝트

## 프로젝트 소개

목적은 파손 컨테이너 데이터 세트를 구축하고, 컨테이너 파손을 구분하는 이미지 분석 AI 모델을 개발하는 것입니다.

이때 기대효과는 컨테이너 파손 여부 자동 판별을 통한 효율적인 컨테이너 보수·수리 프로세스 수립 기여, 공급사슬 내 지점별 컨테이너 파손 여부 파악을 통한 물류 가시성 확보입니다.


![Untitled](../assets/img/container3.PNG)

그림 1 - 항만의 구조 - [컨테이너 조작장(CFS), 컨테이너 야드(CY)] [1]

컨테이너는 다양한 원인으로 파손되고 있습니다.

![Untitled 1](../assets/img/container4.PNG)

그림 2 - 컨테이너 파손원인 [2]


## 데이터 수집·증가·전처리

### 데이터 수집
**수집 데이터 : 정상 및 파손 컨테이너 이미지**
(주요 검색어: container, damaged container, cargo, container accident 등)

**방법 :** 크롤링 코드 작성 후, 각 검색 플랫폼의 html 태그를 확인·수정하여 스크래핑을 수행
보안 문제로 스크래핑이 불가능한 경우, 직접 다운로드하여 이미지 데이터 수집

중복된 사진 제외 576장 수집(정상 299장, 파손 277장)


### 배경제거작업(Cropping)

인식하고자 하는 객체를 배경으로부터 분리하고, 컨테이너 객체를 명확히 하여 알고리즘에게
학습시키기 위하여 객체 주변의 배경을 제거하는 작업

![Untitled 2](../assets/img/container5.PNG)

그림 3 - 포토샵을 활용한 배경제거 예시 [3]

### 현장촬영

부족한 사진 데이터의 추가적인 확보를 위한 CFS 내 장치된 컨테이너 사진 촬영을 진행 하였습니다.

**방문장소 : DW국제물류센터**(운영팀 이정한 과장), **부산크로스독**(창고관리팀 신태용 부장)

정상 컨테이너 사진 약 580여 장 촬영후 데이터 추가완료

![Untitled 5](../assets/img/Untitled%205.png)

그림 6 - Google Open Images Dataset V4에 대한 정보

airbnb 데이터 사이언스 팀과 동일하게 Google Open Images Dataset V4에서 30개의 Amenity label만을 추출하였습니다. 검출 타겟이 되는 30개의 amenity class는 아래와 같습니다.

> ['Toilet', 'Swimming pool', 'Bed', 'Billiard table', 'Sink',
'Fountain', 'Oven', 'Ceiling fan', 'Television', 'Microwave oven',
'Gas stove', 'Refrigerator', 'Kitchen & dining room table', 'Washing machine', 'Bathtub',
'Stairs', 'Fireplace', 'Pillow', 'Mirror', 'Shower',
'Couch', 'Countertop', 'Coffeemaker', 'Dishwasher', 'Sofa bed',
'Tree house', 'Towel', 'Porch', 'Wine rack', 'Jacuzzi']

전체 1,743,042장의 이미지에서 위 30개의 amenity class를 포함한 이미지를 선별한 결과 **총 34,835장의 이미지**를 학습을 위한 이미지로 선별하였습니다. 이중에서 **90%를 트레이닝 데이터로, 10%를 Evaluation을 위한 테스트 데이터**로 활용하였습니다.

# Training

CenterNet 모델을 이용해서 30개의 Amenity class를 포함하고 있는 34,835장의 이미지의 90%인 31,351장의 이미지를 이용해서 140,000 step의 트레이닝을 진행하였습니다.

![Untitled 6](../assets/img/Untitled%206.png)

그림 7 - 140,000 step 동안의 트레이닝 과정에 대한 TensorBoard 스크린샷

# Evaluation

140,000 step의 트레이닝 이후 34,835장의 이미지의 10%인 3,483장의 테스트 이미지에 대해 다음과 같은 Evaluation 결과를 얻을 수 있었습니다. 

![Untitled 7](../assets/img/Untitled%207.png)

그림 8 - Evaluation 과정에 대한 TensorBoard 스크린샷

전체 테스트 이미지에 대해서는 IoU 0.5이상을 정답으로 간주했을때 약 **14.32의 mAP 값**을 얻을 수 있었습니다.
u
개별 class 별로 IoU 0.5 이상을 정답으로 간주했을 때 Swimming pool 레이블 같은 경우 61.28의 mAP 값, Bathtub 레이블 같은 경우 5.33의 mAP 값, Oven 레이블 같은 경우는 19.38의 mAP 값을 얻을 수 있었습니다.

# Future Work

위의 예시에서 볼 수 있듯이 개별 레이블 별로 mAP 값의 편차가 심한 모습을 볼 수 있습니다. 이는 여러 원인이 있을 수 있지만 각 레이블에 대응되는 Training 이미지 개수의 편차가 심한 것이 주요 원인 중 하나로 볼 수 있습니다. 따라서 airbnb 데이터 사이언스팀에서 진행한 것처럼 추가 이미지를 수집한 뒤 이에 대한 정답을 labelling 한뒤 추가 데이터를 이용해서 Training하는 과정을 통해 mAP를 향상시키는 작업을 추후 작업으로 진행해볼 계획입니다.

# References

[1] [https://drive.google.com/drive/folders/1TDt6PFW884Yg8vTDhbJJ9s7PfFcuKQKg?usp=sharing](https://drive.google.com/drive/folders/1TDt6PFW884Yg8vTDhbJJ9s7PfFcuKQKg?usp=sharing)

[2] [https://www.tesla.com/ko_KR/autopilot](https://www.tesla.com/ko_KR/autopilot)

[3] [https://www.youtube.com/watch?v=NrmMk1Myrxc](https://www.youtube.com/watch?v=NrmMk1Myrxc)

[4] [https://medium.com/airbnb-engineering/amenity-detection-and-beyond-new-frontiers-of-computer-vision-at-airbnb-144a4441b72e](https://medium.com/airbnb-engineering/amenity-detection-and-beyond-new-frontiers-of-computer-vision-at-airbnb-144a4441b72e)

[5] [https://www.tensorflow.org/about/case-studies?hl=ko](https://www.tensorflow.org/about/case-studies?hl=ko)

[6] [https://github.com/tensorflow/models/tree/master/research/object_detection](https://github.com/tensorflow/models/tree/master/research/object_detection)

[7] [https://arxiv.org/abs/1904.07850](https://arxiv.org/abs/1904.07850)

[8] [https://storage.googleapis.com/openimages/web/factsfigures_v4.html](https://storage.googleapis.com/openimages/web/factsfigures_v4.html)
