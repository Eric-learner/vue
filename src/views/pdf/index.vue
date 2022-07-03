<!--<template>-->
<!--  <div class="app-container">-->
<!--    <aside style="margin-top:15px;">-->
<!--      Here we use window.print() to implement the feature of downloading .-->
<!--    </aside>-->
<!--    <router-link target="_blank" to="/pdf/download">-->
<!--      <el-button type="primary">-->
<!--        Click to download PDF-->
<!--      </el-button>-->
<!--    </router-link>-->
<!--  </div>-->
<!--</template>-->

<!--<template>-->
<!--  <el-upload-->
<!--    class="upload-demo"-->
<!--    action="https://jsonplaceholder.typicode.com/posts/"-->
<!--    :on-preview="handlePreview"-->
<!--    :on-remove="handleRemove"-->
<!--    :file-list="fileList"-->
<!--    list-type="picture">-->
<!--    <el-button size="small" type="primary">点击上传</el-button>-->
<!--    <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>-->
<!--  </el-upload>-->
<!--</template>-->

<!--<script>-->
<!--export default {-->
<!--  data() {-->
<!--    return {-->
<!--      fileList: [{name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}, {name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}]-->
<!--    };-->
<!--  },-->
<!--  methods: {-->
<!--    handleRemove(file, fileList) {-->
<!--      console.log(file, fileList);-->
<!--    },-->
<!--    handlePreview(file) {-->
<!--      console.log(file);-->
<!--    }-->
<!--  }-->
<!--}-->
<!--</script>-->

<!--<template>-->
<!--  <el-upload-->
<!--    ref="upload"-->
<!--    action="#"-->
<!--    list-type="picture-card"-->
<!--    :auto-upload="false"-->
<!--  >-->
<!--    <i slot="default" class="el-icon-plus" />-->
<!--    <div slot="file" slot-scope="{file}">-->
<!--      <img class="el-upload-list__item-thumbnail" :src="file.url">-->
<!--      <span class="el-upload-list__item-actions">-->
<!--        <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)">-->
<!--          <i class="el-icon-zoom-in" />-->
<!--        </span>-->
<!--        <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleDownload(file)">-->
<!--          <i class="el-icon-download" />-->
<!--        </span>-->
<!--        <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleRemove(file)">-->
<!--          <i class="el-icon-delete" />-->
<!--        </span>-->
<!--      </span>-->
<!--    </div>-->
<!--  </el-upload>-->

<!--</template>-->

<!--<script>-->
<!--export default {-->
<!--  data() {-->
<!--    return {-->
<!--      fileList: [{ name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100' }, { name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100' }]-->
<!--    }-->
<!--  },-->
<!--  methods: {-->
<!--    // 图片删除-->
<!--    // 图片删除-->
<!--    handleRemove(file, fileList) {-->
<!--      this.$refs.upload.handleRemove(file)-->
<!--      // this.$refs.upload.clearFiles()  //删除全部-->
<!--      // 1.获取将要删除图片的临时路径-->
<!--      const url = file.url-->
<!--      // 2.从pics数组中，找到图片对应的索引值-->
<!--      const i = this.fileList.findIndex(x => x.url === url)-->
<!--      // 3.调用splice方法，移除图片信息-->
<!--      this.fileList.splice(i, 1)-->
<!--    },-->
<!--    // 图片预览-->
<!--    handlePictureCardPreview(file) {-->
<!--      this.dialogImageUrl = file.url-->
<!--      this.dialogVisible = true-->
<!--    },-->
<!--    // 图片下载-->
<!--    handleDownload(file) {-->
<!--      var a = document.createElement('a')-->
<!--      var event = new MouseEvent('click')-->
<!--      a.download = file.name-->
<!--      a.href = file.url-->
<!--      a.dispatchEvent(event)-->
<!--    }-->

<!--  }-->
<!--}-->
<!--</script>-->

<!--下面是原版-->
<template>
  <el-upload
    class="upload-demo"
    action="https://jsonplaceholder.typicode.com/posts/"
    :on-preview="handlePreview"
    :on-remove="handleRemove"
    :file-list="fileList"
    list-type="picture">
    <el-button size="big" type="primary">点击上传</el-button>
    <div slot="tip" class="el-upload__tip">只能上传jpg/png文件</div>
    <i slot="default" class="el-icon-plus" />
    <div slot="file" slot-scope="{file}">
      <img class="el-upload-list__item-thumbnail" :src="file.url">
      <span class="el-upload-list__item-actions">
        <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)">
          <i class="el-icon-zoom-in" />
        </span>
        <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleDownload(file)">
          <i class="el-icon-download" />
        </span>
        <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleRemove(file)">
          <i class="el-icon-delete" />
        </span>
      </span>
    </div>
  </el-upload>

</template>

<script>
export default {
  data() {
    return {
      fileList: [{name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}, {name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}]
    };
  },
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    }
  }
}
</script>

