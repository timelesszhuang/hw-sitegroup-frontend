<template>
  <div>
    <div>
      <Modal
        v-model="modal" width="900">
        <p slot="header">
          <span>添加站点</span>
        </p>
        <div>
          <Form ref="site" :model="form" :label-width="90" :rules="AddRule" class="node-add-form">
            <Form-item label="名称" prop="site_name">
              <Input type="text" v-model="form.site_name" placeholder="请输入名称"></Input>
            </Form-item>
            <Form-item label="公司名" prop="com_name">
              <Input type="text" v-model="form.com_name" placeholder="请输入公司名,关键词配置"></Input>
            </Form-item>
            <Form-item label="用户选择" prop="user_id">
              <Select v-model="form.user_id" style="text-align: left;width:200px;"
                      label-in-value filterable　@on-change="changeUser">
                <Option v-for="item in userlist" :value="item.id" :label="item.text" :key="item">
                  {{ item.text }}
                </Option>
              </Select>
            </Form-item>
            <Form-item label="网站应用" prop="is_mobile">
              <Radio-group v-model="form.is_mobile">
                <Radio label=10>
                  <Icon type="social-windows"></Icon>
                  <span>PC机</span>
                </Radio>
                <Radio label=20>
                  <Icon type="android-phone-portrait"></Icon>
                  <span>手机</span>
                </Radio>
              </Radio-group>
            </Form-item>
            <Form-item label="手机网站" prop="m_site_id">
              <Select  clearable v-model="form.m_site_id" style="text-align: left;width:200px;"
                      label-in-value filterable>
                <Option v-for="item in mobileSite" :value="item.id" :label="item.text" :key="item">
                  {{ item.text }}
                </Option>
              </Select>
            </Form-item>

            <Form-item label="关键词" prop="keyword_ids">
              <Select v-model="form.keyword_ids" multiple style="text-align: left;width:200px;">
                <Option v-for="item in keyword" :value="item.id" :label="item.label" :key="item">
                  {{ item.label }}
                </Option>
              </Select>
            </Form-item>
            <Form-item label="栏目" prop="menu">
              <Select filterable v-model="form.menu" multiple style="text-align: left;width:350px;">
                <Option disabled :value="0"><span>栏目名—栏目分类—栏目类型—所属文章分类—详情</span></Option>
                <Option v-for="item in menutype" :value="item.id" :label="item.text" :key="item">
                  {{ item.text}}—{{item.tag_name}}—{{item.flag_name}}
                  <span v-if="item.type_name ==''">{{item.type_name}}</span>
                  <span v-else>{{item.typeName}}</span>—{{item.title}}
                </Option>

              </Select>
            </Form-item>
            <Form-item label="模板" prop="template_id">
              <Select v-model="form.template_id" style="text-align: left;width:200px;"
                      label-in-value filterable　@on-change="changeTemptype">
                <Option v-for="item in temptype" :value="item.id" :label="item.text" :key="item">
                  {{ item.text }}
                </Option>
              </Select>
            </Form-item>
            <Form-item label="联系方式" prop="support_hotline">
              <Select v-model="form.support_hotline" style="text-align: left;width:200px;"
                      label-in-value filterable　@on-change="changeHotline">
                <Option v-for="item in hotline" :value="item.id" :label="item.text" :key="item">
                  {{ item.text }}
                </Option>
              </Select>
            </Form-item>
            <Form-item label="网站分类" prop="site_type">
              <Select v-model="form.site_type" style="text-align: left;width:200px;"
                      label-in-value filterable on-blur=""　@on-change="changeSitetype">
                <Option v-for="item in sitetype" :value="item.id" :label="item.text" :key="item">
                  {{ item.text }}
                </Option>
              </Select>
            </Form-item>
            <Form-item label="域名选择" prop="domain_id">
              <Select v-model="form.domain_id" style="text-align: left;width:200px;"
                      label-in-value filterable　@on-change="changeDomainlist">
                <Option v-for="item in domainlist" :value="item.id" :label="item.text" :key="item">
                  {{ item.text }}
                </Option>
              </Select>
            </Form-item>
            <Form-item label="友链选择" prop="link_id">
              <Select v-model="form.link_id" multiple style="text-align: left;width:200px;" 　@on-change="changeLink">
                <Option v-for="item in link" :value="item.id" :label="item.text" :key="item">
                  {{ item.text }}
                </Option>
              </Select>
            </Form-item>
            <Form-item label="url" prop="url">
                <Input v-model="form.url" placeholder="请输入url"></Input>
            </Form-item>
            <Form-item label="公共代码" prop="public_code">
              <Select v-model="form.public_code" multiple style="text-align: left;width:200px;">
                <Option v-for="item in code" :value="item.id" :label="item.text" :key="item">
                  {{ item.text }}
                </Option>
              </Select>
            </Form-item>
            <Form-item label="图片水印文字" prop="walterString">
              <Input type="text" v-model="form.walterString" placeholder="请输入图片水印文字"></Input>
            </Form-item>
            <Form-item label="head前代码" prop="before_header_jscode">
              <Input v-model="form.before_header_jscode" type="textarea" :rows="3"
                     placeholder="请输入head前代码">
              </Input>
            </Form-item>
            <Form-item label="head后代码" prop="other_jscode">
              <Input v-model="form.other_jscode" type="textarea" :rows="3"
                     placeholder="请输入head后代码">
              </Input>
            </Form-item>

          </Form>
        </div>
        <div slot="footer">
          <Button type="success" size="large" :loading="modal_loading" @click="add">保存</Button>
        </div>
      </Modal>
    </div>
  </div>

</template>

<script type="text/ecmascript-6">
  import http from '../../../assets/js/http.js';
  export default {
    data() {
      const checkmenutype = (rule, value, callback) => {
        if (value=="") {
          callback(new Error('请选择栏目分类'));
        } else {
          callback();
        }
      };
      const checkkeyword = (rule, value, callback) => {
        if (value=="") {
          callback(new Error('请选择关键词'));
        } else if(value.length>5){
          callback(new Error('关键词不能超过5个'));
        }else {
          callback();
        }
      };
      const checktemptype = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请选择模板分类'));
        } else {
          callback();
        }
      };
      const checkhotlinetype = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请选择联系方式'));
        } else {
          callback();
        }
      };
      const checksitetype = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请选择网站分类'));
        } else {
          callback();
        }
      };
      const checkdomain = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请选择域名'));
        } else {
          callback();
        }
      };
      const checkuser = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请选择用户'));
        } else {
          callback();
        }
      };

      return {
        editorOption: {},
        modal: false,
        modal_loading: false,
        form: {
          com_name:"",
          site_name: "",
          menu: [],
          template_id:"",
          support_hotline:"",
          site_type:"",
          domain_id:"",
          before_header_jscode:"",
          other_jscode:"",
          keyword_ids:[],
          link_id:[],
          public_code:[],
          user_id:"",
          url:'',
          is_mobile:10,
          m_site_id:0,
          walterString:''
        },
        AddRule: {
          site_name: [
            {required: true, message: '请输入名称', trigger: 'blur'},
          ],
          com_name: [
            {required: true, message: '请输入公司名', trigger: 'blur'},
          ],
          menu: [
            {required: true,validator: checkmenutype, trigger: 'blur'},
          ],
          template_id: [
            {required: true,validator: checktemptype, trigger: 'blur'},
          ],
          support_hotline: [
            {required: true,validator: checkhotlinetype, trigger: 'blur'},
          ],
          site_type: [
            {required: true,validator: checksitetype, trigger: 'blur'},
          ],
          domain_id: [
            {required: true,validator: checkdomain, trigger: 'blur'},
          ],
          user_id: [
            {required: true,validator: checkuser, trigger: 'blur'},
          ],
          keyword_ids:[
            {required: true,validator: checkkeyword, trigger: 'blur'},
          ],
          url:[
            {required: true, message: '请输入url', trigger: 'blue'}
          ]
        }
      }
    },
    methods: {
      computed: {
        editor() {
          return this.$refs.myTextEditor.quillEditor
        }
      },
      changeLink(){

      },
      changeUser(value){
        this.form.user_name = value.label
        this.form.user_id = value.value
      },
      changeHotline(value) {
        this.form.support_hotline = value.value
      },
      changeSitetype(value) {
        this.form.site_type = value.value
        this.form.site_type_name = value.label
      },
      changeTemptype(value) {
        this.form.template_id = value.value
      },
      changeDomainlist(value) {
        this.form.domain = value.label
        this.form.domain_id = value.value
      },
      add() {
        this.$refs.site.validate((valid) => {
          if (valid) {
            this.modal_loading = true;
            if(!this.form.walterString){
              this.form.walterString=this.form.site_name
            }
            let data = this.form;
            this.apiPost('site', data).then((res) => {
              this.handelResponse(res, (data, msg) => {
                this.modal = false;
                this.$parent.getData();
                this.$Message.success(msg);
                this.modal_loading = false;
                this.$refs.site.resetFields();
              }, (data, msg) => {
                this.modal_loading = false;
                this.$Message.error(msg,5);
              })
            }, (res) => {
              //处理错误信息
              this.modal_loading = false;
              this.$Message.error('网络异常，请稍后重试。');
            })
          }
        })
      }
  }
  ,
  mixins: [http],
    props
  :
  {
    code: {
      default:[]
    },
    menutype: {
    default:
      []
    }
  ,
    temptype: {
    default:
      []
    }
  ,
    sitetype: {
    default:
      []
    }
  ,
    hotline: {
    default:
      []
    }
  ,
    domainlist:{
    default:
      []
    },
    userlist:{
      default:
        []
    },
    keyword:{
      default:
        []
    },
    link:{
      default:
        []
    },
    mobileSite:
      {}
   }
  }
</script>
