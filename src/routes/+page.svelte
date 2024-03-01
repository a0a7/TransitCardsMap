<script lang="ts">
    import { MapLibre, NavigationControl, GeolocateControl, FullscreenControl, ScaleControl, Control, ControlGroup, ControlButton } from 'svelte-maplibre';
    import DarkMode from "svelte-dark-mode";
    
    let style: string;

    $: switchStyle = style === "dark" ? "light" : "dark";    
</script>

<MapLibre 
  center={[0,30]}
  zoom={2}
  class="map"
  style="" 
  let:map
>
<NavigationControl position="top-left" />
<GeolocateControl position="top-left" fitBoundsOptions={{ maxZoom: 12 }} />
<FullscreenControl position="top-left" />
<ScaleControl />
<Control class="flex flex-col gap-y-2">
    <ControlGroup>
        <ControlButton on:click={() => {style = switchStyle}}>
        {#if style === "dark"}
            Light
        {:else}
            Dark
        {/if}
        </ControlButton>
    </ControlGroup>
</Control>

<style>
  :global(.map) {
    height: 100vh;
  }
</style>
</MapLibre>