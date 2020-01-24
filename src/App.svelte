<script>
	import '../node_modules/materialize-css/dist/css/materialize.min.css';
	import '../node_modules/materialize-css/dist/js/materialize.min.js';

	import { Router, Route } from 'svelte-routing';
	import Navbar from './layouts/Navbar.svelte';

	import Home from './pages/Home.svelte';
	import About from './pages/About.svelte';	

	const props = {
		M: window.M,
		apiBaseUrl: "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev",
	}
</script>

<style>

</style>


<Router>
	<Navbar/>
	<div class="container">
		<Route path="/" component={Home} {...props} />
		<Route path="/about" component={About} {...props} />
	</div>	
</Router>

