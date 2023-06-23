<script>
    let inputMin = null 
    let inputMax = null
    let outputMin = null
    let outputMax = null
    let inputValue = null
    import * as d3 from 'd3'
    let result = 0

    function convert() { 
        let scale = d3.scaleLinear()
            .domain([inputMin, inputMax])
            .range([outputMin, outputMax])
        result = scale(inputValue)
    }
</script>


<main>
    <h1>Scale Calculator</h1>
    <p>
        A simple calculator to convert an input range of numbers to an output range. <br/>
        Add your data minimum and maximum as well as an output minimum abnd maximum. <br/> 
        Then add the number you want to convert. 
    </p>

   <div class="row">
        Input minimum: <input type="text" bind:value={inputMin}>
        Input maximum: <input type="text" bind:value={inputMax}>
   </div>
   <div class="row">
        Output minimum: <input type="text" bind:value={outputMin}>
        Output maximum: <input type="text" bind:value={outputMax}>
   </div>
    <div class="row">Convert value: <input type="text" bind:value={inputValue}></div>
    <br/>
    <button on:click={convert}>Convert</button>
    <p>
        {result}
    </p>



</main>


<style>
    main {
      width: 700px;
      margin-left: auto;
      margin-right: auto;
      margin-top: 50px;
    }
    .row { 
        margin-top: 20px;
    }
    .row input { 
        padding: 5px;
        width: 70px;
        margin-right: 20px;
    }
</style>