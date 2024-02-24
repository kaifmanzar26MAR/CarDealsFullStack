<script>
    import { onMount } from 'svelte';
   

    let user = {};
    let type='';
    const logout = () => {
      localStorage.setItem('logindata', '');
      localStorage.setItem('logintype', '');
      alert('Logout successful');
    };
  
    const isLogin = async () => {
      const id = localStorage.getItem('logindata');
      console.log(id);
      
      try {
        const res = await fetch('https://carstore-backend-jv67.vercel.app/getuserdata', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ _id: id })
        });
        if(!res.ok){
          throw new Error("didn't got the res");
        }
        user = await res.json();
        type='user'
        console.log(user);
      } catch (error) {
        console.log(error);
      }
    };
    const isdealerlogin=async()=>{
      const id = localStorage.getItem('logindata');
      console.log('dealer')
      console.log(id);
      
      try {
        const res = await fetch('https://carstore-backend-jv67.vercel.app/getdealserdata', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ _id: id })
        });
        if(!res.ok){
          throw new Error("didn't got the res");
        }
        user = await res.json();
        type='dealer';
        console.log(user);
      } catch (error) {
        console.log(error);
      }
    }
  
    onMount(() => {
      if(localStorage.getItem('logintype')==='user')
        isLogin();
      else if(localStorage.getItem('logintype')==='dealer')
      isdealerlogin();
      
    });
  </script>
  
  <div class="navbar bg-base-100 p-4">
    <div class="flex-1">
      <a href="/" class="btn btn-ghost text-xl">CarDeals</a>
    </div>
    <div class="flex-1 ml-2 mr-5">
      {#if user && user.user_email}
        <a href='/user'>{user.user_email}</a>
      {:else if user && user.dealership_email}
      <a href='/dealer'>{user.dealership_email}</a>
      {:else}
        <!-- If user is not logged in, you can show a different UI or redirect to login -->
        Not Logged In
      {/if}
    </div>
    

    {#if type==='dealer'}
      <div class="flex-1">
        <a href="/dealer/addcars">Add Cars</a>
      </div>
    {/if}


    {#if type==='dealer'}
      <div class="flex-1">
        <a href="/dealer/mycars">MyCar</a>
      </div>
    {/if}

    {#if type==='dealer'}
      <div class="flex-1">
        <a href="/dealer/mydeals">MyDeals</a>
      </div>
    {/if}

    {#if type==='dealer'}
      <div class="flex-1">
        <a href="/dealer/soldvehicles">Veiw SoldVehicles</a>
      </div>
    {/if}
    
      {#if type==='user'}
      <div class="flex-1">
        <a href="/user/vehicles">Purchase vehicle</a>
      </div>

      {/if}
    {#if type==='user' || type==='dealer'}
    <div class="flex-none">
      <div class="dropdown dropdown-end">
        <div tabIndex={0} role="button" class="btn btn-ghost btn-circle avatar">
          <div class="w-10 rounded-full">
            <img alt="Tailwind CSS Navbar component" src="https://daisyui.com/images/stock/photo-1534528741775-53994a69daeb.jpg" />
          </div>
        </div>
        <ul tabIndex={0} class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-base-100 rounded-box w-52">
          <li>
            <a href={type==='user'?'/user':'/dealer'} class="justify-between">
              Profile
              <span class="badge">New</span>
            </a>
          </li>
          <li on:click={logout}><a href="/login">Logout</a></li>
        </ul>
      </div>
    </div>
    {:else}
      <a href="/login">Login</a>
    {/if}
  </div>

  