# Bright targets for a live sky tour

J2000 coordinates, ready to pass to `astro_positions` as `radec:RA,DEC`
(RA in decimal hours, Dec in degrees). Mag = brightness (lower is brighter;
the naked-eye limit in a suburb is about 4).

| Target | `bodies` entry | Mag | Eye / Binos | What to say |
|---|---|---|---|---|
| Sirius | `radec:6.75,-16.72` | −1.5 | Eye | Brightest star in the night sky; it flashes colors when low |
| Arcturus | `radec:14.26,19.18` | −0.1 | Eye | Orange giant; "arc to Arcturus" from the Big Dipper's handle |
| Vega | `radec:18.62,38.78` | 0.0 | Eye | Blue-white; one corner of the Summer Triangle |
| Capella | `radec:5.28,46.00` | 0.1 | Eye | Yellow, like our Sun; high in winter |
| Rigel | `radec:5.24,-8.20` | 0.1 | Eye | Orion's blue-white knee |
| Procyon | `radec:7.66,5.22` | 0.4 | Eye | Little Dog star, east of Orion |
| Betelgeuse | `radec:5.92,7.41` | 0.5 | Eye | Orion's red supergiant shoulder; it will one day go supernova |
| Altair | `radec:19.85,8.87` | 0.8 | Eye | Summer Triangle; it spins once every 9 hours |
| Aldebaran | `radec:4.60,16.51` | 0.9 | Eye | The red eye of Taurus the Bull |
| Antares | `radec:16.49,-26.43` | 1.0 | Eye | "Rival of Mars"; the red heart of Scorpius |
| Spica | `radec:13.42,-11.16` | 1.0 | Eye | "Spike to Spica", continuing the arc from Arcturus |
| Fomalhaut | `radec:22.96,-29.62` | 1.2 | Eye | The lonely bright star of the autumn south |
| Deneb | `radec:20.69,45.28` | 1.3 | Eye | Tail of the Swan; about 200,000× brighter than the Sun |
| Regulus | `radec:10.14,11.97` | 1.4 | Eye | The heart of Leo |
| Polaris | `radec:2.53,89.26` | 2.0 | Eye | The North Star; it barely moves, and its altitude ≈ your latitude |
| Mizar & Alcor | `radec:13.40,54.93` | 2.2 | Eye | A double in the Big Dipper's handle; an old eyesight test |
| Pleiades (M45) | `radec:3.79,24.12` | 1.6 | Both | "Seven Sisters"; dozens of stars in binoculars |
| Orion Nebula (M42) | `radec:5.59,-5.39` | 4.0 | Binos | A stellar nursery in Orion's sword, 1,300 light-years away |
| Andromeda Galaxy (M31) | `radec:0.71,41.27` | 3.4 | Binos | The farthest thing you can see with the naked eye: 2.5 million light-years |
| Double Cluster | `radec:2.33,57.14` | 4.3 | Binos | Two clusters side by side between Perseus and Cassiopeia |
| Beehive (M44) | `radec:8.67,19.67` | 3.7 | Binos | A swarm of stars in Cancer |
| Hercules Cluster (M13) | `radec:16.69,36.46` | 5.8 | Binos | 300,000 stars in a ball; a fuzzy dot in binoculars |
| Albireo | `radec:19.51,27.96` | 3.1 | Scope | Gold and blue double star at the Swan's head |
| Lagoon Nebula (M8) | `radec:18.06,-24.38` | 6.0 | Binos | A glowing cloud above the Sagittarius "teapot" |

## Choosing targets for one `astro_positions` call (max 20 bodies)

Always include `moon` and every planet `astro_sky_today` says is up. Fill the rest
with **about 12** rows from this table, chosen to cover the whole sky (some
winter and some summer objects), because only some will be above the horizon at any time.
Then keep only results with **altitude ≥ 15°** (lower objects are lost in haze
and trees).
