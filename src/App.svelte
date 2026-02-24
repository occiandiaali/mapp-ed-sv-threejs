<script>
  import { onMount } from 'svelte';
  import { createGlobe } from './lib/globe.js';

  let globe;
  let countryInput = '';
  let showDialog = false;
  let countryData = null;

  async function fetchCountry(name) {
    const res = await fetch(`https://restcountries.com/v3.1/name/${encodeURIComponent(name)}?fullText=true`);
    const data = await res.json();
    return data[0];
  }

  async function pinCountry() {
    if (!countryInput) return;
    const country = await fetchCountry(countryInput);
    if (!country) return alert('Country not found');
    globe.addMarker(country.name.common, country.latlng[0], country.latlng[1], async (name) => {
      countryData = await fetchCountry(name);
      showDialog = true;
    });
    globe.moveCameraTo(country.latlng[0], country.latlng[1]);
  }

  onMount(() => {
    const container = document.getElementById('globe');
    globe = createGlobe(container);
  });
</script>

<style>
  #globe { width: 100vw; height: 100vh; }
  .marker { color: red; font-weight: bold; cursor: pointer; }
  dialog { border: none; border-radius: 8px; padding: 20px; }
  dialog::backdrop { background: rgba(0,0,0,0.5); }
  #ui {
    position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%);
    background: rgba(255,255,255,0.8); padding: 10px; border-radius: 6px;
  }
</style>

<div id="globe"></div>

<div id="ui">
  <input bind:value={countryInput} placeholder="Enter country name" />
  <button on:click={pinCountry}>Pin Country</button>
</div>

{#if showDialog}
  <dialog open>
    <h2>{countryData.name.common}</h2>
    <p><strong>Capital:</strong> {countryData.capital?.[0] || 'N/A'}</p>
    <p><strong>Region:</strong> {countryData.region}</p>
    <p><strong>Population:</strong> {countryData.population.toLocaleString()}</p>
    <p><strong>Flag:</strong> {countryData.flag}</p>
    <button on:click={() => showDialog = false}>Close</button>
  </dialog>
{/if}
