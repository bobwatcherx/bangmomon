<script>
  import { Router, Link, Route } from "svelte-navigator";
  import Home from './lib/Home.svelte'
  import Player from './lib/Player.svelte'
  import Request from './lib/Request.svelte'
  import Navbarmenu from './lib/util/Navbarmenu.svelte'
</script>
<Router>
  <div>
    <Navbarmenu/>
    <div class="m-2">
      <Link 
      class="btn btn-danger"
      to="/request">Request Bokep Jepang Disini</Link>
    </div>
    <Route path="/">
      <Home />
    </Route>
     <Route path="/request">
      <Request />
    </Route>
    <Route path="/player/:id" let:params>
      <Player id={params.id} />
    </Route>
  </div>
</Router>