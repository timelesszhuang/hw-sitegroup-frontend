<style scoped>
  .layout {
    border: 1px solid #d7dde4;
    background: #f5f7f9;
    position: relative;
    height: 100%;
  }

  .layout-logo-left {
    width: 100%;
    background-color: #ffffff;
    height: 50px;
  }

  .logo {
    width: 150px;
    display: block;
    padding: 10px;
    margin-left: 10%;
  }

  .layout-breadcrumb {
    padding-left: 15px;
  }

  .layout-content {
    min-height: 63%;
    margin: 15px;
    background: #fff;
    border-radius: 4px;
    flex: 1;
  }

  .layout-content-main {
    padding: 10px;
  }

  .ivu-menu-dark {
    background: #009ca2;
    color: #ffffff;
  }

  .ivu-row-flex {
    height: 100%;
  }

  /**图标颜色改为 白色*/
  .ivu-menu .ivu-icon {
    color: #ffffff;
  }

  .ivu-menu-vertical .ivu-menu-submenu-title-icon {
    color: #ffffff;
  }

  /** 左侧全部都是空*/
  .layout-menu-left {
    background: #009ca2;
  }

  /*字体改为白色*/
  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item, .ivu-menu-dark.ivu-menu-vertical .ivu-menu-submenu-title {
    color: #ffffff;
  }

  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-opened .ivu-menu-submenu-title {
    background: #ffffff;
  }

  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-submenu {
    background: #16b8be !important;
  }

  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-submenu .ivu-menu-item-active, .ivu-menu-dark.ivu-menu-vertical .ivu-menu-submenu .ivu-menu-item-active:hover {
    border-right: none;
    color: #fff;
    background-color: #009ca2 !important;
  }

  .layout-header {
    width: 84%;
    float: right;
    z-index: 999;
    position: fixed;
    left: 16.7%;
    height: 50px;
    background: #fff;
  }

  /**底部版权信息*/
  .layout-copy {
    position: fixed;
    text-align: center;
    padding: 5px 0 10px;
    bottom: 0px;
    height: 40px;
    width: 84%;
    background-color: #fff;
  }


  .layout-content-main {
    height: 100%;
  }

  .ivu-breadcrumb {
    color: #999;
    font-size: 14px;
    padding-top: 5%;
  }

  .layout-hide-text, .layout-text {
    display: inline-block;
    width: 100%;
  }

  .parent-menu-title {
    color: #ffffff !important;
  }


</style>

<template>
  <div class="layout" :class="{'layout-hide-text': spanLeft < 4}" @click="menuClick" ref="menuClickEle">
    <Row type="flex">
      <i-col :span="spanLeft" class="layout-menu-left">
        <Menu active-name="activename" theme="dark" style="overflow-x:visible " width="auto"
              :open-names="opennames" accordion>
          <div class="layout-logo-left" @click="routerChange('/admin/index')">
            <img class="logo" src="../../src/assets/img/logo.png" alt="乐销易创意发布平台">
          </div>
          <Submenu name="1">
            <template slot="title">
              <Icon type="ios-navigate"></Icon>
              <span class="parent-menu-title">站点关键词</span>
            </template>
            <Menu-item name="关键词管理">
              <span class="layout-text" @click="routerChange('/admin/keyword','关键词管理')">
                <Icon type="key"></Icon>
                关键词管理
              </span>
            </Menu-item>
            <Menu-item name="关键词推荐">
              <span class="layout-text" @click="routerChange('/admin/recommendationkeyword','关键词推荐')">
                <Icon type="ios-list-outline"></Icon>
                关键词推荐
              </span>
            </Menu-item>
          </Submenu>
          <Submenu name="8">
            <template slot="title">
              <Icon type="ios-bookmarks-outline"></Icon>
              <span class="parent-menu-title">营销计划</span>
            </template>
            <Menu-item name="主题/活动创意">
              <span class="layout-text" @click="routerChange('/admin/activity','主题/活动创意')">
                <Icon type="beer"></Icon>
                主题/活动创意管理
              </span>
            </Menu-item>
            <Menu-item name="营销模式">
              <span class="layout-text" @click="routerChange('/admin/marketingmodel','主题/营销模式')">
                <Icon type="flag"></Icon>
                 营销模式
              </span>
            </Menu-item>
            <Menu-item name="事件营销">
              <span class="layout-text" @click="routerChange('/admin/eventmarketholiday','事件营销')">
                <Icon type="alert"></Icon>
                &nbsp;&nbsp;事件营销
              </span>
            </Menu-item>
            <Menu-item name="案例中心">
              <span class="layout-text" @click="routerChange('/admin/casecenter','案例中心')">
               <Icon type="ionic"></Icon>
               案例中心
              </span>
            </Menu-item>
          </Submenu>
          <Submenu name="8">
            <template slot="title">
              <Icon type="ios-box"></Icon>
              <span class="parent-menu-title">资源聚合</span>
            </template>
            <Menu-item name="新闻管理">
              <span class="layout-text" @click="routerChange('/admin/hotnews','新闻管理')">
                <Icon type="social-designernews"></Icon>
                新闻管理
              </span>
            </Menu-item>
            <Menu-item name="公众号">
              <span class="layout-text" @click="routerChange('/admin/wechat','公众号')">
                <Icon type="cube"></Icon>
                公众号
              </span>
            </Menu-item>
            <Menu-item name="软文发布">
              <span class="layout-text" @click="ruanwenClick">
                <Icon type="wand"></Icon>
                软文发布
              </span>
            </Menu-item>
            <Menu-item name="视频资源">
              <span class="layout-text" @click="ruanwenClick">
                <Icon type="videocamera"></Icon>
                视频资源
              </span>
            </Menu-item>
          </Submenu>
          <Submenu name="3">
            <template slot="title">
              <Icon type="ios-keypad"></Icon>
              <span class="parent-menu-title">内容管理</span>
            </template>
            <Menu-item name="文章管理">
              <span class="layout-text" @click="routerChange('/admin/articlemanage','文章管理')">
                <Icon type="document"></Icon>
                文章管理
              </span>
            </Menu-item>
            <Menu-item name="问答管理">
              <span class="layout-text" @click="routerChange('/admin/questionmanage','问答管理')">
                <Icon type="ios-help-empty"></Icon>
                 问答管理
              </span>
            </Menu-item>
            <Menu-item name="产品管理">
              <span class="layout-text" @click="routerChange('/admin/productmanage','产品管理')">
                <Icon type="ios-grid-view"></Icon>
                产品管理
              </span>
            </Menu-item>
            <Menu-item name="文章段落管理">
              <span class="layout-text" @click="routerChange('/admin/newsmanage','文章段落管理')">
                <Icon type="social-buffer-outline"></Icon>
                文章段落管理
              </span>
            </Menu-item>
          </Submenu>

          <Submenu name="5">
            <template slot="title">
              <span class="parent-menu-title">
                <Icon type="briefcase"></Icon>
                站点设置
              </span>
            </template>
            <Menu-item name="栏目管理">
              <span class="layout-text" @click="routerChange('/admin/nav','栏目管理')">
                <Icon type="drag"></Icon>
                栏目管理
              </span>
            </Menu-item>
            <Menu-item name="网站元素设置">
              <span class="layout-text" @click="routerChange('/admin/siteelement','网站元素设置')">
                <Icon type="settings"></Icon>
                  网站元素设置
              </span>
            </Menu-item>

            <Menu-item name="站点管理">
              <span class="layout-text" @click="routerChange('/admin/sitemanage','站点管理')">
                <Icon type="android-cloud-circle"></Icon>
                站点管理
              </span>
            </Menu-item>

            <Menu-item name="模板管理">
              <span class="layout-text" @click="routerChange('/admin/templatemanage','模板管理')">
                <Icon type="ios-browsers"></Icon>
                模板管理
              </span>
            </Menu-item>
            <Menu-item name="SEO属性">
              <span class="layout-text" @click="routerChange('/admin/seo','SEO属性')">
                <Icon type="search"></Icon>
                搜索引擎优化管理
              </span>
            </Menu-item>
            <Menu-item name="站点静态化配置">
              <span class="layout-text" @click="routerChange('/admin/staticconfig','站点静态化配置')">
                <Icon type="android-cloud-circle"></Icon>
                站点静态化配置
              </span>
            </Menu-item>
            <Menu-item name="用户管理">
              <span class="layout-text" @click="routerChange('/admin/siteuser','用户管理')">
                <Icon type="person"></Icon>
                用户管理
              </span>
            </Menu-item>
          </Submenu>
          <Submenu name="7">
            <template slot="title">
              <span class="parent-menu-title">
                <Icon type="ios-analytics"></Icon>
                市场反馈
              </span>
            </template>
            <Menu-item name="主关键词排名">
              <span class="layout-text" @click="routerChange('/admin/mainkeyword','主关键词排名')">
                <Icon type="android-archive"></Icon>
                位置排名
              </span>
            </Menu-item>
            <Menu-item name="线索">
              <span class="layout-text" @click="routerChange('/admin/rejection','线索')">
                <Icon type="android-cloud-circle"></Icon>
                线索
              </span>
            </Menu-item>
            <Menu-item name="流量分析">
              <span class="layout-text" @click="routerChange('/admin/pageview','流量分析')">
                <Icon type="android-cloud-circle"></Icon>
                流量分析
              </span>
            </Menu-item>
          </Submenu>
        </Menu>
      </i-col>
      <i-col :span="spanRight" style="overflow-y:scroll;padding-bottom:50px">
        <div class="layout-header">
          <Row type="flex" justify="space-around" align="middle" class="code-row-bg">
            <Col span="2" align="right" style="cursor: pointer">
            <span class="layout-text" @click="routerChange('/admin/index')">
                <Icon type="home" style="font-size: 15px"></Icon>
                首页
            </span>
            </Col>
            <Col span="6" align="right" offset="8" style="cursor: pointer">
            <span class="layout-text" style="font-weight: bolder;">
                {{siteTitle}}
            </span>
            </Col>
            <Col span="0.5"  align="center">
            <Badge :count="systemcount">
              <span @click="routerChange('/admin/systemp','推送消息')" title="系统推送消息" style="cursor:pointer;">
                <Icon type="ios-email-outline" size="26"></Icon>
              </span>
            </Badge>
            </Col>
            <Col span="1" align="right">
            <Badge :count="count">
              <span @click="routerChange('/admin/messageLog','消息')" title="错误信息" style="cursor:pointer;">
                <Icon type="ios-bell-outline" size="26"></Icon>
              </span>
            </Badge>
            </Col>
            <Col span="2" align="center" style="cursor: pointer">
            <span class="layout-text" @click="changePwd()">
              <Icon type="android-lock"></Icon>
              重置密码
            </span>
            </Col>
            <Col span="2" style="cursor: pointer">
            <span class="layout-text" @click="logOut()">
              <Icon type="log-out"></Icon>
              退出系统
            </span>
            </Col>
          </Row>
        </div>
        <div class="layout-breadcrumb">
          <Breadcrumb style="padding-top: 4%;">
            <Breadcrumb-item>{{activeName}}</Breadcrumb-item>
          </Breadcrumb>
        </div>
        <div>
          <div class="layout-content">
            <div class="layout-content-main">
              <router-view></router-view>
            </div>
          </div>
          <div class="layout-copy">
            2015-2017 &copy; 北京易至信科技有限公司
            <div>京ICP12019481号</div>
          </div>
        </div>
      </i-col>
    </Row>
    <logout ref="logout"></logout>
    <changepwd ref="changePwd"></changepwd>
    <cue ref="cueclick"></cue>
  </div>
</template>
<script>
  import logout from './Account/Logout.vue';
  import changepwd from './Account/Changepwd.vue';
  import cue from './Account/cue.vue';
  import http from '../assets/js/http.js';

  export default {
    data() {
      return {
        spanLeft: 4,
        spanRight: 20,
        activeName: '',
        sysname: '',
        count: '无',
        systemcount: '无',
        opennames: ['1'],
        siteTitle: '未获取到'
      }
    },
    components: {
      changepwd,
      logout,
      cue
    },
    methods: {
      menuClick(e) {

      },
      ruanwenClick() {
        this.$refs.cueclick.modal = true
      },

      checkAlert() {
        this.apiGet('article/getErrorStatus').then((res) => {
          this.handelResponse(res, (data, msg) => {
            this.count = data;
          }, (data, msg) => {
//            this.$Message.error(msg);
          })
        }, (res) => {
          //处理错误信息
          this.$Message.error('网络异常，请稍后重试。');
        })
      },
      checkAlertstsyem() {
        this.apiGet('admin/systemNotice/create').then((res) => {
          this.handelResponse(res, (data, msg) => {
            this.systemcount = data;
            if (this.systemcount == 0) {
              this.systemcount = "无"
            }
          }, (data, msg) => {
//            this.$Message.error(msg);
          })
        }, (res) => {
          //处理错误信息
          this.$Message.error('网络异常，请稍后重试。');
        })
      },
      routerChange(path, activeName) {
        this.activeName = activeName;
        router.push(path);
      },
      changePwd() {
        this.$refs.changePwd.modal = true
      },
      logOut() {
        this.$refs.logout.modal = true
      },

      messageLog() {
        router.replace('/admin/messageLog');
      }
    },
    //created 是函数
    created() {
      let _this = this;
      _this.checkAlert();
      _this.checkAlertstsyem();
      setInterval(function () {
        _this.checkAlert();
        _this.checkAlertstsyem();
      }, 120000);
//      console.log(Lockr.get('userInfo'));
      if (!Lockr.get('userInfo')) {
        this.$Message.error("请先登录");
        //表示没有登陆
        setTimeout(() => {
          router.replace('/')
        }, 1500)
        return
      }
      this.sysname = Lockr.get('userInfo').node_name;
      this.siteTitle =document.title = this.sysname
      let rememberKey = Lockr.get('rememberKey')
      let user_id = Lockr.get('user_id')
      let type = Lockr.get('type');
    },
    mixins: [http]
  }
</script>
