<template>
  <div>
    <div class="top">
     企业号关键词管理:
      <Input v-model="name" placeholder="请输入关键词" style="width:300px;"></Input>
      <Select v-model="keyword_typename"  style="width: 200px;" label-in-value filterable clearable >
        <Option v-for="item in wechatKeywordType" :value="item.id" :label="item.text" :key="item">
          {{ item.text }}
        </Option>
      </Select>
      <Button type="primary" @click="queryData">查询</Button>
      <Button type="success" @click="add">添加</Button>
    </div>
    <div class="content" style="margin-top:10px;">
      <Table :context="self" :border="border" :stripe="stripe" :show-header="showheader"
             :size="size" :data="datas" :columns="tableColumns" style="width: 100%">
      </Table>
      <div style="margin: 10px;overflow: hidden">
        <div style="float: right;">
          <Page :total="total" :current="current" @on-change="changePage" @on-page-size-change="changePageSize"
                show-total
                show-ele vator ></Page>
        </div>
      </div>
    </div>
    <wechatkadd ref="add"  :types="wechatKeywordType"></wechatkadd>
    <wechatksave ref="save"  :types="wechatKeywordType" :form="editinfo"></wechatksave>
  </div>
</template>

<script type="text/ecmascript-6">
  import http from '../../../assets/js/http.js';
  import wechatkadd from './add.vue';
  import wechatksave from './save.vue';
  export default {
    data () {
      return {
        self: this,
        border: true,
        stripe: true,
        showheader: true,
        showIndex: true,
        size: 'small',
        current: 1,
        total: 0,
        page: 1,
        rows: 10,
        name: '',
        datas: [],
        editinfo: {},
        keyword_typename:'',
        wechatKeywordType:{},
      }
    },
    components: {wechatkadd,wechatksave},
    created () {
//      this.getData();
      this.getTypes();
    },
    methods: {
      getTypes() {
        this.apiGet('admin/getKeyTypeList').then((data) => {
          this.handelResponse(data, (data, msg) => {
            this.wechatKeywordType=data
          }, (data, msg) => {
            this.$Message.error(msg);
          })
        }, (data) => {
          this.$Message.error('网络异常，请稍后重试');
        })
      },
      getData() {
        let data = {
          params: {
            page: this.page,
            rows: this.rows,
            name: this.name,
            keyword_typeid :this.keyword_typename
          }
        }
        this.apiGet('scrapy/getKeyword', data).then((data) => {
          this.handelResponse(data, (data, msg) => {
            this.datas = data.rows
            this.total = data.total;
          }, (data, msg) => {
            this.$Message.error(msg);
          })
        }, (data) => {
          this.$Message.error('网络异常，请稍后重试');
        })
      },
      changePage(page){
        this.page = page;
        this.getData();
      },
      changePageSize(pagesize){
        this.rows = pagesize;
        this.getData();
      },
      queryData(){
        this.getData();
      },
      add(){
        this.$refs.add.modal = true
      },
      edit(index){
        let editid = this.datas[index].id
        this.apiGet('scrapy/getOneKeyword/' + editid).then((res) => {
          this.handelResponse(res, (data, msg) => {
            this.editinfo = data
            this.modal = false;
            this.$refs.save.modal = true
          }, (data, msg) => {
            this.$Message.error(msg);
          })
        }, (res) => {
          //处理错误信息
          this.$Message.error('网络异常，请稍后重试。');
        })
      },
    },
    computed: {
      tableColumns()
      {
        let columns = [];
        if (this.showCheckbox) {
          columns.push({
            type: 'selection',
            width: 60,
            align: 'center'
          })
        }
        if (this.showIndex) {
          columns.push({
            type: 'index',
            width: 60,
            align: 'center'
          })
        }
        columns.push({
          title: '关键词',
          key: 'name',
          sortable: true
        });
        columns.push({
          title: '关键词所属分类',
          key: 'type_name',
          sortable: true
        });
        columns.push({
          title: '审核状态',
          align: 'center',
          render(row,index){
            if(row.status==10){
              var type = `<Icon type="checkmark-round" style="color:#2db7f5;font-size: 18px"></Icon>`;
              return type;
            }else{
              var type = `<Icon type="close-round" style="color:red;font-size: 18px"></Icon>`;
              return type;
            }

          },
          sortable: true
        });
        columns.push(
          {
            title: '操作',
            key: 'action',
            width: 150,
            align: 'center',
            fixed: 'right',
            render (row, column, index) {
              return `<i-button type="primary" size="small" @click="edit(${index})">修改</i-button>
         `;
            }
          }
        );
        return columns;
      }
    },
    mixins: [http]
  }
</script>
