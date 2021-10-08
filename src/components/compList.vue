<template>
    <ul class="task__list">
        <li :class="[{'task__item': true}, { 'task--complete': taskItem.status}]" v-for="taskItem in taskList" :key="taskItem.id" >
        <div class="task__item-left">
            <input type="checkbox" @click="handleCheckBoxTask(taskItem.id)" :checked="taskItem.status">
            {{ taskItem.content }}
        </div>

        <div class="task__item-right">  
            <button class="btn" @click="handleUpdateTask(taskItem.id)">Update</button>  
            <button class="btn" @click="handleDeleteTask(taskItem.id)">Delete</button>  
        </div>
        </li>
    </ul>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import {taskData} from '../taskData';


@Component
export default class compList extends Vue {
    @Prop() private taskList!: taskData[];
    
    handleDeleteTask(id : number) : void {
        let taskData = [...this.taskList];
        
        let pos = taskData.findIndex((task : {id: number, content: string}) => {
        return task.id == id;
        });
        taskData.splice(pos, 1);

        localStorage.setItem('task', JSON.stringify(taskData));
        this.$emit('changeData', taskData);
    }

    handleUpdateTask(id : number) : void {
        this.$emit('updateData', id);
    }

    handleCheckBoxTask(id : number) : void{
        let taskData = [...this.taskList];
        let pos = taskData.findIndex(task => {
            return task.id == id;
        });

        taskData[pos].status = !taskData[pos].status;
        localStorage.setItem('task', JSON.stringify(taskData));

        this.$emit('changeData', taskData);
    }
}
</script>

<style>
    .task__item{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 32px 0px;
    }

    .task__item-left{
    font-size: 20px;
    }

    .task--complete{
        text-decoration: line-through;
    }
</style>