<script>
    import { onMount } from 'svelte';
    import { navigate } from '$app/navigation';

    onMount(async () => {
        if (localStorage.getItem('logintype') !== '') {
            navigate('/');
        }
    });
</script>

<div class="w-full h-[80vh] flex items-center gap-5 justify-center">
    <a href="/login/userlogin" class="w-40 h-40 bg-white rounded font-bold text-black shadow-sm flex items-center justify-center hover:scale-90 transition-all duration-300">
        User
    </a>
    <a href="/login/dealerlogin" class="w-40 h-40 bg-white rounded font-bold text-black shadow-sm flex items-center justify-center hover:scale-90 transition-all duration-300">
        Dealer
    </a>
</div>
