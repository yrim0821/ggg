<template>
<v-app>
  <div id="header">
    <main-header />
  </div>

  <v-content>
    <router-view />
  </v-content>


  <v-footer>
    <h6> &nbsp; (주)멀티캠퍼스 서울특별시 강남구 언주로 508 10-17층(역삼동, 서울상록빌딩) 고객센터 02-3429-5041 평일 상담시간 : 09:00 ~ 18:00</h6><br>
  </v-footer>

  <!-- <h1 class="h1-clock mt-2 mb-2" style="text-align:center; color:pink ">{{ data.test }}</h1>
  <h1 class="h1-clock mt-2 mb-2" style="text-align:center; color:pink "></h1> -->

  <v-btn fab small @click="$vuetify.goTo(0, 'easeOutQuad')" style="position:fixed; bottom:10px; right:10px" id="MOVE_TOP_BTN">
    <v-icon>keyboard_arrow_up</v-icon>
  </v-btn>



</v-app>
</template>


<script>

import store from './store'
import MainHeader from './views/MainHeader.vue'

var clock = document.querySelector('.h1-clock');
var clock2 = 'time'

function getTime(){
    const time = new Date();
    const hour = time.getHours();
    const minutes = time.getMinutes();
    const seconds = time.getSeconds();
    clock = `${hour<10 ? `0${hour}`:hour}:${minutes<10 ? `0${minutes}`:minutes}:${seconds<10 ? `0${seconds}`:seconds}`
    clock2 = clock
}

function init(){
    setInterval(getTime, 1000);
}

init();

export default {
  name: 'App',
  components: {
    'main-header': MainHeader,
  },
  store,
  data() {
    return{
      data: {
        test: clock2,
      },
  }
}
}

var agent = navigator.userAgent.toLowerCase();
var name = navigator.appName;

// MS 계열 브라우저를 구분  IE 11+, IE 11,Edge
if (name === 'Microsoft Internet Explorer' || agent.indexOf('trident') > -1 || agent.indexOf('edge/') > -1) {
  alert("해당 브라우저는 크롬에 최적화 되어 있습니다.");
} else if (agent.indexOf('safari') > -1) { // Chrome or Safari
  if (agent.indexOf('chrome') > -1) { // Chrome
  } else {
    alert("해당 브라우저는 크롬에 최적화 되어 있습니다.");
  }
} else if (agent.indexOf('firefox') > -1) { // Firefox
  alert("해당 브라우저는 크롬에 최적화 되어 있습니다.");
}


$(function() {
    $(window).scroll(function() {
        if ($(this).scrollTop() > 600) {
            $('#MOVE_TOP_BTN').fadeIn();
        } else {
            $('#MOVE_TOP_BTN').fadeOut();
        }
    })
  });


</script>
