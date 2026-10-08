# ISS Live Tracker

A real-time web app that shows where the International Space Station is right now, with its recent path, speed, altitude and distance from you.

**Live demo:** https://YOUR-USERNAME.github.io/iss-live-tracker/

![ISS Live Tracker screenshot](screenshot.png)

## Features

- **Live position**: updates every 5 seconds from a public satellite API
- **Orbit trail**: draws the ISS's last ~5 minutes of movement as a line behind it
- **Live stats**: latitude, longitude, altitude and speed in a floating panel
- **Distance calculation**: how far the ISS is from you, calculated with the Haversine formula
- **"Use my location" button**: uses the browser's Geolocation API, with a clear error message if permission is denied
- **Follow mode**: a checkbox to keep the map centred on the ISS, or untick it to explore freely
- **Error handling**: if the data source is unreachable, the app shows a message and keeps retrying

## Tech used

- JavaScript (vanilla, no framework)
- [Leaflet](https://leafletjs.com/) for the interactive map
- [MapLibre GL](https://maplibre.org/) with [OpenFreeMap](https://openfreemap.org/) for the map tiles (no API key needed)
- [Where the ISS at?](https://wheretheiss.at/w/developer) REST API for live ISS data
- Hosted on GitHub Pages

## How it works

1. `fetch()` requests the ISS's current position from the API every 5 seconds.
2. The marker moves to the new position and the point is added to the trail.
3. The Haversine formula turns two latitude/longitude pairs into a distance in km.
4. Everything lives in a single `index.html` file, so there is no build step and nothing to install.

## Run it locally

1. Download or clone this repository.
2. In the project folder, start a simple local server:

   ```
   python -m http.server 8000
   ```

3. Open `http://localhost:8000` in your browser.

Serving the page this way (instead of double-clicking the file) matters because browsers only allow geolocation on `https` or `localhost` pages.

## Challenges and what I learned

- **The date-line bug:** when the ISS crosses the edge of the map, its longitude jumps from about 180 to about -180, and a naive trail draws a line straight across the whole world. I fixed it by splitting the trail into segments and starting a new one whenever the longitude jumps by more than 180 degrees.
- **Map tile providers:** my first map used OpenStreetMap's tile server, which blocked requests from a local file (no valid referrer). The next free provider I tried then required an API key. I switched to OpenFreeMap, which serves vector tiles and needs no key, and connected it to Leaflet using MapLibre GL.
- **Geolocation edge cases:** the location feature needs to handle permission denial and unsupported browsers, not just the success case, so both show a clear message instead of failing silently.

## Possible next steps

- Predict and draw the ISS's upcoming orbit path
- Show the astronauts currently on board
- Add an alert for when the ISS passes close to the user's location

## Credits

- ISS data: [Where the ISS at?](https://wheretheiss.at/)
- Map tiles: [OpenFreeMap](https://openfreemap.org/), [OpenMapTiles](https://openmaptiles.org/), data from [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors
- Map library: [Leaflet](https://leafletjs.com/) and [MapLibre GL JS](https://maplibre.org/)
