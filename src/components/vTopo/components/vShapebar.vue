<!--
 * @Author: caojing
 * @Date: 2018-11-23 10:28:53
 * @LastEditors: caojing
 * @LastEditTime: 2018-11-27 10:12:26
 -->
<template>
  <div class="shapebarWrap">
    <div class="shapebarHead">
      设备列表
      <el-popover
        placement="right"
        trigger="click"
        width="600"
      >
        <div class="select-equipment-contair">
          <h1 style="font-weight: bold;padding: 0 0 10px 0;">添加设备</h1>
          <el-form size="mini" :inline="true" :model="formInline" class="demo-form-inline">
            <el-form-item label="起始IP:">
              <el-input v-model="beginIP" placeholder="起始IP"></el-input>
            </el-form-item>
            <el-form-item label="结束IP:">
              <el-input v-model="endIP" placeholder="结束IP" ></el-input>
            </el-form-item>
            <el-form-item>
              <el-button icon="el-icon-search" type="primary" @click="onSubmit">查询</el-button>
            </el-form-item>
            <ul class="select-equipment-contair-show">
              <el-table
                ref="multipleTable"
                :data="tableData"
                tooltip-effect="dark"
                style="width: 100%"
                height="300"
                @selection-change="handleSelectionChange">
                <el-table-column
                  type="selection"
                  width="55">
                </el-table-column>
                <el-table-column
                  label="IP"

                  prop="ip"
                >
                </el-table-column>
                <el-table-column
                  prop="name"
                  label="名称"
                  >
                </el-table-column>
                <el-table-column
                  prop="type"
                  label="类型"
                  >
                </el-table-column>
              </el-table>
            </ul>
          </el-form>
          <br>
          <el-button style="float: right;margin-right: 10px" type=""  size="mini">取消</el-button>
          <el-button style="float: right;margin-right: 10px" type="primary"  size="mini">添加</el-button>

        </div>
        <el-button slot="reference"
                   style="float: right; margin-top: 10px"
                   icon="el-icon-plus"
                   type="success"
                   size="mini">
          </el-button>
      </el-popover>

    </div>
    <div class="shapeNodeLstWrap">
      <ul class="shapeNodeLst">
        <li v-for="(ele,key) in shapeNodeLstData" :key="key" class="shapeNode"
            @mousedown.stop.prevent="dragShapeNode(shapeNodeLstData,key,$event)" :title="ele.name">
          <div class="shapeIcon">
            <img class="shapeIconImg" :src="ele.icon"/>
          </div>
          <div class="shapeName">{{ ele.name }}</div>
          <i v-if="isManage" class="el-icon-delete delete-btn" title="删除" @click.stop.prevent="deleteNode(key)"></i>

        </li>
      </ul>
      <el-button v-if="shapeNodeLstData.length > 0" @click.stop.prevent="isManage = !isManage" type="danger" size="mini" style="float: right; margin-top: 10px" >{{isManage?'隐藏':'管理'}}</el-button>
      <p style="color: #909399; padding: 50px 0px;text-align: left">tips：del键--删除节点或者连线。</p>
    </div>
  </div>
</template>

<script>
import shapeNodeLstData from '../../../data/toolbarNodeData' //初始左侧toolbarNode数据（可从后台获取）
export default {
  name: 'vShapebar',
  props: {
    shapeNodeLstData: {
      type: Array,
      default() {
        return []
      }
    }
  },
  data() {
    return {
      // shapeNodeLstData:[] //shapebar数据
      isManage:false,
      tableData:[
        {ip:'192.168.0.100',name:'CE1',type:'CE交换机'},
        {ip:'192.168.0.100',name:'CE1',type:'CE交换机'},
        {ip:'192.168.0.100',name:'CE1',type:'CE交换机'},
        {ip:'192.168.0.100',name:'CE1',type:'CE交换机'},
        {ip:'192.168.0.100',name:'CE1',type:'CE交换机'},
        {ip:'192.168.0.100',name:'CE1',type:'CE交换机'},
        {ip:'192.168.0.100',name:'CE1',type:'CE交换机'},
        {ip:'192.168.0.100',name:'CE1',type:'CE交换机'},
        ]
    }
  },
  components: {},
  methods: {
    dragShapeNode(shapeNodeLstData, key, $event) {
      this.$emit('click', shapeNodeLstData, key, $event);
    },
    //初始shapeLstData
    initToolbarNodes() {
      let initShapeLstData = shapeNodeLstData //toolbarNodeData从后台获取
      if (!initShapeLstData instanceof Array) { //类型检测，必须为Array
        console.error('shapeNodeLstData must be Array')
        initShapeLstData = []
      }
      this.shapeNodeLstData = initShapeLstData
    },

    //删除节点
    deleteNode(key){
      this.shapeNodeLstData.splice(key,1)
    }
  },
  mounted() {
    // this.initToolbarNodes() //初始化toolbarNodes数据
  },
  created() {
  }
}
</script>
<style lang="less" scoped>
@border-color: #aaaaaa;
/*svgMain左侧工具栏*/
.shapebarWrap {
  height: 100%;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  width: 250px;
  border: 1px solid @border-color;
  border-right: 0;
  background: @theme-color;

  .shapebarHead {
    height: 50px;
    line-height: 50px;
    padding: 0 20px;
    text-align: left;
    font-size: 14px;
    -webkit-user-select: none;
    user-select: none;
    font-weight: 700;
    color: @theme-font-color;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }

  .shapeNodeLstWrap {
    overflow-y: auto;
    box-sizing: border-box;
    padding: 10px 15px;
    flex: 1;

    .shapeNodeLst {
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      box-sizing: border-box;
    }
  }
}

.shapeNode {
  position: relative;
  margin-top: 5px;
  cursor: pointer;
  border: 1px solid #c7d1dd;
  border-radius: 2px;
  -webkit-user-select: none;
  user-select: none;
  background-color: #fff;
  -webkit-user-select: none;
  user-select: none;
  box-sizing: border-box;
  width: calc(~"( 100% - 10px ) / 3");
  box-sizing: border-box;
  padding: 6px 0;
  margin-right: 5px;

  &:nth-child(3n) {
    margin-right: 0
  }
}

/*移动的node*/
.shapeIcon {
  text-align: center;
  -webkit-user-select: none;
  user-select: none;

  .shapeIconImg {
    width: 28px;
    height: 28px;
    -webkit-user-select: none;
    user-select: none;
  }
}

.shapeName {
  font-size: 12px;
  text-align: center;
  padding: 0 5px;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  -webkit-user-select: none;
  user-select: none;
  color: #000
}
.delete-btn{
  color: red;
  position: absolute;
  top: 2px;
  right: 2px;
}
.delete-btn:hover{
  font-weight: bold;
}
</style>
