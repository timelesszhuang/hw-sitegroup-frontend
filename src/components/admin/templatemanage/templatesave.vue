<template>
  <Modal v-model="modal1" title="修改模板" @on-ok="ok" width="700">
  <div style="font-size: 25px;">当前修改模板{{this.filename}}.html
  </div>
    <Input ref="con" v-model="editContent"  type="textarea" :rows="30" ></Input>
  </Modal>
</template>

<script type="text/ecmascript-6">
  import http from '../../../assets/js/http.js';
  export default {
      data() {
          return {
              modal1:false,
          }
      },
      computed: {
          editContent() {
            return this.content
          }
      },
      methods: {
        ok() {
          this.apiPost('templateSave/'+this.site_id+'/'+this.filename,{content:this.$refs.con.$refs.textarea.value}).then((res) => {
            this.handelResponse(res, (data, msg) => {
              this.$Message.success(msg);
              this.$parent.getInfo();
              this.modal1=false
            }, (data, msg) => {
              this.$Message.error(msg);
            })
          }, (res) => {
            //处理错误信息
            this.$Message.error('网络异常，请稍后重试。');
          });
        }
      },
      props: {
        content: {
          default: {
            type: String
          }
        },
        filename:{
            default:{
                type:String
            }
        },
        site_id:{
            default:{
                type:String
            }
        }
      },
    mixins: [http]
  }
</script>
