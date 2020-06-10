<script context="module">
	export function preload({ params, query }) {

	let URL = "https://todolistssr.firebaseio.com/task.json"
		return this.fetch(URL).then(r => r.json()).then(
            posts => {
                let postVal=[];
                let postIdx =[];
                let finalPost = [];

                postVal = Object.values(posts)
                postIdx = Object.keys(posts)
                for(let i=0;i<postVal.length;i++)
                {
                    finalPost.push({id:postIdx[i],task:postVal[i].task,completed:postVal[i].completed})
                }

                console.log(finalPost)

                return {finalPost}

            })
	}
</script>

<script>
    import {Col,Row,Container,Alert} from 'sveltestrap';
    import {fade} from 'svelte/transition'
    export let finalPost;
    import ToDoItem from '../components/todoItem.svelte'

    let visible = false;
    let message = ''

    let showProgress=false

    function updateStatus(event)
    {
        const {id,newStatus} = event.detail;
       

    let URL = "https://todolistssr.firebaseio.com/task/"+id+"/.json"
    
    fetch(URL,{
        method:'PATCH',
        body:JSON.stringify({completed:newStatus}),
        headers:{
            'Content-Type':'application/json'
        }

    }).then(res =>{
         console.log(res)
        console.log(newStatus)
        if(res.status==200)
        {
            if(newStatus)
            {

            message= 'Task Completed'
            }
            else
            {

            message= 'Task InComplete'
            }
            visible=true
            showProgress=false

        }
    }).catch(err =>{
        console.log(err)
        showProgress=false;
    })

    }

    function editTask(event)
    {
        const {id,task} = event.detail;

    let URL = "https://todolistssr.firebaseio.com/task/"+id+"/.json"
    
    fetch(URL,{
        method:'PATCH',
        body:JSON.stringify({task:task}),
        headers:{
            'Content-Type':'application/json'
        }

    }).then(res =>{
         console.log(res)

        if(res.status==200)
        {
            message= 'Task was Successfully Updated'
            visible=true
            showProgress=false

        }
    }).catch(err =>{
        console.log(err)
        showProgress=false;
    })
   
    }
    
</script>

<style>
	ul {
		margin: 0 0 1em 0;
		line-height: 1.5;
	}
</style>

<svelte:head>
	<title>Blog</title>
</svelte:head>

<head>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>

<h1>To Do List</h1>

<div transition:fade>

<Alert color="sucess" isOpen={visible} toggle={() => (visible=false)}>

{message}
</Alert>

</div>
<div class="inner-container">
<ul>
{#each finalPost as post}

<ToDoItem {...post} on:toggle={updateStatus} on:edit={editTask} />
    
{/each}

</ul>

</div>

