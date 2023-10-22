<script>
  import { onMount } from 'svelte';
  import {Spinner, Card, CardText, CardTitle} from 'sveltestrap';

  let relatedVideos = [];
  import {Link} from 'svelte-navigator';

  export let id;
  let fileInfo = null;

  onMount(async () => {
    getdetail();
    getrelated();
  });

  async function getdetail(){
    const fileCode = id;
    const response = await fetch(`https://filemoon.sx/api/file/info?key=26604oo759r7ar6j96q7h&file_code=${fileCode}`);
    const json = await response.json();

    fileInfo = json.result[0];
  }

   async function getrelated(){
    const response = await fetch('https://filemoon.sx/api/file/list?key=26604oo759r7ar6j96q7h');
    const json = await response.json();

    const totalResults = json.result.results_total;
    const totalPages = json.result.pages;

    let randomPage = Math.floor(Math.random() * totalPages) + 1;

    const randomResponse = await fetch(`https://filemoon.sx/api/file/list?key=26604oo759r7ar6j96q7h&page=${randomPage}&per_page=35`);
    const randomJson = await randomResponse.json();
    relatedVideos = randomJson.result.files; // Memperbarui akses ke `files`
  }

  function gotopage(myid){
    id = myid;
    fileInfo = null;
    relatedVideos = [];
    getdetail();
    getrelated();
  }
</script>

<div class="container mt-3">
  <Link class="btn btn-primary" to="/" target="_blank">Kembali</Link>
</div>

<div class="mt-2">
  {#if fileInfo}
    <div>
      <iframe src={`https://filemoon.sx/e/${fileInfo.file_code}/${fileInfo.file_title}`} width="100%" height="300px" frameborder="0" allowscrolling="no" allowfullscreen></iframe>
      <div class="container">
        <p style="font-weight:bold">{fileInfo.file_title}</p>
        <a href={`https://filemoon.sx/d/${fileInfo.file_code}/${fileInfo.file_title}`} style="width: 100%;" target="_blank" class="btn btn-primary mt-2 mb-2">Download Bokep</a>
      </div>
    </div>
  {:else}
    <div style="display: flex;justify-content: center;" class="m-3">
      <Spinner style="color:orange;width: 100px;height: 100px;" />
    </div>
  {/if}
</div>

<!-- RELATED -->
<div class="row m-2">
  <h4 style="font-weight:bold">Video Lainnya</h4>
  {#if fileInfo == null}
    <div style="display: flex;justify-content: center;" class="m-3">
      <Spinner style="color:orange;width: 100px;height: 100px;" />
    </div>
  {:else}
    {#each relatedVideos as video (video.thumbnail)}
      <div class="col-md-4 col-12">
        <Card class="p-1 mt-3" style="background-color: #d9d2d6;" on:click={()=>gotopage(video.file_code)}>
          <img src={video.thumbnail} class="card-img-top" alt="File Thumbnail">
          <CardTitle>{video.title}</CardTitle>
          <CardText>
            <p>Ditonton: {video.views}</p>
            <p>Diupload: {video.uploaded}</p>
          </CardText>
          <button on:click={()=>gotopage(video.file_code)} target="_blank" class="btn" style="background-color:orange;color: black;font-weight: bold;">Nonton Sekarang</button>
        </Card>
      </div>
    {/each}
  {/if}
</div>
