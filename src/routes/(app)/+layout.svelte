<script>
    import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

   function logoutHandler(){
		localStorage.removeItem('token');
		goto('/')
	}

	onMount(() => {
		if(!localStorage.getItem('token')) goto('/');
	})
</script>
<div class="container-fluid">
    <div class="row">
      <div class="col-2">
        <ul class="nav flex-column">
            <li class="nav-item">
                <h1>LOGO</h1>
            </li>
            <li class="nav-item">
               <a class="nav-link" href="/dashboard">Dashboard</a>
            </li>
            <li class="nav-item">
               <a class="nav-link" href="/surat_masuk">Surat Masuk</a>
            </li>
            <li class="nav-item">
               <a class="nav-link" href="/surat_keluar">Surat Keluar</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="/disposisi_surat">Disposisi Surat</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="/tracking_surat">Tracking Surat</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href={"#"} on:click={logoutHandler}>Logout</a>
            </li>
        </ul>
      </div>
      <div class="col-10">
        <slot />
      </div>
    </div>
</div>