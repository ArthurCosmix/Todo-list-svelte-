<script lang="ts">
  import { get } from "svelte/store";
  import TaskForm from "./components/task-form.svelte";
  import TaskList from "./components/task-list.svelte";
  import type {Task, Filter} from './types'

  const appTitle = 'Task App'
  let currentFilter = $state<Filter>('all')
  const tasks = $state<Task[]>([])
  let totalDone = $derived(tasks.reduce((total, task) => total += Number(task.done), 0))
  let FilterTasks = $derived.by(() => {
    switch (currentFilter) {
      case 'all' :  return tasks
      case 'done' : return tasks.filter(task => task.done)
      case 'todo' : return tasks.filter(task => !task.done)
    } 
  })

  function addTask(newTask : string) {
    tasks.push({
      id : crypto.randomUUID(),
      title : newTask,
      done : false
    })
  }

  function removeTask(id : string){
    const index = tasks.findIndex(task => task.id === id)
    tasks.splice(index, 1)
  }

  function toggleTask(task : Task){
    task.done = !task.done
  }

</script>

{#snippet filterButton(filter : Filter)}
    <button
      onclick={() => currentFilter = filter}
      class:filter-active = {currentFilter === filter}
      class='filter-button'
    >
      {filter}
    </button>
{/snippet}


<main class="main-container">
  <h1 class="app-title">{appTitle}</h1>
  <TaskForm {addTask}/>
  {#if tasks.length}
   <div class="filter-menu">
    <p class="task-title">{totalDone} / {tasks.length} tasks completed</p>
    <div class="button-container">
      {@render filterButton('all')}
      {@render filterButton('done')}
      {@render filterButton('todo')}
    </div>
   </div>
    {:else}
    <p class="task-title">
      add task to get started
    </p>
  {/if}
<TaskList tasks={FilterTasks} {removeTask} {toggleTask}/>
</main>

<style>

  .main-container{
    max-width: 960px;
    width:100%;

    margin-inline: auto;

    display: grid;
    justify-content: center;

  }

  .app-title{text-align: center;}
  .task-title{
    margin-block: 7px;
    color: #565656;
    font-size: 14px;
  }

  .filter-menu{
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-block-start: 1rem;
  }

  .filter-button{
    padding : 8px 20px;
    border: none;
    background-color: #3b3b3b;
    color: white;
    border-radius: 5px;
  }

  .filter-active{
    background-color: #d0d0d0;
    color: #3b3b3b;
    box-shadow: 1px 17px 36px -27px rgba(0,0,0,0.77);
  }

</style>