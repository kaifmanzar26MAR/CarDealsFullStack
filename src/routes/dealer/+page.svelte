<script>
    import { onMount } from 'svelte';
    import NavBar from '../../components/NavBar.svelte';
    import {goto} from '$app/navigation'
    let dealer = {};

    const getdealer = async () => {
        const id = localStorage.getItem('logindata');
        try {
            const res = await fetch('https://carstore-backend-jv67.vercel.app/getdealerbyid', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ id })
            });

            const data = await res.json();
            console.log(data);
            dealer = data;
        } catch (error) {
            console.log(error);
        }
    };

    onMount(async () => {
        if(localStorage.getItem('logintype')!='dealer'){
            goto('/');
        }
        getdealer();
    });
</script>
<NavBar/>
<div class="w-full flex flex-col gap-7 items-center justify-center">
    <h1 class="text-3xl font-bold">Abouy Dealer</h1>
    <div class="w-1/2 flex flex-col gap-4 items-center justify-center font-semibold text-xl">
        <p>Dealer Name: {dealer?.dealership_name} </p>
        <p>Dealer Email: {dealer?.dealership_email}</p>
        <p>Dealer Email: {dealer?.dealership_location}</p>
    </div>
</div>
