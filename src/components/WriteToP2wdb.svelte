<script lang="ts">
    import {  Write } from "p2wdb";
 
    let res;
    let usrData:string;
    let WIF:string
    
    // ENTER your WIF
    // const WIF = "...";
    async function writeNode(usrData:string,wif:string) {
        try {
            const write = new Write({ wif: WIF });

            // Generate the data that will be written to the P2WDB.
            const appId = "test";
            const data = {
                now: new Date(),
                data: usrData,
            };

            const result = await write.postEntry(data, appId);
             res=result;
            console.log(
                `Data about P2WDB write: ${JSON.stringify(result, null, 2)}`
            );
            console.log("USER DATA", usrData)
        } catch (err) {
            console.error(err);
        }
    }
</script>
<div>
    <input bind:value={WIF} placeholder="enter wif">
    <input bind:value={usrData} placeholder="write data to p2wdb">
    <button  on:click={()=>writeNode(usrData,WIF)}>submit</button>
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
    .btn {
        justify-content: center;
        
    }

</style>
