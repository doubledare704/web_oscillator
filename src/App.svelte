<script>
    import {Router, Route, Link} from "svelte-routing";
    import SimpleOsc from "./pages/SimpleOsc.svelte";
    import Home from "./pages/Home.svelte";
    import FMosc from "./pages/FMosc.svelte";

    export let url = ""; //This property is necessary declare to avoid ignore the Router
</script>

<Router url="{url}">
    <nav>
        <Link to="/">Home</Link>
        <Link to="simpleosc">SimpleOsc</Link>
        <Link to="fmosc">FMosc</Link>
    </nav>
    <div>
        <Route path="simpleosc" component="{SimpleOsc}"/>
        <Route path="fmosc" component="{FMosc}"/>
        <Route path="/">
            <Home/>
        </Route>
    </div>
</Router>