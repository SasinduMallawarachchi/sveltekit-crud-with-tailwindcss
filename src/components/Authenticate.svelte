<script>
  import { authHandlers } from "../store/store";

    let email = "";
    let password = "";
    let ConfirmPass = "";
    let error = false;
    let register = false;
    let authenticating = false;

    async function handleAuhenticate(){
        if(authenticating){
            return;
        }
        
        if(!email || !password || register && !ConfirmPass){
            error = true;
            return;
        }
        authenticating = true;
        try{
            if(!register){
                await authHandlers.login(email, password);
            }else{
                await authHandlers.signup(email, password);
            }
        }catch(err){
            console.log("There is an Auth error", err);
            error=true;
        }
    }

    function handleRegister(){
        register = !register;
    }
</script>

<div class="flex flex-col items-center justify-center flex-1 ">
    <div class="flex bg-white border-2 border-blue-600 rounded">
<form class="flex flex-col gap-4 mx-auto w-[400px] max-w-[100%] px-3 py-3" action="">
        <h1 class="text-5xl text-center">{register ? "Register" : "Login"}</h1>
        {#if  error}
            <p class="text-base text-red-600">Input is not correct</p>
        {/if}
        <label class="relative border-black border-solid border-1">
            <p class="email" >Email</p>
            <input class="px-2 py-3.5 text-black bg-transparent w-full" bind:value={email} type="email" placeholder="Email" />
        </label>
        <label class="relative border-black border-solid border-1">
            <p class="password">Password</p>
            <input 
                class="px-2 py-3.5 text-black bg-transparent w-full"
                bind:value={password} 
                clas="w-[100%]"
                type="password" 
                placeholder="Password" 
            />
        </label>
        {#if register}
            <label class="relative border-black border-solid border-1">
                <p class="ConfirmPass">Confirm Password</p>
                <input 
                    class="px-2 py-3.5 text-black bg-transparent w-full"
                    bind:value={ConfirmPass} 
                    clas="w-[100%]"
                    type="password" 
                    placeholder="Confirm Password" 
                />
            </label>
        {/if}
        
        <button class=" text-white border-0 px-2.5 py-2 rounded hover:bg-sky-700 bg-blue-700" on:click={handleAuhenticate} type="button">
            {#if authenticating}
            <i class="fa-solid fa-spinner spin"></i>
            {/if}
            Submit
        </button>
    </form>
    </div>
    
    <div class="relative w-full overflow-hidden">
        <p>Or</p>
        {#if register}
        <div class="flex items-center justify-center gap-2">
            <p>Already have an account?</p>
            <p class="cursor-pointer text-cyan-500" on:click={handleRegister} on:keydown={()=>{}}>Login</p>
        </div>
        {:else}
        <div class="flex items-center justify-center gap-2">
            <p>Don't have an account?</p>
            <p class="cursor-pointer text-cyan-500" on:click={handleRegister} on:keydown={()=>{}}>Register</p>
        </div>
        {/if}
    </div>
</div>
