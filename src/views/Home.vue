<template>
  <div id='home' :class="{'language-en': $i18n.locale !== 'zh'}">
    <ul
      class='menu-list'
      v-if="show"
      :style="{
          width: isSpc ? '60% !important': '',
          left: isSpc ? '20% !important': ''
        }"
    >
      <li
        v-for="(l,i) in $t('nav')"
        :key="'nav' + i"
        @click.prevent.stop="anchor(l)"
        :class="{'rank': i === 0}"
      >
        {{l.name}}
      </li>
      <li  v-for="(l,i) in $t('language.langs')"
           :key="'zk'+i" @click.prevent.stop="changeLang(l)">
          {{l.name}}
      </li>
    </ul>

    <!-- 导航栏 -->
    <section class="nav" :class="'nav-' + $i18n.locale">
      <div class="layout-main" id="navMain">
        <div class="page-menu" @click.prevent.stop="show = true"></div>
        <div class="page-logo"></div>
        <div class="layout-banner-title" :class="{ 'layout-banner-title-en': $i18n.locale !== 'zh' && $i18n.locale !== 'ko', 'layout-banner-title-ko': $i18n.locale === 'ko'}"></div>
        <div class="layout-banner-remark">{{$t('home.banner.desc').join('')}}</div>
        <div class="home-body">
          <!-- div 链间共享区域 -->
          <ul class="ctx-main layout-chain">
            <li v-for="(trueItem, index) in $t('home.chainInfo')" :key="'ck'+index" :class="{'bdOther': index > 2}">
              <img src="../assets/images/banner_li1.png" v-if="index === 0"/>
              <img src="../assets/images/banner_li2.png" v-if="index === 1"/>
              <img src="../assets/images/banner_li3.png" v-if="index === 2"/>
              <img src="../assets/images/banner_li4.png" v-if="index === 3"/>
              <img src="../assets/images/banner_li5.png" v-if="index === 4"/>
              <img src="../assets/images/banner_li6.png" v-if="index === 5"/>
              <h1>{{trueItem.title}}</h1>
              <p>{{trueItem.desc.join('')}}</p>
            </li>
          </ul>
          <!-- 点对点 -->
          <section class="points" :class="'points-' + $i18n.locale">
            <div>
              <div class="points-title">
                <span class="title-left" v-if="$i18n.locale === 'en' || $i18n.locale === 'ko' || $i18n.locale === 'zh'"></span>
                <span> {{$t('home.pointsInfo.title')}}</span>
                <span class="title-right" v-if="$i18n.locale === 'en' || $i18n.locale === 'ko' || $i18n.locale === 'zh'"></span>
              </div>
              <div class="ctx-main">
                <div class="points-circle">Dpayment</div>
                <p v-for="(item, i) in $t('home.pointsInfo.leftDescList')" :key="'dp'+i">{{item}}</p>
                <div class="points-circle">DeFi</div>
                <p v-for="(item, i) in $t('home.pointsInfo.centerDescList')" :key="'df'+i">{{item}}</p>
                <div class="points-circle">DEX</div>
                <p v-for="(item, i) in $t('home.pointsInfo.rightDescList')" :key="'dx'+i">{{item}}</p>
              </div>
            </div>
          </section>
          <!-- 社区激励 -->
          <impare-mod></impare-mod>
          <!-- 路线图 -->
          <map-china></map-china>
          <!-- 产品宣传 -->
          <product-media></product-media>
          <!-- 合作伙伴 -->
          <partner-mod></partner-mod>
          <!-- 底部 -->
          <sys-footer></sys-footer>
          <!-- 悬浮框 -->
          <div :class="{'back_top_box showCss': btnFlag, 'back_top_box': !btnFlag}">
            <div class="back_top">
              <a href="javascript:void(0);" @click.prevent.stop="toTopHandler"></a>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import SysFooter from '@/components/SysFooter'
import ImpareMod from '@/components/ImpareMod'
import MapChina from '@/components/MapChina'
import ProductMedia from '@/components/ProductMedia'
import PartnerMod from '@/components/PartnerMod'
import { setStore, getStore, setTitle } from '@/libs/util'
export default {
  name: 'home',
  components: {
    SysFooter,
    ImpareMod,
    MapChina,
    ProductMedia,
    PartnerMod
  },
  data () {
    return {
      scrollTop: 0, // 回到顶部-上滑距离
      btnFlag: false, // 回到顶部-第一屏不展示
      showFlag: false, // 确定是否出现小点点。
      navMainW: 0,
      navMainH: 0,
      selectLang: {
        name: '',
        subName: '',
        tag: ''
      },
      show: false,
      isSpc: false
    }
  },
  methods: {
    // 为了计算距离顶部的高度，当高度大于60显示回顶部图标，小于60则隐藏
    scrollHandler () {
      this.scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
      this.btnFlag = this.scrollTop > 58
    },
    toTopHandler () {
      let that = this
      let topTimer = setInterval(() => {
        let speedVal = Math.floor(-that.scrollTop / 50)
        document.documentElement.scrollTop = document.body.scrollTop = that.scrollTop + speedVal
        if (that.scrollTop === 0) {
          clearInterval(topTimer)
        }
      }, 16)
    },
    anchor (item, index) {
      const { path } = item;
      this.show = false;
      index === 0 ? this.$router.go(0) : (path.includes('://') ? window.open(path) : document.querySelector(`#${item.path}`).scrollIntoView(true))
    },
    changeLang (item) {
      this.show = false;
      setStore('marcoLang', item.tag)
      this.$i18n.locale = item.tag
      this.selectLang.name = item.name
      this.selectLang.subName = item.subName
      this.selectLang.tag = item.tag
      setTitle()
    },
    resize () {
      const clientWidth = document.body.clientWidth
      this.isSpc = clientWidth > 750
    }
  },
  created () {
    let tag = getStore('marcoLang')
    if (tag === null) {
      tag = 'en'
      setStore('marcoLang', 'en')
    }
    let langList = this.$t('language.langs') || []
    let langVo = langList.filter(item => {
      return item.tag === tag
    })
    if (langVo && langVo.length > 0) {
      let langVoS = langVo[0] || {}
      this.selectLang.name = langVoS.name
      this.selectLang.subName = langVoS.subName
      this.selectLang.tag = langVoS.tag
    }
  },
  mounted () {
    let navDom = document.querySelector('#navMain')
    this.navMainW = navDom.clientWidth
    this.navMainH = navDom.clientWidth
    window.addEventListener('scroll', this.scrollHandler, true)
  },
  destroyed () {
    window.removeEventListener('scroll', this.scrollHandler)
  }
}
</script>

<style lang="less" scoped>
  .flex() {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .menu-list {
    position: fixed;
    z-index: 30;
    top: 0;
    left: 0;
    width: 100%;
    background: #fff;
    height: 200px;
    overflow-y: auto;
    li {
      .flex();
      height: 42px;
      cursor: pointer;
      justify-content: center;
      border-bottom: 1px solid #EBEBEB;
      margin: 0 17px 0 18px;
      &.rank {
        color: #7550F8;
      }
      &:first-child {
        margin: 26px 17px 0 18px;
      }
      &:last-child {
        margin: 0 17px 12px 18px;
        border-bottom: 0;
      }
    }
  }
</style>
