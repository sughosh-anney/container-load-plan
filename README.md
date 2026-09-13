# Container Load Plan

> A single-file container stuffing planner — turns a carton mix into a 3D load plan, floor pattern, crew loading sequence and printable manifest. **Zero dependencies. Zero CDN. Zero network calls.**

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/Vanilla%20JS-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Canvas](https://img.shields.io/badge/Canvas%202D-FF6F00?style=flat-square&logoColor=white)
![Dependencies](https://img.shields.io/badge/Dependencies-none-success?style=flat-square)
![Offline](https://img.shields.io/badge/Works%20Offline-100%25-success?style=flat-square)

**Built at ADF Foods Ltd — one of 14 dashboards, pipelines and automations delivered between 16 July and 12 September 2026, in under 8 weeks.**

---

## What is this project?

Export teams planning a container load were working from experience and a tape measure. How many cartons actually fit? In what order should the crew load them? Will the plan survive contact with the dock? Getting it wrong means a half-empty container or a re-stuff.

This tool takes a carton mix and produces a complete, printable stuffing plan — a 3D view of the packed container, the floor pattern, the exact loading sequence for the crew, and a manifest with a sign-off block.

---

## How the development was done

Built as **one HTML file of 2,296 lines**, with a deliberate constraint: no libraries at all.

That constraint drove every major decision. A 3D packing visualiser would normally reach for three.js; a PDF export would reach for jsPDF; an Excel export would reach for SheetJS; sharing a plan would need a server and a database. Each of those was replaced with something built by hand:

| Usual approach | Replaced with |
|---|---|
| three.js for 3D rendering | Hand-written Canvas-2D painter's algorithm with a custom projector |
| jsPDF for export | Native browser print, driven by print-only DOM sections |
| SheetJS for Excel | Tab-separated clipboard copy — pastes straight into Excel |
| Server-side share links | Whole plan base64-encoded into the URL hash |

The reason is practical, not stylistic: the people who need this are standing at a factory dock. The file has to open on any laptop, behind any firewall, with no internet and nothing installed.

The packing engine itself is a zone-based layered packer. Each carton type gets a zone along the container length, and within that zone the crew builds complete floor layers. For every carton the packer evaluates all six axis orientations, filtered by which faces that product is allowed to sit on, and ranks them by how many cartons fit per millimetre of zone length. Two refinements came from watching how crews actually work: leftover width strips get filled with cartons turned 90°, and the length remainder at the end of a zone is reused rather than wasted.

---

## What was used

Vanilla HTML, CSS and JavaScript. The Canvas 2D API for the 3D view. Nothing else — no framework, no bundler, no package manager, no external URL of any kind.

---

## How it works currently

A 7-step wizard: **Start → Cargo dimensions → Loading → Pallets → Equipment → Packing → Result**

The user enters carton dimensions and quantities, picks a container type, and the packer produces the plan. From there:

- **3D view** with four camera presets and two scrubbers — one reveals the load wall by wall, the other reveals it carton by carton in true crew build order
- **Compare equipment** runs the same cargo against every container type and recommends the best fit
- **Shrink sliders** model the space you actually lose to insulation ribs, wheel arches and bracing, since nominal interior dimensions are never what you get
- **Multi-container overflow** keeps adding units until the cargo fits, and only reports a genuine shortfall when a carton cannot fit an empty unit
- **Print sheet** produces every 3D view, floor pattern, loading order and manifest, for every container in the shipment
- **Share link** encodes the entire plan into the URL — no server, no attachment. Opening a shared link restores the full plan and jumps straight to the result

Supports standard dry and high-cube containers plus custom box and truck dimensions, pallet mode with tare and stack-height limits, per-product load priority, and configurable door placement.

---

## Output

A complete, printable stuffing plan that a loading crew can follow step by step, and that a planner can share as a single link or attach as one file.

Deployment is the whole installation procedure:

```
Open index.html in any browser.
```

---

*Built at ADF Foods Ltd. Product presets included are illustrative.*
