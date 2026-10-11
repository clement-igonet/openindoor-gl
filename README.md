# openindoor-gl

[![License](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg?style=flat)](LICENSE.txt) [![CI](https://github.com/clement-igonet/openindoor-gl/actions/workflows/test-all.yml/badge.svg)](https://github.com/clement-igonet/openindoor-gl/actions/workflows/test-all.yml)

A tracking fork of [MapLibre GL JS](https://github.com/maplibre/maplibre-gl-js) for indoor and 3D building maps. It follows upstream `main` and adds a small set of patches that are not in MapLibre:

- **Underground floors**: `fill-extrusion-base` and `fill-extrusion-height` accept negative values and extrude below ground level ([maplibre#8051](https://github.com/maplibre/maplibre-gl-js/issues/8051)).
- **Elevated markers and popups**: `heightOffset` and `heightAnchor` options place a `Marker` or `Popup` at a height above the ground or at an absolute elevation, with terrain and globe ([maplibre#8228](https://github.com/maplibre/maplibre-gl-js/issues/8228)).

Everything else is MapLibre GL JS, same API, same style spec. Base: maplibre-gl 6.13.0.

## Examples

Live at **https://clement-igonet.github.io/openindoor-gl/**, built from `main` on every push:

- [Gare de Lyon, level by level](https://clement-igonet.github.io/openindoor-gl/examples/gare-de-lyon-levels/): seven OSM indoor levels, four below ground, markers following the selected level.
- [Markers on building roofs](https://clement-igonet.github.io/openindoor-gl/examples/markers-on-roofs/): a `Marker` on every tall building, `heightOffset` from `render_height`.
- [Underground, side by side with MapLibre](https://clement-igonet.github.io/openindoor-gl/examples/underground-vs-maplibre/): `maplibre-gl@latest` vs openindoor-gl, synced cameras, three scenes.

Examples are plain HTML files under [`pages/examples/`](https://github.com/clement-igonet/openindoor-gl/tree/main/pages/examples), each loading `dist/maplibre-gl.mjs` built by the Pages workflow. Add one: a folder with an `index.html` and a `thumb.png`, plus a card in [`pages/index.html`](https://github.com/clement-igonet/openindoor-gl/blob/main/pages/index.html).

## Usage

```html
<link href="https://unpkg.com/openindoor-gl@latest/dist/maplibre-gl.css" rel="stylesheet" />
<script type="module">
import * as maplibregl from 'https://unpkg.com/openindoor-gl@latest/dist/maplibre-gl.mjs';

const map = new maplibregl.Map({container: 'map', style: 'https://demotiles.maplibre.org/style.json'});

map.on('load', () => {
    map.addLayer({
        id: 'basement', type: 'fill-extrusion', source: 'building', 'source-layer': 'building',
        paint: {'fill-extrusion-base': -8, 'fill-extrusion-height': -4, 'fill-extrusion-color': '#c08050'}
    });
    new maplibregl.Marker({heightOffset: 30}).setLngLat([2.35, 48.85]).addTo(map);
});
</script>
```

With npm: `npm install openindoor-gl`, then `import * as maplibregl from 'openindoor-gl'`. The `maplibregl` namespace, the CSS class names and the dist file names are unchanged, so it is a drop-in replacement for `maplibre-gl`.

## Documentation

The [MapLibre GL JS documentation](https://maplibre.org/maplibre-gl-js/docs/) applies. The additions are documented in the TypeScript types (`MarkerOptions.heightOffset`, `MarkerOptions.heightAnchor`, `PopupOptions.heightOffset`, `PopupOptions.heightAnchor`) and in the [CHANGELOG](CHANGELOG.md) under the `## main` section.

## Development

Same as upstream: `npm ci`, `npm run build-dev`, `npm run test-unit`, `npm run test-render`. See [CONTRIBUTING.md](CONTRIBUTING.md) and [ARCHITECTURE.md](ARCHITECTURE.md).

Upstream is merged regularly; each patch lives on its own branch and is proposed upstream when possible.

## License and attribution

BSD 3-Clause, see [LICENSE.txt](LICENSE.txt). Copyright MapLibre contributors; derived from mapbox-gl-js (Mapbox, BSD 3-Clause until v1.13).

openindoor-gl is an independent project. It is not affiliated with or endorsed by MapLibre; "MapLibre" is a trademark of the MapLibre organization and is used here only to refer to the upstream project.
