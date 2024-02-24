<script>
    import { onMount } from "svelte";
    import NavBar from "../../../components/NavBar.svelte";
    import {goto} from '$app/navigation'
    let allvehicles = [];
  
    const isLogin = async () => {
      const id = localStorage.getItem('logindata');
      console.log(typeof id);
  
      try {
        const res = await fetch('https://carstore-backend-jv67.vercel.app/getuserdata', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ _id: id })
        });
  
        if (!res.ok) {
          throw new Error("Didn't get the response");
        }
  
        let user = await res.json();
        console.log(user);
  
        const ids = user.vehicle_info;
  
        const getdetail = async (id) => {
          try {
            const resp = await fetch('https://carstore-backend-jv67.vercel.app/getsoldvehiclebyid', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json'
              },
              body: JSON.stringify({ _id: id })
            });
  
            const data = await resp.json();
            return data;
          } catch (error) {
            console.log(error);
          }
        };
  
        // Use Promise.all to wait for all promises to resolve
        const idArray = Object.values(ids);
  
        Promise.all(idArray.map(element => getdetail(element)))
          .then(result => {
            // 'result' is an array containing the resolved values of all promises
            console.log(result);
            allvehicles = result;
          })
          .catch(error => {
            console.error(error);
          });
      } catch (error) {
        console.log(error);
      }
    };
  
    onMount(async () => {
      if(localStorage.getItem('logintype')!='user'){
        goto('/')
      }
      isLogin();
    });
  </script>
  
  <NavBar />
  <div class='w-full'>
    <h1 class='font-bold text-4xl'>Welcome to DealCars</h1>
    <h1 class="font-bold text-xl p-3 pb-0">All vehicles purchased by the user</h1><hr>
    <div class="w-full flex flex-wrap gap-5 items-start justify-start p-5">
      
       {#if allvehicles}
        {#each allvehicles as vehicle (vehicle?._id)}
          
          <div class="card w-72 bg-base-100 shadow-xl">
            <figure><img class="w-72 h-40 object-fill" src={vehicle?.vehicle_info.car_image} alt="car" /></figure>
            <div class="card-body">
                <p>{vehicle?.vehicle_info.car_name}-{vehicle?.vehicle_info.car_type}-{vehicle?.vehicle_info.car_model}</p>
              <p>Price: {vehicle?.vehicle_info.car_price} <br/>Registration: {vehicle?.vehicle_info.registration}</p>
              
            </div>
          </div>
        {/each}
      {:else}
        <p>Loading...</p>
      {/if}
    </div>
  </div>
  