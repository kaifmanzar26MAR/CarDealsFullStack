<script>
    
    import { onMount } from "svelte";
    import NavBar from "../../../components/NavBar.svelte";
    import {goto} from '$app/navigation'
    let DealerCars = [];
    let cardeal=[]
    const getallcars = async () => {
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
            console.log(data.cars);
            const ids = data.cars;

            console.log(ids);

            const getdetails = async (id) => {
                try {
                    const resp = await fetch('https://carstore-backend-jv67.vercel.app/getcarbyid', {
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
                DealerCars = result;
            }).catch((error) => console.log(error));
        } catch (error) {
            console.log(error);
        }
    };

    const handleDealCreation = async () => {
        try {
            
            const price = document.getElementById('price').value;
            const registration = document.getElementById('registration').value;
            const year = document.getElementById('year').value;
            console.log("Car Details:", cardeal);

            const res=await fetch('https://carstore-backend-jv67.vercel.app/adddeal',{
                method: 'POST',
                headers: {
                'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                car_id: cardeal._id,
                
                deal_info: {
                    registration,
                    price,
                    year,
                    car_name: cardeal.name,// Accessing the name property
                    car_model:cardeal.model,
                    car_type:cardeal.type,
                    car_image:cardeal.car_info['imageUrl'],
                    dealer_id:localStorage.getItem('logindata')
                }
                })
            });
            const data=await res.json();
            console.log(data);
            alert('deal created')
           
        } catch (error) {
            console.log(error);
        }
    }
    
    onMount(async () => {
        if(localStorage.getItem('logintype')!='dealer'){
            goto('/home')
        }
        getallcars();
    });
</script>

<NavBar />
<div class='w-full p-4'>
    <h1 class='font-bold text-4xl'>Welcome to DealCars</h1>
    <h1 class="font-bold text-xl p-3 pb-0">All Cars Added By me</h1><hr>
    <div class="w-full flex flex-wrap gap-5 items-start justify-start p-5">
        
        {#if DealerCars}
        {#each DealerCars as car (car?._id)}
        
        <div class="card w-72 bg-base-100 shadow-xl">
            <figure><img class="w-72 h-40 object-fill" src={car?.car_info['imageUrl']} alt="Car" /></figure>
            <div class="card-body">
                <p>{car?.type}-{car?.name}-{car?.model}</p>
                
                <div class="card-actions justify-end">
                                        <button class="btn btn-primary" on:click={() =>{cardeal=car; document.getElementById('my_modal_5').showModal()}}>Create Deal</button>
                    <dialog id="my_modal_5" class="modal modal-bottom sm:modal-middle">
                        
                        <div class="modal-box flex flex-col items-center justify-center gap-5">
                            <input type="text" id="price" placeholder="Car price" class="input input-bordered w-full max-w-xs" />
                            <input type="text" id="registration" placeholder="registration no." class="input input-bordered w-full max-w-xs" />
                            <input type="text" id="year" placeholder="year" class="input input-bordered w-full max-w-xs" />
                            <div class="modal-action">
                                <form method="dialog">
                                    <button class="btn btn-sm btn-circle btn-ghost absolute right-2 top-2">✕</button>
                                    <button class="btn w-full " on:click={() => handleDealCreation()}>Create</button>
                                </form>
                                
                            </div>
                        </div>
                    </dialog>
                </div>
            </div>
        </div>

        {/each}
        {:else}
        <p>Loading...</p>
        {/if}
    </div>
</div>
