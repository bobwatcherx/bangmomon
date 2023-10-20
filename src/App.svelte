<script>
  import { Router, Link, Route } from "svelte-navigator";
  import Home from './lib/Home.svelte'
  import Player from './lib/Player.svelte'
  import Navbarmenu from './lib/util/Navbarmenu.svelte'

</script>
<Router>
  <div>
    <Navbarmenu/>
    <Route path="/">
      <Home />
    </Route>
    <Route path="/player/:id" let:params>
      <Player id={params.id} />
    </Route>
  </div>
</Router>