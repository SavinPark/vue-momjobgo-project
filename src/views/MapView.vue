<template>
  <div class="map_wrap">
    <div id="map" style="width:100%;height:100%;position:relative;overflow:hidden;"></div>

    <div id="menu_wrap" class="bg_white">
      <div class="option">
        <input type="text" v-model="keyword" id="keyword" size="40" placeholder="검색" autocomplete="off">
        <button @click="search"><v-icon id="search-icon">mdi-magnify</v-icon></button>
      </div>
      <hr>
      <ul id="placesList">
        <li v-for="(item, key) in list" :key="key">
          <div class="placeCard">
            <p id="place_name">{{item.place_name}}</p>
            <p id="place_address" >주소 ) {{item.address_name}}</p>
            <p id="place_phone">연락처 ) {{item.phone}}</p>
            <a id="place_link" :href="item.place_url">{{item.place_url}}</a>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: 'Map',
    data: () => ({
      keyword: '',
      list: [],
      positions: [],
    }),
    mounted() {
       if (window.kakao && window.kakao.maps) {
         this.initMap();
       } else {
         const script = document.createElement("script");
         /* global kakao */
         script.onload = () => kakao.maps.load(this.initMap);
         script.src =
           "//dapi.kakao.com/v2/maps/sdk.js?autoload=false&appkey=002e3c9ec122e02346dcf690ccafe40d";
         document.head.appendChild(script);
       }
    },
    methods: {
      initMap() {
        console.log('initMap / positions : ' + this.positions.length)
        var mapContainer = document.getElementById('map'), // 지도를 표시할 div  
          mapOption = {
            center: new kakao.maps.LatLng(33.450701, 126.570667), // 지도의 중심좌표
            level: 3 // 지도의 확대 레벨
          };
        var map = new kakao.maps.Map(mapContainer, mapOption); // 지도를 생성합니다

        // --------------------------------------

        // // // 마커 이미지의 이미지 주소입니다 -------------------------------------
        // var imageSrc = "https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/markerStar.png";
        // var imageSize = new kakao.maps.Size(24, 35);
        // var markerImage = new kakao.maps.MarkerImage(imageSrc, imageSize);

        // for (let i = 0; i < this.positions.length; i++) {
        //   console.log('마커');
        //   var marker = new kakao.maps.Marker({
        //     map: map,
        //     position: new kakao.maps.LatLng(this.positions[i].x, this.positions[i].y),
        //     title: this.positions[i].title,
        //     image: markerImage 
        //   });
        // }
      },
      async search () {
        console.log("검색!!");

        await axios.get(`https://dapi.kakao.com/v2/local/search/keyword.json?query=${this.keyword}`, {
          headers: {
            Authorization: "KakaoAK ef18086f5a267a615faacf3a9315d1a9"
          }
        }).then(response => {
          this.list = response.data.documents;
          // console.log(response.data.documents);

          this.list.forEach(item => {
            this.positions.push({
              title: item.place_name,
              x: Number(item.x), 
              y: Number(item.y)
            });
          });
          
          var mapContainer = document.getElementById('map'), // 지도를 표시할 div  
            mapOption = {
              center: new kakao.maps.LatLng(this.positions[0].x, this.positions[0].y), // 지도의 중심좌표
              level: 5 // 지도의 확대 레벨
            };
          var map = new kakao.maps.Map(mapContainer, mapOption);
          
          var imageSrc = "https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/markerStar.png";

          for (let i = 0; i < this.positions.length; i++) {
            var imageSize = new kakao.maps.Size(24, 35);
            var markerImage = new kakao.maps.MarkerImage(imageSrc, imageSize);
            // console.log('마커');
            // console.log(this.positions[i].x);
            var marker = new kakao.maps.Marker({
              map: map,
              position: new kakao.maps.LatLng(this.positions[i].x, this.positions[i].y),
              title: this.positions[i].title,
              image: markerImage 
            });
          }
        }).catch(error => {
          console.error(error);
        });
      },
    }
  }
</script>

<style scoped>
.map_wrap, .map_wrap * {margin:0;padding:0;font-family:'Malgun Gothic',dotum,'돋움',sans-serif;font-size:12px;}
.map_wrap a, .map_wrap a:hover, .map_wrap a:active{color:#000;text-decoration: none;}
.map_wrap {position:relative;width:100%;height:500px;}
#menu_wrap {position:absolute;top:0;left:0;bottom:0;width:310px;margin:10px 0 30px 10px;padding:5px;overflow-y:auto;background:rgba(255, 255, 255, 0.7);z-index: 1;font-size:12px;border-radius: 10px;}
.bg_white {background:#fff;}
#menu_wrap hr {display: block; height: 1px;border: 0; border-top: 2px solid #5F5F5F;margin:10px 0;}
#menu_wrap .option input { height: 25px; padding: 5px; outline: 1.5px solid #9b9b9b;}
#menu_wrap .option input:focus { outline: 1.5px solid #5d78ff;}
#menu_wrap .option {text-align: center; margin-top: 4px;}
#menu_wrap .option button {margin-left:4px; width: 25px; height: 27px; border: 1.5px solid #9b9b9b;}
#menu_wrap .option button:hover {background-color: #5F5F5F; border: 1.5px solid #5F5F5F;}
#menu_wrap .option button:hover #search-icon {color: #fff;}
#placesList li {list-style: none;}
#placesList .placeCard {background: #fff; margin: 5px; padding: 4px;}
#placesList .placeCard:hover {box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.1);}
#placesList .placeCard #place_name {font-weight: 600;}
#placesList .placeCard #place_link {color: #5d78ff;}
</style>