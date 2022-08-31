<script lang="ts">
    import { Read, Write } from "p2wdb";

    const read = new Read();
    let res;
    let usrData = '';
    
   

    const getData = async () => {
        // Get the second page of results.
        // const results = await read.getPage(2);

        // Get the latest 20 entries in the database.
        const result = await read.getPage(); // default: page = 0
        res = result;
        console.log(res);
    };
 

</script>
<div class="btn">
    <button  on:click={getData}>Get Hashes</button>
</div>


{#if res != undefined}
    {#await res}
        <p>...waiting</p>
    {:then items}
    <div>
        {#each items as item}
            <p id="hash">{item.hash}</p>
        {/each}
    </div>
    {:catch error}
        <p style="color: red">{error.message}</p>
    {/await}
{/if}

<style>
    div {
        text-align: center;
        padding: 1em;
        margin: 0 auto;
    }
    #hash {
        font-family: 'Courier New', Courier, monospace;
    }
 

    .btn {
        justify-content: center;
        
    }
</style>
