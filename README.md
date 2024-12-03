### 今回の実装の目的
- Vueチュートリアルで学んだ仕組みを理解する
- Google Maps APIの使用に慣れる

### 参考記事
https://smile-jsp.hateblo.jp/entry/2023/07/21/184257

### 今回の実装で学んだこと
- VueのOptions APIの仕組み（data, methodsで整理する）
- Vueの`v-model`, `v-on`の仕組み
- Vueにおける、環境変数の設定方法
- Google Maps APIを使った地図の表示
- Google Maps APIを使った緯度・経度の指定

### 実装を理解する
1. 地図をデフォルトの緯度・経度にて表示させる
    1. デフォルトの緯度・経度を設定する
        
        ```jsx
        <script>
        export default {
          name: 'App',
          data: () => ({
            latitude: 35.46248486750884, // 初期緯度
            longitude: 139.6231033539685, // 初期経度
          }),
        ```
        
        - ここでは、`data`や`methods`があることから、[Vueチュートリアル　はじめに](https://ja.vuejs.org/guide/introduction)で解説のあった、Options APIが使われていることが分かる
        
    2. 地図スクリプトをAPIから取得する
        
        ```jsx
          methods: {
            showMap() {
              // APIキーを使用しスクリプトを呼び出す
              // scriptタグを作成
              const script = document.createElement('script');
              // src属性に、google maps apiのURLを設定
              script.src = 'https://maps.googleapis.com/maps/api/js?key=API KEY&callback=initMap';
              // 非同期でロード
              script.async = true;
              // headタグに追加
              document.head.appendChild(script);
        ```
        
        - クエリに`callback=initMap`が設定されており、スクリプトをロード後、initMapが呼び出される仕組みになっている
        
    3. スクリプトを使い、地図を作成する
        
        ```jsx
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
        ```
        
        - Google Maps JavaScript API は、`<script>` タグを使って読み込むことで、グローバルな `window` オブジェクトに自動的に追加されるため、`window`オブジェクトから呼び出せる
        - 緯度・経度を先ほど設定した`data`から取得している
            
            ```jsx
            const myLatLng = { lat: Number(this.latitude), lng: Number(this.longitude) };
            ```
                        
        - divタグに、設定した緯度・経度を中心とした地図を表示させる
            
            ```jsx
            const map = new window.google.maps.Map(this.$refs.map, {
              center: myLatLng,
              zoom: 12,
            });
            ```
            
            - `this.$refs.map`は、以下のdivタグ（`ref=”map”`と設定されている）を指している
                
                ```jsx
                <div ref="map" id="mapArea" style="height: 400px;"></div>
                ```
                
        - 地図の中心にマーカーを立てる
            
            ```jsx
            new window.google.maps.Marker({
              position: myLatLng,
              map
            });
            ```
            
    4. 最後に、ボタンを押すと、`showMap`が呼び出されるようにする
        
        ```jsx
        <button v-on:click="showMap">SHOW</button>
        ```
        
2. 入力された緯度・経度に対応し、`data`の`latitude`プロパティの値を変える
    
    ```jsx
    <input v-model="latitude" /> 
    ```
    

1. SHOWボタンを押すと、入力された緯度・経度を中心とした地図を再表示する

