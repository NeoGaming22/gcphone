<template>
  <div class="phone_app">
    <PhoneTitle :title="currentScreen.title" backgroundColor="#1da1f2" />
    <div class="phone_content">
      <component v-bind:is="currentScreen.component"/>
    </div>
    <div class="twitter_menu">
      <div
        v-for="(s, i) in screen"
        :key="i"
        class="twitter_menu-item">
        <i class="fa" :class="s.icon" :style="{color: i === currentScreenIndex ? '#1da1f2' : '#303030'}"></i>
      </div>
    </div>
  </div>
</template>

<script>
import PhoneTitle from './../PhoneTitle'
import TwitterView from './TwitterView'
import TwitterPostTweet from './TwitterPostTweet'
import TwitterAccount from './TwitterAccount'
import TwitterTopTweet from './TwitterTopTweet'
import { mapGetters } from 'vuex'

export default {
  components: {
    PhoneTitle
  },
  data () {
    return {
      currentScreenIndex: 0
    }
  },
  computed: {
    ...mapGetters(['IntlString']),
    screen () {
      return [
        {
          title: this.IntlString('APP_TWITTER_VIEW_TWITTER'),
          component: TwitterView,
          icon: 'fa-home'
        },
        {
          title: this.IntlString('APP_TWITTER_VIEW_TOP_TWEETS'),
          component: TwitterTopTweet,
          icon: 'fa-heart'
        },
        {
          title: this.IntlString('APP_TWITTER_VIEW_TWEETER'),
          component: TwitterPostTweet,
          icon: 'fa-comment'
        },
        {
          title: this.IntlString('APP_TWITTER_VIEW_SETTING'),
          component: TwitterAccount,
          icon: 'fa-cog'
        }
      ]
    },
    currentScreen () {
      return this.screen[this.currentScreenIndex]
    }
  },
  watch: {
  },
  methods: {
    onLeft: function () {
      this.currentScreenIndex = Math.max(0, this.currentScreenIndex - 1)
    },
    onRight () {
      this.currentScreenIndex = Math.min(this.screen.length - 1, this.currentScreenIndex + 1)
    },
    home () {
      this.currentScreenIndex = 0
    }
  },
  created: function () {
    this.$bus.$on('keyUpArrowLeft', this.onLeft)
    this.$bus.$on('keyUpArrowRight', this.onRight)
    this.$bus.$on('twitterHomme', this.home)
  },
  mounted () {
  },
  beforeDestroy: function () {
    this.$bus.$off('keyUpArrowLeft', this.onLeft)
    this.$bus.$off('keyUpArrowRight', this.onRight)
    this.$bus.$off('twitterHomme', this.home)
  }
}
</script>

<style scoped>
.twitter_menu {
  border-top: 1px solid #CCC;
  height: 56px;
  display: flex;
  width: 100%;
}
.twitter_menu-item {
  flex-grow: 1;
  flex-basis: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
