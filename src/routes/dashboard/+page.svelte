<script>
  import { authHandlers } from "../../store/store";

    let todoList = [];
    let currentTodo = "";
    let error = false;

    function addTodo(){
        if(!currentTodo){
            error = true;
        }
        todoList = [...todoList, currentTodo];
        currentTodo = "";
    }

    function editTodo(index){
        let newTodoList = todoList.filter((val, i) =>{
            return i != index;
        });
        currentTodo = todoList[index]
        todoList = newTodoList
    }

    function deleteTodo(index){
        let newTodoList = todoList.filter((val, i) =>{
            return i != index;
        });
        todoList = newTodoList
    }
</script>

<div class="flex flex-col w-full max-w-screen-lg gap-6 p-6 mx-auto">
    <div class="flex items-center justify-between">
        <h1>Todo List</h1>
        <div class="flex items-center gap-4">
            <button class="bg-[#003c5b] flex text-white items-center gap-3 rounded px-2 py-3 hover:bg-sky-700">
                <i class="fa-solid fa-floppy-disk"></i><p>Save</p>
            </button>
            <button on:click={authHandlers.logout} class="bg-[#003c5b] flex text-white items-center gap-3 rounded px-2 py-3 hover:bg-sky-700">
                <i class="fa-solid fa-right-from-bracket"></i><p>Log out</p>
            </button> 
        </div>
        
    </div>
    <main class="flex flex-col flex-1 gap-0.5">
        {#if todoList.length === 0}
            <p>
                You have nothing to do.
            </p>
        {/if}
        {#each todoList as todo, index }
            <div class="flex items-center content-between px-1 py-2">
                <p class="w-full">
                    {index+1}. {todo}
                </p>
                <div class="flex items-center gap-3.5">      
                    <i 
                        class="cursor-pointer fa-solid fa-pen"
                        on:click={()=>{
                        editTodo(index);
                        }} 
                        on:keydown={() => {}} 
                    />
                    <i 
                        class="cursor-pointer fa-solid fa-trash"
                        on:click={()=>{
                            deleteTodo(index);
                        }}
                        on:keydown={() => {}}
                    />
                </div>
            </div>
        {/each}
    </main>
    <div class="flex items-stretch border-[#003c5b] rounded-md overflow-hidden border-solid border-2 border-black-100">
        <input bind:value={currentTodo} class="bg-transparent flex-1 text-black p-3.5 border-none border-solid border-1 border-blue-50" type="text" placeholder="Enter Todo"/>
        <button on:click={addTodo} class="bg-[#003c5b] py-4 px-3 text-white hover:bg-sky-700 cursor-pointer">Add</button>
    </div>
</div>
