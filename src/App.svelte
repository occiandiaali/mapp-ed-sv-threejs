<!-- <script>
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
{/if} -->

<!-- <script>
  import { onMount } from 'svelte';
  import { createGlobe } from './lib/globe.js';

  let globe;
  let countryInput = $state('');
  let showDialog = $state(false);
  let countryData = $state(null);

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

  function clearAll() {
    globe.clearMarkers();
    showDialog = false;
    countryData = null;
    countryInput = "";
  }

  onMount(() => {
    const container = document.getElementById('globe');
    globe = createGlobe(container);
  });
</script>

<style>
  #globe { width: 100vw; height: 100vh; }
  /* .marker { color: red; font-weight: bold; cursor: pointer; } */
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
  <button onclick={pinCountry}>Pin Country</button>
  <button onclick={clearAll}>Clear All</button>
</div>

{#if showDialog}
  <dialog open>
    <h2>{countryData.name.common}</h2>
    <p><strong>Capital:</strong> {countryData.capital?.[0] || 'N/A'}</p>
    <p><strong>Region:</strong> {countryData.region}</p>
    <p><strong>Population:</strong> {countryData.population.toLocaleString()}</p>
    <p><strong>Flag:</strong> {countryData.flag}</p>
    <button onclick={() => showDialog = false}>Close</button>
  </dialog>
{/if} -->


<script>
  import { onMount } from 'svelte';
  import { createGlobe } from './lib/globe.js';

  let globe;
  let countryInput = $state('');
  let showDialog = $state(false);
  let countryData = $state(null);
  let haveData = $state(false);

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
      if (countryData) {
        haveData = true;
      }
    });
    globe.moveCameraTo(country.latlng[0], country.latlng[1]);
    if (haveData) {
      showDialog = true;
    }
  }

  function clearAll() {
    globe.clearMarkers();
    showDialog = false;
    countryData = null;
    countryInput = "";
    haveData = false;
  }

  onMount(() => {
    const container = document.getElementById('globe');
    globe = createGlobe(container);
  });
</script>

<style>
  button {
    width: 80px;
    height: 38px;
    margin: 4px;
    border: none;
  }
  .pinBtn {
    background: black;
    color: white;
  }
  .clearBtn {
    background: red;
    color: white;
  }
    .pinBtn:hover {
    background: white;
    color: black;
    border: 1px solid black;
  }
    .clearBtn:hover {
    background: white;
    color: red;
    border: 1px solid red;
  }
  input {
    padding: 6px;
  }
  #globe { width: 100vw; height: 100vh; }
  dialog { border: none; border-radius: 8px; padding: 20px; }
  dialog::backdrop { background: rgba(0,0,0,0.5); }
  #ui {
    position: absolute; bottom: 10px; left: 50%; transform: translateX(-50%);
    background: rgba(255,255,255,0.3); padding: 10px; border-radius: 6px;
  }
</style>

<div id="globe"></div>

<div id="ui">
  <input bind:value={countryInput} placeholder="Enter country name" />
  <button class="pinBtn" onclick={pinCountry}>Pin</button>
  <button class="clearBtn" onclick={clearAll}>Reset</button>
</div>

{#if showDialog}
  <dialog open>
    <h2>{countryData.name.common}</h2>
    <p><strong>Capital:</strong> {countryData.capital?.[0] || 'N/A'}</p>
    <p><strong>Region:</strong> {countryData.region}</p>
    <p><strong>Population:</strong> {countryData.population.toLocaleString()}</p>
    <p><strong>Flag:</strong> {countryData.flag}</p>
    <button onclick={() => showDialog = false}>Close</button>
  </dialog>
{/if}
