---
layout: post
title: "[Library] 지도 관련 라이브러리 Leaflet.js"
subtitle: "leaflet"
categories: study
tags: etc
---

> 지도 라이브러리 Leaflet.js

# 주제 : 지도 라이브러리 Leaflet.js

- 지도와 관련된 서비스는 꾸준히 증가하고 있음.
    (ex. 배달의 민족, 쿠팡이츠 실시간 배달현황 / 직방 매물조회 등)

- 지도 데이터는 실생활과 밀접한 관련이 있기 때문에 관련 시장은 앞으로도 확장될 가능성이 큼.

![copang](/assets/img/build/1_배민_쿠팡이츠.png)

## 지도 라이브러리 비교 Leaflet vs Mapbox GL vs OpenLayers

| Leaflet                                                                                                                 | Mapbox GL                                                                                                                                                                | OpenLayers                                                                                                               |
| ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| - 반응형 지도를 위한 오픈 소스 라이브러리<br/>- Docs에 설명이 잘 되어 있으며, 관련 커뮤니티가 활발해 적용하기 쉬움<br/> | - Mapbox에서 제작<br/>- Leaflet 제작자가 개발. Leaflet과 유사<br/>- Leaflet 보다 발전한 라이브러리로, 데이터 시각화에 있어 더 많은 기능 제공<br/>- v2 부터 유료 라이센스 | - GIS 기술에 익숙한 사용자에게 적합<br/>- 지도 표시 + 지리 데이터 시각화 및 분석 기능 제공<br/>- 학습 초기에 적용 어려움 |

## Leaflet

- https://leafletjs.com
- BDS-2 라이센스
- 2010년에 만들어진 라이브러리
- 사용하기 쉽고 가벼운 라이브러리
- 오래된 라이브러리인 만큼 다수의 플러그인과 많은 예시, 커뮤니티 존재
- 3D지도와 같은 고급 기능이 필요하지 않은 간단한 2D 지도 구현에 적합
- Leaflet.js QuickStart sample code

<html>
<head>
 <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css"
   integrity="sha512-xodZBNTC5n17Xt2atTPuE1HxjVMSvLVW9ocqUKLsCC5CXdbqCmblAshOMAS6/keqq/sMZMZ19scR4PsZChSR7A=="
   crossorigin=""/>

 <!-- Make sure you put this AFTER Leaflet's CSS -->
 <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"
   integrity="sha512-XQoYMqMTK8LvdxXYG3nZ448hOEQiglfqkJs1NOQV44cWnUrBc8PkAOcXy20w0vlaXaVUearIOBhiXZ5V3ynxwA=="
   crossorigin=""></script>
<style>
 #mapid { height: 180px; }
</style>
</head>
<body>
 <div id="mapid"></div>
<script>
var mymap = L.map('mapid').setView([51.505, -0.09], 13);
L.tileLayer('https://mt0.google.com/vt/lyrs=m&hl=kr&x={x}&y={y}&z={z}', {
    attribution: 'Map data © <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
    maxZoom: 18,
    id: 'mapbox/streets-v11',
    tileSize: 512,
    zoomOffset: -1
}).addTo(mymap);
</script>
</body>
</html>

```html
<head>
  <link
    rel="stylesheet"
    href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css"
    integrity="sha512-xodZBNTC5n17Xt2atTPuE1HxjVMSvLVW9ocqUKLsCC5CXdbqCmblAshOMAS6/keqq/sMZMZ19scR4PsZChSR7A=="
    crossorigin=""
  />

  <!-- Make sure you put this AFTER Leaflet's CSS -->
  <script
    src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"
    integrity="sha512-XQoYMqMTK8LvdxXYG3nZ448hOEQiglfqkJs1NOQV44cWnUrBc8PkAOcXy20w0vlaXaVUearIOBhiXZ5V3ynxwA=="
    crossorigin=""
  ></script>
  <style>
    #mapid {
      height: 180px;
    }
  </style>
</head>
<body>
  <div id="mapid"></div>
  <script>
    var mymap = L.map("mapid").setView([51.505, -0.09], 13);
    L.tileLayer(
      "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}",
      {
        attribution:
          'Map data © <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
        maxZoom: 18,
        id: "mapbox/streets-v11",
        tileSize: 512,
        zoomOffset: -1,
        accessToken: "your.mapbox.access.token",
      }
    ).addTo(mymap);
  </script>
</body>
```

## Mapbox GL

![mpabox](/assets/img/build/2_mapboxgl.png)

- OpenLayers와 Leaflet에서 제공하지 못하는 3D 지도 렌더링 기능을 제공할 수 있는 - 지도 라이브러리
- Mapbox GL v2 부터 유료 전환
- mapbox gl QuickStart sample code
<html>
<head>
  <script src="https://api.mapbox.com/mapbox-gl-js/v2.2.0/mapbox-gl.js"></script>
  <link
    href="https://api.mapbox.com/mapbox-gl-js/v2.2.0/mapbox-gl.css"
    rel="stylesheet"
  />
</head>

<body>
  <div id="map" style="width: 400px; height: 300px;"></div>
  <script>
    // TO MAKE THE MAP APPEAR YOU MUST
    // ADD YOUR ACCESS TOKEN FROM
    // https://account.mapbox.com
    mapboxgl.accessToken = "<your access token here>";
    var map = new mapboxgl.Map({
      container: "map", // container ID
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: [-74.5, 40], // starting position [lng, lat]
      zoom: 9, // starting zoom
    });
  </script>
</body>
</html>

```html
<head>
  <script src="https://api.mapbox.com/mapbox-gl-js/v2.2.0/mapbox-gl.js"></script>
  <link
    href="https://api.mapbox.com/mapbox-gl-js/v2.2.0/mapbox-gl.css"
    rel="stylesheet"
  />
</head>

<body>
  <div id="map" style="width: 400px; height: 300px;"></div>
  <script>
    // TO MAKE THE MAP APPEAR YOU MUST
    // ADD YOUR ACCESS TOKEN FROM
    // https://account.mapbox.com
    mapboxgl.accessToken = "<your access token here>";
    var map = new mapboxgl.Map({
      container: "map", // container ID
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: [-74.5, 40], // starting position [lng, lat]
      zoom: 9, // starting zoom
    });
  </script>
</body>
```

## OpenLayers

- https://openlayers.org/
- BSD-2 라이센스 : 자유롭게 사용 및 배포 가능
- Leaflet 기본 기능 + 추가 기능
- Leaflet 과 달리 Project (투영법)을 사용하기 때문에 GIS 관점에서 더 정확한 데이터 사용 가능
- 주로 복잡한 GIS 응용 프로그램 개발에 사용됨
- Leaflet 코어에서 GeoJSON 형식만 지원하는 것과 달리 다양한 GIS 형식 지원
- OpenLayers QuickStart sample code

<html lang="en">
  <head>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.5.0/css/ol.css"
      type="text/css"
    />
    <style>
      .map {
        height: 400px;
        width: 100%;
      }
    </style>
    <script src="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.5.0/build/ol.js"></script>
    <title>OpenLayers example</title>
  </head>
  <body>
    <h2>My Map</h2>
    <div id="map" class="map"></div>
    <script type="text/javascript">
      var map = new ol.Map({
        target: "map",
        layers: [
          new ol.layer.Tile({
            source: new ol.source.OSM(),
          }),
        ],
        view: new ol.View({
          center: ol.proj.fromLonLat([37.41, 8.82]),
          zoom: 4,
        }),
      });
    </script>
  </body>
</html>

```html
<head>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.5.0/css/ol.css"
    type="text/css"
  />
  <style>
    .map {
      height: 400px;
      width: 100%;
    }
  </style>
  <script src="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.5.0/build/ol.js"></script>
  <title>OpenLayers example</title>
</head>
<body>
  <h2>My Map</h2>
  <div id="map" class="map"></div>
  <script type="text/javascript">
    var map = new ol.Map({
      target: "map",
      layers: [
        new ol.layer.Tile({
          source: new ol.source.OSM(),
        }),
      ],
      view: new ol.View({
        center: ol.proj.fromLonLat([37.41, 8.82]),
        zoom: 4,
      }),
    });
  </script>
</body>
```

- 좌표계 3 종류
  1. GIS : 제일 정밀함
     - X시작점, Y시작점, X끝점, Y끝점, 줌레벨
     - 메카토르 도법, 원통형 도법(원기둥)
  2. 위경도
     - 위도, 경도
     - LCC, 원추도법(원뿔)
  3. 픽셀
     - 픽셀 좌표

## Npm 트렌드

![npm](/assets/img/build/3_npm_trends.png)
![npm2](/assets/img/build/4_npm_trend.png)

- 다운로드 수는 Leaflet 과 MapBox GL이 많음
- Leaflet 소스의 Star 수가 더 많음
- 지도 관련 라이브러리의 수요는 꾸준히 상승하는 추세

## Leaflet.js 활용사례

### 네이버 지도

![naver](/assets/img/build/5_naver_map.png)

- 2019년 10/23 네이버 지도 대규모 업데이트
- Leaflet.js 라이브러리 적용
- Leaflet 및 Leaflet 플러그인에서 제공하는 기능을 네이버 지도에 적용
  - 거리, 면적, 반경, Zoom 컨트롤러 기능

![naver](/assets/img/build/6_naver-map-bf-af.png)

- 좌측 : 변경 전 네이버 지도 / 우측 : 변경 후 네이버 지도

### 기상청 날씨누리

- 기상청 날씨누리 : https://www.weather.go.kr/wgis-nuri/html/map.html

![weater](/assets/img/build/7_기상청날씨누리.png)

- 2020년 1월 기상청 날씨누리 비주얼맵 기능 신규 화면에 Leaflet.js 적용
- 소스 확인 결과 2021.04 기준 OpenLayers 적용중

### 4월 총선 알리미

- 4월 총선 알리미 : https://produce300.com
- CNS 사원 세 명이서 진행한 사이드 프로젝트
- 2020년 4월 21대 총선 정보 지도 기반 종합서비스
  ![vote](/assets/img/build/8_Produce300_1.gif)

https://www.youtube.com/watch?v=nZaZ2dB6pow

[References]

- [Leaflet](https://leafletjs.com/)
- [Map libraries comparison: Leaflet vs Mapbox GL vs OpenLayers - trends and statistics](https://www.geoapify.com/map-libraries-comparison-leaflet-vs-mapbox-gl-vs-openlayers-trends-and-statistics)
- [Mapbox GL new license and 6 free alternatives](https://www.geoapify.com/mapbox-gl-new-license-and-6-free-alternatives)
