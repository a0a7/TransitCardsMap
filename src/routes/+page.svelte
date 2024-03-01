<script lang="ts">
    import { MapLibre, NavigationControl, GeolocateControl, FullscreenControl, ScaleControl, Control, ControlGroup, ControlButton } from 'svelte-maplibre';
    import DarkMode from "svelte-dark-mode";
    
    let theme: any ;

    $: switchTheme = theme === "dark" ? "light" : "dark";    
</script>

<DarkMode bind:theme />

<MapLibre 
  center={[0,30]}
  zoom={2}
  class="map"
  style="data/style_{theme}.json"
  let:map
>
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