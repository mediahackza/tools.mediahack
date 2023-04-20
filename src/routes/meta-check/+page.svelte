<script>
	import { onMount } from "svelte";

    let metaData = []
    let url  = 'https://perfectstorm.theoutlier.co.za'
    let tags = []

    async function checkMeta() { 
        let metaUrl = 'https://api.mediaverse.co.za/metacheck/?url=' + url
        console.log(metaUrl);
        
        let resp = await fetch(metaUrl)
        tags = await resp.json()
      
        console.log(tags);
        
  
    }

    onMount(() => { 
        checkMeta()
    })

</script>


<main>
    <h1>Meta Tag Checker</h1>
    Check the meta tags on a page by adding a full URL below:<br/>
    <div class="meta-block">
        <input type="text" placeholder="https://example.com" bind:value={url} />
        <button on:click={checkMeta}>Check</button>
        <div class="meta-wrap">
           <div class="row">
            <div class="tag-label">Description:</div>
            <div class="tag-detail">{tags.description}</div>
            <div class="tag-label">Twitter Title:</div>
            <div class="tag-detail">{tags['twitter:title']}</div>
            <div class="tag-label">Twitter Description:</div>
            <div class="tag-detail">{tags['twitter:description']}</div>
            <div class="tag-label">Twitter Image:</div>
            <div class="tag-detail">{tags['twitter:image']}</div>
            <div class="tag-label">Twitter Image Alt:</div>
            <div class="tag-detail">{tags['twitter:image:alt']}</div>
            <div class="tag-label">Twitter Card:</div>
            <div class="tag-detail">{tags['twitter:card']}</div>
            <div class="tag-label">Twitter Site:</div>
            <div class="tag-detail">{tags['twitter:site']}</div>
            <div class="tag-label">Twitter Creator:</div>
            <div class="tag-detail">{tags['twitter:creator']}</div>

           </div>
            <!-- {#each tags as tag}
                <div class="meta-tag">
                    <div class="meta-name">{tag.name}</div>
                    <div class="meta-content">{tag.content}</div>
                </div>
            {/each} -->
        </div>
    </div>
</main>

<style>
    main { 
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        width: 700px; 
        margin-left: auto;
        margin-right: auto;
        padding-bottom: 100px;
        margin-top: 50px;
    }
    input { 
        padding: 6px 10px;
        width: 300px
    }
    button {
     background: dodgerblue;
     color: #fff;
     padding: 10px 20px;
     border: none;
     border-radius: 5px;
     text-transform: uppercase;
     font-weight: 700;
     cursor: pointer;
     font-size: 0.7rem;
     margin-right: 10px;
}
button:hover { 
    background:rgb(12, 113, 215);
}
.meta-block { 
    margin-top: 30px;
}
.meta-wrap { 
    margin-top: 30px;
}
.tag-label { 
    font-weight: 700;
    margin-top: 10px;
}
.tag-detail { 
    color: gray;
}
</style>