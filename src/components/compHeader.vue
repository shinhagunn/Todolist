<template>
  <div class="task__header">
    <p class="task__length">{{ taskLength }} Task</p>
    <p class="task__complete">{{ taskComplete }} Complete</p>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import {taskData} from '../taskData';

@Component
export default class compHeader extends Vue {
  @Prop() private taskList!: taskData[];
  @Prop() private taskLength!: number;
  @Prop() private taskComplete!: number;
  
}
</script>

<style>
  .task__header{
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    margin-bottom: 60px;
  }

  .task__length{
    font-size: 32px;
    font-weight: bold;
  }

  .task__complete{
    font-size: 24px;

  }
</style>