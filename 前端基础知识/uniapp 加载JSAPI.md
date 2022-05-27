```javascript
import AMapLoader from '@amap/amap-jsapi-loader';
export default class GXAMap {
  constructor(name) {
      this.name = name;
      this.instance = null;
  }
  // 构造一个广为人知的接口，供用户对该类进行实例化
  static getInstance(name) {
      if(!this.instance) {
          this.instance = new GXAMap(name);
      }
      return this.instance;
  }

  initMap() {
    const loader = AMapLoader.load({
      key: "b675287944f0fc017a353cec8f9a79c7",
      plugins: ['AMap.PlaceSearch', 'AMap.Geolocation']
    })
    console.log('map实例化');
    GXAMap.getInstance().loader = loader
  }

  /**

   * 获取定位的经纬度
     */
       getPos(successCallback) {

    GXAMap.getInstance().loader.then((AMap) => {
      AMap.plugin('AMap.Geolocation', function () { // 异步加载插件
        var geolocation = new AMap.Geolocation({
          // 是否使用高精度定位，默认：true
          enableHighAccuracy: true,
          // 设置定位超时时间，默认：无穷大
          timeout: 10000,
          needAddress: true,
          extensions: 'base',
        noGeoLocation: 3
        })
        // console.log(geolocation)
    
        geolocation.getCurrentPosition(function (status, result) {
          console.log(status)
          console.log(result)
          if (status === 'complete' && result) {
            successCallback && successCallback(result)
          } else {
            // Toast('定位失败')
          }
        })
      })
    })

  }

  /**

   * 逆地理编码
   * @param {Object} lng 经度
   * @param {Object} lat 纬度
   * @param {Object} successCallback 回调
     */
       getLocation(lng, lat, successCallback) {

    GXAMap.getInstance().loader.then((AMap) => {
      AMap.plugin('AMap.Geocoder', function () {
        const geocoder = new AMap.Geocoder({
          radius: 1000, // 以已知坐标为中心点，radius为半径，返回范围内兴趣点和道路信息
          extensions: 'base' // 返回地址描述以及附近兴趣点和道路信息，默认“base”
        })
        geocoder.getAddress([lng, lat], function (status, result) {
          console.log('地址解析')
          console.log(status)
          console.log(result)
          if (status === 'complete') {
            var address = result.regeocode.formattedAddress
            successCallback && successCallback(address)
          } else {
            // Toast('获取地址失败')
      	console.log('地址解析')
      	console.log(status)
      	console.log(result)
          }
        })
      })
    })

  }

  /**

   * 搜索附近位置
   * @param {Object} keyword 关键字
   * @param {Object} successCallback 回调
     */
       getNearbyPos(keyword, successCallback) {

    GXAMap.getInstance().loader.then((AMap) => {
      AMap.plugin('AMap.PlaceSearch', function () {
        const MSearch = new AMap.PlaceSearch({
          city: '青岛市', // 兴趣点城市
          citylimit: true, // 是否强制限制在设置的城市内搜索
          autoFitView: true
        })
        console.log('keyword')
        console.log(keyword)
        MSearch.search(keyword, function (status, result) {
          console.log(status)
          console.log(result)
          if (status === 'complete' && result) {
            // that.list = result.poiList.pois
            // that.nearAddressList = result.poiList.pois.slice(0, 5)
            successCallback && successCallback(result)
          } else {
            // Toast('搜索失败')
          }
        })
      })
    })

  }

}
```

