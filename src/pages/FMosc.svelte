<script>

    import * as Tone from "tone";
    import {Dial, Slider, Toggle} from "nexusui";

    let pageName = "FM synthesis";
    let location = ""

    function ready() {
        const fmAmp = new Tone.Volume({
            volume: -6
        }).toDestination();

        const fmFilter = new Tone.AutoFilter({
            frequency: 0,
            depth: 0,
            baseFrequency: 200
        }).connect(fmAmp);

        const fmOsc = new Tone.FMOscillator({
            frequency: 0,
            modulationIndex: 0,
            modulationType: 'square',
            harmonicity: 1
        }).connect(fmFilter);

        fmOsc.start();
        fmFilter.start();

// FM Oscillator controls
        const fmFreqDial = new Dial('#fm-freq-dial', {
            'interaction': 'radial', // "radial", "vertical", or "horizontal"
            'mode': 'relative', // "absolute" or "relative"
            'min': 0,
            'max': 1000,
            'step': 0,
            'value': 0
        });


        fmFreqDial.on('change', function (x) {
            fmOsc.frequency.value = x;
        });

        const fmModDial = new Dial('#fm-mod-index', {
            'interaction': 'radial', // "radial", "vertical", or "horizontal"
            'mode': 'relative', // "absolute" or "relative"
            'min': 0,
            'max': 20,
            'step': 0,
            'value': 0
        });


        fmModDial.on('change', function (x) {
            fmOsc.modulationIndex.value = x;
        });


// FM Amplitude controls
        const ampSlider = new Slider('#amp-slider', {
            'mode': 'relative', // 'relative' or 'absolute'
            'min': -24,
            'max': 12,
            'step': 0,
            'value': -24
        });


        ampSlider.on('change', function (x) {
            fmAmp.volume.value = x;
        });

        const filterFreqSlider = new Slider('#filter-freq-slider', {
            'mode': 'relative', // "absolute" or "relative"
            'min': 0,
            'max': 40,
            'step': 0,
            'value': 0
        });


        filterFreqSlider.on('change', function (x) {
            fmFilter.frequency.value = x;

        });

        const filterResSlider = new Slider('#filter-res-slider', {
            'mode': 'relative', // 'relative' or 'absolute'
            'min': 0,
            'max': 1,
            'step': 0,
            'value': 0
        });


        filterResSlider.on('change', function (x) {
            fmFilter.depth.value = x;
        });

//Noise Filter
        const noise = new Tone.Noise('white');
        const noiseFilter = new Tone.AutoFilter({
            frequency: 0,
            depth: 0,
            baseFrequency: 200
        }).connect(fmAmp.volume);
        noise.connect(noiseFilter);


// Noise controls
        const noiseFreqSlider = new Slider('#noise-freq-slider', {
            'mode': 'relative', // "absolute" or "relative"
            'min': 0,
            'max': 40,
            'step': 0,
            'value': 0
        });


        noiseFreqSlider.on('change', function (x) {
            noiseFilter.frequency.value = x;
        });

        const noiseDepthSlider = new Slider('#noise-depth-slider', {
            'mode': 'relative', // 'relative' or 'absolute'
            'min': 0,
            'max': 1,
            'step': 0,
            'value': 0
        });


        noiseDepthSlider.on('change', function (x) {
            noiseFilter.depth.value = x;
        });

// Power noise/filter on
        const filterSwitch = new Toggle('#filter-switch', {
            'state': false
        });


        filterSwitch.on('change', function (x) {
            if (x === true) {
                noise.start();
                noiseFilter.start();
            } else {
                noise.stop();
                noiseFilter.stop();

            }
        });
    }


    document.addEventListener("DOMContentLoaded", ready);
</script>

<main>
    <h1> {pageName}!</h1> {location}
    <p>Welcome this is my <b>{pageName}</b></p>
    <div>
        <div id="fm-freq-dial"></div>
        <label>FM Frequency</label>
        <div id="fm-mod-index"></div>
        <label>FM Mod Depth</label>
        <br><br>

        <div id="filter-freq-slider"></div>
        <label>Filter Frequency</label>
        <div id="filter-res-slider"></div>
        <label>Filter Resonance</label>
        <br><br>

        <div id="amp-slider"></div>
        <label>Volume Level</label>
        <br><br>

        <div id="filter-switch"></div>
        <label>Noise Switch</label>
        <div id="noise-freq-slider"></div>
        <label>Noise Frequency</label>
        <div id="noise-depth-slider"></div>
        <label>Noise Depth</label>
    </div>
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

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>