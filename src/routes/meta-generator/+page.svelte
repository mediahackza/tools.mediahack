<script>
    let robots = "index, follow"
    let type = "article"
    let title = ""
    let description = ""
    let image = ""
    let url = ""
    let sitename = "The Outlier"
    // let twittertitle = "Perfect Storm: Durban floods, climate change and coastal resilience"
    // let twitterdescription = "An investigation of how climate change affects coastal cities and how floods in Durban, South Africa, are an example of how such cities can prepare for the future."
    // let twitterimage = "https://perfectstorm.theoutlier.co.za/perfect-storm-social-opt.png"
    let twitteraccount = "@outlierafrica"
    let canonical = "https://perfectstorm.theoutlier.co.za"
    
</script>


<main>
    <h1>Meta Tag Generator</h1>
    Generate meta tags for websites<br/>
    <div class="meta-block">

        <div class="input-tags">
            <table>
                <tr>
                    <td class="label">Robots</td><td><input type="text" bind:value={robots}/></td>
                </tr>
                <tr>
                    <td class="label">Type</td><td><input type="text" bind:value={type}/></td>
                </tr>
                <tr>
                    <td class="label">Title</td><td><input type="text" bind:value={title}/></td>
                </tr>
                <tr>
                    <td class="label">Description</td><td><textarea bind:value={description}></textarea></td>
                </tr>
                <tr>
                    <td class="label">Image</td><td><input type="text" bind:value={image}/></td>
                </tr>
                <tr>
                    <td class="label">URL</td><td><input type="text" bind:value={url}/></td>
                </tr>
                <tr>
                    <td class="label">Site Name</td><td><input type="text" bind:value={sitename}/></td>
                </tr>
                <tr>
                    <td class="label">Twitter Account</td><td><input type="text" bind:value={twitteraccount}/></td>
                </tr>
                
               


            </table>
        </div>


        <div class="generated-tags">
            <title>Perfect Storm: Durban floods, climate change and coastal resilience</title>
            <meta name="description" content="An investigation of how climate change affects coastal cities and how floods in Durban, South Africa, are an example of how such cities can prepare for the future.">
            <link rel="canonical" href="https://perfectstorm.theoutlier.co.za">
<pre>
    &lt;link rel="canonical" href="{url}">
    &lt;meta name="”robots”" content="{robots}">
    &lt;meta property="og:type" content="article">
    &lt;meta property="og:title" content="{title}">
    &lt;meta property="og:description" content="{description}">
    &lt;meta property="og:image" content="{image}">
    &lt;meta property="og:url" content="{url}">
    &lt;meta property="og:site_name" content="{sitename}">
    &lt;meta name="twitter:title" content="{title}">
    &lt;meta name="twitter:description" content="{description}}">
    &lt;meta name="twitter:image" content="{image}">
    &lt;meta name="twitter:site" content="{twitteraccount}">
    &lt;meta name="twitter:creator" content={twitteraccount}">
</pre>
            
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
    .label { 
        font-weight: 700;
        padding-right: 10px;
    }

    input, textarea { 
        padding: 5px;
        width: 400px;
    }
    textarea { 
        height: 100px;
    }
</style>