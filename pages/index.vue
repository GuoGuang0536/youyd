<template>
  <el-row>
    <el-col :span="17" :xs="24">
      <div class="main">
        <!-- 轮播图 -->
        <carrousel :article="article" />
        <!-- 评论，导语 -->
        <announcement :announcement="announcement" />
        <!-- 热门文章列表 -->
        <article-list :article="article" @loadmore="loadMoreArticle" />
      </div>
    </el-col>
    <div v-if="!isMobile" class="main-right" >
      <el-col :span="6" class="right-list">
        <aside-view key="aside"/>
      </el-col>
    </div>
  </el-row>
</template>

<script>
import ArticleList from '~/components/archive/list'
import Carrousel from '~/components/archive/carrousel'
import Announcement from '~/components/archive/announcement'
import AsideView from '~/components/layout/pc/aside/main'

export default {
  name: 'Index',
  head() {
    return this.isMobile ? { bodyAttrs: { class: 'mobile' }} : {}
  },
  fetch({ store }) {
    return Promise.all([
      store.dispatch('article/fetchList')
    ])
  },
  components: {
    AsideView,
    Carrousel,
    Announcement,
    ArticleList
  },
  computed: {
    isMobile() {
      return this.$store.state.global.isMobile
    },
    article() {
      return this.$store.state.article.list
    },
    announcement() {
      return this.$store.state.announcement
    },
    nextPageParams() {
      return {
        page: this.article.data.pagination.pageNumber + 1
      }
    }
  },
  methods: {
    loadMoreArticle() {
      this.$store.dispatch('article/fetchList', this.nextPageParams)
    }
  }
}
</script>

<style lang="scss" scoped>
.icon {
  width: 2em;
  height: 2em;
  // vertical-align: -0.15em;
  // fill: currentColor;
  // overflow: hidden;
}

  .aside-nav {
    width:19em;
    display: inline-block;
    width: 11.5em;
    top: 5.5em;
    left: calc((100% - 75em) / 2);
    .nav-list {
      padding: 0;
      margin: 0;
      > .item {
        border: none;
        display: block;
        width: 100%;
        height: 3em;
        line-height: 3em;
        padding: 0 1em;
        text-decoration: none;
        text-transform: uppercase;
        font-weight: 700;
        border-radius: 1px;
        color: $secondary;
        font-family: webfont-normal, DINRegular;
        letter-spacing: 0.5px;
        margin-bottom: .5em;
        position: relative;
        &:hover {
          background-color: rgba(0,154,97,0.08);;
        }
      }
    }
  }

</style>
