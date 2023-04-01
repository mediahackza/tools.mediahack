<script>
    // default config
    let tableWidth = 600
    let headerColor = '#BC302F'
    let headerTextColor = '#fff'

    let columns = []

    $: console.log(columns)

    // options
    let colors = ["#91BFDB","#4575B4","#609E79","#E0F3F8","#FEE090","#FC8D59","#BE3131","#7A7A7A","#D3D3D3","#FFFFFF","#000000","#D9D9D9"]
    


    let data = ""

    let outputdata = null
    let headers = ""
    let body = ""

    $: outputdata = data.split('\n')
    $: headers = outputdata[0].split(',')
    $: body = outputdata.slice(1)
    $: if(body) { 
        getRows()
    }

    let result = []
   

    function getRows() { 
        console.log('Running getRows');

        console.log(body)
        
        body.forEach(b => { 
        let obj = {}

        console.log(b)
        
 
       
        let str = b
        let s = ''

        console.log(str);
        
 
  // By Default, we get the comma separated
  // values of a cell in quotes " " so we
  // use flag to keep track of quotes and
  // split the string accordingly
  // If we encounter opening quote (")
  // then we keep commas as it is otherwise
  // we replace them with pipe |
  // We keep adding the characters we
  // traverse to a String s
  let flag = 0
  for (let ch of str) {
    if (ch === '"' && flag === 0) {
      flag = 1
    }
    else if (ch === '"' && flag == 1) flag = 0
    if (ch === ',' && flag === 0) ch = '|'
    if (ch !== '"') s += ch
  }
 console.log(s)
 
  // Split the string using pipe delimiter |
  // and store the values in a properties array
  let properties = s.split("|")

result.push(properties)
  
 
  // For each header, if the value contains
  // multiple comma separated data, then we
  // store it in the form of array otherwise
  // directly the value is stored
//   for (let j in headers) {
//     if (properties[j].includes(", ")) {
//       obj[headers[j]] = properties[j]
//         .split(", ").map(item => item.trim())
//     }
//     else obj[headers[j]] = properties[j]
//   }
 
  // Add the generated object to our
  // result array
//   result.push(zobj)
})

 console.log(result);
 
    }

   

    $: headers.forEach((d,i) => { 
        columns[i] = { name: d, align: 'left' }
    })
    
    
    function handleAlignChange(event, column) {
    column.align = event.target.value;
    columns = [...columns];
  }
    
</script>

<svelte:head><title>Table Maker</title></svelte:head>

<main>
    <h1>Table Maker</h1>
    
    <textarea placeholder="Add CSV text here" bind:value={data}></textarea>
</main>

<div class="settings">
    <div class="colors">
        <div class="settings-row">
            <div class="color-label">Header Colour</div>
            {#each colors as c}
            <div class="color" style="background: {c};" on:click={() => { headerColor = c}}></div>
            {/each}
        </div>
        <div class="settings-row">
            <div class="color-label">Header Text Colour</div>
            {#each colors as t}
            <div class="color" style="background: {t};" on:click={() => { headerTextColor = t}}></div>
            {/each}
        </div>

        <div class="settings-row settings-row-top">
            <div class="color-label">Table Width</div>
            <select bind:value={tableWidth}>
                <option>400</option>
                <option>600</option>
                <option>800</option>
                <option>1000</option>
            </select>
        </div>



        <!-- <div class="settings-row settings-row-top">
            <div class="color-label">Columns</div>
            {#each columns as c}
                <div class="color-label">{c.name}</div>
                <select bind:value={c.align}>
                    <option value="left">left</option>
                    <option value="right">right</option>
                    <option value="center">center</option>
                </select>
            {/each}
        </div> -->



        

    </div>
</div>

<div class="result" style="width: {tableWidth}px;">
    {#if outputdata.length > 2}

   
    
        <div class="source">
           <table>

           <thead>
                <tr class="options">
                    {#each columns as column}
                    <td>
                      <!-- <span>{column.title}</span> -->
                      <select value={column.align} on:change={(e) => handleAlignChange(e, column)}>
                        <option value="left">Left</option>
                        <option value="center">Center</option>
                        <option value="right">Right</option>
                      </select>
                    </td>
                  {/each}
                </tr>
           </thead>
                
                    <tr style="background: { headerColor }; color: { headerTextColor };">
                        {#each headers as header}
                            <th>{header}</th>
                        {/each}
                    </tr>
                
                <tbody>
                    {#each result as r}
                        <tr>
                            {#each r as cell, i}
                                <td style="text-align: {columns[i].align};">{cell}</td>
                                <!-- <td>{cell}</td> -->
                            {/each}
                        </tr>
                    {/each}
                </tbody>
           </table>
        </div>
    
    {/if}
</div>
<div class="bottom"></div>


<style>
    main { 
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        width: 600px; 
        margin-left: auto;
        margin-right: auto;
        padding-bottom: 100px;
    }
    textarea { 
        width: 100%;
        height: 200px;
    }
    .result { 
        margin-top: 30px;
        margin-left: auto;
        margin-right: auto;
    }

    table { 
        width: 100%;
        font-size: 0.9rem;
        border-collapse: collapse;
    }

    th { 
        text-transform: capitalize;
        /* background-color: #BC302F; */
        /* color: #fff; */
        padding: 5px;
        border: 1px solid #ddd;
        text-align: left;
        border: solid 1px #fff;
    }
    td { 
        padding: 5px;
        border: solid 1px #eee;
    }
    .settings { 
        margin-bottom: 30px;
        font-size: 0.8rem;
        text-transform: uppercase;
        max-width: 600px;
        /* border: solid 1px red; */
        margin-left: auto;
        margin-right: auto;
        
        /* padding: 20px; */
        /* border: solid 1px #eee; */
    }
    .colors * { 
        display: inline-block;
    }
    .color { 
        width: 20px; 
        height: 20px;
        margin: 3px;
        cursor: pointer;
        transform: translate(0, 8px);
        border: solid 1px lightgray;
    }
    .settings-row { 
        /* padding: 5px; */
        /* border: solid 1px #eee; */
    }
    .color-label {
        width: 150px;
        text-align: right;
    }
    .settings-row-top { 
        margin-top: 15px;
    }
    .bottom { 
        min-height: 100px;
        /* background: red; */
    }
    .options td { 
        padding-bottom: 20px;
        border: none;
    }

select { 
    padding: 5px;
}
</style>
