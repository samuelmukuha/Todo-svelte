<svelte:head>
    <link rel="stylesheet" href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css">
</svelte:head>

<script>
    let todoList = []; //array of todos

    let textInput = "";

    function addTodo(){
        todoList = [...todoList, {content: textInput, editing: false, checked: false}]
    }

    function setEditing(i, isEditing){
        todoList[i].editing = isEditing; // true / false
    }
    function deleteTodo(i){
        todoList.splice(i, 1);
        todoList = todoList;
    }
</script>

<div style="margin: 0 auto; padding: 20px; width: 700px;">
    <h2 style="text-align: center;">Todo List</h2>
    <p>Enter Your Todo Here</p>
    <div style="display: flex">
        <input type="text" bind:value={textInput}/>
        <button style="width: 200px; height: 60px" on:click={addTodo}>Add</button>
    </div>
</div>
{#each todoList as todo, i}
<div style="display: flex; align-items: baseline; width: 800px; margin: 0 auto;">
    {#if todo.editing}
    <input type="text" bind:value={todo.content}/>
    {:else}
    <input type="checkbox" bind:checked={todo.checked}/>
    <h4 style="flex-grow: 1">{todo.content}</h4>
    {/if}
    <div style="display: flex;">
        {#if todo.editing}
        <button on:click={() => setEditing(i, false)}>Save</button>
        {:else}
        <button on:click={() => setEditing(i, true)}>Edit</button>
        {/if}
        <button on:click={() => deleteTodo(i)}>Delete</button>
    </div>
</div>
{/each}