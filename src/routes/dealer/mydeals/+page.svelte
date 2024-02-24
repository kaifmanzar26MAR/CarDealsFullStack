<script>
    
    import { onMount } from "svelte";
    import {goto} from '$app/navigation'
    import NavBar from "../../../components/NavBar.svelte";
    let alldeals = [];
    let cardeal=[]
    const getalldeals = async () => {
        try {
            const id = localStorage.getItem('logindata');
            console.log(id);

            // Use template literals for better readability
            const res = await fetch(`https://carstore-backend-jv67.vercel.app/getdealerbyid`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ id })
            });

            const data = await res.json();
            console.log(data.deals);
            const ids = data.deals;

            console.log(ids);

            const getdetails = async (id) => {
                try {
                    const resp = await fetch('https://carstore-backend-jv67.vercel.app/getdealsbyid', {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json",
                        },
                        body: JSON.stringify({ _id: id })
                    });
                    const data = await resp.json();
                    return data;
                } catch (error) {
                    console.log(error);
                }
            }

            Promise.all(ids.map((ele) => getdetails(ele))).then((result) => {
                console.log(result);
                alldeals = result;
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
    <h1 class="font-bold text-xl p-3 pb-0">All Deals Added By ME</h1><hr>
    <div class="w-full flex flex-wrap gap-5 items-start justify-start p-5">
        
        {#if alldeals}
        {#each alldeals as deal (deal?._id)}
        
        <div class="card w-72 bg-base-100 shadow-xl">
            <figure><img class="w-72 h-40 object-fill" src={deal?.deal_info.car_image} alt="Car" /></figure>
            <div class="card-body">
                <p>{deal?.deal_info.car_type}-{deal?.deal_info.car_name}-{deal?.deal_info.car_model}</p>
                <p>{deal?.deal_info.price}, {deal?.deal_info.year}</p>
               
            </div>
        </div>

        {/each}
        {:else}
        <p>Loading...</p>
        {/if}
    </div>
</div>
