<script>
  import {onMount} from 'svelte'
  import NavBar from '../../components/NavBar.svelte';
  import {goto} from '$app/navigation'
  let user={}
  const getdealer=async()=>{
      const id=localStorage.getItem('logindata');
      try {
          const res=fetch('https://carstore-backend-jv67.vercel.app/getuserdata',{
              method:"POST",
              headers:{
                  "Content-Type":"application/json",
              },
              body:JSON.stringify({id})
          });
          const data=await res.json();
          console.log(data);
          user=data;
      } catch (error) {
          console.log(error);
      }
  }
  onMount(async()=>{
    if(localStorage.getItem('logintype')!='user'){
        goto('/');
    }
      getdealer();
  })
</script>
<NavBar/>
<div class="w-full flex flex-col gap-7 items-center justify-center">
    <h1 class="text-3xl font-bold">About User</h1>
    <div class="w-1/2 flex flex-col gap-4 items-center justify-center font-semibold text-xl">
    <p>User Email:{user.eamil}</p>
    <p>User Name:{user.location} </p>
  </div>
</div>