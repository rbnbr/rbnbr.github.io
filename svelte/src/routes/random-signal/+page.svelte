<script>
    import * as d3 from "d3";

    let signal = 120;

    let arr = [];

    let maxLength = 100;

    for (let i = 0; i < maxLength; i++) {
        arr.push(signal);
    }

    let width = 1200;
    let height = 450;

    let t = Date.now();

    setInterval(() => {
        let p = Math.random();

        if (p < 0.05) {
            signal += Math.round(Math.random() * 10);
            // arr.push(signal);
        }

        if (p > 0.95) {
            signal -= Math.round(Math.random() * 10);
            // arr.push(signal);
        }

        if (signal !== arr[arr.length-1]) {
            arr[arr.length-1] = signal;
            t = Date.now();
        }
    }, 100);

    setInterval(() => {
        arr.push(signal);

        if (arr.length > maxLength) {
            arr = arr.slice(arr.length - maxLength - 1, arr.length);
        }
    }, 1000);

    let xScale = d3.scaleLinear().domain([0, maxLength]).range([0, width]);
    let yScale = d3.scaleLinear().domain([-300, 300]).range([height, 0]);

    function ct(y) {
        debugger;
        return y - signal;
    }

    function addZero(v) {
        v = v.toFixed();
        if (v.length < 2) {
            return "0" + v;
        }

        return v;
    }

    function addZeros(v) {
        v = v.toFixed();
        if (v.length < 2) {
            return "00" + v;
        } else if (v.length < 3) {
            return "0" + v;
        }

        return v;
    }

    $: tt = new Date(t);

</script>

<div id="signal">
    <h1>Random Signal: {signal}</h1>
    <h2>Time of last signal: {`${addZero(tt.getHours())}:${addZero(tt.getMinutes())}:${addZero(tt.getSeconds())}.${addZeros(tt.getMilliseconds())}`}</h2>
</div>

<svg width={width} height={height}>
    {#each arr.slice(0, -1) as s, idx}
        <line x1={xScale(idx)} x2={xScale(idx+1)} y1={yScale(ct(s))} y2={yScale(ct(arr[idx+1]))} stroke="blue"></line>
    {/each}

    {#each arr as s, idx}
        <circle cx={xScale(idx)} cy={yScale(ct(s))} fill="blue" r=2></circle>
    {/each}
</svg>

<style>
    svg {
        border: 1px black solid;
    }
</style>
