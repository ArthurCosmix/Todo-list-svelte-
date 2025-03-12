<script lang="ts">
    import type { Task } from '../types';
    let { tasks, removeTask , toggleTask}: { 
        tasks: Task[] 
        removeTask : (id : string) => void
        toggleTask : (task : Task) => void
    } = $props();
    console.log(tasks)
</script>

<div class="container">
    {#each tasks as task , index}
        <div class="task-item">
            <label>
                <input type="checkbox" checked={task.done} onchange={() => toggleTask(task)}/>
                <span class:done={task.done}>{task.title}</span>
            </label>
            <button class="button" onclick={() => removeTask(task.id)}>remove</button>
        </div>
    {/each}
</div>

<style>
    .container{
    max-width: 960px;
    width:100%;
    margin-inline: auto;
    margin-block-start: 1rem;
    }
    
    .task-item{
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding-inline: 20px;
        padding-block : 15px;
        background-color: #fff;
        margin-block: 15px;
        box-shadow: 1px 9px 42px -7px rgba(224,220,224,1);
    }
    .button{
        padding: 10px 20px;
        border: none;
        border-radius: 5px;
        background-color: #3b3b3b;
        color: #fff;
        cursor: pointer;
    }
    .done {
        text-decoration: line-through;
    }
</style>