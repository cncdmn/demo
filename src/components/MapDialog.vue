<template>
  <div class="map-dialog">
    <a-modal
      :visible="visible"
      centered
      width="600"
      title="请选择地址"
      @cancel="close"
      @ok="handleOk"
    >
      <div id="container"></div>
      <div class="pickerBox">
        <a-input-search
          placeholder="输入位置进行搜索"
          style="width:400px"
          id="keyWord"
          autocomplete="off"
        >
          <template #enterButton>
            <a-button>搜索</a-button>
          </template>
        </a-input-search>
        <div id="poiInfo"></div>
        <!-- <a-popover title="位置信息" trigger="click" class="locationInfo">
          <template #content>
            <p>Content</p>
            <p>Content</p>
          </template>
        </a-popover> -->
      </div>
      <div class="pt-2">地理位置: {{ locationInfo.location || '--'}}</div>
    </a-modal>
  </div>
</template>
<script setup lang="ts">
import { watch, reactive } from 'vue';
import { shallowRef } from '@vue/reactivity';
import AMapLoader from '@amap/amap-jsapi-loader';
const props = defineProps({
  visible: { type: Boolean, default: false }
});
const emit = defineEmits(['update:visible', 'confirm']);

watch(
  () => props.visible,
  (visible) => {
    visible && initMap();
  }
);

const locationInfo = reactive({
  source: '',
  id: '',
  name: '',
  location: '',
  address: ''
})

const handleOk = () => {
  emit("update:visible", false);
}

const close = () => {
  emit("update:visible", false);
}

let map = shallowRef<any>(null);
const initMap = () => {
    AMapLoader.load({
      key: 'aff5e560e8c3249b07cad737c589c962',
      version: '1.4.15',
      plugins: ['AMap.ToolBar', 'AMap.Scale', 'AMap.Geocoder', 'AMap.Marker', 'AMap.InfoWindow'],
      AMapUI: {}
    })
    .then(() => {
      map = new AMap.Map('container', {
        zoom: 20,
        // center: [116.397428, 39.90923],
        // mapStyle: 'amap://styles/normal',
        // viewMode: '3D',
        // resizeEnable: true
      });

      // 添加控件：放大缩小工具条
      map.addControl(new AMap.ToolBar({
        liteStyle: true
      }))
      // 控件：卡尺
      map.addControl(new AMap.Scale())

      // marker
      const marker = new AMap.Marker();

      // 信息窗
      const infoWindow = new AMap.InfoWindow({
        offset: new AMap.Pixel(0, -20),
      });

      // 信息框设置
      const infoSetting = () => {
        marker.setMap(map);
        infoWindow.setMap(map);
        marker.setPosition(locationInfo.location);
        infoWindow.setPosition(locationInfo.location);
        infoWindow.setContent('地址: ' + locationInfo.address);
        infoWindow.open(map, marker.getPosition());
        map.setCenter(marker.getPosition());
      };

      // 模糊查询
      new AMapUI.loadUI(['misc/PoiPicker'], (PoiPicker) => {
        const poiPicker = new PoiPicker({
          // city:'长沙',
          input: 'keyWord'
        });
        //初始化poiPicker
        poiPickerReady(poiPicker);    
      });

      // 初始化poi
      const poiPickerReady = (poiPicker) => {
        window.poiPicker = poiPicker;
        poiPicker.on('poiPicked', (poiResult) => {
          console.log(poiResult);
          locationInfo.location = poiResult.item.location;
          locationInfo.address = poiResult.item.district;
          infoSetting();
        });
      };
      // 逆地理编码
      const geocoder = new AMap.Geocoder({
        city: '010',
        radius: 1000,
      });
      const regeoCode = () => {
        map.add(marker);
        marker.setPosition(locationInfo.location);

        geocoder.getAddress(locationInfo.location, (status, result) => {
          if (status === 'complete' && result.regeocode) {
            locationInfo.address = result.regeocode.formattedAddress;
          } else {
            log.error('根据经纬度查询地址失败');
          };
        });
      };

      // 地图点击事件
      map.on('click', function (e) {
        locationInfo.location = e.lnglat;
        regeoCode();
        infoSetting();
      });
    })
    .catch((e) => {
      console.log(e);
    });
};

// const poiPickerReady = (poiPicker) => {
//   window.poiPicker = poiPicker;
//   const marker = new AMap.Marker();
//   const infoWindow = new AMap.InfoWindow({
//     offset: new AMap.Pixel(0, -20)
//   });
//   poiPicker.on('poiPicked', (poiResult) => {
//     locationInfo.source = poiResult.source;
//     locationInfo.id = poiResult.item.id;
//     locationInfo.name = poiResult.item.name;
//     locationInfo.location = poiResult.item.location;
//     locationInfo.address = poiResult.item.address;

//     marker.setMap(map);
//     infoWindow.setMap(map);

//     marker.setPosition(locationInfo.location);
//     infoWindow.setPosition(locationInfo.location);

//     infoWindow.setContent('地名: ' + locationInfo.name + '<br/>地址: ' + locationInfo.address);
    
//     infoWindow.open(map, marker.getPosition());

//     map.setCenter(marker.getPosition());
//   });

  // poiPicker.onCityReady(() => {
  //     poiPicker.suggest();
  // });
// }

//构建自定义信息窗体
// function createInfoWindow() {
  // const locationInfo = document.querySelector('.locationInfo');
  // return locationInfo
// }

//关闭信息窗体
// function closeInfoWindow() {
//   map.clearInfoWindow();
// }

</script>
<style scope>
#container {
  position: relative;
  height: 500px;
  width: 500px;
  padding: 0px;
  margin: 0px;
}
.pickerBox {
  z-index: 9999;
  position: absolute;
  width: 300px;
  top: 100px;
  left: 80px;
  bottom: auto;
}
#poiInfo {
  width: 300px;
}
.ant-input-group-addon {
  padding: 0 !important;
}
.pt-2 {
  padding-top: 0.5rem;
}
.amap-ui-poi-picker-sugg-container {
    width: 280px !important;
    left: -1px !important;
    top: 36px !important;
    max-height: 252px !important;
  }
</style>
