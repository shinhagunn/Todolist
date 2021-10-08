<template>
  <div class="box-task">
    <compHeader :taskList="taskList" :taskLength="taskLength" :taskComplete="taskComplete"/>
    <compList :taskList="taskList" @changeData="handleChangeData" @updateData="handleUpdateData"/>
    <compAdd :taskList="taskList" :isUpdate="isUpdate" :idUpdate="idUpdate" @changeData="handleChangeData"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import compHeader from '@/components/compHeader.vue'; // @ is an alias to /src
import compList from '@/components/compList.vue'; // @ is an alias to /src
import compAdd from '@/components/compAdd.vue'; // @ is an alias to /src
import {taskData} from '../taskData';

@Component({
  components: {
    compHeader,
    compList,
    compAdd
  },
})
export default class Home extends Vue {
    taskList = this.handleTaskList;
    taskComplete = this.handleTaskComplete;
    taskLength = this.taskList.length;
    idUpdate = -1;
    isUpdate = false;

    get handleTaskComplete() : number{
      const taskData = [...this.taskList];
      let result = 0;
      taskData.forEach(task => {
        if (task.status) result++;
      })
      return result;
    }
    
    get handleTaskList() : taskData[]{
        let result = localStorage.getItem('task');
        return (result != null)? JSON.parse(result): [];
    }

    handleChangeData(taskData : taskData[]) : void{
      this.taskList = [...taskData];
      this.taskLength = this.taskList.length;
      this.idUpdate = -1;
      this.isUpdate = false;
      this.taskComplete = this.handleTaskComplete;
    }

    handleUpdateData(dataId: number) : void{
      this.idUpdate = dataId;
      this.isUpdate = true;
    }
}
</script>

<style>
  .box-task{
    margin: 60px auto;
    padding: 20px;
    width: 500px;
    border-radius: 15px;
    box-shadow: 0px 0px 10px #000;
    background-color: #fff;;
    font-family: Arial, Helvetica, sans-serif;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: block;
    margin: 0 10px;
  }

  .btn{
    margin-left: 8px;
    padding: 12px;
    border: 1px solid #000;
    border-radius: 20px;
    background-color: #fff;
  }

  .btn:hover{
    cursor: pointer;
  }

  a {
    color: #42b983;
  }
</style>
