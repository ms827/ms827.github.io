---
layout: post
title: Retail heatmap을 통한 소비패턴 분석
subtitle: YOLOv5 를 활용한 히트맵 작성
cover-img: /assets/img/retail/supermercados.jpg
thumbnail-img: /assets/img/retail/supermercados.jpg
share-img: /assets/img/retail/supermercados.jpg
tags: [YOLOv5, Transfer Learning, heatmap]
---

# Retail-heatmap

**모델 구상** : YOLOv5 을 활용하여 동선 히트맵 그리기  
**활용 방안** : 기존 CCTV 혹은 추가로 설치된 카메라를 활용 소비자들의 
동선을 Heat map으로 그려 소비자 트래픽이 높은 지역과 그렇지 못한 콜드스폿 을 파악하여 판매율 을 높이기 위한 플래노그램을 설정할수 있다.



##  Head & Person Detection Model 

![image](https://drive.google.com/uc?export=view&id=1ZOhDBRXj-Ra0vPL7iG6lrxCWAFhJTAti)

그림 1 - YOLOv5를 활용한 Head & Person Detection Model  


**Head & Person Detection Model 을 사용하는 이유** : CCTV 위치에서 내려다 볼 경우 가판대에 몸이 가려지고 머리만 보이는 경우가 많기 때문에 Head Detecion으로 인식률을 높이기 위하여  

![image](/assets/img/retail/head_Digital_CCTV_example.gif)

그림 2 - Head & Person Detection Model 구현 영상 

위 화면을 매장 내 CCTV영상 화면 이라고 가정 후 모델을 돌려보았을때 사람의 머리가 BoundingBox로 마킹되어 잘 인식하는 것을 확인할수 있었다.

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

모델을 돌리게 되면 BoundingBox의 정중앙을 X,Y 좌표로 하여 영상의 각 프레임별로 좌표를 기록하여 csv파일로 저장하여 준다

## heatmap 작성
![image](/assets/img/retail/HD_CCTV_retail_store.png)  

그림 2 - CCTV화면 캡쳐


**YOLOv5를 활용 프레임별로 머리를 기준으로 x,y좌표를 뽑아낸 후 히트맵을 찍어준다**

![image](/assets/img/retail/heatmap(4).png)

## 평면 투영

![image](/assets/img/retail/heatmap(5).png)  


**히트맵 정보의 직관성을 높이기 위하여 원근보정 후 평면투영을 시도해 보았다**
# Link

[1] [프로젝트 코드모음](https://drive.google.com/drive/folders/1TDt6PFW884Yg8vTDhbJJ9s7PfFcuKQKg?usp=sharing)

[2] [VGG-16 논문](https://arxiv.org/abs/1409.1556)

[3] [AlexNet 논문](https://proceedings.neurips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)
