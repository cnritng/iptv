<template>
  <div>
    <header>
      <nav>
        <div class="container">
          <div class="nav-wrapper">
            <a href="#" class="full button-collapse" @click.prevent="menuVisible = true"><i class="material-icons">menu</i></a>
            <a class="brand-logo">清华IPTV</a>
          </div>
        </div>
      </nav>
      <ul id="slide-out" class="side-nav fixed" :style="sideNavStyle">
        <li><div class="userView">
            <div class="background">
              <img src="http://thecatapi.com/api/images/get?format=src&type=gif&size=med">
            </div>
            <a href="#!user"><img class="circle" src="http://thecatapi.com/api/images/get?format=src&type=gif&size=small"></a>
            <a href="#!name"><span class="white-text name">John Doe</span></a>
            <a href="#!email"><span class="white-text email">jdandturk@gmail.com</span></a>
        </div></li>
        <li v-for="c in channels['Categories']"  :class="{active: c['Name'] == category}" @click="menuVisible = false">
          <router-link :to="categoryLink(c)">{{c['Name']}}</router-link>
        </li>
        <li><div class="divider"></div></li>
        <li><a href="#">登出</a></li>
      </ul>
    </header>

    <main>
      <div class="container">
        <div class="row" id="list">
          <channel-thumbnail v-for="c in channelsOfCurrentCategory" class="col l3 m4 s12" :channel="c"></channel-thumbnail>
        </div>
      </div>
    </main>
    <iptv-footer></iptv-footer>
    <div v-if="menuVisible" @click.prevent="menuVisible = false" id="sidenav-overlay" style="opacity: 1;"></div>
  </div>
</template>

<script>
import 'materialize-css/dist/css/materialize.css'
import 'materialize-css/css/ghpages-materialize.css'
import 'material-design-icons/iconfont/material-icons.css'

import ChannelThumbnail from './ChannelThumbnail.vue'
import IPTVFooter from './IPTVFooter.vue'

import { categoryLink } from '../route/link.js'

export default {
  props: ['category', 'channels'],
  components: {
    ChannelThumbnail,
    'iptv-footer': IPTVFooter,
  },
  name: 'list-view',
  data() {
    return {
      menuVisible: false,
    }
  },
  updated() {
    window.scrollTo(0, 0)
  },
  methods: {
    categoryLink,
  },
  computed: {
    sideNavStyle() {
      if (this.menuVisible) {
        return {
          transform: 'translateX(0)',
        }
      }
    },
    channelsOfCurrentCategory() {
      for (const c of this.channels['Categories']) {
        if (c['Name'] === this.category) {
          return c['Channels']
        }
      }
    },
  },
}
</script>

<style scoped>
header, main, footer {
  padding-left: 300px;
}

@media only screen and (max-width : 992px) {
  header, main, footer {
    padding-left: 0;
  }
}

#slide-out {
  transition: transform 0.3s;
}
</style>