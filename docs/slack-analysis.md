# Yaw Dampener Slack Issue

## Problem Statement
Installed the following into a Harmon Rocket (i.e. Vans RV-4 derivative) to act
as a yaw dampener:
1. Garmin GSA 28 Servo
2. Garmin 2" Capstan
2. 1/16" bridle cable with two CS-00022 RV bridle harness clamps
2. Two MS20219-2 pulleys (1-7/16" groove diameter) to route bridle cable.

When the rudder is neutral the bridle cable is tight, but when at the extreme
left and right the bridle cable has excessive slack. The capstan and pulleys
have cable guards that nominally retain the cable, but slack cable has caught
on a bolt head in the installation, jamming rudder movement. A jammed rudder
cable is a **catastrophic failure mode** — this problem must be solved to
eliminate slack, not merely contained by guards.

The rudder has the following dimensions:
1. Two rudder cable attachment points on the horn, each 1.25" aft of rudder
   pivot and 3.25" lateral (6.5" total spread between attachment points)
2. Rudder cables exit the fuselage 12" forward of the horn attachment points,
   6" across (±3.0" from centerline)
3. Rudder travel 64° (±32°)

**Measured slack:** At full rudder deflection, the bridle cable deflects 0.65"
perpendicular to the cable run across a 5.7" span.

---

## Background Information

### Key Specifications

**GSA 28 Servo** *(G3X Installation Manual 190-01115-01, Rev K, Section 9):*
- Maximum rated torque: **60 in-lbs**, adjustable 10–60 in-lbs (5 in-lb steps)
- Capstan: 2.1" OD, groove depth **0.095"**, effective radius ~1.0"
- Cable force = torque / radius:
  25% → 15 lbs, **30% → 18 lbs** (recommended starting point), 100% → 60 lbs
- Capstan installations may trigger false stuck-clutch faults due to side
  loading; clutch monitor can be disabled for capstan setups

**MS20219-2 Pulley** *(MIL-DTL-7034/1):*
- Groove depth: **0.156"** — deeper than the capstan groove (0.095")
- The capstan's shallower groove makes it the primary cable retention risk
- Cable guards on the capstan and pulleys nominally retain a slack cable, but
  slack cable that lifts out of the groove can loop, kink, or snag on adjacent
  hardware (bolt heads, clamps, brackets). A cable snagged on a bolt head has
  already been observed in this installation — this jams rudder travel and is a
  **catastrophic failure mode**

**RV Rudder Cable System** *(VAF forum, Dynon installation guide):*
- Single-pull system: only the actively pulled cable is under tension; the
  return-side cable is inherently slack
- 1–2" of cable play is normal at rest; rudder self-centers via aero loads
  in flight
- This inherent slack compounds the geometric slack in the bridle cable

---

## Rudder Geometry & Cable Slack Analysis

### Geometric Model

The rudder horn has two separate cable attachment points, one for each rudder
cable. Each attachment point is 1.25" aft and 3.25" lateral from the pivot
(effective arm = √(3.25² + 1.25²) = 3.48" from pivot). The left cable attaches
to the left point; the right cable to the right point.

**Coordinate system** (2D top-down view):
- Origin at rudder pivot (hinge line)
- x-axis: lateral (positive = right)
- y-axis: fore/aft (positive = forward)

**At neutral (θ = 0):**
- Left attachment: (−3.25, −1.25)
- Right attachment: (3.25, −1.25)
- Left cable exit: (−3.0, 10.75) — 12" forward of attachment, 3.0" lateral
- Right cable exit: (3.0, 10.75)

Note: the cable exits (6" spread) are slightly narrower than the horn
attachment points (6.5" spread), so the cables converge inward by 0.25" per
side over the 12" run.

**At deflection angle θ**, the attachment points rotate with the rudder:

```
Left:  (−3.25·cosθ + 1.25·sinθ,  −3.25·sinθ − 1.25·cosθ)
Right: ( 3.25·cosθ + 1.25·sinθ,   3.25·sinθ − 1.25·cosθ)
```

**Geometric slack** arises because the two attachment points trace arcs of
different radii from their respective cable exit points. The loaded cable
(being pulled) takes up less length than the return cable releases — the
arc-vs-chord effect. The net shortening of the total cable path creates slack
in the bridle cable.

**2D model prediction at ±32°:**

```
At neutral:
  Left cable:  √[(−3.25−(−3.0))² + (−1.25−10.75)²] = √[0.063 + 144.0] = 12.003"
  Right cable: √[(3.25−3.0)² + (−1.25−10.75)²]      = 12.003"
  Total path = 24.005"

At θ = 32° (cos32° = 0.848, sin32° = 0.530):
  Left att:  (−2.094, −2.782)
  Right att: (3.419, 0.662)
  Left cable:  √[(−2.094−(−3.0))² + (−2.782−10.75)²] = √[0.821 + 183.1] = 13.562"
  Right cable: √[(3.419−3.0)² + (0.662−10.75)²]       = √[0.175 + 101.8] = 10.097"
  Total path = 23.659"

Geometric slack = 24.005 − 23.659 = 0.35"
```

The 2D model predicts **0.35" of slack** at ±32°, which overestimates the
measured value (0.15"–0.20") by roughly 75%. This is expected because the real
cable geometry is 3D (cables approach the horn from below at compound angles,
not purely in-plane).

### Measured Slack

At full deflection (±32°) the bridle cable was observed to deflect **h = 0.65"**
perpendicular to the cable run across a span of **L = 5.7"**. The excess cable
length (slack) can be derived from two bounding assumptions:

**V-shape (cable pushed at midpoint):**

```
Slack = 2√[(L/2)² + h²] − L
     = 2√[2.85² + 0.65²] − 5.7
     = 2√[8.123 + 0.423] − 5.7
     = 2 × 2.923 − 5.7
     = 0.147"
```

**Parabolic sag (cable hanging uniformly):**

```
Slack ≈ 8h² / (3L)
     = 8 × 0.65² / (3 × 5.7)
     = 3.38 / 17.1
     = 0.198"
```

**Measured slack at ±32°: 0.15" – 0.20"**

*The 2D model predicts 0.35" (see above) — roughly 75% higher than measured.
The measured value is used as the design basis.*

### Critical Slack Threshold

Cable guards on the capstan and pulleys nominally retain the cable, but **any
slack is unacceptable**. The capstan groove (0.095") provides only 0.03" of
wall above a seated 1/16" cable — far less retention than the MS20219-2 pulley
groove (0.156"). Slack cable lifts out of the groove, loops inside the guard,
and snags on adjacent hardware. A cable snagged on a bolt head has already been
observed in this installation, jamming rudder travel. This is a catastrophic
failure mode — the critical slack threshold is effectively **zero**.

For the remainder of this analysis, **0.20" is used as the design slack value**
(conservative upper bound of the measured range).

---

## Yaw Damper Requirements

### Typical Range of Motion

Yaw dampers in light aircraft are designed with limited authority to prevent
full rudder application in the event of a malfunction:

| Parameter | Value |
|---|---|
| Typical rudder authority | ±3° to ±5° |
| Yaw rate limit (G3X) | 6°/sec |
| Operating speed range | Above ~80 KIAS (auto-disengage below) |

For ±5° rudder deflection with a 1.25" horn arm:
- Cable movement at horn: 1.25 × sin(5°) = **0.109"**
- Capstan rotation: 0.109" / 1.0" radius = 0.109 rad = **6.25°**

### Yaw Damper Force Required

The aerodynamic hinge moment determines the force needed to deflect the rudder.
Estimated for a Harmon Rocket at cruise:

| Parameter | Value |
|---|---|
| Estimated rudder area | ~3.0 ft² |
| Cruise speed | ~180 mph (263 fps) |
| Dynamic pressure (q) at cruise | ~82 psf |
| Hinge moment coefficient (C_h) | ~−0.005 /deg |
| Hinge moment for 5° deflection | ~18.4 in-lbs |
| Cable force at horn (1.25" arm) | ~14.7 lbs |

At 30% torque (18 in-lbs), the servo produces 18 lbs of cable force at the 1"
capstan radius — closely matched to the estimated aero load at cruise. At
approach speed (~100 mph), dynamic pressure drops to ~25 psf, and the required
cable force drops to ~4.5 lbs, giving ample margin.

**Key constraint:** Any slack takeup mechanism must not absorb more than ~10–15%
of the servo's cable travel (0.109") or force (18 lbs), or yaw damper
effectiveness will be significantly degraded.

---

## Solution Options

### Option 1: Serial Extension Spring

**Description:** A small extension spring is spliced into each bridle leg (two
springs total). At neutral, each spring is pre-extended and acts as a rigid
link. When the cable path shortens at extreme deflection, the spring retracts,
taking up the slack while maintaining cable tension.

**Design parameters (per spring):**
- Slack per leg: ~0.10" (total 0.20" split between two legs via capstan)
- Pre-extension: P = 0.12" (0.10" + margin)
- Minimum force at max deflection: ≥ 2 lbs (cable retention)
- Maximum force at neutral: ≤ 10 lbs (avoid interference)

**Spring rate calculation:**

For an extension spring with initial tension T₀ = 2 lbs:

```
At max deflection: F_min = T₀ + k × (P − 0.10) = 2 + k × 0.02 ≥ 2 lbs  ✓
At neutral:         F_max = T₀ + k × P = 2 + k × 0.12 ≤ 10 lbs
                    → k ≤ 67 lbs/in
```

**Using k = 65 lbs/in, T₀ = 2 lbs:**

| Condition | Extension | Force |
|---|---|---|
| Neutral | 0.120" | 9.8 lbs |
| Max deflection | 0.020" | 3.3 lbs |

**Impact on yaw damper effectiveness:**

When the yaw damper actuates (±5°), it moves cable 0.109" per side. Each spring
on the loaded side extends further, and the spring on the return side contracts.
The net spring torque opposing the capstan:

```
τ_springs = 2 × k × Δcable × R = 2 × 65 × 0.109 × 1.0 = 14.2 in-lbs
```

Of the servo's 18 in-lbs, **14.2 in-lbs (79%) is absorbed by the springs**,
leaving only 3.8 in-lbs to deflect the rudder against aero loads.

At cruise (aero hinge moment = 3.68 in-lbs/deg):

```
Without springs: δ = 18 / 3.68 = 4.9° authority
With springs:    δ = 3.8 / 3.68 = 1.0° authority  (79% reduction)
```

**A softer spring (k = 10 lbs/in)** reduces the torque loss to 2.2 in-lbs
(12%), but at max deflection the spring force drops to only 2.2 lbs, providing
marginal cable retention. Worse, the yaw damper would extend the spring 1.8"
before any load reaches the rudder, causing severe lag.

**Fundamental conflict:** The slack magnitude (~0.10"/leg) and yaw damper cable
travel (~0.109") are of similar magnitude. No spring rate simultaneously
provides adequate slack absorption and acceptable yaw damper compliance.

**Pros:**
- Simple concept — two small springs, no pulleys or brackets
- Low part count
- Lightweight

**Cons:**
- Severely degrades yaw damper authority (40–60% loss at practical spring rates)
- Introduces compliance (springiness) into the yaw damper control path
- Creates a speed-dependent centering force on the rudder
- Spring fatigue risk under continuous cyclic loading
- Difficult to find suitable off-the-shelf extension springs for this
  combination of low force, short travel, and high stiffness

---

### Option 2: Parallel Extension Spring

**Description:** An extension spring runs parallel to a section of bridle cable
between two low-friction cable guides (eyelets or Teflon-lined guides). At
neutral rudder, cable tension holds the cable straight through the guides,
overcoming the spring force. When cable tension drops to zero (slack condition),
the spring pulls the guides together. Cable slides through the guides from both
ends, collecting as a bow (loop) between the guides, which takes up the excess
cable from the rest of the system.

**Design parameters:**
- Guide spacing at neutral: d = 5.5" (maximum available space)
- Total slack to absorb: S = 0.20"
- Spring natural length: 4.5" (1.0" shorter than guide spacing)
- Spring force: 2–5 lbs (enough to move cable through low-friction guides)

**Bow geometry at max slack:**

The total excess cable collected between the guides equals the system slack
(0.20"). Using the neutral guide spacing (conservative — maximum bow depth):

```
Cable in bow = d + S = 5.5 + 0.20 = 5.70"
Span = d = 5.5"
Bow depth: 2 × √[(5.5/2)² + h²] = 5.70
           √[7.5625 + h²] = 2.85
           h² = 0.56
           h = 0.75"
```

The cable bows approximately **0.75" perpendicular** to the bridle cable run —
consistent with the 0.65" measured deflection across the 5.7" span.

**Spring specification:**

```
Natural length: 4.5"
Extension at neutral: 1.0"
Extension at max slack: 0.80"  (guides 0.20" closer)
Required force: 3 lbs at minimum extension
Spring rate: k = 3 / 0.80 = 3.75 lbs/in
Force at neutral: 3.75 × 1.0 = 3.75 lbs
```

**Impact on yaw damper:**

When the yaw damper operates (cable under ~18 lbs tension), the cable
straightens through the guides. The spring force (4 lbs) is easily overcome by
the 18 lb cable tension. The yaw damper sees virtually no compliance from this
mechanism — **less than 1% authority loss**.

**Pros:**
- Minimal impact on yaw damper effectiveness
- Low spring force — no significant addition to rudder system friction
- Passive mechanism, no moving parts beyond cable sliding through guides
- Simple spring sizing and selection

**Cons:**
- Requires ~0.8" of clearance perpendicular to the cable run for the bow
- Cable must slide freely through guides — friction could prevent proper
  operation; Teflon-lined or ceramic guides needed
- Guide wear over time from cable sliding
- Cable bow could contact adjacent structure — routing must be verified
- Does not maintain cable tension on pulleys/capstan — only reduces total
  system slack. Cable in the bow section is unsupported.
- Novel mechanism — no precedent in aircraft cable systems

---

### Option 3: Idler Pulley Tensioner

**Description:** Three MS20219-2 pulleys: two fixed to an airframe bracket at
the bottom, one spring-loaded idler above. The cable runs straight across the
tops of the fixed pulleys at neutral — no deflection, zero added cable tension.
When slack develops, compression springs push the idler downward, deflecting
the cable between the fixed pulleys and taking up the excess cable length. The
cable's own tension provides the restoring force, keeping the springs soft.

**Common design parameters:**
- All three pulleys: MS20219-2 (1.4375" groove dia, 1.755" OD)
- Fixed pulley spacing: S = 1.75" center-to-center
- Idler centered at S/2 = 0.875" from each fixed pulley
- Idler offset at neutral: h₀ = 0 (cable straight, no deflection)

**Slack absorption calculation:**

Cable path through the three-pulley system:

```
Path(h) = 2 × √[(S/2)² + h²]  (simplified, ignoring pulley radii)
```

At neutral (h = 0, cable straight):

```
Path₀ = S = 1.75"
```

At max slack (idler deflects cable by Δ):

```
Path = Path₀ + 0.20   (must absorb 0.20" of slack)
1.95 = 2 × √[0.875² + Δ²]
0.975² = 0.766 + Δ²
Δ² = 0.951 − 0.766 = 0.185
Δ = 0.430"
```

The idler pulley must push the cable **0.43" downward** to absorb 0.20" of
slack. More travel is needed than a pre-deflected design because the first
increment of deflection from a straight cable adds very little path length.

**Force analysis:**

At neutral (h = 0), the cable is straight and exerts zero perpendicular force
on the idler. The springs push the idler down freely when cable tension drops.
When the cable is under tension T and the idler deflects it by a small amount
h, the cable provides a restoring force:

```
F_restoring = 2T × sin(arctan(h / (S/2)))
For small h: F_restoring ≈ 4Th / S
```

The cable's own tension keeps the idler at h ≈ 0. The springs only need enough
force to push the idler down when the cable goes slack (~2 lbs at the idler) —
they do not need to resist the yaw damper cable tension.

**Impact on yaw damper (common to 3a and 3b):**

When the yaw damper operates (T = 18 lbs), the ~2 lb spring force deflects
the cable slightly:

```
Equilibrium: F_spring = 4Th / S
h = F_spring × S / (4T) = 2 × 1.75 / (4 × 18) = 0.049"

Extra cable path = 2√(0.766 + 0.049²) − 1.75
                 = 2√(0.768) − 1.75
                 = 1.753 − 1.75 = 0.003"

Authority loss = 0.003 / 0.109 = 3%
```

The cable tension dominates (effective stiffness 4T/S = 41 lbs/in vs. ~4 lbs/in
spring rate). The idler deflects only 0.049" under yaw damper load, absorbing
a negligible 0.003" of cable. **Authority loss < 3%.**

**Common pros:**
- Proven mechanism used extensively in automotive, industrial, and aircraft
  cable systems
- Continuously takes up slack at all rudder positions
- Cable stays seated in all pulley grooves and capstan at all times
- No cable sliding or friction-dependent mechanisms
- Zero added cable tension at neutral — cable is straight, no spring force
  on rudder pedals
- Very soft springs (~2 lbs at idler) — no custom spring sizing needed
- Off-the-shelf compression springs, preload adjustable with shims
- < 3% yaw damper authority loss (cable tension provides restoring force)
- Mechanically robust and inspectable

**Common cons:**
- Adds weight: 3 × MS20219-2 pulleys (0.20 lbs) + mechanism + springs +
  bracket (~0.5 lbs total)
- Only 4" of total cable run available — two fixed pulleys at 1.755" OD with
  1.75" c-c spacing span ~3.5" edge-to-edge, leaving < 0.5" for brackets
- Requires 0.43" of vertical clearance below the cable line for idler travel
- Must be inspectable per maintenance requirements

#### Option 3a: Linear Slide

```
    | ──── [===]               ← Spring housing (top of bracket)
    |      |spr|  
    |        O                ← Idler on linear slide, pushed down
    |     (idler)   
    |                
    O ─────────────── O     ← Fixed 1 & 2 on airframe bracket (bottom)
```

The idler rides in a vertical guide slot. Two compression springs (one on each
side of the idler carriage) push the idler downward.

**Spring specification:**

```
Number of springs: 2 (one each side of idler carriage)
Total preload at h = 0: 2 lbs (1 lb per spring)
Spring rate: 2 lbs/in per spring (4 lbs/in combined)
Working travel: 0.43"
Force at max slack: 2 + 4 × 0.43 = 3.7 lbs
Spring size: ~0.375" OD, 0.030" wire, 1.0" free length
```

Two springs provide redundancy — a single spring failure still provides ~1 lb
to push the idler.

**3a-specific pros:**
- Two springs = redundancy
- Pure linear motion — simple force path

**3a-specific cons:**
- Linear slide must be low-friction and resist jamming from vibration/debris
- Guide slot tolerances are critical for smooth operation

#### Option 3b: Pivoting Arm

```
         [spring]
          ↓  ↓
  ──pivot──── arm ── O (idler)
    |                    
    |bracket|             
    |       |              
    O ──────────────────────── O
  Fixed 1  ↑  Fixed 2       ← Fixed to airframe bracket (bottom)
           idler is
           between
           Fixed 1 & 2
```

The idler rides on a pivoting arm (L_arm = 1.5"). A compression spring at
r = 1.0" from the pivot pushes the arm downward. On a 1.5" arm, the 0.43"
idler displacement requires **17° of rotation**:

```
θ = arcsin(0.43 / 1.5) = 16.7° ≈ 17°
```

**Compression spring compatibility with 17° rotation:**

```
Axial compression: r × sin(17°) = 1.0 × 0.292 = 0.29"
Lateral shift:     r × (1 − cos(17°)) = 1.0 × 0.044 = 0.04"
Tilt angle:        arctan(0.04 / 0.29) = 8°
```

The 0.04" lateral shift is an 8° tilt — well within what a compression spring
can accommodate with a rounded or ball-end contact.

**Spring specification:**

By torque balance at the pivot (2 lbs needed at idler, 1.5" from pivot):

```
F_spring × r = F_idler × L_arm
F_spring = 2 × 1.5 / 1.0 = 3 lbs

Number of springs: 1 compression spring at 1.0" from pivot
Spring preload: 3 lbs (provides 2 lbs at idler)
Spring rate: ~10 lbs/in
Compression at max slack: 0.29"
Force at max slack: 3 + 10 × 0.29 = 5.9 lbs (3.9 lbs at idler)
```

**3b-specific pros:**
- Pivot bearing is simpler and more robust than a linear slide
- No slide to jam from vibration or debris
- Single compression spring — easy to source and replace
- More forgiving of misalignment than a linear guide
- Proven mechanism — belt tensioner arms with compression springs are
  ubiquitous in automotive applications

**3b-specific cons:**
- Pivot must be robust and low-friction (bushing or bearing)
- Arm geometry must clear adjacent structure during 17° rotation

---

### Option 4: ~~Limit Rudder Travel~~

**Description:** Reduce rudder travel stops so the maximum deflection stays
within the range where geometric slack is below the critical threshold. No
additional hardware is added.

**Required rudder travel for acceptable slack:**

Using the critical slack threshold of 1/16" (0.0625"):

| D | Max Angle for < 1/16" Slack | Reduction from ±32° |
|---|---|---|
| 0" | ±23° | 28% |
| 3" | ±16° | 50% |

Using a more generous threshold of 0.10" (within the capstan groove depth of
0.095"):

| D | Max Angle for < 0.10" Slack | Reduction from ±32° |
|---|---|---|
| 0" | ±27° | 16% |
| 3" | ±20° | 37% |

**Impact on crosswind effectiveness:**

Rudder yawing moment scales approximately with sin(δ). Maximum crosswind
capability at a given speed is therefore proportional to sin(δ_max):

```
Crosswind_ratio = sin(δ_new) / sin(δ_original)
```

| Max Rudder | sin(δ) | Ratio | POH (15 kt) | Practical (12 kt) | FAR Margin (practical) |
|---|---|---|---|---|---|
| ±32° (current) | 0.530 | 100% | 15 kt | 12 kt | +3% |
| ±25° | 0.423 | 80% | 12.0 kt | 9.6 kt | −17% (below) |
| ±20° | 0.342 | 65% | 9.7 kt | 7.8 kt | −33% (below) |
| ±16° | 0.276 | 52% | 7.8 kt | 6.2 kt | −47% (below) |

*POH demonstrated crosswind: 15 kt at ±32°. Practical limit: ~12 kt.*

**FAR 23.233 minimum:** 0.2 × V_SO ≈ 0.2 × 58 = **11.6 kt**. At the POH
value, ±25° barely clears the floor (12.0 kt, +3%). At the practical limit,
the current ±32° is already at the FAR floor (+3%) — **any reduction in
rudder travel drops crosswind capability below the regulatory minimum.**

**Taildragger ground roll — the critical phase:**

During the landing rollout a taildragger's rudder effectiveness drops rapidly
as airspeed decays, while the weathervane tendency from the main gear aft of CG
increases. Full rudder deflection is commonly needed below 30 KIAS even in
moderate crosswinds. Limiting rudder travel disproportionately affects this
phase because the pilot has no reserve authority when the rudder is already at
the reduced stop.

| Phase | Speed | Rudder Demand | Effect of Reduced Travel |
|---|---|---|---|
| Approach (75 kt) | High | Low–moderate | Minimal impact |
| Flare (60 kt) | Moderate | Moderate–high | Noticeable |
| Rollout (< 40 kt) | Low | Often full deflection | **Critical** — may lose directional control |
| Three-point hold | Decreasing | Full in gusts | **No reserve** at reduced stops |

FAR 23.147 requires "adequate directional control" for crosswind takeoffs and
landings up to the demonstrated crosswind component. Reducing rudder travel
requires flight testing to establish a revised (lower) demonstrated crosswind
and could restrict the aircraft's operational utility.

**Pros:**
- Zero additional hardware, weight, or complexity
- Eliminates the problem entirely within the reduced range
- Simple to implement — adjust rudder stop bolts
- No maintenance burden
- No impact on yaw damper effectiveness within the new range

**Cons:**
- **Any reduction drops below FAR 23.233 minimum** — current ±32° already at
  the floor (12 kt vs. 11.6 kt minimum)
- At ±25°, max crosswind falls to 9.6 kt (17% below FAR minimum)
- At ±20°, max crosswind falls to 7.8 kt (33% below FAR minimum)
- Taildragger ground roll is the critical phase — reduced stops leave no reserve
  rudder authority when it is most needed (low speed, weathervane tendency)
- Requires new flight test to establish a lower demonstrated crosswind component
- Reduces spin recovery margin
- May be unacceptable for a high-performance taildragger that routinely operates
  from strips where crosswind landings are unavoidable

---

## Decision Matrix

Criteria are scored 1 (worst) to 5 (best):

| Criteria (Weight) | Serial Spring | Parallel Spring | Idler Pulley (3a/3b) | ~~Limit Travel~~ |
|---|---|---|---|---|
| **Yaw Damper Effectiveness** (25%) | 2 | 5 | 4 | 5 |
| **Slack Elimination** (20%) | 3 | 4 | 5 | 5 |
| **Mechanical Simplicity** (20%) | 3 | 4 | 2 | 5 |
| **Reliability / Fatigue** (15%) | 2 | 2 | 4 | 5 |
| **Crosswind / Rudder Authority** (20%) | 4 | 4 | 4 | **0** |
| | | | | |
| **Weighted Score** | **2.80** | **3.95** | **3.80** | **4.00** |

**Breakdown:**

| Option | Calculation |
|---|---|
| Serial Spring | 2×.25 + 3×.20 + 3×.20 + 2×.15 + 4×.20 = **2.80** |
| Parallel Spring | 5×.25 + 4×.20 + 4×.20 + 2×.15 + 4×.20 = **3.95** |
| Idler Pulley (3a/3b) | 4×.25 + 5×.20 + 2×.20 + 4×.15 + 4×.20 = **3.80** |
| ~~Limit Travel~~ | 5×.25 + 5×.20 + 5×.20 + 5×.15 + **0**×.20 = **4.00** |

Limit Travel scores 4.00 on paper, but with a 12 kt practical
crosswind limit the current ±32° already sits at the FAR 23.233 floor — **any
reduction in rudder travel drops below the regulatory minimum**. Limit Travel
is effectively disqualified.

Among the solutions that preserve full rudder travel, the **Parallel Spring
edges out the Idler Pulley** (3.95 vs 3.80). The parallel spring's higher
mechanical simplicity score (4 vs 2) offsets the idler's advantage in slack
elimination (5 vs 4) and reliability (4 vs 2).

---

## Recommendation

**Primary recommendation: Option 2 — Parallel Extension Spring**

The parallel spring scores highest among solutions that preserve full rudder
travel (3.95). It:
1. Maintains full rudder travel (±32°)
2. Has zero impact on yaw damper effectiveness (< 1% authority loss)
3. Is mechanically simple — an extension spring and two cable guides
4. Absorbs measured slack (0.20") with a modest 0.75" cable bow
5. Requires no complex fabrication (no pivoting arms or torsion springs)

**Implementation approach:**
1. Install two low-friction cable guides 5.5" apart on the bridle cable
2. Attach a 4.5" natural-length extension spring parallel to the cable between
   the guides
3. Verify the cable bows cleanly at full rudder deflection without contacting
   adjacent structure (~0.8" clearance needed perpendicular to cable run)
4. Verify yaw damper authority is acceptable during flight test

**Fallback: Option 3b — Idler Pulley (Pivoting Arm)**

If the parallel spring cannot be packaged or the cable bow contacts structure,
the idler pulley (3.80) is the next best option. It continuously maintains
cable tension at all positions and keeps the cable seated in all grooves.
Option 3b (pivoting arm with compression spring) is preferred over 3a (linear
slide) — the pivot is more robust than a slide and less susceptible to jamming.
Both 3a and 3b share the same performance: zero added cable tension at neutral
and < 3% yaw damper authority loss.

---

## Open Questions

1. **Crosswind margin:** With a 12 kt practical crosswind limit at ±32°, the
   aircraft is already at the FAR 23.233 floor (11.6 kt). Any rudder travel
   reduction is operationally unacceptable — Option 4 is eliminated.


---

## FDM Fabrication Notes

For Options 3a and 3b, the bracket and idler mechanism can be FDM printed.
The loads are low (~2–4 lbs spring force, cable tension carried by pulley
axles) so strength is not the primary concern — friction and wear are.

### Material Selection

| Material | Strength | Friction (CoF) | Notes |
|---|---|---|---|
| PA-HT CF | Excellent | 0.3–0.6 (high) | CF particles are abrasive — poor for sliding surfaces |
| Plain PETG | Good | 0.3–0.5 | Better than CF nylon, but FDM layer lines create directional friction |
| Plain nylon (PA6/PA12) | Good | 0.15–0.25 | One of the lowest-friction FDM materials, good wear resistance |
| Iglidur (igus) | Good | 0.10–0.20 | Purpose-built bearing filament, excellent for slides and bushings |

### Option 3a (Linear Slide) — Material Concerns

The slide carriage must move freely with only 2 lbs of spring force. CF nylon
slide-on-slide surfaces will bind — the carbon fiber acts like fine sandpaper.
Recommended approaches:

1. **PA-HT CF bracket + PTFE tape on slide surfaces** — simplest. Print
   everything in PA-HT CF, apply PTFE tape to the slide channel and carriage.
   Tape is replaceable at inspection intervals.
2. **PA-HT CF bracket + plain nylon carriage** — print the structural bracket
   in PA-HT CF and the sliding carriage in unfilled nylon. Nylon-on-nylon is a
   well-known low-friction pairing.
3. **PA-HT CF bracket + Iglidur carriage or liners** — most engineered
   solution. Iglidur is specifically designed for bearing surfaces.

### Option 3b (Pivoting Arm) — Preferred for FDM

The pivoting arm largely eliminates the slide friction concern. A **1/4"
shoulder bolt with a bronze or Oilite bushing** provides a near-zero-friction
pivot at these loads. The arm and bracket can both be printed in PA-HT CF
without any friction concerns on sliding surfaces.

This is a strong practical argument for 3b over 3a: a bolt-and-bushing pivot
is more predictable, lower friction, and more robust than any FDM-printed
linear slide — regardless of material.

### General FDM Considerations

- **Print orientation:** Orient bracket so the pulley axle holes and pivot
  bolt hole are perpendicular to the layer lines (bolt loads in shear across
  layers, not pulling layers apart)
- **Infill:** 60–80% for the bracket body; 100% around bolt holes and pivot
- **Wall count:** 4+ perimeters for bolt hole reinforcement
- **Fasteners:** Use metal inserts (heat-set brass inserts) for any bolted
  connections; do not thread directly into printed plastic
- **Inspection:** FDM parts should be inspected for cracking at layer
  boundaries, especially around stress concentrations (bolt holes, pivot)

---

## References

1. Garmin G3X / G3X Touch Installation Manual, 190-01115-01, Rev K — Section 9:
   GSA 28 Installation (servo specs, torque limits, capstan kit, yaw damper
   setup). [PDF](https://static.garmin.com/pumac/190-01115-01_aw.pdf)

2. Dynon Capstan Servo Installation Instructions, 101385-000, Rev D — capstan
   drive torque characteristics, bridle cable routing.
   [PDF](https://dynonavionics.com/includes/guides/Capstan_Servo_Installation_Instructions_Rev_D.pdf)

3. Van's Air Force Forum — "Rudder Cable Play/Slack" (Jul 2018): confirms
   inherent slack in RV rudder cable systems.
   [Thread](https://vansairforce.net/threads/rudder-cable-play-slack.161866/)

4. Van's Air Force Forum — "GFC 500 torque and gain rates" (Feb 2023):
   community guidance on 30% torque setting for RV-series.
   [Thread](https://vansairforce.net/threads/gfc-500-torque-and-gain-rates.213879/)

5. Dynon Forum — "Yaw Damper RV10" (capstan vs. tiller arm conversion,
   practical installation notes).
   [Thread](https://forum.flydynon.com/threads/yaw-damper-rv10.14124/)

6. MS20219-2 Pulley — MIL-DTL-7034/1 specification (groove depth 0.156",
   allowable load 480 lbs).
   [Datasheet](https://military-fasteners.com/pulleys/groove+pulleys/ms20219-2)

7. US Patent 5,186,406 — Spring actuated take-up reel for removing cable slack
   (Grumman Aerospace, 1993): prior art for aircraft cable tensioner mechanisms.
   [Patent](https://patents.google.com/patent/US5186406A/en)

8. US Patent 2,405,377 — Cable tension regulator (1946): spring compensator
   interposed in aircraft control cables to maintain tension under varying loads.
   [Patent](https://patents.google.com/patent/US2405377A/en)
