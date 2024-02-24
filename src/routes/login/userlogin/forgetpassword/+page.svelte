<script>
  import {goto} from '$app/navigation'
    let newPassword=''
    let oldPassword=''
    let user_email=''

    const handleSubmit = async () => {
      try {
        const res=await fetch('https://carstore-backend-jv67.vercel.app/user-change-password',{
            method:'POST',
            headers:{
                "Content-Type":"application/json"
            },
            body:JSON.stringify({user_email,oldPassword,newPassword})
        });
        if(!res.ok){
            throw new Error("user not found");
        }
        const user=await res.json(res);
        console.log(user);
        alert('Password change')
        goto('/login/userlogin')
      } catch (error) {

        console.log(error);
        alert('Invalid credentials',error);
      }
    }


  import { onMount } from 'svelte';
    onMount(async()=>{
        if(localStorage.getItem('logintype')!=''){
            goto('/')
        }
    })

</script>

<div class="w-full flex flex-col items-center justify-center">
    
    <div class="w-1/2 p-5 flex gap-5 flex-col">
      <label class="input input-bordered flex items-center gap-2">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="w-4 h-4 opacity-70">
          <path d="M8 8a3 3 0 1 0 0-6 3 3 0 0 0 0 6ZM12.735 14c.618 0 1.093-.561.872-1.139a6.002 6.002 0 0 0-11.215 0c-.22.578.254 1.139.872 1.139h9.47Z" />
        </svg>
        <input type="text" class="grow" placeholder="Username" bind:value={user_email} />
      </label>
      <label class="input input-bordered flex items-center gap-2">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="w-4 h-4 opacity-70">
          <path fillRule="evenodd" d="M14 6a4 4 0 0 1-4.899 3.899l-1.955 1.955a.5.5 0 0 1-.353.146H5v1.5a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1-.5-.5v-2.293a.5.5 0 0 1 .146-.353l3.955-3.955A4 4 0 1 1 14 6Zm-4-2a.75.75 0 0 0 0 1.5.5.5 0 0 1 .5.5.75.75 0 0 0 1.5 0 2 2 0 0 0-2-2Z" clipRule="evenodd" />
        </svg>
        <input type="password" class="grow" placeholder="old password" bind:value={oldPassword} />
      </label>
      <label class="input input-bordered flex items-center gap-2">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="w-4 h-4 opacity-70">
          <path fillRule="evenodd" d="M14 6a4 4 0 0 1-4.899 3.899l-1.955 1.955a.5.5 0 0 1-.353.146H5v1.5a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1-.5-.5v-2.293a.5.5 0 0 1 .146-.353l3.955-3.955A4 4 0 1 1 14 6Zm-4-2a.75.75 0 0 0 0 1.5.5.5 0 0 1 .5.5.75.75 0 0 0 1.5 0 2 2 0 0 0-2-2Z" clipRule="evenodd" />
        </svg>
        <input type="password" class="grow" placeholder="new password" bind:value={newPassword} />
      </label>
      <button class="btn btn-success" on:click={handleSubmit}>Success</button>
    </div>
</div>