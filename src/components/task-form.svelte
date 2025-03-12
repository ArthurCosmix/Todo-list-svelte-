<script lang="ts">
  import { on } from "svelte/events";
  import type { Task } from "../types";
    let {
    addTask,
  }: {
    addTask: (newTask: string) => void
  } = $props();

    let newTask = $state("");

    function submitFrom(event : SubmitEvent) {
        event.preventDefault()
        if(!newTask) return
        addTask(newTask)
        newTask = ""
        
    }
</script>

<form class="form-container" onsubmit={submitFrom}>
    <label for="task-input" class="label-input">
        <input type="text" name="task-input" class="task-input" placeholder="create new task..." bind:value={newTask}>
    </label>
    <button class="button-submit">
        Add Task
    </button>
</form>

<style>
    .form-container{
        width: 960px;
        display: flex;
        justify-content: space-between;   
        align-items: center;
    }

    .label-input{
        width: 90%;
        margin-inline-end: 20px;
    }

    .task-input{
        width: 100%;
        display: block;
        padding: 15px 20px;
        border-radius: 5px;
        border: 0.2px solid #8c8b8b;
    }

    .task-input:checked{
        outline: 1px solid #262626;
    }

    .button-submit{
        width: 110px;
        height: 50px;
        border-radius: px;
        outline: none;
        border: none;
        background: #3b3b3b;
        color: white;
    }

</style>