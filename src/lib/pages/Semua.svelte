<script>
  import { onMount } from 'svelte';
  import { Alert, Button, Card, CardText, CardTitle, Pagination } from 'sveltestrap';
  import { Link } from 'svelte-navigator'
import {Spinner} from 'sveltestrap'
  let data = null;
  let currentPage = 1;
  let perPage = 35;
  let isLoading = true; // New variable to track loading state

  async function fetchData(page, perPage) {
    const response = await fetch(`https://api.filelions.com/api/file/list?key=1730m2zzxzv2tzii249x&page=${page}&per_page=${perPage}`);
    const json = await response.json();
    data = json.result.files;
    isLoading = false; // Set loading state to false when data is loaded
  }

  onMount(() => {
    fetchData(currentPage, perPage);
  });

  function nextPage() {
    currentPage++;
    fetchData(currentPage, perPage);
  }

  function previousPage() {
    if (currentPage > 1) {
      currentPage--;
      fetchData(currentPage, perPage);
    }
  }
</script>

<main>
  <div class="container">
    {#if isLoading}
    <div style="display: flex;justify-content: center;"
    class="m-3" 
    >
      <Spinner
    style="color:orange;width: 100px;height: 100px;"
    />
    </div>
    <p style="font-weight: bold;text-align: center;">Sedang Menyiapkan data ...</p>

    {:else}
    <div class="row">
      {#each data as item (item.thumbnail)}
       <Link to={`/player/${item.file_code}`}>
       <div class="col-md-3 col-12">
          <Card class="p-1 mt-3" style="background-color: #d9d2d6;">
          <img src={item.thumbnail} class="card-img-top" alt="File Thumbnail">
          <CardTitle>{item.title}</CardTitle>
          <CardText>
            <p>Ditonton : {item.views}</p>
            <p>Diupload : {item.uploaded}</p>
          </CardText>
          <Link to={`/player/${item.file_code}`}  class="btn "
        style="background-color: orange;
        color: black;
        font-weight: bold;"
          >Nonton</Link>
        </Card>
       </div>
     </Link>
      {/each}
       <div class="d-flex justify-content-between mt-3">
        <Button on:click={previousPage} disabled={currentPage === 1}>Previous</Button>
        <Button on:click={nextPage}>Next</Button>
      </div>
      </div>
    {/if}
  </div>
</main>

