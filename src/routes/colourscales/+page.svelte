<script>
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
  
    import ColorPalettes from './ColorPalettes.svelte'
  
    $: inputValues = inputValues
    $: convertValues(values)
  
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
  </script>
  

  <svelte:head><title>Colour Scales</title></svelte:head>

  <main>
    <h1>ColourScales</h1>
    <div class="intro">
      A utility tool for generating colours based on a range of numbers. Replace
      the values in the "Number Range" with your selected numbers and add a
      minimum or maximum value for your range.
    </div>
    <div class="input-wrap">
      <div class="input-row">
        Start Value: <input type="text" bind:value={start} /> &nbsp; End Value:
        <input type="text" bind:value={end} /><button on:click={minMax}
          >Set Min-Max
        </button>
        <div class="info">
          <!-- svelte-ignore a11y-mouse-events-have-key-events -->
          <img
            src="./assets/icons/info.svg"
            alt="info"
            on:mouseover={() => {
              infoMinMax = !infoMinMax
            }}
            on:mouseout={() => {
              infoMinMax = !infoMinMax
            }}
          />
        </div>
        {#if infoMinMax}
          <div class="description">
            <div>
              You can either provide starting/ending values here, or you can
              calculate them automatically based on the numbers added below.
            </div>
          </div>
        {/if}
      </div>
      <div class="input-row">
        Number Range: <input
          type="text"
          class="values values-long"
          bind:value={values}
        />
        <div class="info">
          <!-- svelte-ignore a11y-mouse-events-have-key-events -->
          <img
            src="./assets/icons/info.svg"
            alt="info"
            on:mouseover={() => {
              infoRange = !infoRange
            }}
            on:mouseout={() => {
              infoRange = !infoRange
            }}
          />
        </div>
        {#if infoRange}
          <div class="description">
            <div>Add a list of comma-separated numbers here.</div>
          </div>
        {/if}
      </div>
      <div class="input-row">
        Choose Palette:
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
            <div class="label">{scale(c)}</div>
          </div>
        {/each}
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
      padding: 20px 10px;
      border: dotted 1px lightgray;
      /* background: #eee; */
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
      text-align: center;
    }
  </style>
  