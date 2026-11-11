# SMC-Net Dispatch

**Stuttgart Multi-Modal Consolidation Network: route planning and carbon-reporting dashboard**

A single-file web app (`index.html`) used to plan consolidated last-mile delivery routes for Stuttgart SMEs from one micro-consolidation hub, and to estimate the CO₂e of those routes. It was built as the operational tool behind the SMC-Net business plan (Production & Operations Management, PG POM 2026, team *Fast and Curious*).

> Live demo: https://ealins.github.io/SMC-Net_Dispatch/

---

## What SMC-Net is

Small shops in Stuttgart receive many small, irregular deliveries from many suppliers. They cannot justify their own fleet, driver or route planner, and hub space in the city is scarce. SMC-Net is a shared micro-hub that sorts and consolidates these deliveries into planned low-emission routes, served by cargo e-bikes and an e-van. It does not compete with DHL or Hermes. It targets SMEs with irregular, fragile, low-volume, same-city or scheduled deliveries.

| Item | Launch setup |
|---|---|
| Hub | 48.7756 °N, 9.1783 °E (central Stuttgart) |
| Fleet | 3 motor cargo e-bikes + 1 e-van |
| Staff | 4 riders/drivers + 2 hub staff (dispatch, sorting) |
| Working day | 7.5 h per resource, about 7.5 min service time per stop |
| Planning capacity | about 122 deliveries per day (daytime) |
| Target market | 213 SMEs screened inside the service buffer; 43 active in year 1 (20% adoption) |

## What this dashboard does

- **Planner:** place delivery stops on a MapLibre map, assign them to vehicles (motor e-bike or e-van), and generate routes from the hub.
- **Routing:** uses the HERE Routing API for road routes and falls back to OSRM, then to a distance estimate, if HERE is unavailable. E-bike routes avoid controlled-access highways.
- **Operations overview and resource plan:** checks route time against the 7.5 h daily limit per resource, using modeled speeds of 15 km/h (e-bike) and 28 km/h (e-van) when needed.
- **Carbon report (GLEC / ISO 14083):** a calculator for well-to-wheel emissions of the electric fleet versus a diesel-van baseline. It covers vehicle energy, energy emission factors, hub operation, data quality and assurance notes, and factor traceability.
- **Export:** results can be exported as CSV or JSON.

### Carbon results from the business plan (120 deliveries/day, full scope)

| Scenario | CO₂e per delivery | vs. diesel |
|---|---|---|
| Diesel van fleet | 391 g | baseline |
| Electric, German grid | 198 g | −49% |
| Electric, green power | 11 g | −97% |

## How it was built (build-up process)

The tool is the last step of a staged, evidence-based study. Each stage fed the next.

1. **Problem and idea.** Fragmented SME deliveries, repeated supplier trips, scarce hub space, and Stuttgart's low-emission rules led to the shared micro-hub concept.
2. **Market research.** Five methods: desk research, government and news evidence, institutional case studies (KoMoDo Berlin, Stadsleveransen, Binnenstad), GIS buffer analysis, and SME field interviews.
3. **SME screening.** 3,535 listed SMEs were filtered by business type (needs delivery, parcel under 15 kg, deliverable in 1–3 days) and by distance. This gave 780 SMEs within 5 km and 213 inside the launch buffer.
4. **Adoption scenarios.** 10%, 20% (base case) and 30% adoption of the 213 SMEs, converted into daily drops with a route-density multiplier of about 2.5 deliveries per SME per day.
5. **Benchmark check.** KoMoDo Berlin implies about 58 parcels per bike per day. SMC-Net plans for 30 per FTE per day, about 52% of that, so the target is deliberately conservative.
6. **Travel and capacity test.** Routes from the hub to the farthest districts (Stammheim, Plieningen, Weilimdorf, Birkach, Hedelfingen) were tested with the HERE routing API. This produced the 122/day daytime planning cap. **This dashboard grew out of that routing test.**
7. **Business model and financials.** Business Model Canvas, revenue model (delivery fees, subscriptions, retainers, pickups, storage, returns), 2026–2030 cost build-up, BAFA e-cargo-bike grant treated as one-time CAPEX reduction, launch investment of about €116k.
8. **Legal, risk and sustainability.** GmbH (preferred) or UG, GDPR, insurance, risk register, and GLEC-based emissions method, which became the carbon module in this dashboard.
9. **Roadmap.** Staged, density-led launch: add adjacent buffers only after route reliability and density are proven.

Full detail is in the business plan *SMC-Net: Stuttgart Multi-Modal Consolidation Network* (PG POM 2026, 18 Jun 2026).

## Run it

No build step and no dependencies to install.

```bash
# open directly
open index.html

# or serve locally
python3 -m http.server 8000   # then visit http://localhost:8000
```

It loads MapLibre GL from unpkg and map tiles from OpenFreeMap, so it needs an internet connection.

### HERE API key

The key is **not stored in this repository**. On first use the app asks for a HERE API key and keeps it in your browser only (`localStorage`). If you skip it, routing falls back to OSRM. Get a free key at [developer.here.com](https://developer.here.com) and restrict it to your site's domain.

## Tech

HTML, CSS and vanilla JavaScript in one file · [MapLibre GL JS](https://maplibre.org/) 4.7 · [OpenFreeMap](https://openfreemap.org/) tiles · HERE Routing API v8 · OSRM fallback.

## Team

Fast and Curious: Ashmeera Dahal, Binita Shahi, Ealin Sapkota, Sandesh Upadhyaya, Rowshon Ara Toma.
Presented to Dr. Prof. Payam Dehdari.

## License

See [LICENSE](LICENSE).
