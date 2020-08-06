<script>

    import {FMSynth, AMSynth, Loop, Transport, Oscillator, context} from "tone";
    import Oscilloscope from "../oscilloscope"

    let selected = "sine4", freq = 56, play = false, volume;
    let osc = new Oscillator(freq, selected);
    let oscilloscope;
    let oscTypesSine = ["sine1", "sine2", "sine3", "sine4", "sine5", "sine6", "sine7", "sine8", "sine9",
        "sine10", "sine11", "sine12", "sine13", "sine14", "sine15", "sine16", "sine17", "sine18", "sine19",
        "sine20", "sine21", "sine22", "sine23", "sine24", "sine25", "sine26", "sine27", "sine28", "sine29",
        "sine30", "sine31", "sine32"];
    const squares = [
        "square1", "square2", "square3", "square4", "square5", "square6", "square7", "square8", "square9",
        "square10", "square11", "square12", "square13", "square14", "square15", "square16", "square17", "square18", "square19",
        "square20", "square21", "square22", "square23", "square24", "square25", "square26", "square27", "square28", "square29",
        "square30", "square31", "square32"]

    const SawtoothWithPartials = [
        "sawtooth1", "sawtooth2", "sawtooth3", "sawtooth4", "sawtooth5", "sawtooth6", "sawtooth7", "sawtooth8", "sawtooth9",
        "sawtooth10", "sawtooth11", "sawtooth12", "sawtooth13", "sawtooth14", "sawtooth15", "sawtooth16", "sawtooth17", "sawtooth18", "sawtooth19",
        "sawtooth20", "sawtooth21", "sawtooth22", "sawtooth23", "sawtooth24", "sawtooth25", "sawtooth26", "sawtooth27", "sawtooth28", "sawtooth29",
        "sawtooth30", "sawtooth31", "sawtooth32"]

    const TriangleWithPartials = [
        "triangle1", "triangle2", "triangle3", "triangle4", "triangle5", "triangle6", "triangle7", "triangle8", "triangle9",
        "triangle10", "triangle11", "triangle12", "triangle13", "triangle14", "triangle15", "triangle16", "triangle17", "triangle18", "triangle19",
        "triangle20", "triangle21", "triangle22", "triangle23", "triangle24", "triangle25", "triangle26", "triangle27", "triangle28", "triangle29",
        "triangle30", "triangle31", "triangle32"]

    function playExample() {
        // create two monophonic synths
        const synthA = new FMSynth().toDestination();
        const synthB = new AMSynth().toDestination();
        //play a note every quarter-note
        const loopA = new Loop(time => {
            synthA.triggerAttackRelease("C2", "8n", time);
        }, "4n").start(0);
        //play another note every off quarter-note, by starting it "8n"
        const loopB = new Loop(time => {
            synthB.triggerAttackRelease("C4", "8n", time);
        }, "4n").start("8n");
        // all loops start until the Transport is started
        Transport.start()
    }

    function ready() {
        document.querySelector('button').addEventListener('click', async () => {
            osc.toDestination().start();
            play = true;
            volume = osc.volume.value
            if (!oscilloscope) {
                oscilloscope = new Oscilloscope('.js-oscilloscope', context);
                document.querySelector('path').setAttribute("style", "fill: none;stroke: #000000; stroke-width: 2px;");
            }
            osc.connect(oscilloscope.analyserNode);
            oscilloscope.start();
        })
    }


    function toggleOsc() {
        osc.stop()
        oscilloscope.stop();
    }

    function toggleOscType() {
        osc.type = selected
    }

    function changeFreq() {
        if (play) {
            osc.stop()
            oscilloscope.stop()
        }
        osc = new Oscillator(freq, selected);
        osc.connect(oscilloscope.analyserNode);
        oscilloscope.start()

        if (play) {
            osc.toDestination().start();
        }
    }

    function upFreq(ev, change = 1) {
        freq += change
        changeFreq()
    }

    function downFreq(ev, change = 1) {
        if (freq > change) {
            freq -= change
            changeFreq()
        }
    }

    function changeVolume() {
        osc.volume.value = volume
    }

    document.addEventListener("DOMContentLoaded", ready);
</script>

<main>
    <button>
        play
    </button>
    <button on:click={toggleOsc}>
        stop
    </button>

    <select bind:value={selected} id="cars" on:change={toggleOscType}>
        {#each oscTypesSine as oscType}
            <option value={oscType}>{oscType}</option>
        {/each}
    </select>

    <select bind:value={selected} id="cars2" on:change={toggleOscType}>
        {#each squares as oscType}
            <option value={oscType}>{oscType}</option>
        {/each}
    </select>

    <select bind:value={selected} id="cars3" on:change={toggleOscType}>
        {#each SawtoothWithPartials as oscType}
            <option value={oscType}>{oscType}</option>
        {/each}
    </select>
    <select bind:value={selected} id="cars4" on:change={toggleOscType}>
        {#each TriangleWithPartials as oscType}
            <option value={oscType}>{oscType}</option>
        {/each}
    </select>
    <p>
        {selected}, {freq}, Change freq:
        <input id="freq" bind:value={freq} type="number" min="1" max="1000" on:change={changeFreq}>
        <button on:click={upFreq}>up+1</button>
        <button on:click={downFreq}>down -1</button>
        <input class="range" type=range bind:value={freq} min=0 max=1000 on:change={changeFreq}>
    </p>
    <div>
        <label for="volume">Volume of oscillator</label>
        <input class="range"
               id="volume"
               type=range bind:value={volume} min=-12 max=12 on:change={changeVolume}>
    </div>
    <div class="js-oscilloscope"></div>
</main>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    .range {
        width: 100%;
    }

    @media (min-width: 320px) {
        main {
            max-width: none;
        }

        .js-oscilloscope {
            width: 100%;
        }
    }
    .js-oscilloscope > svg > path {
        fill: none;

        stroke: #000000;
        stroke-width: 2px;
    }

    .js-oscilloscope {
        width: 50%;
        height: 400px;
        max-height: 100%;
        margin: 0 auto;
        background-color: #f3f6fa;
        border: solid 1px #dce6f0;
        border-radius: 0.3rem;
    }
</style>
