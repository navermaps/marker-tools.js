# 마커 클러스터링

NAVER Maps JavaScript API v3를 이용해 마커 클러스터링를 구현한 예제입니다. 마커 클러스터링은 여러 개의 마커를 하나의 클러스터로 표현하는 것입니다.

> NAVER Maps JavaScript API v3를 반드시 함께 사용해야 합니다.

![marker-clustering](https://cloud.githubusercontent.com/assets/1366046/20373568/03027a26-acb6-11e6-8380-08f80981ce64.png)
[Demo Link](https://navermaps.github.io/marker-tools.js/marker-clustering/marker-clustering.html)
<br>

> 예제를 정상적으로 실행하려면 NAVER Maps JavaScript API v3를 이용할 수 있는 `clientId`가 있어야 합니다.



## 사용 예제

```html
...
<head>
	...
	<script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?clientId=MK_1sBFRuO6XEvDi29iW"></script>
	<script type="text/javascript" src="../src/MarkerClustering.js"></script>
	...
</head>
<body>
	<div id="map" style="width:100%;height:100%;padding:0;margin:0;"></div>
	<script>
		var map = new naver.maps.Map("map", {
	        zoom: 3,
	        center: new naver.maps.LatLng(36.2253017, 127.6460516)
	    });

	    var markers = data;	// Array

	    var htmlMarker1 = {
	            content: '<div style="cursor:pointer;width:40px;height:40px;line-height:42px;font-size:10px;color:white;text-align:center;font-weight:bold;background:url(/example/images/cluster-marker-1.png);background-size:contain;"></div>',
	            size: N.Size(40, 40),
	            anchor: N.Point(20, 20)
	        },
	        htmlMarker2 = {
	            content: '<div style="cursor:pointer;width:40px;height:40px;line-height:42px;font-size:10px;color:white;text-align:center;font-weight:bold;background:url(/example/images/cluster-marker-2.png);background-size:contain;"></div>',
	            size: N.Size(40, 40),
	            anchor: N.Point(20, 20)
	        },
	        htmlMarker3 = {
	            content: '<div style="cursor:pointer;width:40px;height:40px;line-height:42px;font-size:10px;color:white;text-align:center;font-weight:bold;background:url(/example/images/cluster-marker-3.png);background-size:contain;"></div>',
	            size: N.Size(40, 40),
	            anchor: N.Point(20, 20)
	        },
	        htmlMarker4 = {
	            content: '<div style="cursor:pointer;width:40px;height:40px;line-height:42px;font-size:10px;color:white;text-align:center;font-weight:bold;background:url(/example/images/cluster-marker-4.png);background-size:contain;"></div>',
	            size: N.Size(40, 40),
	            anchor: N.Point(20, 20)
	        },
	        htmlMarker5 = {
	            content: '<div style="cursor:pointer;width:40px;height:40px;line-height:42px;font-size:10px;color:white;text-align:center;font-weight:bold;background:url(/example/images/cluster-marker-5.png);background-size:contain;"></div>',
	            size: N.Size(40, 40),
	            anchor: N.Point(20, 20)
	        };

	    var markerClustering = new MarkerClustering({
	        minClusterSize: 2,
	        maxZoom: 8,
	        map: map,
	        markers: markers,
	        disableClickZoom: false,
	        gridSize: 120,
	        icons: [htmlMarker1, htmlMarker2, htmlMarker3, htmlMarker4, htmlMarker5],
	        indexGenerator: [10, 100, 200, 500, 1000],
	        stylingFunction: function(clusterMarker, count) {
	            $(clusterMarker.getElement()).find('div:first-child').text(count);
	        }
	    });
	</script>
</body>
```

## 라이선스

```
Copyright 2016 NAVER Corp.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
    http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
