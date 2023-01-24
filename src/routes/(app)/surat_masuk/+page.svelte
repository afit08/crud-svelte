<script>
    import { onMount } from "svelte";
    import axios from "axios";

    let surat_masuk = [];

    const fetctDataSuratMasuk = async () => {
        try {
            await axios.get('http://localhost:8000/api/surat_masuk')
                .then(response => {
                    surat_masuk = response.data.data
                    console.log(surat_masuk)
                })   
        } catch (error) {
            return error;
        }
    }

    onMount(async () => {
        fetctDataSuratMasuk();
    });

    const deleteSuratMasuk = async(id)=>{
      try {
        await axios.delete(`http://localhost:8000/api/surat_masuk/delete/${id}`)
        .then(() => {
          fetctDataSuratMasuk();
        })

      } catch (error) {
          return error;
      }
    }
</script>
<h1>Surat Masuk</h1>

<a class="btn btn-primary" href="/surat_masuk/create" role="button">Create</a>

<table class="table">
    <thead>
      <tr>
        <th scope="col">No</th>
        <th scope="col">No Surat</th>
        <th scope="col">Tanggal</th>
        <th scope="col">Asal Surat</th>
        <th scope="col">Keterangan</th>
        <th scope="col">Sifat Surat</th>
        <th scope="col">File</th>
        <th scope="col">Action</th>
      </tr>
    </thead>
    <tbody>
    {#each surat_masuk as item}
      <tr>
        <th scope="row">1</th>
        <td>{item.no_surat_masuk}</td>
        <td>{item.tgl_masuk}</td>
        <td>{item.asal_surat}</td>
        <td>{item.keterangan}</td>
        <td>{item.sifat_surat}</td>
        <td>{item.file_masuk}</td>
        <td>
            <a href="{`/surat_masuk/update/${item.id}`}" class="font-medium text-blue-600 hover:underline dark:text-blue-500">
                Edit
              </a>
              |
              <button type="button" on:click={() => deleteSuratMasuk(item.id)} class="btn btn-danger">Delete</button>
        </td>
      </tr>
    {/each}
    </tbody>
</table>