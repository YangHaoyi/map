<template>
  <div class="hello">
    <div style="height:100vh" id="container" tabindex="0"></div>
  </div>
</template>

<script>
  import AMap from 'AMap';
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    init: function () {
      let map = new AMap.Map('container', {
        center: [116.397428, 39.90923],
        // mapStyle: 'amap://styles/macaron',
        resizeEnable: true,
        zoom: 10,
        lang: 'zh'
      });
      AMap.plugin(['AMap.ToolBar', 'AMap.Scale'], function () {
        map.addControl(new AMap.ToolBar())
        map.addControl(new AMap.Scale())
      });
      AMap.plugin('AMap.Geolocation', function () {

        var geolocation = new AMap.Geolocation({

          // 是否使用高精度定位，默认：true

          enableHighAccuracy: true,

          // 设置定位超时时间，默认：无穷大

          timeout: 10000,

          // 定位按钮的停靠位置的偏移量，默认：Pixel(10, 20)

          buttonOffset: new AMap.Pixel(10, 20),

          // 定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false

          zoomToAccuracy: true,

          // 定位按钮的排放位置, RB表示右下

          buttonPosition: 'RB',
          showMarker: true

        });

        geolocation.getCurrentPosition()

        AMap.event.addListener(geolocation, 'complete', (e) => {

          console.log(e); // 定位成功之后做的事
          map.setZoomAndCenter(16,  e.position);

          // 定位成功之后再定位处添加一个marker

          marker = new AMap.Marker({

            position: e.position, // （e.position）--->定位点的点坐标, position ---> marker的定位点坐标，也就是marker最终显示在那个点上，

            icon: '', // marker的图标，可以自定义，不写默认使用高德自带的

            map: map,  // map ---> 要显示该marker的地图对象

        })

        });

        AMap.event.addListener(geolocation, 'error', (e) => {

          console.log(e) ;// 定位失败做的事

        })

      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
