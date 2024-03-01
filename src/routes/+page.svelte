<script lang="ts">
    import { MapLibre, NavigationControl, Marker,  Popup, FullscreenControl, ScaleControl, Control, ControlGroup, ControlButton } from 'svelte-maplibre';
    import DarkMode from "svelte-dark-mode";
    import { parse } from 'yaml';
    import { onMount } from 'svelte';
	import type { Theme } from 'svelte-dark-mode/types/DarkMode.svelte';

    let theme: Theme | undefined;
    let cards: { name: string, link: string, place: string, issuer: string, lngLat: [number, number] }[];

    $: switchTheme = (theme === "dark" ? "light" : "dark") as Theme;
    
    onMount(async () => {
        fetch('data/cards.yml')
            .then(response => response.text())
            .then(data => {
                cards = parse(data)
                console.log(cards)
            })
            .catch((error) => {
                console.error('Error:', error);
            });
    });
</script>

<svelte:head>
    <title>Transit Card Map</title> 
    <meta name="description" content="Gallery of transit fare cards worldwide.">
    <meta name="keywords" content="Transit, Cards, Map">
    <meta name="author" content="Alexander Akira Weimer">
</svelte:head>

<DarkMode bind:theme />

<MapLibre 
  center={[15,35]}
  zoom={2}
  class="map"
  style="data/style_{theme}.json"
  let:map
>
    {#each Object.entries(cards) as [id, { name, link, place, issuer, lngLat }]} 
        <Marker {lngLat} class="w-14 h-14" >
            <img src="img/cards/{id}.png" alt="{name}" class="drop-shadow-xl">

            <Popup openOn="click" offset={[0, -20]}>
                <a href="{link}" target="_blank">
                    <h2 class="text-xl font-bold">{name}</h2>
                </a>
                <p class="text-md italic">{place}</p>
            </Popup>
        </Marker>
    {/each}
    <NavigationControl position="top-left" />
    <FullscreenControl position="top-left" />
    <ScaleControl />
    <Control class="flex flex-col gap-y-2" position="top-left">
        <ControlGroup>
            <ControlButton on:click={() => {theme = switchTheme}} >
                <img src="img/icons/switch_from_{theme}.svg" alt="Switch Theme" class="w-5 h-5" title="Switch theme"/>
            </ControlButton>
        </ControlGroup>
    </Control>
</MapLibre>

<style>
    :global(.map) {
      height: 100vh;
    }
</style>