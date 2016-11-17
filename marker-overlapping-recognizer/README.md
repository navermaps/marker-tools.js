# 겹침 마커

NAVER Maps JavaScript API v3를 이용해 겹침 마커 처리를 구현한 예제입니다.

> NAVER Maps JavaScript API v3를 반드시 함께 사용해야 합니다.

![marker-overlapping-recognizer](https://cloud.githubusercontent.com/assets/1366046/20373572/06c0f746-acb6-11e6-86b4-ffc2587bdc0a.png)

<br>

> 예제를 정상적으로 실행하려면 NAVER Maps JavaScript API v3를 이용할 수 있는 `clientId`가 있어야 합니다.



## 사용 예제

```html
...
<head>
	...
    <script src="https://code.jquery.com/jquery-1.12.4.min.js" integrity="sha256-ZosEbRLbNQzLpnKIkEdrPv7lOy9C27hHQ+Xp8a4MxAQ=" crossorigin="anonymous"></script>
	<script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?clientId=MK_1sBFRuO6XEvDi29iW"></script>
	<script type="text/javascript" src="../src/MarkerOverlappingRecognizer.js"></script>
	...
</head>
<script>
    var map = new naver.maps.Map('map', {
        center: new naver.maps.LatLng(37.3595704, 127.105399),
        zoom: 10
    });

    var recognizer = new MarkerOverlappingRecognizer({
        highlightRect: false,
        tolerance: 5
    });
    
    recognizer.setMap(map);

    var bounds = map.getBounds(),
        southWest = bounds.getSW(),
        northEast = bounds.getNE(),
        lngSpan = northEast.lng() - southWest.lng(),
        latSpan = northEast.lat() - southWest.lat();

    function highlightMarker(marker) {
        var icon = marker.getIcon();

        if (icon.url !== MARKER_HIGHLIGHT_ICON_URL) {
            icon.url = MARKER_HIGHLIGHT_ICON_URL;
            marker.setIcon(icon);
        }

        marker.setZIndex(1000);
    }

    function unhighlightMarker(marker) {
        var icon = marker.getIcon();

        if (icon.url === MARKER_HIGHLIGHT_ICON_URL) {
            icon.url = MARKER_ICON_URL;
            marker.setIcon(icon);
        }

        marker.setZIndex(100);
    }

    for (var key in MARKER_SPRITE_POSITION) {
        var position = new naver.maps.LatLng(
            southWest.lat() + latSpan * Math.random(),
            southWest.lng() + lngSpan * Math.random());

        var marker = new naver.maps.Marker({
            map: map,
            position: position,
            title: key,
            icon: {
                url: MARKER_ICON_URL,
                size: new naver.maps.Size(24, 37),
                anchor: new naver.maps.Point(12, 37),
                origin: new naver.maps.Point(MARKER_SPRITE_POSITION[key][0], MARKER_SPRITE_POSITION[key][1])
            },
            shape: {
                coords: [11, 0, 9, 0, 6, 1, 4, 2, 2, 4,
                    0, 8, 0, 12, 1, 14, 2, 16, 5, 19,
                    5, 20, 6, 23, 8, 26, 9, 30, 9, 34,
                    13, 34, 13, 30, 14, 26, 16, 23, 17, 20,
                    17, 19, 20, 16, 21, 14, 22, 12, 22, 12,
                    22, 8, 20, 4, 18, 2, 16, 1, 13, 0],
                type: 'poly'
            },
            zIndex: 100
        });

        marker.addListener('mouseover', function(e) {
            highlightMarker(e.overlay);
        });
        marker.addListener('mouseout', function(e) {
            unhighlightMarker(e.overlay);
        });
        marker.addListener('click', function(e) {
            var m = e.overlay;

            alert(m.title);
        });

        recognizer.add(marker);

        window.MARKER = marker;
    };

    var overlapCoverMarker = null;

    naver.maps.Event.addListener(recognizer, 'overlap', function(list) {
        if (overlapCoverMarker) {
            unhighlightMarker(overlapCoverMarker);
        }

        overlapCoverMarker = list[0].marker;

        naver.maps.Event.once(overlapCoverMarker, 'mouseout', function() {
            highlightMarker(overlapCoverMarker);
        });
    });

    naver.maps.Event.addListener(recognizer, 'clickItem', function(e) {
        recognizer.hide();

        if (overlapCoverMarker) {
            unhighlightMarker(overlapCoverMarker);

            overlapCoverMarker = null;
        }
    });

    map.setZoom(7);
</script>
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
