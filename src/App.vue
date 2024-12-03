<template>
  <div>
    <form>
      <div>
        <label>Latitude: </label>
        <input type="text" v-model="latitude" />
      </div>
      <div>
        <label>Longitude: </label>
        <input type="text" v-model="longitude" />
      </div>
    </form>
    <button class="btn" style="width: 200px; background: skyblue;" v-on:click="showMap">SHOW</button>
  </div>
  <div ref="map" id="mapArea" style="height: 400px;"></div>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    latitude: 35.46248486750884, // 初期緯度
    longitude: 139.6231033539685, // 初期経度
  }),
  methods: {
    showMap() {
      const apiKey = process.env.VUE_APP_GOOGLE_MAPS_API_KEY; // 環境変数からAPIキーを取得
      // APIキーを使用しスクリプトを呼び出す
      const script = document.createElement('script');
      script.src = `https://maps.googleapis.com/maps/api/js?key=${apiKey}&callback=initMap`;
      script.async = true;
      document.head.appendChild(script);

      // スクリプト呼び出し後のコールバック
      window.initMap = () => {
        const myLatLng = { lat: Number(this.latitude), lng: Number(this.longitude) };
        const map = new window.google.maps.Map(this.$refs.map, {
          center: myLatLng,
          zoom: 12,
        });
        new window.google.maps.Marker({
          position: myLatLng,
          map
        });
      };
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
