<script>
    function formatHTML(html) {
    var tab = '\t';
    var result = '';
    var indent= '';

    html.split(/>\s*</).forEach(function(element) {
        if (element.match( /^\/\w/ )) {
            indent = indent.substring(tab.length);
        }

        result += indent + '<' + element + '>\r\n';

        if (element.match( /^<?\w[^>]*[^\/]$/ ) && !element.startsWith("input")  ) { 
            indent += tab;              
        }
    });

    return result.substring(1, result.length-3);
}

    let tableCode = ''
    let source = ""
    import outlierlogo from '$assets/outlierlogo.png'
    let sampleData = `University,Stage3,Stage4,Stage5,Stage 6
CPUT,"R30,500","R44,650","R56,400","R82,250"
UCT,"R98,510","R131,346","R164,183","R197,019"
DUT,"R5,870","R7,044","R11,741","R16,437"
UFS,"R7,747","R15,494","R20,659","R25,824"
UJ,"R178,494","R535,483","R713,978","R892,473"
NMU,"R75,600","R100,800","R108,360","R115,920"
NWU,"R196,800","R295,200","R392,600","R492,000"
UP,"R1,200,933","R1,501,167","R1,801,400","R2,201,711"
Sefako Makgatho University,"R1,233","R1,513","R2,104","R2,603"
Sol Plaatje University,"R3,880","R7,276","R7,276","R8,731"
Stellenbosch University,"R194,221","R234,576","R288,473","R342,739"
TUT,"R26,250","R39,375","R42,000","R42,000"
UWC,"R21,871","R33,333","R41,667","R49,911"`

    // default config
    let tableWidth = 600
    let headerColor = '#BC302F'
    let headerTextColor = '#fff'

    let columns = []

    // options
    let colors = ["#91BFDB","#4575B4","#609E79","#E0F3F8","#FEE090","#FC8D59","#BE3131","#7A7A7A","#D3D3D3","#FFFFFF","#000000","#D9D9D9"]
    
    let data = ""
    let outputdata = []
    let headers = ""
    let body = ""

    let result = []

    function getRows() { 

        outputdata = []  
        headers = []    
        result = []  
 
        outputdata = data.split('\n')
        headers = outputdata[0].split(',')
        headers.forEach((d,i) => { 
            columns[i] = { name: d, align: 'left' }
         })
        body = outputdata.slice(1)

        body.forEach(b => { 
        let obj = {}

        let str = b
        let s = ''


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

 
  // Split the string using pipe delimiter |
  // and store the values in a properties array
  let properties = s.split("|")

result.push(properties)
})


 
    }

   

    
    
    
    function handleAlignChange(event, column) {
    column.align = event.target.value;
    columns = [...columns];
  }

  function addSampleData() { 
    data = sampleData
  }

  setInterval(() => { 
    tableCode = formatHTML(document.getElementById('table').innerHTML)
  }, 1000)
    
</script>

<svelte:head><title>Table Maker</title></svelte:head>

<main>
    <h1>Table Maker</h1>
    <div class="example">Try some <span class="sample-link" on:click={addSampleData}>Sample Data</span></div>
    <textarea placeholder="Add CSV text here" bind:value={data}></textarea>
    <div class="sources"><span class="label">Source:</span > <input type="text" bind:value={source}></div>
    <div class="buttons">
        <button on:click={getRows}>Update Table</button>
        <button class="clear" on:click={() => data = []}>Clear Data</button>
    </div>
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

    </div>
</div>

<div class="result" style="width: {tableWidth}px;">
    {#if outputdata.length > 2}

        <div class="source">
           <table id="table">

           <thead>
                <tr class="options">
                    {#each columns as column}
                    <td>
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
                            {/each}
                        </tr>
                    {/each}
                </tbody>
                <tfoot>
                    <tr>
                        <td class="footer source" colspan="{Math.ceil(columns.length/2)}">
                        {#if source.length > 0}
                        Source: {source}
                        {/if}
                        </td>
                        <td class="footer logo" colspan="{Math.floor(columns.length/2)}">
                        <img src="{outlierlogo}"></td>
                    </tr>
                </tfoot>
           </table>
        </div>
    
    {/if}

    <div>
        <div id="fake_textarea" contenteditable><pre>{tableCode}</pre></div>
    </div>
</div>


<div class="bottom"></div>

<style>
    main { 
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        width: 700px; 
        margin-left: auto;
        margin-right: auto;
        padding-bottom: 100px;
        margin-top: 50px;
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
        color: #000;
        padding: 5px;
        border: solid 1px #eee;
        font-size: 0.8rem;
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

.example { 
    margin-bottom: 15px;
    font-size: 0.9rem;
}
.sample-link { 
    color: dodgerblue;
    text-decoration: underline;
    cursor: pointer;
}
.sample-link:hover { 
    color: indianred;
}
.buttons { 
    margin-top: 10px;
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
.clear { 
    background: lightgray;
    color: #000;
}
.clear:hover { 
  background: rgb(181, 181, 181);  
}
.footer { 
    
    border: none;
}
.footer img { 
    max-width: 100%;
}
.logo { 
    text-align: right;
}
.source { 
    color: rgb(148, 148, 148);
    font-size: 0.8rem;
    line-height: 1.2rem;
}
input { 
    padding: 5px;
    border: solid 1px lightgray;
    width: 400px;
    
}
.label { 
    text-transform: uppercase;
    font-size: 0.8rem;
    font-weight: 700;
}

</style>
