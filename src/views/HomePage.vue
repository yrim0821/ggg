<template>
<div>
  <ImgBanner imgSrc="https://source.unsplash.com/random">
    <div style="line-height:1.2em; font-family:'Jeju Hallasan'; font-size:60pt ;
    text-shadow: 5px 5px 3px rgb(255,255,255)" slot="text">MY SITE<br></div>
  </ImgBanner>
  <v-container>
    <!-- About Me -->
    <v-layout my-5 id="ttest">
      <v-flex class="aboutMe" :class="$mq" xs8>
        <h2 class="headline mb-3"><span style="font-family:'Jeju Hallasan' ; font-size:20pt">About Me</span></h2>
        <span style="font-family:'KHNPHU'">
        <!-- <p class="mr-4" v-for ="item in data" v-if="item.where == item.info">{{ item.greeting }}<br></p> -->
        <p class="mr-4" v-for ="item in data" v-if="item.where == item.info" v-html="item.greeting"><br></p>
      </span>
      </v-flex>


      <v-flex class="profileImg" :class="$mq" xs4>
        <v-img :src="getImgUrl('me.jpg')" aspect-ratio="1.5" />
      </v-flex>
    </v-layout>

    <!-- Portfolio -->
    <v-layout my-5>
      <v-flex xs12>
        <router-link to="/portfolio">
          <h2 class="headline my-5 text-xs-center"><span style="font-family:'Jeju Hallasan' ; font-size:25pt">Portfolio</span></h2>
        </router-link>
        <PortfolioList></PortfolioList>
      </v-flex>
    </v-layout>

    <!-- Post -->
    <v-layout my-5>
      <v-flex xs12>
        <router-link to="/post">
          <h2 class="headline my-5 text-xs-center"><span style="font-family:'Jeju Hallasan' ; font-size:25pt">Post</span></h2>
        </router-link>
        <PostList :column="$mq==='mobile' ? 1:2"></PostList>
      </v-flex>
    </v-layout>


    <!-- Github -->
    <v-layout my-5>
      <v-flex xs12>
        <h2 class="headline my-5 text-xs-center"><span style="font-family:'Jeju Hallasan' ; font-size:25pt">Project</span>
        </h2>
        <RepositoryList></RepositoryList>
      </v-flex>
    </v-layout>

    <!-- 모바일  페이지에서는 그래프가 안 보이도록 -->
    <v-layout my-5 v-show="$mq==='mobile' ? false : true">
      <v-flex xs12>
        <h2 class="headline my-5 text-xs-center"><span style="font-family:'Jeju Hallasan' ; font-size:25pt">GRAPH</span>
        </h2>
        <gitgraph></gitgraph>
      </v-flex>
    </v-layout>

  </v-container>
</div>
</template>

<script>
import ImgBanner from '../components/ImgBanner'
import PortfolioList from '../components/PortfolioList'
import PostList from '../components/PostList'
import RepositoryList from '../components/RepositoryList'
import Repository from '../components/Repository'
import gitgraph from '../components/gitgraph'

var country = navigator.language.substr(0,2)

var korea = "안녕하세요, 교육이수중입니다<br>코딩은 이곳에서 처음 배웠습니다<br>이 사이트는 한국어기반으로 로딩되었습니다"
var english = "Hi ssafy. <br> This page is loaded on English"

export default {
  name: 'HomePage',
  components: {
    ImgBanner,
    PortfolioList,
    PostList,
    RepositoryList,
    Repository,
    gitgraph
  },
  methods: {
    getImgUrl(img) {
      return require('../assets/' + img)
    }
  },

  data()
  {
    return{
      data: [{
        where: 'ko',
        greeting: korea,
        info : country
      },
      {
        where: 'en',
        greeting: english,
        info : country}],

      info : country
    }
  }
}


</script>

<style>
@import url(//fonts.googleapis.com/earlyaccess/jejuhallasan.css); .jejuhallasan * { font-family: 'Jeju Hallasan', cursive; }
@font-face { font-family: 'KHNPHU'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_one@1.0/KHNPHU.woff') format('woff'); font-weight: normal; font-style: normal; }
.profileImg.mobile {
  display: none;
}

.aboutMe.mobile {
  text-align: center;
  margin: 0 auto;
  font-size: 2vw;
}

.my-5 {
  font-size: 1.7vw;
}
</style>
