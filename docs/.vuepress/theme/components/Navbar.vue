<template>
  <div>
    <!-- TODO: remove when product hunt banner removed  -->
    <header class="banner">
      <a
        href="https://www.producthunt.com/posts/remote-work-encyclopedia?utm_source=badge-featured&utm_medium=badge&utm_souce=badge-remote-work-encyclopedia"
        target="_blank"
      >
        <img
          src="https://api.producthunt.com/widgets/embed-image/v1/featured.svg?post_id=175063&theme=light"
          alt="Remote Work Encyclopedia - Moonlight’s tactical guide to working from anywhere | Product Hunt Embed"
          style="width: 175px; height: 54px;"
          width="175px"
          height="54px"
        />
      </a>
      <span>
        Show your support for remote work on Product Hunt!
      </span>
    </header>
    <header class="navbar">
      <SidebarButton @toggle-sidebar="$emit('toggle-sidebar')"/>
  
      <router-link
        :to="$localePath"
        class="home-link"
      >
        <img
          ref="siteLogo"
          class="logo"
          v-if="$site.themeConfig.logo"
          :src="$withBase($site.themeConfig.logo)"
          :alt="$siteTitle"
        >
        <span
          ref="siteName"
          class="site-name"
          v-if="$siteTitle"
          :class="{ 'can-hide': $site.themeConfig.logo }"
        >{{ $siteTitle }}</span>
      </router-link>
  
      <div
        class="links"
        :style="linksWrapMaxWidth ? {
          'max-width': linksWrapMaxWidth + 'px'
        } : {}"
      >
        <NavLinks class="can-hide"/>
        <AlgoliaSearchBox
          v-if="isAlgoliaSearch"
          :options="algolia"
        />
        <SearchBox v-else-if="$site.themeConfig.search !== false && $page.frontmatter.search !== false"/>
      </div>
    </header>
  </div>
</template>

<script>
// https://github.com/vuejs/vuepress/blob/master/packages/%40vuepress/theme-default/components/Navbar.vue
import AlgoliaSearchBox from '@AlgoliaSearchBox'
import SearchBox from '@SearchBox'
import SidebarButton from '@parent-theme/components/SidebarButton.vue'
import NavLinks from '@theme/components/NavLinks.vue'

export default {
  components: {
    AlgoliaSearchBox,
    NavLinks,
    SearchBox,
    SidebarButton,
  },

  data () {
    return {
      linksWrapMaxWidth: null
    }
  },

  mounted () {
    const MOBILE_DESKTOP_BREAKPOINT = 720 // refer to config.styl
    const NAVBAR_VERTICAL_PADDING = parseInt(css(this.$el, 'paddingLeft')) + parseInt(css(this.$el, 'paddingRight'))
    const handleLinksWrapWidth = () => {
      if (document.documentElement.clientWidth < MOBILE_DESKTOP_BREAKPOINT) {
        this.linksWrapMaxWidth = null
      } else {
        this.linksWrapMaxWidth = this.$el.offsetWidth - NAVBAR_VERTICAL_PADDING
          - (this.$refs.siteName && this.$refs.siteName.offsetWidth || 0)
      }
    }
    const handleMobileLogo = () => {
      if (document.documentElement.clientWidth < MOBILE_DESKTOP_BREAKPOINT) {
        this.$refs.siteLogo.src = this.$withBase(this.$site.themeConfig.mobileLogo)
      } else {
        this.$refs.siteLogo.src = this.$withBase(this.$site.themeConfig.logo)
      }
    }

    handleLinksWrapWidth()
    window.addEventListener('resize', handleLinksWrapWidth, false)
    handleMobileLogo()
    window.addEventListener('resize', handleMobileLogo, false)
  },

  beforeDestroy () {
    window.removeEventListener('resize', handleLinksWrapWidth, false)
    window.removeEventListener('resize', handleMobileLogo, false)
  },

  computed: {
    algolia () {
      return this.$themeLocaleConfig.algolia || this.$site.themeConfig.algolia || {}
    },

    isAlgoliaSearch () {
      return this.algolia && this.algolia.apiKey && this.algolia.indexName
    }
  }
}

function css (el, property) {
  // NOTE: Known bug, will return 'auto' if style value is 'auto'
  const win = el.ownerDocument.defaultView
  // null means not to return pseudo styles
  return win.getComputedStyle(el, null)[property]
}
</script>

<style lang="stylus">
$navbar-vertical-padding = 0.7rem
$navbar-horizontal-padding = 1.5rem

// TODO: remove when product hunt banner removed
.banner
  align-items center
  background #fae9e4
  box-sizing border-box
  color #cc4d29
  display flex
  font-size 1rem
  height 3rem
  justify-content center
  left 0
  padding 0 2rem
  position fixed
  right 0
  top 0
  z-index 21

  span
    display none

    @media (min-width: ($MQMobile))
      display block

  img
    margin-right 1rem

.navbar
  top 3rem !important // TODO: remove when product hunt banner removed
  padding $navbar-vertical-padding $navbar-horizontal-padding
  line-height $navbarHeight - 1.4rem
  a, span, img
    display inline-block
  .logo
    height $navbarHeight - 1.4rem
    min-width $navbarHeight - 1.4rem
    margin-right 0.8rem
    vertical-align top
  .site-name
    font-size 1.3rem
    font-weight 600
    color $textColor
    position relative
  .links
    padding-left 1.5rem
    box-sizing border-box
    background-color white
    white-space nowrap
    font-size 0.9rem
    position absolute
    right $navbar-horizontal-padding
    top $navbar-vertical-padding
    display flex
    .search-box
      flex: 0 0 auto
      vertical-align top
      margin-left 1rem
      margin-right 0

@media (max-width: $MQMobile)
  .navbar
    padding-left 4rem
    .can-hide
      display none
    .links
      padding-left 1.5rem
    .site-name
      width calc(100vw - 9.4rem)
      overflow hidden
      white-space nowrap
      text-overflow ellipsis
</style>
