<template>
  <div>
    <div id="map" class="w-52 h-40"></div>
  </div>
</template>

<script setup lang="ts">
const config = useRuntimeConfig();
// declare global {
//   interface Window {
//     kakao: any;
//   }
// }

// kakao 를 글로벌로 선언해 주어야 하므로 아래와 같은 코드가 필요하다.

const map = ref();
const latitude = ref(37.39843974939604);
const longitude = ref(127.10972941510465);
const initMap = () => {
  const container = document.getElementById('map');

  const options = {
    center: new window.kakao.maps.LatLng(latitude.value, longitude.value),
    level: 8,
  };

  map.value = new window.kakao.maps.Map(container, options);
};
onMounted(() => {
  if (!window.kakao || !window.kakao.maps) {
    const script = document.createElement('script');
    script.type = 'text/javascript';
    script.src = `//dapi.kakao.com/v2/maps/sdk.js?appkey=${config.public.kakaoKey}&autoload=false`;
    /*
      스크립트 로드가 다되고나서 kakao.maps.load함수를 호출하게끔 스크립트 주소 끝에 &autoload=false를 붙여줘야합니다
      그렇지 않으면 스크립트 호출전에 kakao.maps.load가 호출되서 window.kakao.maps.latLngis not a constuructor오류가 나옴
    */
    /* eslint를 사용한다면 kakao 변수가 선언되지 않았다고
     * 오류를 내기 때문에 아래 줄과 같이 전역변수임을
     * 알려주어야 한다. */
    /* global kakao */
    script.addEventListener('loadka', () => {
      window.kakao.maps.load(() => {
        // 카카오맵 API가 로딩이 완료된 후 지도의 기본적인 세팅을 시작해야 한다.
        initMap();
      });
    });
    document.head.appendChild(script);
  } else {
    // 이미 카카오맵 API가 로딩되어 있다면 바로 지도를 생성한다.
    initMap();
  }
});
</script>

<style scoped></style>
