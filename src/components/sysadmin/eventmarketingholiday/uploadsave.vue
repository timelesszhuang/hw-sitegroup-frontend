<template>
  <div>
    <div>
      <Modal
        v-model="modal" width="900">
        <p slot="header">
          <span>修改模板</span>
        </p>
        <div>
          <div style="width: 300px;margin: 20px auto 20px">
            <div style="width: 300px;margin: 20px auto 20px;text-align: center">
              <a :href=downloadPath() target="_blank">点击下载模板</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
              <a  :href=viewPath() target="_blank">点击预览模板</a>
            </div>
            <Input v-model="form.template_name" placeholder="请输入模板名称" style="width: 300px"></Input>

          </div>
          <Upload
            width="800"
            type="drag"
            ref="upImg"
            with-credentials
            name="img_name"
            :format="['jpg','jpeg','png','gif']"
            :on-success="getResponse"
            :on-error="getErrorInfo"
            :on-format-error="formatError"
            :action="action">
            <div style="padding: 20px 0">
              <Icon type="ios-cloud-upload" size="52" style="color: #3399ff"></Icon>
              <p>修改上传图片 仅支持(jpg jpeg png gif)类型图片</p>
            </div>
          </Upload>
          <Upload
            type="drag"
            ref="uploadzip"
            with-credentials
            name="file_name"
            :format="['zip']"
            :on-success="getRespons"
            :on-error="getErrorInf"
            :on-format-error="formatErro"
            :action="actionurl">
            <div style="padding: 20px 0">
              <Icon type="ios-cloud-upload" size="52" style="color: #3399ff"></Icon>
              <p>修改模板zip文件</p>
            </div>
          </Upload>

        </div>
        <div slot="footer">
          <Button type="success" size="large" :loading="modal_loading" @click="uploadsave">保存</Button>
        </div>
      </Modal>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import http from '../../../assets/js/http.js';

  export default {
    data() {
      return {
        modal: false,
        modal_loading: false,
        action: HOST + 'sys/uploadHtmlTemplateImg',
        actionurl: HOST + 'sys/uploadHtmlTemplate',
        pathform: {

        },
        AddRule: {}
      }
    },
    props: {
      form: {
        default: Object
      }
    },
    methods: {
      downloadPath() {
        return ROOTHOST + this.form.path;
      },
      viewPath () {
        return ROOTHOST +"upload/"+this.form.generated_path;

      },
      getResponse(response, file, filelist) {
        this.img = response.data;
        this.$Message.success(response.msg);
      },
      getRespons(respons, file, filelist) {
        this.path = respons.data;
        this.$Message.success(respons.msg);
      },
      getErrorInfo(error, file, filelist) {
        this.$Message.error(error);
      },
      getErrorInf(error, file, filelist) {
        this.$Message.error(error);
      },
      formatError() {
        this.$Message.error('文件格式只支持 jpg,jpeg,png三种格式。');
      },
      formatErro() {
        this.$Message.error('文件格式只支持 zip格式。');
      },
      uploadsave() {

        this.modal_loading = true;
        this.pathform.img = this.img
        this.pathform.path = this.path
        this.pathform.holiday_name = this.form.name
        this.pathform.template_name = this.form.template_name
        this.pathform.id = this.form.id;
        let data = this.pathform;
        let id =  this.form.id;
        this.apiPut('sys/HtmlTemplate/'+ id, data).then((res) => {
          this.handelResponse(res, (data, msg) => {
            this.modal = false;
            this.$parent.getData();
            this.$Message.success(msg);
            this.modal_loading = false;
            this.$refs.upImg.clearFiles()
            this.$refs.uploadzip.clearFiles()
          }, (data, msg) => {
            this.modal_loading = false;
            this.$Message.error(msg);
          })
        }, (res) => {
          //处理错误信息
          this.modal_loading = false;
          this.$Message.error('网络异常，请稍后重试。');
        })
      }
    },

    mixins: [http],
  }
</script>


