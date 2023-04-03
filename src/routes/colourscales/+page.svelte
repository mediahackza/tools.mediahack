<script>
   import { Info } from 'lucide-svelte';
    let start = 0
    let end = 1000
    let palette
    let newPalette = palette
    import chroma from 'chroma-js'
    let scale = chroma.scale(palette).domain([start, end])
    let values = '100,200,300,400,500,600,700,800,900'
    let inputValues = []
    $: scale = chroma.scale(palette).domain([start, end])
    import { colors } from './colors.js'
    let string  = ''
  
    import ColorPalettes from './ColorPalettes.svelte'
  
    $: inputValues = inputValues
    $: convertValues(values)
    $: if(inputValues) { 
      string = ''
      inputValues.forEach(d => { 
        string = string + scale(d) + ','
      })
      string = string.replace(/,\s*$/, "");
      string = `[${string}]`
      
    }
    $: if(palette) { 
      string = ''
      convertValues(values)
      inputValues.forEach(d => { 
        string = string + scale(d) + ','
      })
      string = string.replace(/,\s*$/, "");
      string = `[${string}]`
      
    }

    // $: if(inputValues) { 
    //   inputValues.forEach(d => { 
    //     console.log(scale(d));
        
    //   })
    // }
  
    function convertValues(values) {
      let v = values.replaceAll(' ', '')
      inputValues = v.split(',')
    }
  
    function minMax() {
      start = Math.min(...inputValues)
      end = Math.max(...inputValues)
    }
    function updatePalette() {
      palette = newPalette
    }
  
    let infoRange = false
    let infoMinMax = false

    function calculateColor(value) { 
      return scale(value)
    }
  </script>
  

  <svelte:head><title>Colour Scales</title></svelte:head>

  <main>
    <h1>ColourScales</h1>
    <div class="intro">
      A simple tool for generating a collection of colours based on a collection of numbers.<br/>
     
    </div>
    <div class="input-wrap">
     
      <div class="input-row">
        <div class="section-title">Range</div>
        <div class="instructions">Set a minimum and maximum for your collection of numbers <strong>OR</strong> click the button to set the min/max automatically from your numbers in the "NUmbers" section   </div>
        <span class="label">Minimum:</span> <input class="input-small" type="text" bind:value={start} /> &nbsp; <span class="label">Maximum:</span>
        <input  class="input-small" type="text" bind:value={end} /><button on:click={minMax}
          >Set Min-Max
        </button>
        <!-- svelte-ignore a11y-mouse-events-have-key-events -->
       
      
      </div>
      <div class="input-row">
        <div class="section-title">Numbers</div>
        <div class="instructions">Add as many numbers as you want colours. A colour will be created for each number based on the range and palette you choose. Separate numbers with commas.</div>
        <span class="label">Numbers:</span> <input
          type="text"
          class="values values-long input-wide"
          bind:value={values}
        />
        <!-- svelte-ignore a11y-mouse-events-have-key-events -->
       
        
      </div>
      <div class="input-row">
        <div class="section-title">Palette</div>
        <div class="instructions">Select a palette from the list below. </div>
        <select bind:value={palette}>
          {#each colors as c}
            <option>{c}</option>
          {/each}
        </select>
      </div>
      <div class="colors">
        {#each inputValues as c}
          <div class="box-wrap">
            <div class="box" style="background: {scale(c)};" />
            <div class="label">{c}</div>
            <div class="label">{calculateColor(c)}</div>
          </div>
        {/each}
      </div>
   

      <div class="array">
        {string}
      </div>


    </div>
  
    <ColorPalettes {palette} />
  </main>
  
  <style>
    main {
      width: 700px;
      margin-left: auto;
      margin-right: auto;
      margin-top: 50px;
    }
    .input-row {
      position: relative;
      margin: 10px 0px;
      padding: 5px 20px;
      border: dotted 1px lightgray;
      /* background: #eee; */
      padding-bottom: 20px;
      margin-bottom: 20px;
      border-top: solid 1px gray;
    }
    .input-wrap { 
      margin-top: 20px;
      margin-bottom: 20px;
      padding-bottom: 20px;
     
    }
    .description {
      position: absolute;
      background: #000;
      padding: 10px;
      color: #fff;
      max-width: 300px;
      top: 0px;
      right: 30px;
      font-size: 0.8rem;
      margin-top: 5px;
      margin-bottom: 0px;
      z-index: 200;
    }
    .info {
      position: absolute;
      top: 5px;
      right: 5px;
      text-align: right;
      padding: 5px;
    }
    .info img {
      width: 15px;
      opacity: 0.3;
      cursor: pointer;
    }
    .info img:hover {
      opacity: 1;
    }
  
    .colors {
      display: grid;
      grid-template-columns: repeat(10, 1fr);
    }
    .box {
      width: 50px;
      height: 50px;
    }
    .label {
      font-size: 0.8rem;
      padding: 0px 10px 0px 0px;
      text-transform: uppercase;
      font-weight: 700;
    }

    input, button, select { 
      padding: 7px;
      margin-left: 5px; 
      margin-right: 5px;
    }
    button { 
      cursor: pointer;
    }
    .input-small { 
      width: 50px;
    }
    .input-wide { 
      width: 300px;
    }
    .instructions { 
      padding-top: 10px; 
      padding-bottom: 20px;
      font-style: italic;
      color: gray;
    }
    .section-title { 
      text-transform: uppercase;
      font-weight: 700; 
      margin-top: 10px;
      font-size: 1.2rem;

    }
    .color-palettes { 
      padding-bottom: 100px;
    }
    .array { 
      margin-top: 20px;
    }
  </style>
  