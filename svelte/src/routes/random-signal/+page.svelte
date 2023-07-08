<script>
    import * as d3 from "d3";

    let signal = 100;

    let arr = [];

    let maxLength = 100;

    for (let i = 0; i < maxLength; i++) {
        arr.push(signal);
    }

    let width = 500;
    let height = 350;

    setInterval(() => {
        let p = Math.random();

        if (p < 0.05) {
            signal += 1;
            // arr.push(signal);
        }

        if (p > 0.95) {
            signal -= 1;
            // arr.push(signal);
        }
    }, 100);

    setInterval(() => {
        arr.push(signal);

        if (arr.length > maxLength) {
            arr = arr.slice(arr.length - maxLength - 1, arr.length);
        }
    }, 1000);

    let xScale = d3.scaleLinear().domain([0, maxLength]).range([0, width]);
    let yScale = d3.scaleLinear().domain([-100, 100]).range([height, 0]);

    function ct(y) {
        debugger;
        return y - signal;
    }

</script>

<h1>Random Signal: {signal}</h1>

<svg width={width} height={height}>
    {#each arr as s, idx}
        <circle cx={xScale(idx)} cy={yScale(ct(s))} fill="blue" r=3></circle>
    {/each}
</svg>

<style>
    svg {
        border: 1px black solid;
    }
</style>
