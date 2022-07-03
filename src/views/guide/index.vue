<template>
  <div class="app-container">
    <aside>
      <a href="http://www.5guav.com.cn/" target="_blank">5G-UAV</a>
      致力于实现无人机网联化、网络化、智能化、微型化。我们以网联无人机的研发、试验和产业化为核心，
      现已开发出小型、微型两系列5G无人机及其周边装备。现已建立全系列网联无人机产品线，包含灯塔系列网联无人
      机地面站、5G无人机云系统、无人机高清成像载荷，无人机热成像载荷等。
      我们可以向警用安防、消防救援、线路巡检和军事侦察领域的行业级客户提供完整解决方案。
      我们紧跟国际先进技术及发展趋势，始终奉行自主研发的理念，注重前瞻性的技术开发储备及集成技术的深入研究，
      努力为客户提供安全高效、自主可控的高性能产品。
<!--      <a href="http://oa01.5guav.com.cn:2450/" target="_blank">driver.js.</a>-->
    </aside>
    <el-button icon="el-icon-question" type="primary" target="_blank" onclick="window.open('http://www.5guav.com.cn/') " >
      我们的官网——点击这里
    </el-button>
    <el-button icon="el-icon-question" type="primary" target="_blank" onclick="window.open('http://oa01.5guav.com.cn:2450/')" >
      我们的在线办公系统——点击这里
    </el-button>
<!--    <el-image-->
<!--      style="width: 50px; height: 50px"-->
<!--      :src="url"-->
<!--      :preview-src-list="srcList">-->
<!--    </el-image>-->
<!--    <img src="http://www.5guav.com.cn/wp-content/uploads/2020/07/QQ%E6%88%AA%E5%9B%BE20200726090642-e1595725713186.png" alt="Products Image" width="580" height="600" />-->
  </div>
</template>

<script>
import Driver from 'driver.js' // import driver.js
import 'driver.js/dist/driver.min.css' // import driver.js css
import steps from './steps'

export default {
  name: 'Guide',
  data() {
    return {
      url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
      srcList: [
        'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
        'https://fuss10.elemecdn.com/1/8e/aeffeb4de74e2fde4bd74fc7b4486jpeg.jpeg'
      ]
    }
  },
  mounted() {
    this.driver = new Driver()
  },
  methods: {
    guide() {
      this.driver.defineSteps(steps)
      this.driver.start()
    }
  }
}
</script>

<!--<template>-->
<!--  <div class="renzheng">-->
<!--    <div class="rz-notice" :style="{ display: isno}">-->
<!--      <p>请上传图片</p>-->
<!--    </div>-->
<!--    <van-tabs v-model="active" swipeable>-->
<!--      <van-tab v-for="(item,index) in navArr" :key="item.label" title="item.name" class="v-tab">-->
<!--        <div v-if="index==0">-->
<!--          <div class="color-black" style="text-align: center;margin-top: 0.4rem;">请上传本人名片照片</div>-->
<!--          <div class="rz-picter">-->
<!--            <img :src="avatar" class="img-avatar">-->
<!--            <input id="uppic" ref="avatarInput" type="file" name="avatar" accept="image/gif,image/jpeg,image/jpg,image/png" class="uppic" @change="changeImage($event)">-->
<!--          </div>-->

<!--          <div class="cuxian" />-->

<!--          <div class="my-content-list">-->
<!--            <div class="color-black"><span style="margin-right: 0.6rem;">真实姓名:</span><input v-model="yoursname" placeholder="请输入您的真实姓名"></div>-->
<!--          </div>-->
<!--          <div class="my-content-list">-->
<!--            <div class="color-black"><span style="margin-right: 0.6rem;">手机号码:</span><input v-model="yoursphone" type="number" max="11" placeholder="请输入您的手机号码"></div>-->
<!--          </div>-->
<!--          <div class="yzbtn" @click="submitBtn">-->
<!--            立即认证-->
<!--          </div>-->
<!--        </div>-->
<!--        <div v-if="index==1">-->

<!--          <div class="color-black" style="text-align: center;margin-top: 0.4rem;">请上传本人身份证照片</div>-->
<!--          <div class="rz-picter">-->
<!--&lt;!&ndash;            <img src='./img/register.png'>&ndash;&gt;-->

<!--            <p>上传人像页</p>-->
<!--          </div>-->
<!--          <div class="rz-picter">-->
<!--&lt;!&ndash;            <img src='./img/register.png'>&ndash;&gt;-->
<!--            <p>上传国辉页</p>-->
<!--          </div>-->
<!--          <div class="cuxian" />-->
<!--          <div class="my-content-list">-->
<!--            <div class="color-black"><span style="margin-right: 0.6rem;">真实姓名:</span><input placeholder="请输入您的真实姓名"></div>-->
<!--          </div>-->
<!--          <div class="my-content-list">-->
<!--            <div class="color-black"><span style="margin-right: 0.6rem;">手机号码:</span><input type="number" placeholder="请输入您的手机号码"></div>-->
<!--          </div>-->
<!--          <div class="yzbtn">-->
<!--            立即认证-->
<!--          </div>-->
<!--        </div>-->
<!--      </van-tab>-->
<!--    </van-tabs>-->

<!--  </div>-->
<!--</template>-->

<!--<script>-->

<!--// import { Toast } from 'vant'-->
<!--export default {-->
<!--  name: 'Zhengfeng',-->
<!--  data() {-->
<!--    return {-->
<!--      yoursname: '',-->
<!--      yoursphone: '',-->
<!--      avatar: require('./img/register.png'),-->
<!--      isno: 'block',-->
<!--      active: 0,-->
<!--      navArr: [{-->
<!--        name: '身份认证'-->
<!--      },-->
<!--      {-->
<!--        name: '实名认证'-->
<!--      }-->
<!--      ]-->

<!--    }-->
<!--  },-->
<!--  mounted: function() {-->

<!--  },-->
<!--  methods: {-->
<!--    hiddenwords() {-->
<!--      this.isno = 'none'-->
<!--    },-->
<!--    // submitBtn() {-->
<!--    //   if (this.yoursname === '') {-->
<!--    //     Toast('请填写你的真实姓名')-->
<!--    //     return-->
<!--    //   }-->
<!--    //   if (this.yoursphone === '' || this.yoursphone == null) {-->
<!--    //     Toast('请填写你的手机号码')-->
<!--    //     return-->
<!--    //   }-->
<!--    //-->
<!--    //   if (document.getElementById('uppic').value.length === '') {-->
<!--    //     Toast('请上传图片')-->
<!--    //     return-->
<!--    //   }-->
<!--    // },-->
<!--    changeImage(e) {-->
<!--      var file = e.target.files[0]-->
<!--      var reader = new FileReader()-->
<!--      var that = this-->
<!--      reader.readAsDataURL(file)-->
<!--      reader.onload = function(e) {-->
<!--        that.avatar = this.result-->
<!--      }-->
<!--    }-->

<!--  }-->

<!--}-->
<!--</script>-->

<!--<style>-->
<!--.rz-notice {-->
<!--  padding: 0.2rem 0.3rem;-->
<!--  display: flex;-->
<!--  flex-direction: row;-->
<!--  justify-content: space-between;-->
<!--  align-items: center;-->
<!--  background: #F0F5FB;-->
<!--  color: #28D9EF;-->
<!--}-->

<!--.rz-notice img {-->
<!--  height: 0.22rem;-->
<!--  width: 0.22rem;-->
<!--}-->

<!--.rz-picter {-->
<!--  height: 3rem;-->
<!--  width: 6rem;-->
<!--  margin: 0.3rem auto;-->
<!--  border: 0.01rem solid #ededed;-->
<!--  display: flex;-->
<!--  flex-direction: column;-->
<!--  align-items: center;-->
<!--  justify-content: center;-->
<!--}-->

<!--.uppic {-->
<!--  height: 3rem;-->
<!--  width: 6rem;-->
<!--  margin: 0 auto;-->
<!--  opacity: 0;-->
<!--  z-index: 99999;-->
<!--}-->
<!--.img-avatar {-->
<!--  position: absolute;-->
<!--}-->
<!--/*立即验证*/-->

<!--.yzbtn {-->
<!--  width: 90%;-->
<!--  height: 0.8rem;-->
<!--  background: #FF6600;-->
<!--  border-radius: 0.06rem;-->
<!--  margin: 30px auto;-->
<!--  text-align: center;-->
<!--  line-height: 0.8rem;-->
<!--  color: #FFFFFF-->
<!--}-->

<!--/*修改原有tab样式*/-->

<!--.van-tab {-->
<!--  color: #A3A3A3!important;-->
<!--}-->

<!--.van-tab&#45;&#45;active {-->
<!--  color: #000!important;-->
<!--}-->

<!--.van-tabs__line {-->
<!--  background-color: #FF6600!important;-->
<!--  width: 0.7rem!important;-->
<!--  text-align: center!important;-->
<!--  align-items: center;-->
<!--  margin-left: 1.5rem;-->
<!--}-->

<!--.van-toast {-->
<!--  background-color: #FF6600;-->
<!--  color: #FFFFFF-->
<!--}-->

<!--.my-content-list {-->
<!--  padding: 0.3rem;-->
<!--  display: flex;-->
<!--  flex-direction: row;-->
<!--  justify-content: space-between;-->
<!--  border-bottom: 0.01rem solid #EDEDED;-->
<!--}-->
<!--</style>-->
