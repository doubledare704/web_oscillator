<script>
    import {Router, Route, Link} from "svelte-routing";
    import SimpleOsc from "./pages/SimpleOsc.svelte";

    export let url = ""; //This property is necessary declare to avoid ignore the Router
</script>

<Router url="{url}">
    <nav>
        <Link to="/">SimpleOsc</Link>
    </nav>
    <div>
        <Route path="/">
            <SimpleOsc/>
        </Route>
    </div>
</Router>
<style>
    .js-oscilloscope > svg > path {
        fill: none;

        stroke: #000000;
        stroke-width: 2px;
    }
</style>