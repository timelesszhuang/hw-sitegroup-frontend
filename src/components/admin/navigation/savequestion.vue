<template>
  <div>
    <Modal
      v-model="modal" width="900">
      <p slot="header">
        <span>修改问答型栏目</span>
      </p>
      <div>
        <Form ref="questionadd" :model="form" :label-width="90" :rules="AddRule" class="node-add-form">
          <Form-item label="栏目/菜单名称" prop="name">
            <Input type="text"
                   v-model="form.name" placeholder="请填写栏目/菜单名字"></Input>
          </Form-item>
          <Form-item label="英文名" prop="generate_name">
            <Input type="text" v-model="form.generate_name" placeholder="请填写英文名，用于生成静态页命名"></Input>
          </Form-item>
          <Form-item label="详情" prop="title">
            <Input type="text" v-model="form.title" placeholder="请填写栏目的详情"></Input>
          </Form-item>
          <Form-item label="问答分类" prop="type_id">
            <Select v-model="form.type_id" style="text-align: left;width:200px;"
                    label-in-value 　@on-change="changeQuestiontype">
              <Option v-for="item in questiontype" :value="item.id" :label="item.name" :key="item">
                {{ item.name }}
              </Option>
            </Select>
          </Form-item>
          <Form-item label="分类" prop="tag_name">
            <Select v-model="form.tag_id" style="text-align: left;width:200px;"
                    label-in-value filterable　@on-change="changeNavtype">
              <Option v-for="item in navtype" :value="item.id" :label="item.text" :key="item">
                {{ item.text }}
              </Option>
            </Select>
          </Form-item>
        </Form>
      </div>
      <div slot="footer">
        <Button type="success" size="large" :loading="modal_loading" @click="savequestion">保存</Button>
      </div>
    </Modal>
  </div>
</template>

<script type="text/ecmascript-6">
  import http from '../../../assets/js/http.js';
  export default {
    data() {
      const checkNavtype = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请选择栏目分类'));
        } else {
          callback();
        }
      };
      const checkquestiontype = (rule, value, callback) => {
        if (value === 0) {
          callback(new Error('请选择问答分类'));
        } else {
          callback();
        }
      };
      return {
        editorOption: {},
        modal: false,
        modal_loading: false,
        AddRule: {
          name: [
            {required: true, message: '请填写菜单名字', trigger: 'blur'},
          ],
          title: [
            {required: true, message: '请填写栏目的详情', trigger: 'blur'},
          ],
          type_id: [
            {required: true,validator: checkquestiontype, trigger: 'blur'}
          ],
          generate_name:[
            {required: true, message: '请填写生成的文件名', trigger: 'blur'}
          ],
          tag_name: [
            {required: true,validator: checkNavtype, trigger: 'blur'}
          ],
        }
      }
    },
    methods: {
      changeNavtype(value) {
        this.form.tag_name= value.label
        this.form.tag_id = value.value
      },
      changeQuestiontype(value) {
        this.form.type_name= value.label
        this.form.type_id = value.value
      },
      savequestion() {
        this.$refs.questionadd.validate((valid) => {
          if (valid) {
            this.modal_loading = true;
            let data = this.form;
            let id = data.id;
            this.apiPut('menu/' + id, data).then((res) => {
              this.handelResponse(res, (data, msg) => {
                this.modal = false;
                this.$parent.getData();
                this.$Message.success(msg);
                this.modal_loading = false;
                this.$refs.questionadd.resetFields();
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
        })
      }
    },
    mixins: [http],
    props: {
      questiontype: {
        default: {

        }
      },
      navtype: {
        default: []
      },
      form: {
        default: {
          name: "",
          title: '',
          type_name: ''
        }
      }
    }
  }
</script>
<style>
  .ql-container .ql-editor {
    min-height: 20em;
    padding-bottom: 1em;
    max-height: 25em;
  }
</style>
