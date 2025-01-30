
<svelte:head>
    <link rel="stylesheet" href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</svelte:head>

<script>
    let todoList = []; // Array of todos
    let textInput = "";

    function addTodo() {
        if (textInput.trim()) {
            todoList = [...todoList, { content: textInput, editing: false, checked: false }];
            textInput = ""; // Clear the input
        }
    }

    function setEditing(i, isEditing) {
        todoList[i].editing = isEditing; // true / false
        todoList = todoList; // Trigger reactivity
    }

    function deleteTodo(i) {
        todoList.splice(i, 1);
        todoList = todoList; // Trigger reactivity
    }
</script>

<div class="container mx-auto p-5 max-w-4xl">
    <h2 class="text-2xl font-bold text-center mb-4">Todo List</h2>
    <p class="text-gray-600 mb-4">Enter Your Todo Here</p>
    <div class="flex gap-2 mb-8">
        <input
            type="text"
            bind:value={textInput}
            class="flex-grow p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Add a new todo"
        />
        <button
            on:click={addTodo}
            class="px-6 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500"
        >
            Add
        </button>
    </div>

    {#each todoList as todo, i}
        <div class="flex items-center gap-4 mb-4 p-4 bg-white rounded-lg shadow-sm">
            {#if todo.editing}
                <input
                    type="text"
                    bind:value={todo.content}
                    class="flex-grow p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                />
            {:else}
                <input
                    type="checkbox"
                    bind:checked={todo.checked}
                    class="w-5 h-5"
                />
                <h4 class="flex-grow text-lg {todo.checked ? 'line-through text-gray-400' : 'text-gray-700'}">
                    {todo.content}
                </h4>
            {/if}
            <div class="flex gap-2">
                {#if todo.editing}
                    <button
                        on:click={() => setEditing(i, false)}
                        class="px-4 py-2 bg-green-500 text-white rounded-lg hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-green-500"
                    >
                        Save
                    </button>
                {:else}
                    <button
                        on:click={() => setEditing(i, true)}
                        class="px-4 py-2 bg-yellow-500 text-white rounded-lg hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-yellow-500"
                    >
                        Edit
                    </button>
                {/if}
                <button
                    on:click={() => deleteTodo(i)}
                    class="px-4 py-2 bg-red-500 text-white rounded-lg hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-500"
                >
                    Delete
                </button>
            </div>
        </div>
    {/each}
</div>