<template>
  <div id="qiniu-view">
    <el-row :gutter="16">
      <el-col :span="16" :offset="4">
        <div class="view-title">
          七牛图床设置
        </div>
        <el-form 
          ref="qiniu"
          label-position="right"
          label-width="120px"
          :model="form"
          size="mini">
          <el-form-item
            label="设定AccessKey"
            prop="accessKey"
            :rules="{
              required: true, message: 'AccessKey不能为空', trigger: 'blur'
            }">
            <el-input v-model="form.accessKey" placeholder="AccessKey" @keyup.native.enter="confirm('weiboForm')"></el-input>
          </el-form-item>
          <el-form-item
            label="设定SecretKey"
            prop="secretKey"
            :rules="{
              required: true, message: 'SecretKey不能为空', trigger: 'blur'
            }">
            <el-input v-model="form.secretKey" type="password" @keyup.native.enter="confirm" placeholder="SecretKey"></el-input>
          </el-form-item>
          <el-form-item
            label="设定存储空间名"
            prop="bucket"
            :rules="{
              required: true, message: 'Bucket不能为空', trigger: 'blur'
            }">
            <el-input v-model="form.bucket" @keyup.native.enter="confirm" placeholder="Bucket"></el-input>
          </el-form-item>
          <el-form-item
            label="设定访问网址"
            prop="url"
            :rules="{
              required: true, message: '网址不能为空', trigger: 'blur'
            }">
            <el-input v-model="form.url" @keyup.native.enter="confirm" placeholder="例如：http://xxx.yyy.glb.clouddn.com"></el-input>
          </el-form-item>
          <el-form-item
            label="确认存储区域"
            >
            <el-radio-group v-model="form.area" size="mini">
              <el-radio-button label="z0">华东</el-radio-button>
              <el-radio-button label="z1">华北</el-radio-button>
              <el-radio-button label="z2">华南</el-radio-button>
              <el-radio-button label="na0">北美</el-radio-button>
            </el-radio-group>
          </el-form-item>
          <el-form-item
            label="设定网址后缀"
            >
            <el-input v-model="form.options" @keyup.native.enter="confirm" placeholder="例如?imageslim"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="confirm">确定</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import mixin from '../mixin'
export default {
  name: 'qiniu',
  mixins: [mixin],
  data () {
    return {
      form: {
        accessKey: '',
        secretKey: '',
        bucket: '',
        url: '',
        area: 'z0',
        options: ''
      }
    }
  },
  created () {
    const config = this.$db.get('picBed.qiniu').value()
    if (config) {
      for (let i in config) {
        this.form[i] = config[i]
      }
    }
  },
  methods: {
    confirm () {
      this.$refs.qiniu.validate((valid) => {
        if (valid) {
          this.$db.set('picBed.qiniu', this.form).write()
          const successNotification = new window.Notification('设置结果', {
            body: '设置成功'
          })
          successNotification.onclick = () => {
            return true
          }
        } else {
          return false
        }
      })
    }
  }
}
</script>
<style lang='stylus'>
.view-title
  color #eee
  font-size 20px
  text-align center
  margin 20px auto
#qiniu-view
  .el-form
    label  
      line-height 22px
      padding-bottom 0
      color #eee
    .el-button
      width 100%
      border-radius 19px
    .el-input__inner
      border-radius 19px
  .el-radio-group
    width 100%
    label
      width 25%  
    .el-radio-button__inner
      width 100%
  .el-radio-button:first-child
    .el-radio-button__inner
      border-left none
      border-radius 14px 0 0 14px
  .el-radio-button:last-child
    .el-radio-button__inner
      border-left none
      border-radius 0 14px 14px 0
</style>