<template>
  <div class='home'>
    <div class='head'>
    <el-row>
      <el-col :span='6'>
        <p class='name'>数据源</p>
        <div class='text'>
          <el-select v-model='currTable' placeholder='请选择'>
            <el-option
              v-for='item in tables'
              :key='item.value'
              :label='item.label'
              :value='item.value'
             ></el-option>
          </el-select>
        </div>
      </el-col>
      <el-col :span='6'>
        <p class='name'>日期</p>
        <div class='text'>
          <el-date-picker
            v-model='date'
            type='daterange'
            align='right'
            unlink-panels
            range-separator='至'
            start-placeholder='开始日期'
            end-placeholder='结束日期'
            :picker-options='pickerOptions2'
          ></el-date-picker>
        </div>
      </el-col>
      <el-col :span='6'>
        <p class='name'>HS编码</p>
        <div class='text'>
          <el-input v-model='hsCode' placeholder='HS编码' clearable></el-input>
        </div>
      </el-col>
      <el-col :span='6'>
        <p class='name'>货描</p>
        <div class='text'>
          <el-input v-model='describe' placeholder='产品、型号、规格等' clearable></el-input>
        </div>
      </el-col>
    </el-row>

    <el-row>
       <el-col :span='6'>
        <p class='name'>进口商</p>
        <div class='text'>
          <el-input v-model='importer' placeholder='采购商、买家' clearable></el-input>
        </div>
      </el-col>
       <el-col :span='6'>
        <p class='name'>出口商</p>
        <div class='text'>
          <el-input v-model='exporter' placeholder='供应商、卖家' clearable></el-input>
        </div>
      </el-col>
       <el-col :span='6'>
        <p class='name'>原产国</p>
        <div class='text'>
          <el-input v-model='origin' placeholder='原产国' clearable></el-input>
        </div>
      </el-col>
       <el-col :span='6'>
        <p class='name'>起运港</p>
        <div class='text'>
          <el-input v-model='beginPort' placeholder='起运港' clearable></el-input>
        </div>
      </el-col>
    </el-row>

    <el-row>
       <el-col :span='6'>
        <p class='name'>卸货港</p>
        <div class='text'>
          <el-input v-model='endPort' placeholder='采购商、买家' clearable></el-input>
        </div>
      </el-col>
       <el-col :span='6'>
        <p class='name'>提单号</p>
        <div class='text'>
          <el-input v-model='billNo' placeholder='货代提单 or 船东提单' clearable></el-input>
        </div>
      </el-col>
       <el-col :span='6'>
        <p class='name'>集装箱号</p>
        <div class='text'>
          <el-input v-model='boxNo' placeholder='集装箱号' clearable></el-input>
        </div>
      </el-col>
       <el-col :span='6'>
          <p class='name'>日期排序</p>
        <div class='text'>
          <el-select v-model='sort' placeholder='日期排序'>
            <el-option label='倒序' value='desc' ></el-option>
            <el-option label='正序' value='aec' ></el-option>
          </el-select>
        </div>
      </el-col>
    </el-row>
    <div class='head-btn'>
      <el-button type='primary' icon='el-icon-search' :loading="isQuery" @click='query'>搜 索</el-button>
      <el-button   @click='clear'>重 置</el-button>
    </div>
    </div>
    <div class='content' id='content'>
      <el-table
        :data='tableData'
        height='250'
        border
        style='width: 100%'>
        <el-table-column
          prop='date'
          label='日期'
          width='180'>
        </el-table-column>
        <el-table-column
          prop='name'
          label='姓名'
          width='180'>
        </el-table-column>
        <el-table-column
          prop='address'
          label='地址'>
        </el-table-column>
      </el-table>
      <div class='pager'>
      <el-pagination
        background
        @size-change='handleSizeChange'
        @current-change='handleCurrentChange'
        :current-page.sync='currentPage'
        :page-sizes='[10, 20, 30, 40]'
        :page-size='100'
        layout='total, sizes, prev, pager, next, jumper'
        :total='1000'>
      </el-pagination>
      </div>
    </div>
  </div>
</template>

<script>
import  'nprogress/nprogress.css'
import NProgress from 'nprogress'
import {get} from '@/api/api'
export default {
  name: 'home',
  data() {
    return {
      currentPage:3,
      tables: [
        { label: '表一', value: '88343' },
        { label: '表二', value: '8834333' }
      ],
      currTable: '',
      date: [],
      hsCode: '',
      describe:'',
      importer:'',
      exporter:'',
      origin:'',
      beginPort:'',
      endPort:'',
      billNo:'',
      boxNo:'',
      sort:'desc',
      tableData:[{
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
      //
      isQuery:false,
      pickerOptions2: {
        shortcuts: [
          {
            text: '最近一周',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', [start, end]);
            }
          },
          {
            text: '最近一个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit('pick', [start, end]);
            }
          },
          {
            text: '最近三个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit('pick', [start, end]);
            }
          }
        ]
      }
    };
  },
  mounted(){
      NProgress.configure({showSpinner : false });
      NProgress.configure({parent : '#content' });
      document.onkeydown = ()=> {
        let key = window.event.keyCode;
        if (key === 13){
          this.query();//方法
        }
      }

  },
  methods:{
    query(){
      
      NProgress.start();
      let params = {
          currTable:this.currTable,
          date: this.date,
          hsCode:this.hsCode,
          describe:this.describe,
          importer:this.importer,
          exporter:this.exporter,
          origin:this.origin,
          beginPort:this.beginPort,
          endPort:this.endPort,
          billNo:this.billNo,
          boxNo:this.boxNo,
          sort:this.sort
      }
      console.log(params)
      this.isQuery=true
      get('./getData',params).then((data)=>{
        NProgress.done();
        this.isQuery=false;
      })
      
    },
    clear(){
        this.currTable=''
        this.date=[]
        this.hsCode=''
        this.describe=''
        this.importer=''
        this.exporter=''
        this.origin=''
        this.beginPort=''
        this.endPort=''
        this.billNo=''
        this.boxNo=''
        this.sort='desc'
    },
    handleSizeChange(){

    },
    handleCurrentChange(){

    }
  },
  components: {}
};
</script>
<style>
body,html{
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
 
}
*{
  box-sizing: border-box;
}
#nprogress .bar {
    background: #409eff;
}
#nprogress .spinner-icon {
    border-top-color: #409eff;
    border-left-color: #409eff;
}
</style>

<style  scoped>

.home{
   background:#eee;
}
.head{
  background:#fcfcfc;
  padding: 40px;
}
.head-btn button{
  width:130px;
  position: relative;
  top:16px;
}
.content{
  background:#fcfcfc;
  margin-top:20px;
}
.name {
      display: inline-block;
    width: 20%;
    text-align: right;
    padding-right: 10px;
    font-size: 12px;
}
.text {
  display: inline-block;
    width: 80%;
    text-align: left;
}
.pager{
  text-align:right;
}
.el-select, .el-date-editor--daterange.el-input, .el-date-editor--daterange.el-input__inner, .el-date-editor--timerange.el-input, .el-date-editor--timerange.el-input__inner
{
  width: 100%;
}
</style>

