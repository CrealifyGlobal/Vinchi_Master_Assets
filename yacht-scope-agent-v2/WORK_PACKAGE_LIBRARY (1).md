# Work Package Library
## Knowledge File — Superyacht Construction Scope Agent

**Purpose:** Defines all standardised work packages, their applicable locations, and baseline total system hours drawn from historical builds.

---

## HOW TO READ AN ENTRY

- **WP ID** — unique reference code
- **System** — parent system
- **Description** — what the WP covers
- **Applicable Locations** — which vessel zones this WP spans
- **Baseline Total Hours** — experience-based total across all locations
- **Basis** — how many past builds this figure is drawn from
- **Spec Sensitivity** — Low / Medium / High

---

## SECTION 1 — PROPULSION & MACHINERY

### WP-01 | Bilge System Installation

| Field | Value |
|-------|-------|
| System | Bilge & Drainage |
| Description | Supply, install, and test bilge pumps, pipework, valves, and alarm connections |
| Applicable Locations | Propeller Room, Engine Room, Void Spaces (fwd & aft), Crew Quarters Bilge |
| Baseline Total Hours | 80h |
| Basis | 6 builds |
| Spec Sensitivity | Medium |

**Notes:**
- Vacuum bilge spec adds ~25%
- Confirm whether fwd void is included in demarcation

---

### WP-02 | Exhaust System Routing & Insulation

| Field | Value |
|-------|-------|
| System | Exhaust |
| Description | Fabricate, install, and insulate exhaust lines from engine to hull exit |
| Applicable Locations | Engine Room, Propeller Room, Below Waterline Chase |
| Baseline Total Hours | 120h |
| Basis | 5 builds |
| Spec Sensitivity | High |

**Notes:**
- Wet vs. dry exhaust dramatically changes scope
- IPS/pod drive eliminates propeller room scope entirely

---

### WP-03 | Fuel System — Primary Lines & Tank Connections

| Field | Value |
|-------|-------|
| System | Fuel |
| Description | Install primary fuel feed/return lines, tank connections, day tank, and valves |
| Applicable Locations | Engine Room, Tank Farm Space, Propeller Room |
| Baseline Total Hours | 95h |
| Basis | 7 builds |
| Spec Sensitivity | Medium |

**Notes:**
- Day tank location drives Engine Room vs. Tank Farm split significantly
- If generator fuel is in scope, add WP-03a

---

### WP-03a | Fuel System — Generator Sub-Circuit

| Field | Value |
|-------|-------|
| System | Fuel |
| Description | Generator fuel feed from day tank, including filtration and monitoring |
| Applicable Locations | Engine Room, Generator Room |
| Baseline Total Hours | 30h |
| Basis | 4 builds |
| Spec Sensitivity | Low |

---

### WP-04 | Shaft Line & Sterntube Installation

| Field | Value |
|-------|-------|
| System | Propulsion |
| Description | Install shaft line, sterntube, cutlass bearing, shaft seals, and gearbox coupling |
| Applicable Locations | Propeller Room, Engine Room |
| Baseline Total Hours | 110h |
| Basis | 8 builds |
| Spec Sensitivity | Low |

**Notes:**
- Most consistent WP across builds — baseline is reliable
- Twin shaft = two separate WP-04 instances

---

## SECTION 2 — ELECTRICAL & SYSTEMS

### WP-10 | Main Cable Runs — Engine Room to Distribution

| Field | Value |
|-------|-------|
| System | Electrical |
| Description | Pull, terminate, and label main power cables from engine room to zonal panels |
| Applicable Locations | Engine Room, Cable Chase (Fwd), Cable Chase (Aft), Distribution Room |
| Baseline Total Hours | 200h |
| Basis | 5 builds |
| Spec Sensitivity | High |

**Notes:**
- 50m+ vessels: expect 30% uplift on baseline
- Lighting circuits not included — see WP-11

---

### WP-11 | Lighting Circuits — Below Decks

| Field | Value |
|-------|-------|
| System | Electrical |
| Description | Install and connect lighting circuits, dimmers, and control panels for crew and service areas |
| Applicable Locations | Crew Quarters, Galley Service Space, Engine Room, Propeller Room |
| Baseline Total Hours | 75h |
| Basis | 6 builds |
| Spec Sensitivity | Medium |

---

### WP-12 | Navigation & Comms Infrastructure

| Field | Value |
|-------|-------|
| System | Electrical / AV |
| Description | Install cable infrastructure for navigation, comms, and AIS — excluding equipment supply |
| Applicable Locations | Wheelhouse, Mast Base, Engine Room |
| Baseline Total Hours | 60h |
| Basis | 4 builds |
| Spec Sensitivity | Medium |

---

## SECTION 3 — HVAC & PLUMBING

### WP-20 | HVAC Ductwork — Primary Runs

| Field | Value |
|-------|-------|
| System | HVAC |
| Description | Fabricate and install primary supply and return ductwork from HVAC units to zone distribution |
| Applicable Locations | Engine Room, Main Deck Service Void, Lower Deck Service Void |
| Baseline Total Hours | 140h |
| Basis | 5 builds |
| Spec Sensitivity | High |

**Notes:**
- Chilled water vs. DX system changes engine room scope substantially
- Each additional HVAC zone adds ~15h

---

### WP-21 | Domestic Hot & Cold Water System

| Field | Value |
|-------|-------|
| System | Plumbing |
| Description | Install hot and cold water lines from calorifier to all outlets, including isolation valves |
| Applicable Locations | Engine Room, Crew Quarters, Guest Accommodation, Galley |
| Baseline Total Hours | 110h |
| Basis | 6 builds |
| Spec Sensitivity | Medium |

---

### WP-22 | Grey & Black Water System

| Field | Value |
|-------|-------|
| System | Plumbing |
| Description | Install waste pipework, holding tank connections, macerators, and overboard discharge |
| Applicable Locations | Propeller Room, Engine Room, Crew Quarters, Guest Accommodation |
| Baseline Total Hours | 90h |
| Basis | 6 builds |
| Spec Sensitivity | Low |

---

## SECTION 4 — STRUCTURAL & OUTFITTING

### WP-30 | Insulation & Acoustic Lining

| Field | Value |
|-------|-------|
| System | Structural Outfitting |
| Description | Install acoustic foam, spray foam, and vapour barrier lining to hull and bulkheads |
| Applicable Locations | Engine Room, Propeller Room, Void Spaces, Crew Quarters |
| Baseline Total Hours | 160h |
| Basis | 7 builds |
| Spec Sensitivity | Medium |

---

### WP-31 | Deck Penetrations & Cable Glands

| Field | Value |
|-------|-------|
| System | Structural Outfitting |
| Description | Cut, frame, seal, and gland all deck and bulkhead penetrations for cables, pipes, and ducts |
| Applicable Locations | All (distributed across all active locations) |
| Baseline Total Hours | 50h |
| Basis | 8 builds |
| Spec Sensitivity | Low |

**Notes:**
- Distributes proportionally to penetration count per location

---

## ADDING NEW WORK PACKAGES

Minimum data required: at least 3 completed builds with recorded actuals.

```
### WP-XX | [Name]
| Field | Value |
|-------|-------|
| System | |
| Description | |
| Applicable Locations | |
| Baseline Total Hours | |
| Basis | |
| Spec Sensitivity | |
```

---

*Library Version: 1.0 | Last Updated: 2026-04-08*
