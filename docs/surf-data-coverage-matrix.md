# Surf data coverage matrix (requested layer order)

This is the exact layer order you asked for, with a quick fetchability check against the current codebase.

## Requested-first layer order

1. 🎯 Intel Hotspots (`hotspots`) — **Available now**
2. ⚔ Conflict Zones (`conflicts`) — **Available now**
3. 🏛 Military Bases (`bases`) — **Available now**
4. ☢ Nuclear Sites (`nuclear`) — **Available now**
5. ⚠ Gamma Irradiators (`irradiators`) — **Available now**
6. 🚀 Spaceports (`spaceports`) — **Available now**
7. 🔌 Undersea Cables (`cables`) — **Available now**
8. 🛢 Pipelines (`pipelines`) — **Available now**
9. 🖥 AI Data Centers (`datacenters`) — **Available now**
10. ✈ Military Activity (`military`) — **Available now**
11. 🚢 Ship Traffic (`ais`) — **Available now**
12. ⚓ Trade Routes (`tradeRoutes`) — **Available now**
13. ✈ Flight Delays (`flights`) — **Available now**
14. 📢 Protests (`protests`) — **Available now**
15. ⚔ UCDP Events (`ucdpEvents`) — **Available now**
16. 👥 Displacement Flows (`displacement`) — **Available now**
17. 🌫 Climate Anomalies (`climate`) — **Available now**
18. ⛈ Weather Alerts (`weather`) — **Available now**
19. 📡 Internet Outages (`outages`) — **Available now**
20. 🌋 Natural Events (`natural`) — **Available now**
21. 🔥 Fires (`fires`) — **Available now**
22. ⚓ Strategic Waterways (`waterways`) — **Available now**
23. 💰 Economic Centers (`economic`) — **Available now**
24. 💎 Critical Minerals (`minerals`) — **Available now**

## What this means for "can we fetch?"

- **Map-layer toggles are implemented and wired for your order in surf variant UI.**
- **Async fetch-backed layers** are wired in the data loader task runner (weather, ship traffic/AIS, cables, flights, protests, military activity, UCDP, displacement, climate, outages, natural events). Static/config-backed overlays (bases, nuclear, pipelines, spaceports, waterways, economic centers, minerals, hotspots, conflicts, datacenters, irradiators, trade routes, fires) are rendered from configured datasets and layer builders.
- **Surfline / MagicSeaweed / WindGuru / WSL / ISA are not yet first-class built-in providers in this repo**; they should be integrated via feed packs/APIs/sidecar endpoints in the next phase.

## Next integration pass (recommended)

- Add `surf` source packs in `src/config/feeds.ts` and bind to surf panels.
- Add sidecar/API adapters for commercial or non-RSS surf providers (Surfline/MagicSeaweed/WindGuru).
- Add competition/news adapters for WSL + ISA + Olympics surf pathway.
