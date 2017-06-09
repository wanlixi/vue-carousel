<!-- 功能描述：公共相关案例展示模块 -->
<template>
  <div class="case-discuss" 
       @mouseover="mouseoverCase" 
       @mouseout="autoPlayCase"
       v-show="isCaseLength">
    <p class="case-discuss-tit">相关案例展示</p>
    <div class="case-discuss-content">
      <div class="case-discuss-cont" :style="'width: ' + currentCase.length * 360 + 'px'">
        <div class="case-discuss-con" v-for="(item, index) in currentCase" :key="index" @click="clickCase(item)">
          <div class="case-img">
            <img :src="item.img" />
          </div>
          <div class="case-tit">{{item.tit}}</div>
          <p class="case-con">{{item.content}}</p>
        </div>
      </div>
    </div>
    <div class="case-control-l case-control-btn" v-if="controlBtnStatus" @click="lastCase">◀</div>
    <div class="case-control-r case-control-btn" v-if="controlBtnStatus" @click="nextCase">▶</div>
  </div>
</template>
<script type="text/babel">
import $ from 'jquery'
import {otherCase} from '~assets/services/case/othercase'
export default {
  data () {
  	return {
      cases: otherCase,
      timerCase: null,
      selectedCase: 0,
      controlBtnStatus: true,
      currentCase: [],
      isCaseLength: false,
  	}
  },
  props:{
    caseId:{
      type:Number,
      default:null
    }
  },
  mounted () {
    let self = this;
    // if (self.caseId == 10) {
    //   self.cases[19].caseId = 15;
    //   self.cases[20].caseId = 16;
    //   self.cases[21].caseId = 17;
    //   self.cases[22].caseId = 18;
    // }
    if (self.caseId == 30) {
      self.cases[25].caseId = 35;
    }
    if (self.caseId == 50) {
      self.cases[26].caseId = 53;
      self.cases[22].caseId = 54;
    }

    self.cases.forEach(item => {
      if (parseInt(item.caseId / 10) == parseInt(self.caseId / 10)) {
        self.currentCase.push(item);
      }
    });

    if (self.currentCase.length > 3) {
      self.autoPlayCase();
    } else {
      self.controlBtnStatus = false;
      $('.case-discuss-content').css('width', 360 * self.currentCase.length + 'px');
    }

    if (self.currentCase.length > 0) {
      self.isCaseLength = true;
    }
  },
  destroyed () {//生命周期：实例被销毁之后出发
    let self =this;
    clearInterval(self.timerCase);
  },
  methods:{
  	autoPlayCase () {//自动轮播
      let self = this;
      self.timerCase = setInterval(function () {
        self.selectedCase++;
        if (self.selectedCase > self.currentCase.length-3){
          self.selectedCase = 0
        }
        $(".case-discuss-cont:visible").animate({left: -self.selectedCase * 360}, 500)
      },3000)
    },
    caseScroll () {//上下页操作
      let self = this;
      if (self.selectedCase < 0) {
          self.selectedCase = 0
      }
      if (self.selectedCase > self.currentCase.length - 3) {
          self.selectedCase = self.currentCase.length - 3
      }
      $(".case-discuss-cont:visible").animate({left: -self.selectedCase * 360}, 500)
    },
    mouseoverCase () {//鼠标进入暂停轮播
      let self = this;
      clearInterval(self.timerCase);
    },
    nextCase () {//下一页
      let self = this;
      self.selectedCase++;
      self.caseScroll();
    },
    lastCase () {//下一页
      let self = this;
      self.selectedCase--;
      self.caseScroll();
    },
    clickCase (item) {
      let self = this;
      if (window.location.href.indexOf('case') > -1) {
        self.$emit('change-case',item.caseId);
      } else {
        self.$router.push({name:'case-caseId',params:{caseId: item.caseId}})
      }
    }
  }
}
</script>
<style lang="stylus" scoped>
  @import '~assets/css/var.styl'
  @import '~assets/css/function.styl'

  .case-discuss {
    position: relative
    padding: $paddingTop 0
    color: $fontColorBlack
  }

  .case-discuss-tit {
    font-size: $fontTitle1
    text-align: center
  }
    
  .case-discuss-content {
    overflow: hidden
    position: relative
    width: 1080px
    height: 364px
    margin: 0 auto
  }
  
  .case-discuss-cont {
    position: absolute
    top: 0
    left: 0
    display: flex
  }
  
  .case-discuss-con {
    width: 360px
    padding: 0 20px
    cursor: pointer
  }
  
  .case-img {
    display: flex
    justify-content: center
    align-items: center
    width: 320px
    height: 180px
    margin: 30px 0
    
    img {
      width: 100%
      height: 100%
    }
  }
  
  .case-tit {
    text-align: center
    font-size: $fontTitle3
    margin-bottom: 30px
    color: #3399ff
  }
  
  .case-con {
    text-align: left
    color: $fontColor
  }
  
  .case-control-btn {
    position: absolute
    top: 50%
    font-size 100px
    color #eee
    margin-top: (-(150px - 100px)/2)
    cursor: pointer
    
    &.case-control-l {
      left: 40px
    }
    
    &.case-control-r {
      right: 40px
    }
  }
</style>