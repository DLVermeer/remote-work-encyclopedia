<template>
  <main class="page">
    <slot name="top" />

    <Content class="theme-default-content" />
    <PageEdit />

    <PageNav v-bind="{ sidebarItems }" />

    <div
      class="table-of-content"
      :class="hideTableOfContent"
    >
      <h3 class="title">Table of contents</h3>
      <Sidebar class="intro-sidebar" :items="sidebarItems" />
    </div>

    <slot name="bottom" />

    <footer class="footer">
      <ConvertKit class="subscribe-container" />
      <div>
        Made with ❤️ by
        <a
          href="https://www.moonlightwork.com/"
          rel="noopener"
          target="_blank"
        >Moonlight</a>.
      </div>
      <span class="copyright">&copy; {{ new Date().getFullYear() }} Moonlight Work Inc.</span>
    </footer>
  </main>
</template>

<script>
import { isActive } from '@parent-theme/util';
import ConvertKit from '@theme/components/ConvertKit.vue';
import PageEdit from "@parent-theme/components/PageEdit.vue";
import PageNav from "@parent-theme/components/PageNav.vue";
import Sidebar from "@parent-theme/components/Sidebar.vue";


export default {
  name: 'Page',

  components: { ConvertKit, PageEdit, PageNav, Sidebar },

  props: ["sidebarItems"],

  computed: {
    hideTableOfContent() {
      return { 'hidden': !isActive(this.$route, '/') }; 
    }
  },
};
</script>

<style lang="stylus">
$light-3 = #F6F6F6
$dark-3 = #3D3D4B
$dark-5 = #92929F
$font-xs = 0.625rem /* 10px */

.table-of-content
  &.hidden
    display none

  .title
    padding-top 2.875rem
    margin 0 2rem 1rem
    border-top 1px solid $borderColor

@media (min-width: $MQMobile)
  .table-of-content
    display none

// TODO: remove when product hunt banner removed
.sidebar
  top 6.6rem !important

.theme-default-content:not(.custom)
  padding-top 6.6rem !important

.intro-sidebar.sidebar
  background-color #fff
  background-color transparent
  border-right none
  bottom initial
  box-sizing border-box
  font-size 16px
  left initial
  margin 0
  overflow-y auto
  padding-top 0
  position static
  top initial
  width initial
  z-index 1

  .nav-links
    display none

  .sidebar-link
    padding 0.35rem 2rem 0.35rem 1.75rem


@media (max-width: $MQMobile)
  .intro-sidebar.sidebar
    transform initial

.footer
  background-color $light-3
  color $dark-3
  font-size 0.875rem
  padding 2rem
  text-align center
  display flex
  flex-direction column
  align-items center

  .formkit-form
    border 2px solid #EFEFF0
    box-shadow none

.subscribe-container
  margin-top 4rem
  margin-bottom 2rem

.copyright
  color $dark-5
  font-size $font-xs
  padding 1rem

.navbar .logo
  height 2rem
  vertical-align middle

@media (min-width ($MQMobile + 1px))
  .navbar .logo
    height 1.4rem
</style>
