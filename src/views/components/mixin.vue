<template>
  <div class="components-container">
    <div class='component-item'>
      <el-form :model="demo" :rules="demoRules">
        <el-form-item prop="title">
          <md-input icon="search" name="title" placeholder="输入标题" v-model="demo.title">标题</md-input>
        </el-form-item>
      </el-form>
      <code class='code-part'>Material Design 的input</code>
    </div>

    <div class='component-item'>
      <pan-thumb image='https://wpimg.wallstcn.com/577965b9-bb9e-4e02-9f0c-095b41417191'>
        上海花裤衩
      </pan-thumb>
      <code class='code-part'>图片hover效果</code>
    </div>

    <div class='component-item'>
      <el-button v-waves type="primary">水波纹效果</el-button>
      <code class='code-part'>水波纹 v-directive</code>
    </div>
  </div>
</template>

<script>
<<<<<<< HEAD
import MdInput from '@/components/MDinput'
import PanThumb from '@/components/PanThumb'
import waves from '@/directive/waves.js' // 水波纹指令

export default {
  components: { MdInput, PanThumb },
=======
import PanThumb from '@/components/PanThumb'
import MdInput from '@/components/MDinput'
import waves from '@/directive/waves/index.js' // 水波纹指令

export default {
  components: {
    PanThumb,
    MdInput
  },
>>>>>>> c769c5cd6980d39bd192a39120999e832790ada1
  directives: {
    waves
  },
  data() {
<<<<<<< HEAD
    return {
      title: ''
=======
    const validate = (rule, value, callback) => {
      if (value.length !== 6) {
        callback(new Error('请输入六个字符'))
      } else {
        callback()
      }
    }
    return {
      demo: {
        title: ''
      },
      demoRules: {
        title: [{ required: true, trigger: 'change', validator: validate }]
      }
>>>>>>> c769c5cd6980d39bd192a39120999e832790ada1
    }
  }
}
</script>

<style scoped>
.component-item{
  margin-top: 100px;
}
.code-part{
  margin-top: 20px;
}
</style>
