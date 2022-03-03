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

### 데이터 증가작업

파손된 컨테이너 이미지가 부족하여 Keras의 ImageDataGenerator을 사용하여 데이터 증가작업을 실행하였다.

![Untitled 3](../assets/img/container6.PNG)

그림 4 - 데이터 증가작업 예시

### 데이터 전처리

![Untitled 4](../assets/img/container7.PNG)

그림 5 - 데이터 전처리 과정

# 알고리즘 적용 결과

## 머신러닝



# References

[1] [https://drive.google.com/drive/folders/1TDt6PFW884Yg8vTDhbJJ9s7PfFcuKQKg?usp=sharing](https://drive.google.com/drive/folders/1TDt6PFW884Yg8vTDhbJJ9s7PfFcuKQKg?usp=sharing)

[2] [https://www.tesla.com/ko_KR/autopilot](https://www.tesla.com/ko_KR/autopilot)

[3] [https://www.youtube.com/watch?v=NrmMk1Myrxc](https://www.youtube.com/watch?v=NrmMk1Myrxc)

[4] [https://medium.com/airbnb-engineering/amenity-detection-and-beyond-new-frontiers-of-computer-vision-at-airbnb-144a4441b72e](https://medium.com/airbnb-engineering/amenity-detection-and-beyond-new-frontiers-of-computer-vision-at-airbnb-144a4441b72e)

[5] [https://www.tensorflow.org/about/case-studies?hl=ko](https://www.tensorflow.org/about/case-studies?hl=ko)

[6] [https://github.com/tensorflow/models/tree/master/research/object_detection](https://github.com/tensorflow/models/tree/master/research/object_detection)

[7] [https://arxiv.org/abs/1904.07850](https://arxiv.org/abs/1904.07850)

[8] [https://storage.googleapis.com/openimages/web/factsfigures_v4.html](https://storage.googleapis.com/openimages/web/factsfigures_v4.html)
