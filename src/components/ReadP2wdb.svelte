<script lang="ts">
  import { Read } from "p2wdb";

  const read = new Read();
  let res;
  let usrData:string;
  let latestEntry: number;
//   let txid; //="acace5dd0aa7590e101c7db1f58abdbfb417f2b2c16f35486ece11467bad0c6a"
  let result;

  const getData = async (txidOrHash: string) => {
    try {
      // Get the second page of results.
      // const results = await read.getPage(2);
      txidOrHash.length === 64
        ? (result = await read.getByTxid(txidOrHash))
        : (result = await read.getByHash(txidOrHash));

      // Get the latest 20 entries in the database.
      // const result = await read.getPage(latestEntry); // default: page = 0
      res = [result];
      console.log(res);
    } catch (err) {
      console.error(err);
    }
  };
</script>

<div class="btn">
  <input bind:value={usrData} placeholder="txid or hash" />
  <button on:click={getData(usrData)}>submit </button>
</div>

<section class="features">
  <div class="container">
    {#if res != undefined}
      {#await res}
        <h3>...waiting</h3>
      {:then items}
        <div>
          {#each items as item}
            <h3>appID</h3>
            <p>{item.appId}</p>
            <!-- <p>created at {Unix_timestamp(item.createdAt)}</p> -->
            <h3>Hash</h3>
            <p id="hash">{item.hash}</p>
            <h3>Message</h3>
            <p>{JSON.parse(item.value.data).data.data}</p>
          {/each}
        </div>
      {:catch error}
        <p style="color: red">{error.message}</p>
      {/await}
    {/if}
  </div>
</section>

<style>
  div.btn {
    text-align: center;
    padding: 0.1em;
    margin: 0.1;
  }

  #hash {
    font-family: "Courier New", Courier, monospace;
    padding: 0.1em;
  }
  /*
  .btn {
    justify-content: center;
  } */

  .features .container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 20px;
    padding: 0 10px;
    margin: 10px auto;
  }
  @media (max-width: 500px) {
    .features .container {
      grid-template-columns: 1fr;
    }
  }
</style>
