百廿母校。

<style>
  .my-map { margin: 0 auto; width: 800px; height: 640px; } 
  .my-map .icon { background: url(//a.amap.com/lbs-dev-yuntu/static/web/image/tools/creater/marker.png) no-repeat; } 
  .my-map .icon-cir { height: 31px; width: 28px; } 
  .my-map .icon-cir-red { background-position: -11px -5px; }
  .amap-container { height: 100%; }
  .myinfowindow { width: 240px; min-height: 50px; }
  .myinfowindow h5 { height: 20px; line-height: 20px; overflow: hidden; font-size: 14px; font-weight: bold; width: 220px; text-overflow: ellipsis; word-break: break-all; white-space: nowrap; }
  .myinfowindow div { margin-top: 10px; min-height: 40px; line-height: 20px; font-size: 13px; color: #6f6f6f; }
</style>

<div id="wrap" class="my-map">
  <div id="mapContainer"></div>
</div>

<script src="//webapi.amap.com/maps?v=1.3&key=e07ffdf58c8e8672037bef0d6cae7d4a"></script>
<script>
!function(){
  var infoWindow, map, level = 17,
    center = {lng: 106.269275, lat: 38.450567},
    features = [{"icon":"cir","color":"red","name":"未命名标注","desc":"未命名标注描述","lnglat":{"Q":38.45016379898232,"R":106.26884536787867,"lng":106.268845,"lat":38.450164},"offset":{"x":-9,"y":-31},"type":"Marker"}, {"strokeWeight":2,"strokeOpacity":0.8,"fillOpacity":0.2,"name":"未命名标注","desc":"未命名标注描述","path":[{"Q":38.45179382303599,"R":106.26766519591212,"lng":106.267665,"lat":38.451794},{"Q":38.44939918738354,"R":106.26689271971583,"lng":106.266893,"lat":38.449399},{"Q":38.44876060445501,"R":106.27006845518946,"lng":106.270068,"lat":38.448761},{"Q":38.451062837421446,"R":106.27083020254969,"lng":106.27083,"lat":38.451063},{"Q":38.45178542094459,"R":106.26767592474818,"lng":106.267676,"lat":38.451785}],"lnglat":{"Q":38.45178542094459,"R":106.26767592474818,"lng":106.267676,"lat":38.451785},"type":"Polygon"}];

  function loadFeatures(){
    for(var feature, data, i = 0, len = features.length, j, jl, path; i < len; i++){
      data = features[i];
      switch(data.type){
        case "Marker":
          feature = new AMap.Marker({ map: map, position: new AMap.LngLat(data.lnglat.lng, data.lnglat.lat),
            zIndex: 3, extData: data, offset: new AMap.Pixel(data.offset.x, data.offset.y), title: data.name,
            content: '<div class="icon icon-' + data.icon + ' icon-'+ data.icon +'-' + data.color +'"></div>' });
          break;
        case "Polyline":
          for(j = 0, jl = data.path.length, path = []; j < jl; j++){
            path.push(new AMap.LngLat(data.path[j].lng, data.path[j].lat));
          }
          feature = new AMap.Polyline({ map: map, path: path, extData: data, zIndex: 2,
            strokeWeight: data.strokeWeight, strokeColor: data.strokeColor, strokeOpacity: data.strokeOpacity });
          break;
        case "Polygon":
          for(j = 0, jl = data.path.length, path = []; j < jl; j++){
            path.push(new AMap.LngLat(data.path[j].lng, data.path[j].lat));
          }
          feature = new AMap.Polygon({ map: map, path: path, extData: data, zIndex: 1,
            strokeWeight: data.strokeWeight, strokeColor: data.strokeColor, strokeOpacity: data.strokeOpacity,
            fillColor: data.fillColor, fillOpacity: data.fillOpacity });
          break;
        default: feature = null;
      }
      if(feature){ AMap.event.addListener(feature, "click", mapFeatureClick); }
    }
  }

  function mapFeatureClick(e){
    if(!infoWindow){ infoWindow = new AMap.InfoWindow({autoMove: true,isCustom: false}); }
    var extData = e.target.getExtData();
    infoWindow.setContent("<div class='myinfowindow'><h5>" + extData.name + "</h5><div>" + extData.desc + "</div></div>");
    infoWindow.open(map, e.lnglat);
  }

  map = new AMap.Map("mapContainer", {center: new AMap.LngLat(center.lng, center.lat), level: level, keyboardEnable:true, dragEnable:true, scrollWheel:true, doubleClickZoom:true});
  new AMap.TileLayer.Satellite({map: map, zIndex: 1});
  loadFeatures();

  map.on('complete', function(){
    map.plugin(["AMap.ToolBar", "AMap.OverView", "AMap.Scale"], function(){
      map.addControl(new AMap.ToolBar({ruler: true, direction: true, locate: false})); map.addControl(new AMap.OverView({isOpen: true})); map.addControl(new AMap.Scale);
    }); 
  })
}();
</script>