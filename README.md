# ![Transit Card Map](https://github.com/sudolev/TransitCardsMap/assets/61996958/9afbf869-a356-4080-bda0-1f155ff9896c)

### A WIP simple, free and accessible visual database of fare cards around the world. Use it [here](https://transitcards.pages.dev/).

## Features

- Efficient map with zoom, pan and tilt functionality.

- Card infobox (opens on click) with name, location, operator and an external resource (usually Wikipedia) for each card.

- Persistent light & dark mode that follows the user’s preferred color scheme. Togglable via custom theme control.

- Cards increase in size & are brought to foreground on hover

- Card markers are generated dynamically and iteratively on page load from a simple [YAML data file](https://github.com/sudolev/TransitCardsMap/blob/main/static/data/cards.yml).

- Not dependent on any CDN-delivered resources other than map tiles

## Screenshots



## Building

To build this app for yourself, clone this respository into your file system. Install [pnpm](https://pnpm.io/installation) and run

```bash
pnpm install

pnpm run dev
```

This will start the app on `localhost:5173`.

<p align="center">
<b>License: <a href="https://github.com/sudolev/TransitCardsMap/blob/main/LICENSE" target="_blank">GPL-3.0</a></b>
</p>
