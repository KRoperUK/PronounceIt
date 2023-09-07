<script>
    export let option = "none";

    import PocketBase from 'pocketbase';
	import { onMount } from 'svelte';
    import { pbStore } from 'svelte-pocketbase';
    
    const pb = new PocketBase("http://127.0.0.1:8090");

    let api_up = false;

    async function getUsernameById(id){
        let user = await pb.collection("users").getOne(id);
        return user.username;
    }

    function getInstances(){
        option = "give";
        pb.collection("instances").getFullList()
            .then((res) => {
                console.log(res);
                let instances = res;
                let instances_landing = document.getElementById("instances-landing");
                instances_landing.innerHTML = "";
                // instances_landing.innerHTML = "";
                for (let i = 0; i < instances.length; i++) {
                    let instance = instances[i];
                    let card = document.createElement("div");
                    card.className = "card card-instances";

                    let card_header = document.createElement("div");
                    card_header.className = "card-header";
                    card_header.innerHTML = instance.inviter;
                    card.appendChild(card_header);

                    let card_body = document.createElement("div");
                    card_body.className = "card-body";
                    card_body.innerHTML = instance.word;
                    card.appendChild(card_body);
                    instances_landing.appendChild(card);
                }
            })
            .catch((err) => {
                console.log(err);
            });
    }

    onMount(async () => {
        fetch("http://127.0.0.1:8090/api/health")
        .then((res) => {
            if(res.status == 200){
                api_up = true;
                console.log("API is up");
            }else{
                console.log("API is down");
            }
        })
        .catch((err) => {
            console.log("API is down");
            console.log(err);
        });
    });


</script>

<div class="pt-4 pb-5 card-body text-center">
    {#if !api_up}
        <div class="alert">
            <h2>API is down</h2>
        </div>
    {:else if option == "none"}
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
        <h2 class="mb-4">Give a Pronunciation</h2>
        
        <div class="card-deck" id="instances-landing">

        </div>

        <button on:click={() => option = "none"} class="btn btn-primary mt-4">Back</button>
    {:else if option == "ask"}
        <h2>Ask for a Pronunciation</h2>
        <button on:click={() => option = "none"} class="btn btn-primary">Back</button>
    {:else}
        <h2>Something went wrong</h2>
        <p>Sorry about that, please try again later.</p>
        <button on:click={() => option = "none"} class="btn btn-primary">Back</button>
    {/if}
</div>

<style>
    .card-instances {
        width: 18rem;
        height: 18rem;
        margin: 1rem;
    }
</style>