<script>
    import { onMount } from 'svelte';
    import { page } from '$app/stores';
    import { goto } from '$app/navigation';
    import axios from 'axios';

    let files   = '' ;
    let no_surat_masuk   = '';
    let tgl_masuk = '';
    let asal_surat = '';
    let keterangan = '';
    let sifat_surat = '';
    let validation = {};

    const fetchDataSuratMasuk = async () => {

        await axios.get(`http://localhost:8000/api/surat_masuk/${$page.params.id}`)
        .then(response => {
            no_surat_masuk = response.data.data.no_surat_masuk;
            tgl_masuk = response.data.tgl_masuk;
            asal_surat = response.data.asal_surat;
            keterangan = response.data.keterangan;
            sifat_surat = response.data.sifat_surat;
            console.log(no_surat_masuk)
        });
    }

    onMount(async () => {
        fetchDataSuratMasuk();
    })

    const updateSuratMasuk = ( async () => {

        const formData = new FormData();

        formData.append('file_masuk', files[0]);
        formData.append('no_surat_masuk', no_surat_masuk);
        formData.append('tgl_masuk', tgl_masuk);
        formData.append('asal_surat', asal_surat);
        formData.append('keterangan', keterangan);
        formData.append('sifat_surat', sifat_surat);

        await axios.post(`http://localhost:8000/api/surat_masuk/${$page.params.id}`, formData)
        .then(() => {
            goto("/surat_masuk");
        })
        .catch((error) => {
            validation = error.response.data
        })

    });
</script>

<h1>Update Surat Masuk</h1>

<form on:submit|preventDefault={updateSuratMasuk}>
    <div class="mb-3">
        <label class="form-label" for="">No Surat</label>
        <input type="text" bind:value={no_surat_masuk} class="form-control">
        {#if validation.no_surat_masuk}
            <div class="alert alert-danger">
                {validation.no_surat_masuk}
            </div>
        {/if}
    </div>
    <div class="mb-3">
        <label class="form-label" for="">Tanggal Surat</label>
        <input type="date" bind:value={tgl_masuk} class="form-control">
        {#if validation.tgl_masuk}
            <div class="alert alert-danger">
                {validation.tgl_masuk}
            </div>
        {/if}
    </div>
    <div class="mb-3">
        <label class="form-label" for="">Asal Surat</label>
        <input type="text" bind:value={asal_surat} class="form-control">
        {#if validation.asal_surat}
            <div class="alert alert-danger">
                {validation.asal_surat}
            </div>
        {/if}
    </div>
    <div class="mb-3">
        <label class="form-label" for="">Keterangan</label>
        <input type="text" bind:value={keterangan} class="form-control">
        {#if validation.keterangan}
            <div class="alert alert-danger">
                {validation.keterangan}
            </div>
        {/if}
    </div>
    <div class="mb-3">
        <label class="form-label" for="">Sifat Surat</label>
        <input type="text" bind:value={sifat_surat} class="form-control">
        {#if validation.sifat_surat}
            <div class="alert alert-danger">
                {validation.sifat_surat}
            </div>
        {/if}
    </div>
    <div class="mb-3">
        <label class="form-label" for="">File</label>
        <input type="file" accept="pdf,xlxs" bind:files class="form-control">
        {#if validation.file_masuk}
            <div class="alert alert-danger">
                {validation.file_masuk}
            </div>
        {/if}
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>