---
layout: post
title: "[Library] 지도 관련 라이브러리 Leaflet.js"
subtitle: "leaflet"
categories: study
tags: etc
---

> 지도 라이브러리 Leaflet.js

# 지도 라이브러리


## 1. Leaflet vs Mapbox GL vs OpenLayers


|Leaflet|Mapbox GL|OpenLayers|
|-------|---------|----------|
|- 반응형 지도를 위한 오픈 소스 라이브러리<br/>- Docs에 설명이 잘 되어 있으며, 관련 커뮤니티가 활발해 적용하기 쉬움<br/>|- Mapbox에서 제작<br/>- Leaflet 제작자가 개발. Leaflet과 유사<br/>- Leaflet 보다 발전한 라이브러리로, 데이터 시각화에 있어 더 많은 기능 제공<br/>- v2 부터 유료 라이센스|- GIS 기술에 익숙한 사용자에게 적합<br/>- 지도 표시 + 지리 데이터 시각화 및 분석 기능 제공<br/>- 학습 초기에 적용 어려움|


## Npm 트렌드

![map-lib-trends](/assets/img/map-library-trends.png)

## 2. Leaflet
- 2010년에 만들어진 라이브러리
- 오래된 라이브러리인 만큼 다수의 플러그인 존재
- 3D지도와 같은 고급 기능이 필요하지 않은 간단한 2D 지도 구현에 적합

## 3. 활용사례
### 3-1. 네이버 지도
![naver-map](/assets/img/naver-map.png)

- 2019년 10/23 네이버 대규모 업데이트
- Leaflet.js 라이브러리 적용
- Leaflet 및 Leaflet 플러그인에서 제공하는 기능을 네이버 지도에 적용
    + 거리, 면적, 반경, Zoom 컨트롤러 기능

![naver-map](/assets/img/naver-map-bf-af.png)


### 3-2. 기상청 날씨누리
기상청 날씨누리 : [https://www.weather.go.kr/wgis-nuri/html/map.html](https://www.weather.go.kr/wgis-nuri/html/map.html)
- 2019년 기상청 날씨누리 비주얼맵 기능 신규 화면에 Leaflet.js 적용
- 2021.04 소스 확인 결과 OpenLayers 적용중

![기상청날씨누리](/assets/img/기상청날씨누리.png)

### 3-3. 4월 총선 알리미

4월 총선 알리미 : [https://produce300.com](https://produce300.com)

- CNS 사원 세 명이서 진행한 사이드 프로젝트
- 2020년 4월 21대 총선 정보 지도 기반 종합서비스

![4월총선알리미](/assets/img/Produce300_1.gif)



## [References]
- [Leaflet](https://leafletjs.com/)
- [Map libraries comparison: Leaflet vs Mapbox GL vs OpenLayers - trends and statistics](https://www.geoapify.com/map-libraries-comparison-leaflet-vs-mapbox-gl-vs-openlayers-trends-and-statistics)
- [Mapbox GL new license and 6 free alternatives](https://www.geoapify.com/mapbox-gl-new-license-and-6-free-alternatives)