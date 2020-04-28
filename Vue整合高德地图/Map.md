申请key：https://console.amap.com/dev/key/app

高德官方：https://lbs.amap.com/api/javascript-api/reference/overlay/

vue整合高德地图官方文档：https://elemefe.github.io/vue-amap/#/zh-cn/introduction/install

注意事项（采坑点）如何配置不做详细说明

1. 高德地图key类型选择： Vue===>Web(JS API)

2. 精准定位时：plugin必须导入AMap.Geolocation

   ```xml
   VueAMap.initAMapApiLoader({
     key: '4ca5f7e2483b349dd91e07143790671b', // 高德地图key
     plugin: ['AMap.Autocomplete', 'AMap.PlaceSearch', 
   			'AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 
   			'AMap.MapType', 'AMap.PolyEditor', 
   			'AMap.CircleEditor', 'AMap.Geolocation'],
     uiVersion: '1.0.11', // ui库版本，不配置不加载,
     v: '1.4.4'
   })
   ```

   

example (其中包含精准定位，坐标点，圆圈等)

```javascript
<template>
  <div id="app">
    <div class="amap-wrapper">
       <el-amap class="amap-box" :zoom="zoom" :center="center" :plugin="plugin">
        <el-amap-circle :center="center" radius="200" />
        <el-amap-circle v-for="(Y,index) in list" :key="index" :center="Y.center" 			 		  :radius="Y.radius" />
        <el-amap-marker v-for="(X,index) in pointList" :key="index" 				      				:position="X.position" />
      </el-amap>
      <el-amap vid="amap" :plugin="myplugin" class="amap-demo" :center="center101" />
      <div class="toolbar">
        <span v-if="loaded">
          location: lag = {{ lng }}  lat = {{ lat }}
        </span>
        <span v-else>正在定位</span>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    const self = this
    return {
      center101: [121.59996, 31.197646],
      lng: 0,
      lat: 0,
      loaded: false,
      myplugin: [{
        pName: 'Scale',
        events: {
          init(instance) {
            console.log(instance)
          }
        }
      },
      {
        pName: 'Geolocation',
        events: {
          init(instance) {
            instance.getCurrentPosition((status, result) => {
              if (result && result.position) {
                self.lng = result.position.lng
                self.lat = result.position.lat
                self.center101 = [self.lng, self.lat]
                self.loaded = true
                self.$nextTick()
              }
            })
          }
        }
      }
      ],
      zoom: 12,
      center: [115.59996, 26.197646],
      position1: [115.59, 26.20],
      pointList: [{ position: [115.591, 26.205] }, { position: [115.595, 26.209] }, { position: [115.599, 26.201] }],
      list: [{ center: [115.59996, 26.197646], radius: 200 }, { center: [115.69996, 26.197646], radius: 200 }, { center: [115.79996, 26.197646], radius: 200 }]
    }
  }
}
</script>

<style>
  .amap-wrapper {
    width: 1300px;
    height: 500px;
  }
  .amap-demo {
    height: 300px;
  }
</style>

```

