# Klang Valley Reach and Risk

Interactive 3D maps of who can reach everyday services on foot in Malaysia's Klang Valley, built on a hexagon grid with Python, pydeck and H3.

**Live maps**

- 15-minute walking map: https://likrentai.github.io/klreach/
- Where people live: https://likrentai.github.io/klreach/population.html

## What the maps show

The Klang Valley (Kuala Lumpur, Putrajaya and the Selangor districts of Petaling, Gombak, Ulu Langat, Klang and Sepang) is divided into about 4,000 hexagons of about 0.74 km² each (H3 resolution 8).

- **Stage 1, where people live:** column height and colour show the number of people in each hexagon.
- **Stage 2, the 15-minute walk:** for every hexagon, the walking time along real streets and footpaths to the nearest clinic or hospital, school, park or playground, grocery and rail station. The colour shows how many of these five are within a 15-minute walk; the height still shows population. Hover over a hexagon to see the minutes to each service.

## Key findings (Stage 2)

Share of the Klang Valley's 8.3 million people (Kontur estimate) within a 15-minute walk:

| Service | Share of people |
|---|---|
| Grocery | 66% |
| Park or playground | 66% |
| School | 57% |
| Clinic or hospital | 47% |
| Rail station | 18% |

- About **10%** of people can reach all five services within 15 minutes; about **16%** can reach none.
- Kuala Lumpur scores highest (on average 3.5 of 5 services), followed by Petaling (2.6) and Ulu Langat (2.5); Klang (1.7) and Sepang (1.4) score lowest.
- Rail is the scarcest service on foot: 34% of people in Kuala Lumpur live within 15 minutes' walk of a station, but only 5% in Gombak and 7% in Klang.

## Method

1. **Grid and population:** H3 hexagons over the seven districts, filled with Kontur Population (2023-11-01), which is published on the same H3 grid.
2. **Services:** OpenStreetMap features downloaded with OSMnx in small tiles from several Overpass servers. Parks are represented by points every 100 m along their edges, since people enter from the nearest side. Convenience stores (7-Eleven, KK Super Mart, myNEWS and similar) are not counted as groceries; supermarkets including 99 Speedmart, markets and greengrocers are. Rail stations are the Rapid Rail and KTM Komuter stations from my earlier project, [Klang Valley Rail Activity Map](https://likrentai.github.io/klmobility/).
3. **Walking network:** all walkable streets and paths (about 38,700 km) read with pyosmium from the Geofabrik extract for Malaysia, Singapore and Brunei, OpenStreetMap data as of 6 October 2026. Motorways and private roads are excluded; one-way rules are ignored for walking.
4. **Walking times:** the network is stored as a sparse matrix, and one multi-source Dijkstra search per service type (SciPy) gives every street point its distance to the nearest service. Hexagon centres and services are joined to their nearest street point. Walking speed 4.8 km/h, so 15 minutes is 1.2 km.

## Limitations

- **OpenStreetMap is incomplete.** Private clinics in particular are better mapped in Kuala Lumpur than in Gombak or Klang, so some low healthcare scores outside Kuala Lumpur reflect missing map data rather than missing clinics.
- **Kontur underestimates Putrajaya's population** (about 8,600 people against about 110,000 in the 2020 census).
- **One point per hexagon:** each hexagon is measured from its centre, so people at its edge may be a few minutes closer or farther.
- **Walking only:** the maps do not include buses, cars or the quality of footpaths (shade, crossings, pavements).

## Data sources and licences

- OpenStreetMap contributors, ODbL 1.0 (services and walking network), via Overpass and Geofabrik
- Kontur Population, 2023-11-01, CC BY 4.0
- Rapid Rail and KTM Komuter station locations, as prepared for the klmobility project
- Basemap: CARTO, OpenStreetMap contributors

## Notebooks (run in this order)

| Notebook | What it does |
|---|---|
| 01hexgrid | Builds the H3 grid and adds Kontur population |
| 02buildmap | Stage 1 map: population in 3D |
| 03services | Downloads the five service types from OpenStreetMap |
| 04walknetwork | Reads the walking network from the Geofabrik file |
| 05access | Walking minutes, 15-minute score and summaries |
| 06buildaccess | Stage 2 map, and both pages in `docs/` for GitHub Pages |

Built with Python: pandas, GeoPandas, H3, pydeck, OSMnx, pyosmium and SciPy.

## Coming next

- **Stage 3:** a day of Rapid KL buses, animated from the GTFS timetables
- **Stage 4:** the December 2021 floods, mapped from Sentinel-1 radar images
- **Stage 5:** how well served are the areas that flooded in 2021, today?

Lik Ren Tai, 2026
