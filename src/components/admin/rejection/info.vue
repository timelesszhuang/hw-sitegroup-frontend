<template>
  <Modal v-model="modal" width="460">
    <p slot="header" style="color:dodgerblue;text-align:center;font-size: 18px;">
      <Icon type="flag"></Icon>
      <span>线索信息</span>
    </p>
    <div class="content">
      <div class="column">{{field1}}</div>
      <div class="column">{{field2}}</div>
      <div class="column">{{field3}}</div>
      <div class="column">{{field4}}</div>
    </div>
    <div slot="footer">
    </div>
  </Modal>

</template>

<script type="text/ecmascript-6">
  export default {
      data() {
          return {
              modal:false
          }
      },
    props:{
      field1:String,
      field2:String,
      field3:String,
      field4:String
    }
  }

</script>
<style scoped>
  .content{
     display:flex;
     flex-direction:column;
     padding-left:50px;
  }
  .content .column{
    padding:10px 0;
    box-sizing:border-box;
    margin: 0 auto;
  }

</style>
