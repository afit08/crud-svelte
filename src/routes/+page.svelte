<script>
    import axios from 'axios';

    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

        //define variable
    let email   = '';
    let password = '';
    let validation = {};

    //method "storePost"
    const storePost = ( async () => {
        const formData = new FormData();

        formData.append('email', email);
        formData.append('password', password);

        try {
            const res = await axios.post('http://localhost:8000/api/login', formData)
            localStorage.setItem('token', res.data.token);
            goto('/dashboard')
          
        } catch (error) {
            return validation.error;
        }

    });

    onMount(() => {
        if(localStorage.getItem('token')) goto('/dashboard')
    })

    let field = {email:'', password:'',};
    let error = {email:'', password:'',};
    let valid = {email:'', password:'',};
</script>

<h1>Login</h1>
<form on:submit|preventDefault={storePost}>
    <div class="mb-3">
      <label for="exampleInputEmail1" class="form-label">Email address</label>
      <input type="email" bind:value={email} class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
    </div>
    <div class="mb-3">
      <label for="exampleInputPassword1" class="form-label">Password</label>
      <input type="password" bind:value={password} class="form-control" id="exampleInputPassword1">
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>