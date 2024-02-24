<script>
  import { onMount } from 'svelte';
  import NavBar from '../components/NavBar.svelte';

  let alldeals = [];
  let cardetails = [];
let logintype='';
  const getDeals = async () => {
    try {
      const response = await fetch('https://carstore-backend-jv67.vercel.app/alldeals');

      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }

      const deals = await response.json();
      console.log("all deals", deals);
      return deals;
    } catch (error) {
      console.error('Error fetching deals:', error.message);
    }
  };

  const getCar = async (car_id) => {
    console.log(car_id);
    try {
      const res = await fetch('https://carstore-backend-jv67.vercel.app/getcardet', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ _id: car_id })
      });
      const data = await res.json();
      console.log(data);
      return data;
    } catch (error) {
      console.log(error);
      return { name: 'Car Name' }; // Default value if fetching fails
    }
  };

  const buyfun = async (deal) => {
    const user_id = localStorage.getItem('logindata');
    console.log(user_id);
    
    try {
      const res = await fetch('https://carstore-backend-jv67.vercel.app/addsoldvehicle', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          car_id: deal.car_id,
          user_id,
          vehicle_info: {
            dealer_id:deal.deal_info.dealer_id,
            registration:deal.deal_info.registration,
            car_name: deal.deal_info.car_name,// Accessing the name property
            car_model: deal.deal_info.car_model,
            car_type: deal.deal_info.car_type,
            car_image:deal.deal_info.car_image,
            car_price:deal.deal_info.price
          }
        })
      });

      const data = await res.json();
      console.log(data);
      alert('Bought successfully');
    } catch (error) {
      console.log(error);
    }
  };

  onMount(async () => {
    logintype=localStorage.getItem('logintype');
    alldeals = await getDeals();

    // Fetch car details for each deal and update the cardetails array
    cardetails = await Promise.all(alldeals.map(deal => getCar(deal.car_id)));
    console.log(cardetails);
  });
</script>

<NavBar />
<div class='w-full p-4'>
  <h1 class='font-bold text-4xl'>Welcome to DealCars</h1>
  <h1 class="font-bold text-xl p-3 pb-0">All Deals</h1><hr>
  <div class="w-full flex flex-wrap items-start justify-start p-5 gap-5">
    
    {#if alldeals}
      {#each alldeals as deal, index (deal?._id)}
        <div class="card w-72 bg-base-100 shadow-xl">
          <figure><img class="w-72 h-40 object-fill" src={deal?.deal_info.car_image} alt="car" /></figure>
          <div class="card-body">
            <h2 class="card-title text-lg">Car Name: {deal?.deal_info.car_name}</h2>
            <p>Price: {deal?.deal_info.price}, year: {deal?.deal_info.year} </p>
            <p>Reg. no {deal?.deal_info.registration}</p>
            {#if logintype==='user'}
            <div class="card-actions justify-end">
              <button class="btn btn-primary" on:click={() => buyfun(deal)}>Buy Now</button>
            </div>
            {/if}
          </div>
        </div>
      {/each}
    {:else}
      <p>Loading...</p>
    {/if}
  </div>
</div>
