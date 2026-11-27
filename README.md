# Daidala

**A swarm of eight-legged robots for finding survivors in rubble.**

*Daidala* (Greek δαίδαλα) — cunningly wrought objects, the works of Daedalus.
In Homer, the self-moving tripods of Hephaestus are the mythical ancestor of the
machine that behaves as if alive. This project is a child of `daedalforge`.

---

## Why

In June 2021, the collapse of Champlain Towers South in Surfside, Florida, killed 98
people. Nine of them are believed to have survived the initial collapse and died
because they were not found in time. **No ground robot took part in the search for
survivors**, even though robots had been deployed at the World Trade Center, at Berkman
Plaza II, in Mexico City and in Christchurch.

The reason is not a lack of will. Existing UGVs are simply not designed to search the
inside of a rubble pile.

The reference study — Rao, Murphy, Merrick & Choset, *Analysis of Interior Rubble Void
Spaces at Champlain Towers South Collapse*, IEEE SSRR 2022
([arXiv:2301.04743](https://arxiv.org/abs/2301.04743)) — characterised those voids, and
the numbers drive every design decision in this repository:

| Finding | Consequence for the robot |
|---|---|
| 6 candidate voids out of 225 locations examined (4 × 4 m grid) | voids are rare; coverage matters more than depth |
| void height **15 – 37 cm** | the robot must crawl flat, not compact |
| void section **2.7 – 6.9 m** wide | wide and flat beats narrow and tall — a spider, not a snake |
| no connected tunnels, no entrance visible from above | paths must be found and probed at ground level |
| survivors beyond **6 m** into the pile | a tether long enough to reach and come back |

## Approach

Four decisions follow from that analysis.

**1. Flat, not compact.**
The robot walks tall for speed and crawls flat (6–8 cm) to enter low voids. The posture
change costs no extra actuator: with two degrees of freedom per leg, lowering the pitch
and opening the yaw is enough.

**2. No camera.**
Dust, darkness and dirty lenses make video useless where this robot has to work. Sensing
is thermal, CO₂, audio, time-of-flight and touch — the leg currents are the primary
contact sense. The operator works on a map and on signatures, not on an image.

**3. Two bodies, one tether.**
A **station** robot anchors (spikes driven by its own legs, because friction alone never
holds) and pays out the line: winch, power, fibre. A **diver** robot descends. The myth
is literal — Ariadne holds the thread, Theseus walks into the labyrinth. It also solves
the two hardest problems underground at once: communication and energy.

**4. Swarm, not a single hero.**
Coverage is the bottleneck. In real rubble one robot covers roughly 50 m²/h, so a
1 000 m² collapse needs ten robots over two hours — or one robot for twenty, which is
never. The swarm is a sensor network that happens to walk.

## What this is not

- Not a product. There is no certification, no deployment, no finished hardware.
- Not a replacement for search dogs, which remain far better at scenting a live victim.
- Not a promise that a spider is the right morphology for the deep interior of a pile —
  the literature argues for burrowing and snake-like platforms there. Daidala targets the
  voids that *are* reachable, and the paths that are not visible from above.

## Status

**Framing stage.** The functional specification is being written; no hardware has been
built yet. Expect the documents to move and the requirements to change.

## Design principles

- Eight **identical** leg modules: one spare part repairs any leg.
- No soldering in the field, one tool, any module replaced in under five minutes.
- Ground pressure below 5 kPa — the robot must not punch through a plasterboard floor.
- The battery is a consumable, not a component.
- Never anchor closer than 40 cm from an edge.
- One scenario at a time: **rubble only**. Forest fire and ATEX are explicitly out of scope.

## Repository layout (planned)

```
docs/       framing documents, specification, test protocols  (written in French)
hardware/   mechanical design, bill of materials, assembly plans
firmware/   ESP32 real-time gait control
sim/        gait and stability development, swarm simulation
```

## Documentation

The framing documents are written in French. They cover, in order: need assessment and
sources, functional specification, energy and battery, compute architecture,
communications, modularity and repairability, rubble mobility, ground pressure and the
tether, the anchor concept, and the build plan.

Nothing here is frozen yet. Corrections and counter-arguments are welcome — especially
from people who have actually worked a collapse site.

## Contributing

This is a personal research project, currently at specification stage. If you work in
urban search and rescue, in legged robotics, or in open-hardware manufacturing and want
to challenge a design decision, open an issue. The most useful contribution right now is
a critique, not a pull request.

## License

To be decided. The likely split is **CERN-OHL-S** for hardware and **Apache-2.0** for
software, so that the design stays open even if it is manufactured commercially.
