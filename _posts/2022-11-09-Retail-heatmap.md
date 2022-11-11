---
layout: post
title: Retail heatmap을 통한 소비패턴 분석
subtitle: YOLOv5 를 활용한 히트맵 작성
cover-img: /assets/img/retail/supermercados.jpg
thumbnail-img: /assets/img/retail/supermercados.jpg
share-img: /assets/img/retail/supermercados.jpg
tags: [YOLOv5, heatmap]
---

# Retail-heatmap

**모델 구상** : YOLOv5 을 활용하여 동선 히트맵 그리기  
**활용 방안** : 기존 CCTV 혹은 추가로 설치된 카메라를 활용 소비자들의 
동선을 Heat map으로 그려 소비자 트래픽이 높은 지역과 그렇지 못한 콜드스폿 을 파악하여 판매율 을 높이기 위한 플래노그램을 설정할수 있다.



##  Head & Person Detection Model 

![image](https://drive.google.com/uc?export=view&id=1ZOhDBRXj-Ra0vPL7iG6lrxCWAFhJTAti)
###### 그림 1 - YOLOv5를 활용한 Head & Person Detection Model  

Retail-heatmap을 그리기 위해서 YOLOv5를 이용한 Head & Person Detection Model을 활용합니다.

**Head & Person Detection Model 을 사용하는 이유** : CCTV 위치에서 내려다 볼 경우 가판대에 몸이 가려지고 머리만 보이는 경우가 많기 때문에 Head Detecion으로 인식률을 높이기 위하여  

![image](/assets/img/retail/head_Digital_CCTV_example.gif)
###### 그림 2 - Head & Person Detection Model 구현 영상 

위 화면을 매장 내 CCTV영상 화면 이라고 가정 후 모델을 돌려보았을때 사람의 머리가 BoundingBox로 마킹되어 잘 인식하는 것을 확인할수 있습니다.

|프레임|X좌표|Y좌표|
|----|------|------|
|1|0.807874|0.710417|
|1|0.710417|0.40625|
|2|0.061417|0.275|
|2|0.301575|0.0375|
|...|...|...|
|1283|0.174016|0.174016|
|1283|0.699213|0.699213|
|1283|0.893701|0.4|

모델을 돌리게 되면 BoundingBox의 정중앙을 X,Y 좌표로 하여 영상의 각 프레임별로 좌표를 기록하여 csv파일로 저장하여 줍니다.

## heatmap 작성
![image](/assets/img/retail/HD_CCTV_retail_store.png)  
###### 그림 3 - CCTV화면 캡처

![image](/assets/img/retail/heatmap(4).png)
###### 그림 4 - 동선 히트맵 
csv파일에 저장된 좌표와 프레임을 기준으로 히트맵을 찍어보았습니다. 
결과는 괜찮았지만 모델의 활용 방안에 필요했던 정보를 분석하기에는 직관성이 떨어져 어려워 보인다고 판단하였습니다.

## 평면 투영

![image](/assets/img/retail/heatmap(5).png)
###### 그림 5 - 평면투영

왼쪽 사진과 같이 CCTV시점 화면에 찍힌 히트맵의 직관성을 높이기 위하여 원근보정 후 매장 평면도 위에 평면투영을 시도해 보았습니다.  
모델의 활용 방안에 필요했던 정보를 더 직관적으로 볼수 있게 되어서 히트맵에 대한 정보를 분석하기에 더욱 좋아졌습니다.  

## Results & Discussions
* YOLO를 활용한 Head & Person Detection Model의 성능은 만족스러웠다.
* 평면투영 과정에서 원근보정을 실시간으로 구현해 내는 방법 검토 
* 하나의 카메라로만 매장 히트맵을 그려낼 경우 좌표상 오류가 있을수도 있다고 예상되어 오류상황 검출 해보기
* 오류 검출로 추가 카메라 설치가 필요한 경우 카메라 대수를 최소화 하기 위한 방안 검토 

# Link

[1] https://github.com/ms827/Retail-heatmap.git

[2] https://github.com/deepakcrk/yolov5-crowdhuman
 
[3] https://docs.google.com/presentation/d/1d64s3t7sHgqcHHlNzcVv0wqbnUh-vqrH/edit?usp=sharing&ouid=116268806099058489984&rtpof=true&sd=true             
