<template>
    <div class="task__add">
        <input type="text" v-model="textAddTask" :placeholder="afterUpdateTask">
        <div class="task__add-btn">
        <button v-if="!isUpdate" class="btn" @click="handleAddTask">Add</button>
        <button v-if="isUpdate" class="btn" @click="updateTask">Update</button>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import {taskData} from '../taskData';

@Component
export default class compAdd extends Vue {
    @Prop() private taskList!: taskData[];
    @Prop() private isUpdate!: boolean;
    @Prop() private idUpdate!: number;

    taskLength = this.taskList.length;
    textAddTask = '';

    get afterUpdateTask() : string{
        const taskData = [...this.taskList];
        let result = '';
        taskData.forEach(task => {
            if(task.id == this.idUpdate) {
                result = task.content;
            }
        });
        return result;
    }
  
    handleAddTask() : void {
        this.$emit('addData', this.textAddTask);
        this.textAddTask = '';
    }

    updateTask() : void {
        this.$emit('updateData', this.textAddTask);
        this.textAddTask = '';
    }
}
</script>

<style>
    .task__add{
        display: flex;
    }

    .task__add input{
        flex: 1;
        padding-left: 12px;
        border: 1px solid #000;
        border-radius: 20px;
        font-size: 18px;
    }
</style>