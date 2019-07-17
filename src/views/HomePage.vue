<template>
<div>
  <ImgBanner imgSrc="https://source.unsplash.com/random">
    <div style="line-height:1.2em; font-family:'Jeju Hallasan'; font-size:60pt ;
    text-shadow: 5px 5px 3px rgb(255,255,255)" slot="text">MY SITE<br></div>
  </ImgBanner>

  <v-container>
    <!-- About Me -->
    <v-layout row wrap my-5 id="ttest" style="text-align:center">
      <v-flex xs12 class="aboutMe" :class="$mq">
        <h2 class="headline mb-3"><span style="font-family:'Jeju Hallasan' ; font-size:20pt">About Me</span></h2>
      </v-flex>

      <v-flex xs12 span style="text-align:left">
      <v-carousel>
        <!-- <v-carousel-item v-for="(item,i) in items" :key="i" :src="item.src"> -->
        <v-carousel-item v-for="(item,i) in items" :sentence="item.sentece">
                {{ item.sentence }}



          <v-flex class="profileImg" :class="$mq" xs4>
            <v-img :src="getImgUrl('me.jpg')" aspect-ratio="1.5" />
          </v-flex>
        </v-carousel-item>
      </v-carousel>
    </v-flex xs12>
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
        <h2 class="headline my-5 text-xs-center"><span style="font-size:25pt;">Project</span>
        </h2>
        <v-layout column v-if="$mq==='mobile'">
          <v-flex xs12>
            <RepositoryList class="notranslate"></RepositoryList>
          </v-flex>
          <v-flex xs12 class="show-on-scroll">
            <membersgraph></membersgraph>
          </v-flex>
        </v-layout>
        <v-layout v-else align-center>
          <v-flex xs6 grow>
            <RepositoryList class="notranslate"></RepositoryList>
          </v-flex>
          <v-flex xs6 shrink class="show-on-scroll">
            <membersgraph></membersgraph>
          </v-flex>
        </v-layout>
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
import membersgraph from '../components/membersgraph'

export default {
  name: 'HomePage',
  components: {
    ImgBanner,
    PortfolioList,
    PostList,
    RepositoryList,
    Repository,
    gitgraph,
    membersgraph
  },
  methods: {
    getImgUrl(img) {
      return require('../assets/' + img)
    }
  },

  data() {
    return {
      items: [{
          sentence:'안녕',
          src: 'https://cdn.vuetifyjs.com/images/carousel/squirrel.jpg'
        },
        {sentence:'하세',
          src: 'https://cdn.vuetifyjs.com/images/carousel/sky.jpg'
        },
        {sentence:'요',
          src: 'https://cdn.vuetifyjs.com/images/carousel/bird.jpg'
        },
        {sentence:'잉',
          src: 'https://cdn.vuetifyjs.com/images/carousel/planet.jpg'
        }
      ]
    }
  }
}
</script>

<style>
@import url(//fonts.googleapis.com/earlyaccess/jejuhallasan.css);

.jejuhallasan * {
  font-family: 'Jeju Hallasan', cursive;
}

@font-face {
  font-family: 'KHNPHU';
  src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_one@1.0/KHNPHU.woff') format('woff');
  font-weight: normal;
  font-style: normal;
}

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
