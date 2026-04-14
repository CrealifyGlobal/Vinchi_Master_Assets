# Lead Time Experience File
## Knowledge File — Superyacht Construction Scope Agent

**Purpose:** Records historical location split patterns by work package. This is the agent's primary reference for suggesting splits. It is NOT a formula — it is structured human experience.

---

## SECTION 1 — PROPULSION & MACHINERY

### WP-01 | Bilge System

| Configuration | Prop Room | Engine Room | Void Spaces | Crew Bilge | Builds |
|--------------|-----------|-------------|-------------|------------|--------|
| Standard 2-zone | 35% | 45% | 20% | — | 3 |
| Standard 3-zone | 30% | 40% | 20% | 10% | 2 |
| Vacuum system, 2-zone | 30% | 50% | 20% | — | 1 |

**Suggested Anchor (2-zone, standard):** Prop Room 35% / Engine Room 45% / Void 20%
**Confidence:** Medium-High

**Adjustment Triggers:**
- Vacuum bilge → shift 5% from Prop Room to Engine Room
- Large fwd void → add 5-10% to Void, reduce Engine Room
- No crew bilge → redistribute 10% to Engine Room

---

### WP-02 | Exhaust System

| Configuration | Engine Room | Prop Room | Below WL Chase | Builds |
|--------------|-------------|-----------|----------------|--------|
| Wet exhaust, shaft drive | 55% | 25% | 20% | 3 |
| Dry exhaust, shaft drive | 65% | 20% | 15% | 1 |
| IPS/pod drive | 80% | 0% | 20% | 1 |

**Suggested Anchor (wet exhaust, shaft):** Engine Room 55% / Prop Room 25% / Chase 20%
**Confidence:** Medium

**Adjustment Triggers:**
- Custom cladding → +10% Engine Room
- Extended shaft line → +5% Prop Room
- IPS → Engine Room 80%, Prop Room 0%, Chase 20%

---

### WP-03 | Fuel System

| Configuration | Engine Room | Tank Farm | Prop Room | Builds |
|--------------|-------------|-----------|-----------|--------|
| Day tank in ER | 50% | 35% | 15% | 4 |
| Day tank separate | 40% | 45% | 15% | 2 |
| No prop room scope | 55% | 45% | 0% | 1 |

**Suggested Anchor (day tank in ER):** Engine Room 50% / Tank Farm 35% / Prop Room 15%
**Confidence:** Medium

---

### WP-04 | Shaft Line

| Configuration | Prop Room | Engine Room | Builds |
|--------------|-----------|-------------|--------|
| Single shaft | 70% | 30% | 5 |
| Single shaft, long | 75% | 25% | 2 |
| Twin shaft (per shaft) | 70% | 30% | 1 |

**Suggested Anchor:** Prop Room 70% / Engine Room 30%
**Confidence:** High

**Adjustment Triggers:**
- LOA >50m → Prop Room 75% / Engine Room 25%
- CPP → Prop Room 75% / Engine Room 25%

---

## SECTION 2 — ELECTRICAL

### WP-10 | Main Cable Runs

| Configuration | Engine Room | Chase Fwd | Chase Aft | Dist Room | Builds |
|--------------|-------------|-----------|-----------|-----------|--------|
| Standard, <45m | 40% | 25% | 20% | 15% | 3 |
| Standard, 45-55m | 35% | 28% | 22% | 15% | 1 |
| Extended chase run | 35% | 30% | 25% | 10% | 1 |

**Suggested Anchor (<45m):** ER 40% / Chase Fwd 25% / Chase Aft 20% / Dist Room 15%
**Confidence:** Medium

**Adjustment Triggers:**
- Vessel >50m → reduce ER 5%, add to chases proportionally
- Additional distribution zone → +8% Dist Room, reduce chases

---

### WP-11 | Lighting Circuits

| Configuration | Crew Quarters | Galley | Engine Room | Prop Room | Builds |
|--------------|---------------|--------|-------------|-----------|--------|
| Standard | 45% | 25% | 20% | 10% | 4 |
| Extended crew | 55% | 20% | 15% | 10% | 1 |
| No prop room lighting | 50% | 25% | 25% | 0% | 1 |

**Suggested Anchor:** Crew 45% / Galley 25% / ER 20% / Prop Room 10%
**Confidence:** Medium-High

---

## SECTION 3 — HVAC & PLUMBING

### WP-20 | HVAC Ductwork

| Configuration | Engine Room | Main Deck Void | Lower Deck Void | Builds |
|--------------|-------------|----------------|-----------------|--------|
| Chilled water, 2 zones | 35% | 40% | 25% | 3 |
| DX system, 2 zones | 45% | 35% | 20% | 1 |
| 3-zone system | 30% | 38% | 32% | 1 |

**Suggested Anchor (chilled water, 2-zone):** ER 35% / Main Deck Void 40% / Lower Deck Void 25%
**Confidence:** Medium

---

### WP-21 | Domestic Water System

| Configuration | Engine Room | Crew | Guest | Galley | Builds |
|--------------|-------------|------|-------|--------|--------|
| Standard | 20% | 30% | 35% | 15% | 4 |
| Large guest accommodation | 20% | 25% | 40% | 15% | 1 |
| Extended crew area | 20% | 38% | 27% | 15% | 1 |

**Suggested Anchor:** ER 20% / Crew 30% / Guest 35% / Galley 15%
**Confidence:** Medium-High

**Adjustment Triggers:**
- >6 guest cabins → +5% to Guest from Crew
- Crew >8 berths → +5% to Crew from Guest

---

### WP-22 | Grey & Black Water

| Configuration | Prop Room | Engine Room | Crew | Guest | Builds |
|--------------|-----------|-------------|------|-------|--------|
| Standard | 25% | 20% | 25% | 30% | 4 |
| No guest scope | 25% | 20% | 55% | 0% | 1 |
| Extended guest | 20% | 15% | 20% | 45% | 1 |

**Suggested Anchor:** Prop Room 25% / ER 20% / Crew 25% / Guest 30%
**Confidence:** Medium

---

## SECTION 4 — STRUCTURAL

### WP-30 | Insulation & Acoustic Lining

| Configuration | Engine Room | Prop Room | Voids | Crew | Builds |
|--------------|-------------|-----------|-------|------|--------|
| Standard acoustic | 40% | 25% | 20% | 15% | 4 |
| Enhanced acoustic (ER) | 50% | 20% | 18% | 12% | 2 |
| No crew in scope | 45% | 28% | 27% | 0% | 1 |

**Suggested Anchor (standard):** ER 40% / Prop Room 25% / Voids 20% / Crew 15%
**Confidence:** Medium-High

---

### WP-31 | Deck Penetrations

Distributes by penetration count per location. Ask user to estimate count per zone.

| Location | Typical % of Penetrations |
|----------|--------------------------|
| Engine Room | 30-40% |
| Prop Room | 10-15% |
| Cable Chases | 20-30% |
| Accommodation | 20-30% |

**Confidence:** High (once count is known)

---

## GENERAL ADJUSTMENT PRINCIPLES

| Factor | Effect |
|--------|--------|
| LOA >50m | +10-15% to run-heavy locations |
| LOA <35m | -10% on run-heavy locations |
| First-of-type system | +15-20% buffer; flag Low confidence |
| Repeat system from prior build | High confidence; anchor to actuals |
| Constrained access | +10-15% to that location |

---

*Experience File Version: 1.0 | Last Updated: 2026-04-08*
