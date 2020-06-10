<script>
	import {Button,Row,Col,Container,Spinner,Alert} from 'sveltestrap';


	let task;
	let showProgress=false;

	let message='Task Successfully Added';
	let visible=false

	let URL = "https://todolistssr.firebaseio.com/task.json"

	function addTask()
	{
			showProgress=true

			fetch(URL,{
				method:'POST',
				body:JSON.stringify({task:task,completed:false}),
				headers:{
					'Content-Type':'application/json'
				}
			}).then(res =>{
				console.log(res)
				if(res.status==200)
				{
					message= 'Task Successfully Created'
						visible=true
						showProgress=false

				}
			}).catch(err =>{
				console.log(err)
			})

	}

</script>

<svelte:head>
	<title>To Do List APP using SSR</title>
</svelte:head>

<head>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>

{#if visible}
<Alert color="info" isOpen={visible} toggle={() => (visible = false)}>
  {message}
</Alert>
{/if
}

<h1>Add New Task...</h1>
<div class="new-task">
<Container>
<Row>
<Col><input type="text" name="task" placeholder="Enter The Task..." bind:value={task}></Col>
<Col xs="auto"> 
{#if showProgress}
<Spinner dark type="grow" />
{/if}
</Col>
<Col xs="auto"><Button color="danger" on:click={addTask}>Add Task</Button></Col>

</Row>

</Container>
</div>


<style>
.new-task{
	margin-top: 20px;
	
}
input{
	
	width: 100%;
	font-size: 20px;
}
</style>