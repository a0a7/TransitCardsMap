<script lang="ts">
    import { MapLibre, NavigationControl, Marker,  Popup, FullscreenControl, ScaleControl, Control, ControlGroup, ControlButton } from 'svelte-maplibre';
    import DarkMode from "svelte-dark-mode";
    import { parse } from 'yaml';
    import { onMount } from 'svelte';
	import type { Theme } from 'svelte-dark-mode/types/DarkMode.svelte';
    import { getFlagEmoji } from '$lib/utils';

    let theme: Theme | undefined;
    let cards: { name: string, link: string, place: string, issuer: string, country: string, lngLat: [number, number] }[];

    $: switchTheme = (theme === "dark" ? "light" : "dark") as Theme;
    
    onMount(async () => {
        fetch('data/cards.yml')
            .then(response => response.text())
            .then(data => {
                cards = parse(data)
            })
            .catch((error) => {
                console.error('Error:', error);
            });
    });
</script>

<DarkMode bind:theme />

<MapLibre 
  center={[15,35]}
  zoom={2}
  class="map font-varela"
  style="data/style_{theme}.json"
  let:map
>
    {#each Object.entries(cards) as [id, { name, link, place, issuer, country, lngLat }]} 
        <Marker {lngLat} class="w-14 h-14 hover:z-10" >
            <img src="img/cards/{id}.png" alt="{name}" class="drop-shadow-xl hover:scale-[2] transition-all duration-[125ms]">
            
            <Popup openOn="click" offset={[0, -35]}>
                <a href="{link}" target="_blank" class="px-2">
                    <h2 class="text-xl font-bold inline font-varela">
                        {name}
                    </h2>
                    <img src="img/icons/outlink.svg" alt="Open in new tab" class="w-4 h-4 inline align-top" title="Open in new tab"/>
                </a>
                <p class="leading-none text-base font-varela italic px-2">
                    {issuer}
                </p>
                <p class="leading-loose text-base font-varela px-2">
                    {place} 
                    <span class="leading-[0rem] text-lg align-middle" title="{country}">
                        {getFlagEmoji(country)}
                    </span>
                </p>
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
    <Control class="gap-y-2" position="top-right">
        <a href="https://github.com/sudolev/TransitCardsMap" target="_blank">
            <img src="img/icons/github.svg" alt="Github Icon" title="Open Github repo" class="w-8 h-8 text-lg font-varela w-full bg-white rounded border-[#ddd] border-[1px] p-1">
        </a>
    </Control>
</MapLibre>

<style>
    :global(.map) {
      height: 100vh;
    }
</style>

<svelte:head>
    <title>Transit Card Map</title> 
    <meta name="description" content="Gallery of transit fare cards worldwide.">
    <meta name="keywords" content="Transit, Cards, Map">
    <meta name="author" content="Alexander Akira Weimer">
</svelte:head>
