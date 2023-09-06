<script>
    export let option = "none";

    import PocketBase from 'pocketbase';
    import { pbStore } from 'svelte-pocketbase';
    
    const pb = new PocketBase("http://127.0.0.1:8090");

    async function getInstances() {
        option = "give";
            
        const resultList = await pb.collection('instances').getFullList();
        const instances = resultList.data;

        const instancesLanding = document.getElementById("instances-landing");

    }
</script>

<div class="article container pt-5">
    <div class="card">
        <div class="card-header">
            <h1 id="title">PronounceIt</h1>
            <p class="site-description">Helping people get it right first time.</p>
        </div>
        <div class="pt-4 pb-5 card-body text-center">
            {#if option == "none"}
                <h2>What do you want to do?</h2>
                <div class="row pt-5">
                    <div class="col-sm">
                        <button on:click={getInstances} class="btn btn-primary btn-lg">Give Pronunciation</button>
                    </div>
                    <div class="col-sm">
                        <button on:click={() => option = "ask"} class="btn btn-info btn-lg">Ask for Pronunciations</button>
                    </div>
                </div>
            {:else if option == "give"}
                <h2>Give a Pronunciation</h2>
                
                <div class="card-deck" id="instances-landing">

                </div>

                <button on:click={() => option = "none"} class="btn btn-primary">Back</button>
            {:else if option == "ask"}
                <h2>Ask for a Pronunciation</h2>
                <button on:click={() => option = "none"} class="btn btn-primary">Back</button>
            {:else}
                <h2>Something went wrong</h2>
                <p>Sorry about that, please try again later.</p>
                <button on:click={() => option = "none"} class="btn btn-primary">Back</button>
            {/if}
        </div>
        <div class="card-footer row">
            <h5 class="col-sm">Written by Kieran Roper</h5>
            <span class="col-sm text-center"></span>
            <h5 class="col-sm text-end">Github</h5>
        </div>
    </div>
</div>