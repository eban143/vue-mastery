<template lang='pug'>
  .container

    .courses-header
      .wrapper
        h1.title Our Courses

    .courses-body.wrapper
      CourseList(:courses='courses' :account='account')
      Latest(:courses='courses' :latests='latests' :account='account')

    .vue-conf
      VueConfBanner
    CheatSheetAlt(location='Course page cheat sheet download')
</template>

<script>
import { mapState } from 'vuex'

import CourseList from '~/components/courses/All'
import Latest from '~/components/courses/Latest'
import CheatSheetAlt from '~/components/static/CheatSheetAlt'
import VueConfBanner from '~/components/static/VueConfBanner'

export default {
  name: 'page-courses',

  middleware: 'anonymous',

  head () {
    return {
      title: 'Course Listing | Vue Mastery',
      meta: [{
        hid: 'og:url',
        property: 'og:url',
        content: `${process.env.url}/courses`
      }]
    }
  },

  components: {
    CourseList,
    Latest,
    CheatSheetAlt,
    VueConfBanner
  },

  async fetch ({ store }) {
    await store.dispatch('latests')
    await store.dispatch('getAllCourses')
  },

  computed: {
    ...mapState({
      account: result => result.account.account,
      courses: result => result.courses.courses,
      latests: result => result.courses.latests
    })
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
  font-size 60px
  font-weight 600
  color #FFFFFF
  line-height 60px

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
