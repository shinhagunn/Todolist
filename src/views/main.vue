<template>
  <div class="box-task">
    <compHeader :taskList="taskList" :taskLength="taskLength" :taskComplete="taskComplete"/>
    <compList :taskList="taskList" @updateChangeForm="handleUpdateChangeForm" @taskComplete="handleTaskComplete" @deleteTask="handleDeleteTask"/>
    <compAdd :taskList="taskList" :isUpdate="isUpdate" :idUpdate="idUpdate" @addData="handleAddData" @updateData="handleUpdateData"/>
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
    taskComplete = this.getTaskComplete;
    taskLength = this.taskList.length;
    idUpdate = -1;
    isUpdate = false;

    get getTaskComplete() : number{
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

    handleDeleteTask(id : number) : void {
      this.taskList.splice(this.getPosById(id), 1);

      localStorage.setItem('task', JSON.stringify(this.taskList));
      this.taskLength = this.taskList.length;
      this.taskComplete = this.getTaskComplete;
    }

    handleAddData(text : string) : void {
      let newId = (this.taskList.length > 0) ? (this.taskList[this.taskList.length - 1].id + 1) : 1;

      this.taskList.push({
          content: text,
          id: newId,
          status: false
      });

      localStorage.setItem('task', JSON.stringify(this.taskList));
      this.taskLength = this.taskList.length;
    }

    handleUpdateData(text: string) : void{
      this.taskList[this.getPosById(this.idUpdate)].content = text;
      localStorage.setItem('task', JSON.stringify(this.taskList));

      this.idUpdate = -1;
      this.isUpdate = false;
    }

    handleUpdateChangeForm(id : number) : void{
      this.idUpdate = id;
      this.isUpdate = true;
    }

    handleTaskComplete(id : number) : void {
      this.taskList[this.getPosById(id)].status = !this.taskList[this.getPosById(id)].status;
      localStorage.setItem('task', JSON.stringify(this.taskList));
      this.taskComplete = this.getTaskComplete;
    }

    getPosById(id : number) : number {
      let pos = this.taskList.findIndex(task => {
        return task.id == id;
      });

      return pos;
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
