<template>
  <div class="home">
    <input v-model="txt" placeholder="请输入内容" @keydown.enter="add" /><button @click="submit">添加</button>
    <template>
    <p>
      <label for="">
        {{checkWill}}
        <input type="checkbox" v-model="checkWill" @change='checkFn' />未完成
      </label>
    <label for="">
      <input type="checkbox" v-model="checkDone" @change='checkFn' />已完成
    </label>
    </p>
  <el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column
      width="180">
      <template slot-scope="scope">
        <el-popover trigger="hover" placement="top">
          <p>姓名: {{ scope.row.name }}</p>
          <p>住址: {{ scope.row.address }}</p>
          <div slot="reference" class="name-wrapper">
            <el-tag size="medium">{{ scope.row.name }}</el-tag>
          </div>
        </el-popover>
      </template>
    </el-table-column>
    <el-table-column >
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)" >删除</el-button>
        <el-button
          size="mini"
          type="danger"
         @click="handleDelete(scope.$index)" :class="scope.row.falg?'active':'a'">{{scope.row.falg?'已完成':'未完成'}}</el-button>
      </template>
    </el-table-column>
    </el-table>
</template>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  components: {
    // HelloWorld
  },
  data () {
      return {
        input: '',
        tableData: [{
        falg:false,
          name: '张三',
        }, {
        falg:false,
          name: '李四',
        }],
        curIndex:0,
        copyTaskList:[],
        txt:'',
      checkWill:false,
      checkDone:false
      };

    },
    computed: {
    doneLength() {
      return this.copyTaskList.filter(item => item.flag).length;
    }
  },
  created(){
    this.copyTaskList=JSON.parse(JSON.stringify(this.tableData))
  },
     methods: {
       add(e){
        //  debugger
         let obj={
           id:new Date()*1,
           name:this.txt,
           falg:false
         }
         if(e.keyCode===13){
           this.tableData.push(obj)
           this.val=''
         }
        this.txt=''  

        // if(!this.txt){return}
        // this.taskList.push({name:this.txt,falg:false})
        // this.txt=''
        // this.deepCopy()
       },
       submit(){
        this.tableData.push({
          name:this.txt
        })
        this.txt=''  

      },
      handleEdit(index, row) {
        console.log(index, row);
        this.tableData.splice(index,1)
        this.input=''  
      },
      handleDelete(index) {
        console.log(index)
        this.tableData[index].falg=!this.tableData[index].falg     
      },
      checkFn(){
        if(this.checkDone===this.checkWill){
          this.tableData=this.copyTaskList
        }else{
          this.tableData=this.copyTaskList.filter(item=>{
            if(this.checkWill){
              return item.falg===false
            }else if(this.checkDone){
              return item.falg
            }
          })
        }
      }
    }
}
</script>


<style lang="scss" scoped>
.active{
  background: greenyellow !important;
}
</style>