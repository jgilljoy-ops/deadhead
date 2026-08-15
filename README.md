# Deadhead

**Route planning that respects what actually constrains a truck.**

Most routing tools optimize distance. Distance is not what costs money. Empty miles, illegal hours, idling time, and a bridge you can't fit under are what cost money. Deadhead plans multi-vehicle delivery routes around the constraints drivers and dispatchers actually work inside.

🔗 **Live demo:** _coming soon_ · no signup required - hit **Load demo fleet** to seed vehicles and deliveries


---

## Why this exists

I drove long-haul for seven years before studying computer science.

The dispatch software I used treated a 53-foot trailer like a car with a bigger engine. It routed me down streets trucks aren't allowed on. It planned days that were impossible under Hours of Service rules. It measured success in kilometres when my pay and my company's margin lived in loaded-mile ratio and fuel burn.

Deadhead is the tool I wanted on the road. Every constraint it models is one I hit personally.

---

## What it does

**v1 - trip economics**
- Log a run: distance, fuel, tolls, hours, load weight
- Cost per kilometre, cost per loaded kilometre, margin per load
- History and trend charts

**v2 - fleet assignment**
- Enter your fleet and your deliveries
- Assigns deliveries across vehicles and sequences each route
- Map view of every vehicle's plan

**v3 - real constraints**
- Hours of Service scheduling
- Truck-legal routing
- Seasonal weight restrictions
- Idling and load-weight fuel model

---

## The constraint model

This is the part that makes it different from a distance minimizer.

### Hours of Service
Canadian federal rules: 13 hours driving, 14 hours on-duty, 10 consecutive hours off, and a 70-hour limit across 7 days. A route that is optimal on distance but illegal on hours is not a route — it's a fine and a shut-down driver. Deadhead treats HOS as a hard constraint, which turns routing into a combined routing-and-scheduling problem.

### Truck-legal routing
Bridge clearances, posted weight limits, and truck-prohibited roads. Consumer routing APIs answer "how would a car get there." That is the wrong question for a vehicle 4.1 m tall and 36,000 kg loaded.

### Deadhead (loaded-mile ratio)
The app is named for the thing it minimizes. Empty running is pure cost. A route 40 km longer with 80 km less deadhead is the cheaper route, and distance-based optimizers pick the wrong one every time.

### Seasonal weight restrictions
Quebec's spring thaw (*dégel*) drops allowable axle weights on many roads for several weeks each spring. A plan that's legal in July is illegal in April on the same road.

### Idling burn
Fuel burned while stationary — reefer units, cab heat through a Montreal winter, waiting at a dock. Distance-based fuel estimates miss this entirely, and over a week it is not a rounding error.

---

## Solver approach

Assigning deliveries across vehicles is the **Vehicle Routing Problem** — NP-hard, so exact solutions are off the table at useful sizes. Deadhead implements heuristics and shows its work.

| Solver | Approach | Total cost | Distance | Compute |
|---|---|---|---|---|
| Greedy | Nearest neighbour | _TBD_ | _TBD_ | _TBD_ |
| 2-opt | Local search on greedy | _TBD_ | _TBD_ | _TBD_ |
| VROOM | Reference solver | _TBD_ | _TBD_ | _TBD_ |

The comparison view animates routes improving as the local search runs, and a **why this route** panel names the binding constraint for each vehicle — hours exhausted, weight limit, or time window. Dispatchers don't trust a black box, and neither should a reviewer.

---

## Tech stack

| Layer | Choice |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| Backend | Node.js, Express |
| Database | _TBD_ |
| Routing & distances | OpenRouteService |
| Reference solver | VROOM |
| Charts | _TBD_ |
| Hosting | _TBD_ |

---

## Running locally

```bash
git clone https://github.com/<user>/deadhead.git
cd deadhead
npm install
cp .env.example .env      # add your OpenRouteService API key
npm run dev
```

Open `http://localhost:3000`.

---

## Status

- [ ] v1 - trip logging, cost per km, charts
- [ ] Deployed with live demo data
- [ ] v2 - fleet and delivery entry, greedy assignment, map view
- [ ] 2-opt improvement and solver comparison
- [ ] Hours of Service constraints
- [ ] Truck-legal routing
- [ ] Fuel model with load weight and idling

---

## Notes

Deadhead is a portfolio project, not production dispatch software. The Hours of Service implementation follows Canadian federal rules and is a modelling exercise, not compliance advice.

Underneath the trucking framing, the problem is constrained resource allocation under operational limits - the same shape as convoy planning and fleet tasking.

---

**Built by Jiteshwar Gill** 
