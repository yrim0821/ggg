<template>
<v-app style="position:absolute">

  <div class="toolbar">
    <v-toolbar style="background-color: rgba(189,189,189,0.9)">
      <v-toolbar-side-icon @click.stop="sideNav = !sideNav" class="hidden-sm-and-up">
        <v-icon>menu</v-icon>
      </v-toolbar-side-icon>
      <v-toolbar-title>
        <router-link to="/" tag="span" style="cursor: pointer"><span style="color:blue">SSAFY</span></router-link>

      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn flat v-for="item in menuItems" :key="item.title" :to="item.link">
          <v-icon left dark>{{ item.icon }}</v-icon>
          {{ item.title }}
        </v-btn>
        <v-btn flat v-on:click="bookmarksite('타이틀', 'http://www.naver.com')">북마크</v-btn>
      </v-toolbar-items>
    </v-toolbar>
  </div>


  <v-navigation-drawer temporary style="position:fixed" v-model="sideNav">
    <v-list>
      <v-list-tile v-for="item in menuItems" :key="item.title" :to="item.link">
        <v-list-tile-action>
          <v-icon>{{ item.icon }}</v-icon>
        </v-list-tile-action>
        <v-list-tile-content>{{ item.title }}</v-list-tile-content>
      </v-list-tile>
    </v-list>
  </v-navigation-drawer>



</v-app>
</template>

<script type="text/javascript">

export default {
  data() {
    return {
      sideNav: false,
      menuItems: [{
          icon: 'portrait',
          title: 'Portfolio',
          link: '/portfolio'
        },
        {
          icon: 'photo_filter',
          title: 'Post',
          link: '/post'
        },
        {
          icon: 'exit_to_app',
          title: 'Login',
          link: '/login'
        },
        {
          icon: 'border_color',
          title: 'Portfoliowriter',
          link: '/portfoliowriter'
        },
      ]
    }
  },
  methods: {
    bookmarksite(title, url) {
      // Internet Explorer
      if (document.all) {
        window.external.AddFavorite(url, title);
      }
      // Google Chrome
      else if (window.chrome) {
        alert("즐찾즐찾");
      }
      // Firefox
      else if (window.sidebar) // firefox
      {
        window.sidebar.addPanel(title, url, "");
      }
      // Opera
      else if (window.opera && window.print) { // opera
        var elem = document.createElement('a');
        elem.setAttribute('href', url);
        elem.setAttribute('title', title);
        elem.setAttribute('rel', 'sidebar');
        elem.click();
      }
    }
  },
}
</script>


<style>
.toolbar {
  position: fixed;
  width: 100%;
  z-index: 2;
}
</style>
