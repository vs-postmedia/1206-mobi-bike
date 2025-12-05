<script>
    // COMPONENTS
    import { onMount } from 'svelte';
    import { csvParse } from 'd3-dsv';
	import Map from './components/Map.svelte';

    
    /// VARIABLES
    let data;
    const dataUrl = 'https://vs-postmedia-data.sfo2.digitaloceanspaces.com/misc/mobi-top-bike-data.csv';
    // create .env in root dir & add VITE_MAPTILER_API_KEY
    const apiKey = import.meta.env.VITE_MAPTILER_API_KEY; 

    


    async function fetchData(url) {
        const resp = await fetch(url);
        const data = await resp.text();
        
        return csvParse(data);
    }

    async function init() {
        // fetch remote data
        data = await fetchData(dataUrl);
    }

    onMount(init);
</script>

<header>
    <h1>The little bike that could</h1>
    <p class="subhead">Watch Mobi’s busiest shared bike travel 17,330 kilometers from 2017-2025 .</p>
</header>

<main>
    {#if data}
        <Map
            data={data}
            apiKey={apiKey}
        />
    {/if}
</main>

<footer>
    <!-- <p class="note">NOTE: tk.</p> -->
    <p class="source">Source:  <a href="https://www.mobibikes.ca/en/system-data" target="_blank">Mobi</a></p>
</footer>
  
<style>
    @import '$css/normalize.css';
    @import '$css/fonts.css';
    @import '$css/colors.css';
    @import '$css/app.css';

    header {
		margin-bottom: 2rem;
	}
	header .subhead {
		max-width: 525px;
	}
</style>
