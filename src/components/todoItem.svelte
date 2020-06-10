<script>
import {createEventDispatcher} from 'svelte';

import {Col,Row,Container} from 'sveltestrap';

const dispatch = createEventDispatcher();

export let id;
export let task;
export let completed;

let isEditing=false


function updateTask(event)
{
    isEditing = !isEditing
}

function edit()
{
    isEditing=!isEditing
    dispatch('edit',{
        id,
        task
    })
}

function toggleStatus()
{
    completed  = !completed;
    let newStatus = completed
    dispatch('toggle',{
        id,
        newStatus
    })
}

</script>

<head>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>


<li>
<Container>
<Row>
{#if !isEditing}
<Col xs="auto"><button on:click={updateTask}>✏️</button></Col>
{:else}
<Col xs="auto"><button on:click={edit}>💾</button></Col>
{/if}

{#if completed}
<Col xs="auto"><button on:click={toggleStatus}>✅</button></Col>
<Col sm="6"><span class="is-completed">{task}</span></Col>
{:else}
<Col xs="auto"><button on:click={toggleStatus}>❌</button></Col>

{#if !isEditing}
<Col sm="6"><span>{task}</span></Col>
{:else}
<Col sm="6"><input type="text" bind:value={task}></Col>
{/if}


{/if}

</Row>

</Container>
<hr>

</li>


<style>

.is-completed{
    text-decoration: line-through;
    color: gray;
}

button{
    background-color: transparent;
    border: 0px;
}

li{
    list-style-type: none;
}


</style>