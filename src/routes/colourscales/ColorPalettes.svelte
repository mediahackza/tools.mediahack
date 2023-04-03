<script>
    export let palette
    import { colors } from './colors.js'
    let cols = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    import chroma from 'chroma-js'
    function getColor(palette, value) {
      let scale = chroma.scale(palette).domain([0, 9])
      console.log(scale(value))
  
      return scale(value)
    }
  </script>
  
  <div class="color-palettes">
    {#each colors as c}
      <div
        class="color-row"
        on:click={() => {
          clickColor(c)
        }}
      >
        <div class="color-name">{c}</div>
        <div class="colors-boxes">
          {#each cols as col}
            <div class="color-box" style="background: {getColor(c, col)};" />
          {/each}
        </div>
      </div>
    {/each}
  </div>
  
  <style>
    .color-row {
      padding-top: 5px;
      padding-bottom: 5px;
    }
    .color-row * {
      display: inline-block;
    }
    .color-box {
      width: 20px;
      height: 10px;
      /* border: solid 1px lightgray; */
      padding: 5px;
    }
    .color-name {
      width: 70px;
      text-align: right;
      margin-right: 10px;
    }
    .color-palettes {
      margin-top: 50px;
      margin-bottom: 100px;
    }
  </style>
  