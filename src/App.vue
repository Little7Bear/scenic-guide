<template>
  <div id="map"></div>
</template>

<script setup lang="ts">
import { onMounted } from 'vue';
import L from 'leaflet';

onMounted(async () => {
  initMap();
});

let map: L.Map = null as any;
let center: L.LatLngExpression = [25.31132, 110.416325]; //中心点,x上下，y左右
function initMap() {
  const corner1 = L.latLng(25.31332, 110.416325);
  const corner2 = L.latLng(25.31132, 110.416325);
  const bounds = L.latLngBounds(corner1, corner2);

  map = L.map('map', {
    center,
    preferCanvas: false, //是否使用 Canvas 来渲染
    attributionControl: true, //版权
    closePopupOnClick: true, //点击地图时 popups（弹出窗口）自动关闭
    doubleClickZoom: true, //地图是否可以通过双击来放大，以及在按住 shift 的同时双击来缩小。如果设置为 'center'，不管鼠标在哪里，双击缩放都将缩放到视图的中心
    dragging: true, //地图是否可以通过 mouse/touch 进行拖动
    zoomControl: false,
    // zoom: 16.1,
    minZoom: 3,
    // maxZoom: 18,
    zoomSnap: 0.1, //强制让地图的缩放级别始终为这个值的倍数
    zoomDelta: 0.5, //地图缩放级别改变的值
    maxBounds: bounds, //地图将被限制在指定的地理边界内，当用户平移将地图拖动到视图以外的范围时会出现弹回的效果， 并且也不允许缩小视图到指定范围以外的区域
    maxBoundsViscosity: 1.0, //如果设置了 maxBounds，这个选项将控制拖动地图时边界的稳固程度。默认值为 0.0，允许用户以正常速度在界外拖动，更高的值会减慢地图在界外的拖动速度，而 1.0 使界外完全稳固，防止用户在界外拖动
  });

  //添加瓦片图层
  L.tileLayer('http://webrd0{s}.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}', {
    attribution: '&copy; 高德地图',
    subdomains: '1234', //瓦片服务的子域。 可以以一个字符串（其中每个字母是一个子域名）或字符串数组的形式传递。
    // minZoom: 0,
    // maxZoom: 18,
    detectRetina: true, //如果 true 并且用户在视网膜显示器上，它将请求四个指定大小一半的瓦片和更大的缩放级别，以使用高分辨率
  }).addTo(map);

  //设置边界
  map.fitBounds([
    [25.320297, 110.406732],
    [25.31132, 110.416325],
    [25.304046, 110.407709],
    [25.309923, 110.428084],
  ]);

  addMarker();
}

function addMarker() {
  //添加标记
  const marker = L.marker(center).addTo(map);
  //添加圆
  const circle = L.circle([25.31, 110.412], {
    color: 'red',
    fillColor: '#f03',
    fillOpacity: 0.5,
    radius: 200,
  }).addTo(map);
  //添加多边形
  const polygon = L.polygon([
    [25.319874, 110.413825],
    [25.319855, 110.418117],
    [25.317721, 110.419576],
    [25.317081, 110.414126],
  ]).addTo(map);
  //弹窗附加到标记上
  marker.bindPopup('<b>Hello world!</b><br>I am a popup.').openPopup();
  circle.bindPopup('I am a circle.');
  polygon.bindPopup('I am a polygon.');

  //弹窗单独做图层
  // L.popup().setLatLng([110.416325,25.31132]).setContent('我是独立的弹窗').openOn(map);

  //监听事件
  // const popup = L.popup();
  // function onMapClick(e: any) {
  //   popup
  //     .setLatLng(e.latlng)
  //     .setContent('你点击的位置 ' + e.latlng.toString())
  //     .openOn(map);
  // }
  // map.on('click', onMapClick);
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body,
#app {
  width: 100%;
  height: 100%;
}

#map {
  width: 100%;
  height: 100%;
}
</style>
