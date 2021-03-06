<template lang='pug'>
  .container.vue-conf-page
    VueConfHero

    .wrapper
      .body
        p.lead Vue Mastery is the sole destination for VueConf US 2018 conference videos. We'll be releasing videos over the next few weeks as we receive them.

      //- .callout.-success
      //-   CourseSubscribe(:account='account' :slug='conference' message='Notify me when new talks are available.')

      ConfList(:talks='talks' :account='account' :conference='conference')
</template>

<script>
import { mapState } from 'vuex'
import ConfList from '~/components/courses/ConfList'
import CheatSheetAlt from '~/components/static/CheatSheetAlt'
import CourseSubscribe from '~/components/account/CourseSubscribe'
import VueConfHero from '~/components/static/VueConfHero'

export default {
  name: 'page-vueconf',

  middleware: 'anonymous',

  head () {
    return {
      title: 'VueConf US | Vue Mastery',
      meta: [{
        hid: 'og:url',
        property: 'og:url',
        content: `${process.env.url}/vue-conf`
      }]
    }
  },

  components: {
    ConfList,
    CheatSheetAlt,
    CourseSubscribe,
    VueConfHero
  },

  data () {
    return {
      conference: 'vueconf-2018'
    }
  },

  computed: {
    ...mapState({
      account: result => result.account.account,
      talks: result => result.courses.conference.talks
    })
  },

  async fetch ({ store }) {
    await store.dispatch('getConference', this.conference)
  }
}
</script>

<style lang='stylus' scoped>
.courses-header
  height 220px
  display flex
  align-items center
  background url(/static/images/bkg-courses.svg) no-repeat
  background-size cover

.title
  margin 0
  padding-top 0
  color $secondary-color
  font-weight 600
  line-height 60px

.callout
  display flex
  justify-content center

  >>> form
    width: auto

.body
  justify-self center
  text-align center
  max-width 700px
  margin-bottom ($vertical-space/4)
  +tablet-up()
    margin-bottom ($vertical-space/3)

.lead
  margin-top 0
  +tablet-up()
    font-size 22px

.courses-body
  display grid
  width 100%
  grid-column-gap 4%
  grid-row-gap 45px
  padding-top ($vertical-space/2)
  padding-bottom ($vertical-space/2)

  +laptop-up()
    grid-template-columns 63% 33%

</style>
