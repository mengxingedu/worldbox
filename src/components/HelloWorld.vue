<template>
  <div class="activeConfig">
    <div class="activeConfig-container">
      <Editor id="tinymce" v-model="tinymceHtml" :init="editorInit" />
    </div>
  </div>
</template>

<script>
// 引入组件
import tinymce from 'tinymce/tinymce'
import Editor from '@tinymce/tinymce-vue'
// 引入富文本编辑器主题的js和css
import 'tinymce/themes/silver/theme.min.js'
import 'tinymce/skins/ui/oxide/skin.min.css'
// 扩展插件
import 'tinymce/plugins/image'
import 'tinymce/plugins/link'
import 'tinymce/plugins/code'
import 'tinymce/plugins/table'
import 'tinymce/plugins/lists'
import 'tinymce/plugins/wordcount'
export default {
  name: 'ActiveConfig',
  components: { Editor },
  data() {
    return {
      // tinymce的绑定值
      tinymceHtml: '',
      // tinymce的初始化配置
      editorInit: {
        selector: '#tinymce',
        language_url: '/tinymce/langs/zh_CN.js',
        language: 'zh_CN',
        skin_url: '/tinymce/skins/ui/oxide',
        height: 400,
        plugins: 'link lists image code table wordcount importword',
        toolbar: 'bold italic underline strikethrough | fontsizeselect | forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist | outdent indent blockquote | undo redo | link unlink image code | removeformat | importword',
        // 此处为图片上传处理函数
        importword_handler: (editor,files,next) => {
          // this.handleImgUpload(blobInfo, success, failure)
          console.log(editor,files,next)
        },
        images_upload_handler: (blobInfo, success) => {
          const img = 'data:image/jpeg;base64,' + blobInfo.base64()
          success(img)
        },
        importword_filter: function(result, insert, message) {
          console.log(result)
          console.log(insert)
          console.log(message)
          // 自定义操作部分
          insert(result) // 回插函数
        },
        // statusbar: false // 是否隐藏底部的状态栏
        // menubar: false, // 是否隐藏最上方的菜单
        branding: false // 是否禁用“Powered by TinyMCE”
      }
    }
  },
  mounted() {
    tinymce.init({})
  },
  methods: {
    // 图片上传
    handleImgUpload(blobInfo, success, failure) {
      console.log(blobInfo, success, failure)
    }
  }
}
</script>
<!--
<style lang="scss" scoped>
.activeConfig {
  &-container {
    margin: 30px;
  }
}
</style>
-->