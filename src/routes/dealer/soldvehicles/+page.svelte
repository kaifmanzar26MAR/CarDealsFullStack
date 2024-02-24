<script>
    import { onMount } from "svelte";
    import NavBar from "../../../components/NavBar.svelte";
    import {goto} from '$app/navigation'
    let allvehicles = [];

    const getalldeals = async () => {
        try {
            const id = localStorage.getItem('logindata');
            console.log(id);

            const res = await fetch(`https://carstore-backend-jv67.vercel.app/getdealerbyid`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ id })
            });

            if (!res.ok) {
                throw new Error(`HTTP error! Status: ${res.status}`);
            }

            const data = await res.json();
            console.log(data.sold_vehicles);
            const ids = data.sold_vehicles;

            console.log(ids);

            const getdetails = async (id) => {
                try {
                    const resp = await fetch('https://carstore-backend-jv67.vercel.app/getvehiclesbyid', {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json",
                        },
                        body: JSON.stringify({ _id: id })
                    });

                    if (!resp.ok) {
                        throw new Error(`HTTP error! Status: ${resp.status}`);
                    }

                    const vehicleData = await resp.json();
                    return vehicleData;
                } catch (error) {
                    console.log(error);
                    return null;
                }
            };

            Promise.all(ids.map((ele) => getdetails(ele))).then((result) => {
                console.log(result);
                allvehicles = result.filter(Boolean); // Filter out null values
            }).catch((error) => console.log(error));
        } catch (error) {
            console.log(error);
        }
    };

    onMount(async () => {
        if(localStorage.getItem('logintype')!='dealer'){
            goto('/');
        }
        getalldeals();
    });
</script>

<NavBar />
<div class='w-full p-4'>
    <h1 class='font-bold text-4xl'>Welcome to DealCars</h1>
    <h1 class="font-bold text-xl p-3 pb-0">All vehicles Sold by the Me</h1><hr>
    <div class="w-full flex flex-wrap gap-5 items-start justify-start p-5">
        
        {#if allvehicles}
        {#each allvehicles as vehicle (vehicle?._id)}
        
        <div class="card w-72 bg-base-100 shadow-xl">
            <figure><img class="w-72 h-40 object-fill" src={vehicle?.vehicle_info.car_image} alt="Car" /></figure>
            <div class="card-body">
                <p>{vehicle?.vehicle_info.car_type}-{vehicle?.vehicle_info.car_name}-{vehicle?.vehicle_info.car_model}</p>
                <p>{vehicle?.vehicle_info.car_price}</p>
               
            </div>
        </div>

        {/each}
        {:else}
        <p>Loading...</p>
        {/if}
    </div>
</div>
