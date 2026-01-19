# 📊 Models Overview with Detailed Attributes

## 1. tower.py - Tower Master Data

### Purpose

Basic information about each cooling tower - the "identity card" for each tower.

### Complete Attribute List (14 attributes)

| Attribute             | Type   | Description                           | Example              | Units      |
|-----------------------|--------|---------------------------------------|----------------------|------------|
| **tower_id**          | string | Unique identifier for the tower       | `CT-001`             | -          |
| **tower_name**        | string | Human-readable name                   | `Main Plant Tower A` | -          |
| **client_id**         | string | Unique identifier for the client      | `CLIENT-001`         | -          |
| **client_name**       | string | Company/facility name                 | `ABC Manufacturing`  | -          |
| **location**          | string | City and state where tower is located | `Houston TX`         | -          |
| **installation_date** | date   | When tower was installed              | `2018-03-15`         | YYYY-MM-DD |
| **tower_type**        | string | Cooling tower design type             | `Counterflow`        | -          |
| **capacity_tons**     | float  | Cooling capacity of the tower         | `500.0`              | tons       |
| **manufacturer**      | string | Company that built the tower          | `Baltimore Aircoil`  | -          |
| **model**             | string | Manufacturer's model number           | `VXC-500`            | -          |
| **design_approach**   | float  | Target approach temperature           | `10.0`               | °F         |
| **design_range**      | float  | Target temperature range              | `15.0`               | °F         |
| **latitude**          | float  | Geographic latitude                   | `29.7604`            | degrees    |
| **longitude**         | float  | Geographic longitude                  | `-95.3698`           | degrees    |

### Detailed Attribute Explanations

#### tower_id

**What it is**: Unique identifier assigned to each cooling tower  
**Format**: `CT-###` (CT = Cooling Tower, ### = sequential number)  
**Example**: `CT-001`, `CT-045`, `CT-234`  
**Used for**: Linking all other datasets to this specific tower  
**Never changes**: This ID stays with the tower for its lifetime

#### tower_name

**What it is**: Descriptive name for easy identification  
**Format**: Free text, usually includes location/building  
**Examples**:

- `Main Plant Tower A` - Primary tower at main building
- `East Wing Tower B` - Second tower in east wing
- `Data Center Tower` - Dedicated to data center cooling
- `Building 3 North` - Geographic identifier

**Used for**: Reports, dashboards, communication with technicians

#### client_id

**What it is**: Unique identifier for the customer/facility  
**Format**: `CLIENT-###`  
**Example**: `CLIENT-001`, `CLIENT-025`  
**Used for**: Grouping towers by customer, billing, SLA tracking  
**Important**: Multiple towers can have same client_id

#### client_name

**What it is**: Full name of the company or facility  
**Examples**:

- `ABC Manufacturing` - Manufacturing facility
- `TechCorp Solutions` - Technology company
- `Memorial Hospital East` - Healthcare facility
- `University Data Center` - Educational institution

**Used for**: Invoicing, reports, client communications

#### location

**What it is**: Physical location of the tower  
**Format**: `City State` or `City, State`  
**Examples**:

- `Houston TX`
- `Dallas TX`
- `Phoenix AZ`
- `Los Angeles CA`

**Used for**: Weather correlation, service dispatch, regional analysis  
**Important**: Affects expected temperature ranges and seasonal patterns

#### installation_date

**What it is**: Date when tower was originally installed  
**Format**: `YYYY-MM-DD` (ISO 8601)  
**Example**: `2018-03-15` means installed March 15, 2018  
**Used for**:

- Calculating tower age: `age_years = (today - installation_date) / 365`
- Warranty tracking
- Depreciation calculations
- Predicting maintenance needs (older towers need more maintenance)

**Real-world impact**:

- Tower < 5 years old: Minimal issues expected
- Tower 5-10 years: Regular maintenance cycle
- Tower 10-20 years: Increased maintenance, component replacements
- Tower > 20 years: Consider replacement evaluation

#### tower_type

**What it is**: Design configuration of the cooling tower  
**Options**:

- `Counterflow` - Air flows upward, opposite to water (most efficient)
- `Crossflow` - Air flows horizontally across falling water
- `Induced Draft` - Fan pulls air through (most common)
- `Forced Draft` - Fan pushes air through

**Example**: `Counterflow`  
**Used for**: Maintenance procedures, efficiency expectations, part sourcing  
**Impact**: Different types have different efficiency characteristics and maintenance needs

#### capacity_tons

**What it is**: Maximum cooling capacity of the tower  
**Definition**: 1 ton = ability to remove 12,000 BTU/hour of heat  
**Range**: Typically 100-2000 tons (small to large commercial/industrial)  
**Examples**:

- `100-300 tons` - Small commercial building
- `300-800 tons` - Medium industrial facility
- `800-2000 tons` - Large industrial plant or data center
- `2000+ tons` - Very large industrial applications

**Used for**: Normalizing all other metrics (cost per ton, efficiency per ton)  
**Critical**: All performance metrics should be evaluated relative to capacity

#### manufacturer

**What it is**: Company that designed and built the tower  
**Common manufacturers**:

- `Baltimore Aircoil` (BAC)
- `SPX Cooling Technologies`
- `Evapco`
- `Tower Tech`
- `Cooling Tower Systems`
- `AMCOT`

**Example**: `Baltimore Aircoil`  
**Used for**: Parts ordering, service manuals, warranty claims, expected specifications

#### model

**What it is**: Manufacturer's specific model/product code  
**Format**: Varies by manufacturer, usually includes capacity  
**Examples**:

- `VXC-500` - BAC model, 500 ton capacity
- `PTXI-750` - SPX model, 750 ton capacity
- `ATC-1200` - Evapco model, 1200 ton capacity

**Used for**: Finding exact specifications, ordering compatible parts, performance curves

#### design_approach

**What it is**: Target difference between outlet water temp and wet bulb air temp  
**Formula**: `design_approach = outlet_water_temp - wet_bulb_temp`  
**Typical range**: 7-15°F depending on tower size and application  
**Examples**:

- Small tower (100-300 tons): `7-10°F`
- Medium tower (300-800 tons): `8-12°F`
- Large tower (800-2000 tons): `10-15°F`

**Why it matters**: Lower approach = better cooling performance  
**Used for**: Evaluating if tower is meeting design specifications

**Real-world example**:

```
Wet bulb temp: 75°F
Outlet water temp: 85°F
Actual approach: 10°F
Design approach: 10°F
✓ Tower performing at design specification
```

#### design_range

**What it is**: Target temperature drop across the tower  
**Formula**: `design_range = inlet_water_temp - outlet_water_temp`  
**Typical range**: 10-25°F depending on tower and load  
**Examples**:

- Light load: `10-15°F`
- Medium load: `15-20°F`
- Heavy load: `20-25°F`

**Why it matters**: Indicates how much heat the tower is removing  
**Used for**: Capacity verification, load analysis

**Real-world example**:

```
Inlet water temp: 100°F (hot water from process)
Outlet water temp: 85°F (cooled water)
Actual range: 15°F
Design range: 15°F
✓ Tower removing design heat load
```

#### latitude

**What it is**: North-south position on Earth  
**Range**: -90 to +90 degrees  
**Example**: `29.7604` (Houston, TX)  
**Used for**: Weather data correlation, solar load calculations

#### longitude

**What it is**: East-west position on Earth  
**Range**: -180 to +180 degrees  
**Example**: `-95.3698` (Houston, TX)  
**Used for**: Weather data correlation, mapping, service routing

---

## 2. operational_data.py - Operational Metrics

### Purpose

Real-time or daily performance measurements from sensors and meters.

### Complete Attribute List (13 attributes)

| Attribute                    | Type     | Description                               | Example               | Units      |
|------------------------------|----------|-------------------------------------------|-----------------------|------------|
| **tower_id**                 | string   | Tower identifier (links to towers_master) | `CT-001`              | -          |
| **date** (daily)             | date     | Calendar date of reading                  | `2024-12-18`          | YYYY-MM-DD |
| **timestamp** (hourly/15min) | datetime | Exact time of reading                     | `2024-12-18 14:30:00` | ISO 8601   |
| **inlet_water_temp**         | float    | Hot water entering tower                  | `95.5`                | °F         |
| **outlet_water_temp**        | float    | Cold water leaving tower                  | `85.2`                | °F         |
| **wet_bulb_temp**            | float    | Ambient air humidity temperature          | `75.0`                | °F         |
| **approach_temp**            | float    | Outlet water minus wet bulb               | `10.2`                | °F         |
| **range_temp**               | float    | Inlet minus outlet temperature            | `10.3`                | °F         |
| **flow_rate_gpm**            | float    | Water flow through tower                  | `2500.0`              | GPM        |
| **cycles_of_concentration**  | float    | Water reuse ratio                         | `4.5`                 | ratio      |
| **fan_speed_percent**        | float    | Fan operating speed                       | `75.0`                | %          |
| **runtime_hours**            | float    | Hours tower operated                      | `22.5`                | hours      |
| **power_consumption_kwh**    | float    | Electrical energy used                    | `850.0`               | kWh        |
| **makeup_water_gallons**     | float    | Fresh water added                         | `1200.0`              | gallons    |
| **blowdown_gallons**         | float    | Water discharged                          | `600.0`               | gallons    |

### Detailed Attribute Explanations

#### tower_id

**What it is**: Links this reading to a specific tower  
**Example**: `CT-001`  
**Must match**: An existing tower_id in towers_master.csv

#### date / timestamp

**Daily mode**: `date = 2024-12-18` (one reading per day)  
**Hourly mode**: `timestamp = 2024-12-18 14:00:00` (24 readings per day)  
**15-min mode**: `timestamp = 2024-12-18 14:30:00` (96 readings per day)

#### inlet_water_temp

**What it is**: Temperature of hot water entering the tower from the process  
**Source**: Temperature sensor at tower inlet  
**Typical range**: 85-105°F (varies by application)  
**Examples**:

- HVAC chiller: `85-95°F`
- Industrial process: `95-105°F`
- Data center: `90-100°F`

**Why it matters**: Shows the heat load from the process  
**Problems indicated**:

- Rising inlet temp → Process working harder
- Very high inlet → Overloaded system

**Real-world example**:

```
Normal: 95°F
Hot day: 100°F (building cooling load increased)
Problem: 110°F (system overload or chiller issue)
```

#### outlet_water_temp

**What it is**: Temperature of cooled water leaving tower back to process  
**Source**: Temperature sensor at tower outlet  
**Typical range**: 75-90°F  
**Target**: Wet bulb + design approach

**Why it matters**: This is what you're paying for - cold water  
**Problems indicated**:

- Too high → Tower not cooling effectively
- Much higher than design → Fouling, poor performance

**Real-world example**:

```
Wet bulb: 75°F
Design approach: 10°F
Target outlet: 85°F
Actual outlet: 85.2°F
✓ Performing well
```

#### wet_bulb_temp

**What it is**: Temperature air would reach if water evaporated into it  
**Source**: Weather station or wet bulb thermometer  
**Range**: 40-85°F (varies by climate and season)  
**Different from dry bulb**: Lower temperature that accounts for humidity

**Why it matters**: Physics limit - tower cannot cool water below this  
**Use case**: Theoretical minimum outlet temperature

**Real-world examples**:

```
Houston summer: 78°F wet bulb (humid)
Phoenix summer: 68°F wet bulb (dry)
Winter anywhere: 40-60°F wet bulb
```

**Critical concept**:

```
Outlet water temp = Wet bulb + Approach
Cannot cool below wet bulb temperature!
```

#### approach_temp

**What it is**: How close tower gets to wet bulb temperature  
**Formula**: `approach = outlet_water_temp - wet_bulb_temp`  
**Typical range**: 7-15°F  
**Lower is better**: Closer to wet bulb = better performance

**Examples**:

```
Excellent tower: 7-8°F approach
Good tower: 8-10°F approach
Average tower: 10-12°F approach
Poor tower: 12-15°F approach
Problem: >15°F approach
```

**Why it matters**: Primary performance metric  
**Deviation from design indicates**:

- Fouling (fill media clogged)
- Air flow restrictions
- Water distribution problems
- Fan issues

**Real-world scenario**:

```
Tower design approach: 10.0°F
Week 1: Actual 10.2°F ✓ OK
Week 2: Actual 10.5°F ⚠ Starting to drift
Week 3: Actual 12.0°F ❌ Problem - investigate
Week 4: Actual 14.0°F 🔴 Critical - immediate action
```

#### range_temp

**What it is**: Temperature drop across the tower  
**Formula**: `range = inlet_water_temp - outlet_water_temp`  
**Typical range**: 10-25°F  
**Meaning**: How much heat the tower is removing

**Examples**:

```
Light load: 10°F range (tower coasting)
Design load: 15°F range (normal operation)
Heavy load: 20-25°F range (working hard)
```

**Why it matters**: Indicates system heat load  
**Use case**:

- Low range → Process not running or low load
- High range → Process at full capacity
- Declining range → Possible tower fouling

**Real-world example**:

```
Chemical plant:
Day shift (full production): 20°F range
Night shift (reduced load): 12°F range
Weekend (shutdown): 8°F range
```

#### flow_rate_gpm

**What it is**: Gallons of water flowing through tower per minute  
**Source**: Flow meter  
**Typical**: 2-4 GPM per ton of capacity  
**Example**: 500-ton tower = 1,000-2,000 GPM

**Why it matters**: Affects cooling performance  
**Too low**: Reduced cooling capacity  
**Too high**: Pump energy waste, possible carryover

**Real-world example**:

```
500-ton tower:
Design flow: 2.5 GPM/ton = 1,250 GPM
Current flow: 1,250 GPM ✓ OK
Low flow (900 GPM): ❌ Pump issue or valve problem
High flow (1,800 GPM): ⚠ Pump running too fast
```

#### cycles_of_concentration

**What it is**: How many times water is reused before discharge  
**Formula**: `COC = conductivity_tower_water / conductivity_makeup_water`  
**Typical range**: 3-6 cycles  
**Higher is better**: Less water waste

**Examples**:

```
Poor management: 2-3 COC (wasting water)
Good management: 4-5 COC (efficient)
Excellent management: 5-7 COC (very efficient)
```

**Why it matters**: Water conservation and cost  
**Calculation**:

```
COC = 4 means:
- For every 4 gallons evaporated
- Need to add only 1 gallon makeup
- Discharge only 0.25 gallons as blowdown
```

**Real-world impact**:

```
Tower using 1,000 gallons/day at COC=3:
Increase to COC=5:
- Save ~200 gallons/day makeup water
- Save ~$150/month water cost
- Reduce sewer charges
```

#### fan_speed_percent

**What it is**: How fast the fan is running (0-100%)  
**Source**: Variable Frequency Drive (VFD) or control system  
**Range**: 0-100%  
**Typical operation**: 60-80% during normal load

**Why it matters**:

- Higher speed = more cooling but more energy
- Can indicate load conditions
- Stuck at 100% may indicate problem

**Examples**:

```
Light load (cool day): 50% fan speed
Normal operation: 70% fan speed
Hot day (high load): 90% fan speed
Problem (fouling): 100% and still can't cool
```

**Energy relationship**:

```
Fan power = (Speed)³
50% speed = 12.5% power
75% speed = 42% power
100% speed = 100% power
```

#### runtime_hours

**What it is**: Hours tower operated during period  
**Daily mode**: Up to 24 hours  
**Hourly mode**: Up to 1 hour  
**15-min mode**: 0.25 hours

**Examples**:

```
24/7 operation: 24.0 hours/day
Day shift only: 8-10 hours/day
Variable load: 15-18 hours/day
```

**Why it matters**:

- Calculate energy costs
- Predict maintenance needs
- Track capacity utilization

#### power_consumption_kwh

**What it is**: Electrical energy used by tower (mainly fan motor)  
**Source**: Power meter  
**Typical**: 0.01-0.02 kW per ton of capacity  
**Example**: 500-ton tower = 5-10 kW

**Calculation for daily reading**:

```
Power (kWh) = kW rating × runtime_hours × (fan_speed/100)

Example:
10 kW motor × 22.5 hours × 75% speed = 169 kWh
```

**Why it matters**: Operating cost  
**Cost calculation**:

```
850 kWh/day × $0.12/kWh = $102/day = $3,060/month
```

#### makeup_water_gallons

**What it is**: Fresh water added to replace evaporated water  
**Source**: Makeup water meter  
**Typical**: Varies widely by tower size and weather

**Why it matters**: Water cost, conservation  
**Affected by**:

- Evaporation rate (higher in hot, dry weather)
- Cycles of concentration
- Blowdown rate
- Leaks

**Example calculation**:

```
500-ton tower, hot day:
Evaporation: ~1.5 gal/min per 100 tons
= 7.5 gal/min × 60 min × 24 hr = 10,800 gal/day
At COC=4: makeup = 10,800 / 0.75 = 14,400 gal/day
```

#### blowdown_gallons

**What it is**: Water intentionally discharged to control mineral buildup  
**Source**: Blowdown meter or controller  
**Relationship**: `blowdown = makeup × (1 / (COC - 1))`

**Why it matters**: Balance between water waste and water quality  
**Example**:

```
Makeup: 1,200 gallons
COC target: 4
Blowdown needed: 1,200 / 3 = 400 gallons
```

---

## 3. water_quality.py - Water Chemistry

### Purpose

Laboratory test results for water chemistry and microbiology.

### Complete Attribute List (16 attributes)

| Attribute               | Type    | Description                        | Example      | Units        |
|-------------------------|---------|------------------------------------|--------------|--------------|
| **tower_id**            | string  | Tower identifier                   | `CT-001`     | -            |
| **test_date**           | date    | When sample was collected          | `2024-12-15` | YYYY-MM-DD   |
| **ph**                  | float   | Acidity/alkalinity                 | `8.2`        | pH scale     |
| **conductivity**        | float   | Total dissolved solids indicator   | `1500`       | μS/cm        |
| **tds**                 | float   | Total dissolved solids             | `1000`       | ppm          |
| **hardness**            | float   | Calcium/magnesium content          | `400`        | ppm as CaCO₃ |
| **alkalinity**          | float   | Buffering capacity                 | `200`        | ppm as CaCO₃ |
| **chlorides**           | float   | Chloride ion concentration         | `150`        | ppm          |
| **sulfates**            | float   | Sulfate ion concentration          | `100`        | ppm          |
| **silica**              | float   | Silicon dioxide content            | `50`         | ppm          |
| **calcium**             | float   | Calcium ion concentration          | `120`        | ppm          |
| **magnesium**           | float   | Magnesium ion concentration        | `30`         | ppm          |
| **phosphate**           | float   | Phosphate content                  | `15.0`       | ppm          |
| **iron**                | float   | Iron content (corrosion indicator) | `0.5`        | ppm          |
| **bacteria_count**      | integer | Total aerobic bacteria             | `50000`      | CFU/ml       |
| **legionella_detected** | boolean | Legionella presence                | `false`      | true/false   |
| **oxidizer_level**      | float   | Biocide residual                   | `2.0`        | ppm          |

### Detailed Attribute Explanations

#### ph

**What it is**: Measure of acidity (low) or alkalinity (high)  
**Scale**: 0-14 (7 is neutral)  
**Target range**: 7.0-9.0 for cooling towers  
**Ideal**: 7.5-8.5

**Ranges and meanings**:

```
< 6.0: Very acidic - corrosion risk ❌
6.0-7.0: Slightly acidic - monitor closely ⚠
7.0-9.0: Acceptable range ✓
9.0-10.0: High alkalinity - scaling risk ⚠
> 10.0: Very alkaline - severe scaling ❌
```

**Why it matters**:

- pH too low → Metal corrosion
- pH too high → Mineral scaling
- Affects biocide effectiveness

**Real-world example**:

```
Week 1: pH 8.2 ✓ Good
Week 2: pH 9.5 ⚠ Add acid to lower pH
Week 3: pH 8.1 ✓ Corrected
```

#### conductivity

**What it is**: Measure of dissolved ions (minerals) in water  
**Units**: microSiemens per centimeter (μS/cm)  
**Typical range**: 1,200-2,500 μS/cm  
**Use**: Indicates concentration of dissolved minerals

**Relationship to COC**:

```
Makeup water conductivity: 500 μS/cm
Tower water conductivity: 2,000 μS/cm
COC = 2,000 / 500 = 4.0
```

**Why it matters**: Quick indicator of water concentration  
**Problems**:

- Too high (>3,000): Scaling risk
- Too low (<1,000): Wasting water (low COC)
- Rapidly increasing: Inadequate blowdown

#### tds (Total Dissolved Solids)

**What it is**: Total amount of dissolved minerals  
**Relationship**: `TDS ≈ conductivity × 0.7`  
**Example**: 1,500 μS/cm × 0.7 = 1,050 ppm TDS  
**Target**: < 2,000 ppm

**Why it matters**: High TDS → scaling deposits

#### hardness

**What it is**: Calcium and magnesium content  
**Units**: ppm as calcium carbonate (CaCO₃)  
**Categories**:

```
0-75 ppm: Soft water
75-150 ppm: Moderately hard
150-300 ppm: Hard
> 300 ppm: Very hard
```

**Why it matters**: Primary cause of scale formation  
**Calculation**: `Hardness = (2.5 × Ca) + (4.1 × Mg)`

**Real-world impact**:

```
Hardness 200 ppm: Moderate scaling potential
Hardness 400 ppm: High scaling risk
Hardness 600 ppm: Severe scaling - chemical treatment needed
```

#### alkalinity

**What it is**: Water's ability to resist pH changes  
**Units**: ppm as CaCO₃  
**Target**: 100-300 ppm  
**Technical**: Carbonate and bicarbonate content

**Why it matters**: Buffering capacity affects pH stability  
**Problems**:

- Too low: pH swings easily
- Too high: Scaling issues

#### chlorides

**What it is**: Chloride ion (Cl⁻) concentration  
**Source**: Makeup water, chemical treatment  
**Target**: < 250 ppm  
**Concern**: Corrosion of stainless steel

**Why it matters**: High chlorides → pitting corrosion  
**Warning signs**:

```
< 200 ppm: Safe ✓
200-400 ppm: Moderate risk ⚠
> 400 ppm: High corrosion risk ❌
```

#### sulfates

**What it is**: Sulfate ion (SO₄²⁻) concentration  
**Source**: Makeup water  
**Target**: < 200 ppm  
**Concern**: Scaling when combined with calcium

**Why it matters**: Forms gypsum scale (calcium sulfate)

#### silica

**What it is**: Silicon dioxide (SiO₂) content  
**Source**: Makeup water, dissolved minerals  
**Target**: < 150 ppm  
**Problem**: Forms very hard scale

**Why it matters**: Silica scale is extremely difficult to remove  
**Limits COC**:

```
Makeup water silica: 15 ppm
Silica limit: 150 ppm
Maximum COC: 150/15 = 10
```

#### calcium

**What it is**: Calcium ion (Ca²⁺) concentration  
**Typical**: 80-180 ppm  
**Primary cause**: Scale formation (calcium carbonate)

**Scale formation risk**:

```
Calcium × Alkalinity > 50,000: High scaling risk
Example: 200 ppm Ca × 300 ppm Alk = 60,000 ❌
```

#### magnesium

**What it is**: Magnesium ion (Mg²⁺) concentration  
**Typical**: 20-50 ppm  
**Contributes to**: Total hardness, scaling

#### phosphate

**What it is**: Phosphate (PO₄³⁻) concentration  
**Source**: Corrosion inhibitor chemicals  
**Target**: 10-20 ppm (when used as treatment)

**Why it matters**:

- Corrosion control
- But can cause scaling if too high

#### iron

**What it is**: Iron (Fe) content in water  
**Source**: Corrosion of steel components  
**Target**: < 0.5 ppm  
**Warning sign**: Rising iron = active corrosion

**Interpretation**:

```
< 0.3 ppm: Normal ✓
0.3-1.0 ppm: Moderate corrosion ⚠
> 1.0 ppm: Severe corrosion ❌
```

**Visual indicator**: Water appears rusty/brown when iron is high

#### bacteria_count

**What it is**: Total aerobic bacteria count  
**Units**: Colony Forming Units per milliliter (CFU/ml)  
**Test method**: Heterotrophic Plate Count

**Ranges**:

```
< 10,000 CFU/ml: Excellent ✓
10,000-100,000: Acceptable ✓
100,000-1,000,000: High - increase biocide ⚠
> 1,000,000: Very high - immediate action ❌
```

**Why it matters**:

- Biofilm formation
- MIC (Microbiologically Influenced Corrosion)
- Plugging of fill media
- Legionella growth environment

#### legionella_detected

**What it is**: Presence of Legionella bacteria  
**Values**: `true` or `false`  
**Test method**: PCR or culture  
**Target**: `false` (not detected)

**CRITICAL**:

```
legionella_detected = true → IMMEDIATE ACTION REQUIRED
- Public health hazard
- Can cause Legionnaires' disease
- Legal liability
- Shutdown may be required
```

**Response**:

1. Hyperchlorinate immediately
2. Shut down or divert mist
3. Notify health authorities (in some jurisdictions)
4. Retest until negative
5. Review and improve water treatment program

#### oxidizer_level

**What it is**: Biocide residual in water  
**Common biocides**:

- Chlorine: 1-3 ppm
- Bromine: 2-4 ppm
- Other: per manufacturer

**Target**: Maintain consistent level per treatment program

**Why it matters**:

- Controls bacteria growth
- Too low → bacteria proliferation
- Too high → corrosion, chemical waste

**Real-world monitoring**:

```
Target: 2.0 ppm chlorine
Monday: 2.1 ppm ✓
Tuesday: 1.5 ppm ⚠ Increase feed rate
Wednesday: 0.5 ppm ❌ System failure - investigate
```

---

## 4. maintenance.py - Maintenance History

### Purpose

Record of all maintenance work performed on towers.

### Complete Attribute List (12 attributes)

| Attribute            | Type   | Description             | Example                | Units      |
|----------------------|--------|-------------------------|------------------------|------------|
| **maintenance_id**   | string | Unique work order ID    | `MAINT-000001`         | -          |
| **tower_id**         | string | Tower identifier        | `CT-001`               | -          |
| **date**             | date   | When work was performed | `2024-11-20`           | YYYY-MM-DD |
| **maintenance_type** | string | Category of work        | `Preventive`           | enum       |
| **category**         | string | System affected         | `Mechanical`           | enum       |
| **description**      | string | What was done           | `Quarterly inspection` | text       |
| **technician_id**    | string | Who performed work      | `TECH-012`             | -          |
| **duration_hours**   | float  | How long work took      | `3.5`                  | hours      |
| **cost**             | float  | Total cost of work      | `850.00`               | dollars    |
| **parts_replaced**   | string | Components replaced     | `Fan Belt`             | text       |
| **downtime_hours**   | float  | Tower offline time      | `0.0`                  | hours      |
| **next_scheduled**   | date   | Next service due date   | `2025-02-20`           | YYYY-MM-DD |

### Detailed Attribute Explanations

#### maintenance_type

**What it is**: Classification of maintenance work  
**Options**:

1. **Preventive** - Scheduled routine maintenance
2. **Corrective** - Fixing known problems
3. **Emergency** - Unplanned urgent repairs

**Examples**:

```
Preventive:
- Quarterly inspection
- Annual motor service
- Monthly water treatment check

Corrective:
- Replace worn fan belt (known issue)
- Repair minor leak
- Adjust water distribution

Emergency:
- Fan motor failure
- Major leak repair
- Structural damage
```

**Why it matters**:

- Preventive → Low cost, planned
- Corrective → Medium cost, semi-planned
- Emergency → High cost, unplanned downtime

**Cost comparison**:

```
Preventive: $200-800 typical
Corrective: $500-2,000 typical
Emergency: $1,500-5,000+ typical
```

#### category

**What it is**: Which system was worked on  
**Options**:

1. **Mechanical** - Moving parts, fans, pumps
2. **Electrical** - Motors, controls, wiring
3. **Chemical** - Water treatment
4. **Structural** - Basin, frame, fill media

**Examples by category**:

```
Mechanical:
- Fan bearing lubrication
- Belt replacement
- Gearbox service

Electrical:
- Motor replacement
- VFD repair
- Control sensor calibration

Chemical:
- Water treatment adjustment
- Chemical feed pump repair
- Biocide system maintenance

Structural:
- Basin leak repair
- Fill media replacement
- Drift eliminator repair
```

#### duration_hours

**What it is**: Actual labor time spent  
**Range**: 0.5-12+ hours  
**Use**: Labor cost calculation

**Typical durations**:

```
Quick inspection: 0.5-1 hour
Routine maintenance: 2-4 hours
Minor repair: 2-6 hours
Major repair: 6-12 hours
Major overhaul: 12-40 hours
```

#### cost

**What it is**: Total cost including labor and parts  
**Components**:

- Labor: hours × rate
- Parts: actual part cost
- Overhead: travel, equipment

**Example calculation**:

```
Labor: 3.5 hours × $125/hr = $437.50
Parts: Fan belt = $85.00
Travel: 1 hour × $100 = $100.00
Total cost: $622.50
```

#### parts_replaced

**What it is**: List of components that were replaced  
**Examples**:

```
Simple: "None" (inspection only)
Single: "Fan Belt"
Multiple: "Bearings, Motor, Coupling"
Major: "Fill Media, Drift Eliminators, Nozzles"
```

**Why track**:

- Parts inventory management
- Reliability tracking
- Warranty claims
- Budgeting for next year

#### downtime_hours

**What it is**: How long tower was offline/non-operational  
**Range**: 0-24+ hours  
**Impact**: Production loss, SLA violations

**Examples**:

```
Preventive (running): 0 hours (no shutdown)
Minor repair: 1-2 hours
Major repair: 4-8 hours
Component replacement: 8-24 hours
```

**Cost of downtime**:

```
Data center: $5,000-50,000/hour
Manufacturing: $1,000-10,000/hour
HVAC: $100-500/hour discomfort
```

#### next_scheduled

**What it is**: When next service is due  
**Only for**: Preventive maintenance  
**Calculation**: date + interval

**Typical intervals**:

```
Monthly: Chemical treatment check
Quarterly: General inspection
Semi-annual: Detailed inspection
Annual: Major service
Bi-annual: Overhaul consideration
```

---

## 5. inspection.py - Inspection Reports

### Purpose

Detailed physical condition assessment by trained inspectors.

### Complete Attribute List (20 attributes)

| Attribute                      | Type    | Description                  | Example               | Values                     |
|--------------------------------|---------|------------------------------|-----------------------|----------------------------|
| **inspection_id**              | string  | Unique inspection ID         | `INSP-000001`         | -                          |
| **tower_id**                   | string  | Tower identifier             | `CT-001`              | -                          |
| **inspection_date**            | date    | When inspection performed    | `2024-12-10`          | YYYY-MM-DD                 |
| **inspector_id**               | string  | Who performed inspection     | `INSP-005`            | -                          |
| **inspection_type**            | string  | Scheduled or special         | `Monthly`             | enum                       |
| **fill_condition**             | string  | Condition of fill media      | `Good`                | Excellent/Good/Fair/Poor   |
| **drift_eliminator_condition** | string  | Drift eliminator state       | `Good`                | Excellent/Good/Fair/Poor   |
| **basin_condition**            | string  | Basin/sump condition         | `Excellent`           | Excellent/Good/Fair/Poor   |
| **fan_condition**              | string  | Fan assembly condition       | `Good`                | Excellent/Good/Fair/Poor   |
| **motor_condition**            | string  | Motor condition              | `Good`                | Excellent/Good/Fair/Poor   |
| **drive_system_condition**     | string  | Drive/transmission condition | `Good`                | Excellent/Good/Fair/Poor   |
| **structure_condition**        | string  | Frame/structure condition    | `Excellent`           | Excellent/Good/Fair/Poor   |
| **vibration_level**            | string  | Vibration assessment         | `Normal`              | Normal/Elevated/Excessive  |
| **noise_level**                | string  | Noise assessment             | `Normal`              | Normal/Elevated/Excessive  |
| **leaks_detected**             | boolean | Any water leaks found        | `false`               | true/false                 |
| **corrosion_level**            | string  | Corrosion severity           | `Minor`               | None/Minor/Moderate/Severe |
| **scaling_level**              | string  | Scale buildup severity       | `Minor`               | None/Minor/Moderate/Severe |
| **biological_growth**          | string  | Biofilm/algae/slime          | `None`                | None/Minor/Moderate/Severe |
| **overall_rating**             | integer | Inspector's overall score    | `8`                   | 1-10                       |
| **findings**                   | string  | Summary of issues            | `All components good` | text                       |
| **recommendations**            | string  | Action items                 | `Continue schedule`   | text                       |

### Detailed Attribute Explanations

#### inspection_type

**What it is**: Frequency/purpose of inspection  
**Types**:

- **Monthly** - Quick visual inspection
- **Quarterly** - Detailed component check
- **Annual** - Comprehensive evaluation
- **Special** - After incident or for specific issue

**Scope by type**:

```
Monthly (30-60 min):
- Visual check
- Basic measurements
- Safety check

Quarterly (2-3 hours):
- Detailed visual
- Vibration testing
- All components checked
- Sample photos

Annual (4-8 hours):
- Full shutdown inspection
- Internal examination
- Non-destructive testing
- Detailed report
```

#### fill_condition

**What it is**: Condition of the fill media (heat transfer surface)  
**Ratings**:

**Excellent**:

- Clean, no visible fouling
- Proper spacing intact
- No damage or deterioration
- Full heat transfer capability

**Good**:

- Minor fouling present
- Small amount of scale
- All sheets/channels intact
- 90-95% effective

**Fair**:

- Moderate fouling
- Visible scale buildup
- Some damaged sections
- 70-85% effective
- Cleaning recommended within 30 days

**Poor**:

- Heavy fouling/plugging
- Significant scale
- Multiple damaged areas
- 50-70% effective
- Cleaning/replacement needed immediately

**Why critical**: Fill media is the tower's "heart" - where cooling happens

#### drift_eliminator_condition

**What it is**: Condition of mist/droplet catchers  
**Purpose**: Prevent water droplets from leaving tower

**Condition indicators**:

```
Excellent: All blades intact, properly positioned
Good: Minor damage, mostly effective
Fair: Several broken/missing blades
Poor: Extensive damage, excessive drift
```

**Why matters**: Poor drift eliminators = water loss and environmental issues

#### basin_condition

**What it is**: Condition of water collection basin/sump  
**Check for**:

- Cracks or leaks
- Corrosion
- Sediment buildup
- Structural integrity

**Ratings explained**:

```
Excellent: No cracks, clean, no corrosion
Good: Minor surface rust, no leaks
Fair: Some corrosion, small cracks, sediment present
Poor: Active leaks, severe corrosion, structural concerns
```

#### fan_condition

**What it is**: Condition of fan assembly  
**Components checked**:

- Fan blades (cracks, erosion, balance)
- Hub (corrosion, cracks)
- Balance (wobble, vibration)
- Mounting (secure, aligned)

**Specific issues to note**:

```
Excellent: Balanced, no damage, quiet
Good: Minor wear, properly balanced
Fair: Visible wear, slight imbalance
Poor: Cracks, severe imbalance, risk of failure
```

**Critical**: Fan failure can be catastrophic

#### motor_condition

**What it is**: Electric motor condition  
**Checked**:

- External condition
- Temperature (infrared scan)
- Noise/vibration
- Electrical connections
- Lubrication

**Ratings**:

```
Excellent: Cool, quiet, no vibration
Good: Normal temperature, minimal vibration
Fair: Running warm, increased vibration
Poor: Overheating, excessive vibration, failing
```

#### drive_system_condition

**What it is**: Belt, gearbox, or direct drive condition  
**For belt drives**:

- Belt wear/cracks
- Tension
- Sheave condition

**For gear drives**:

- Oil level/condition
- Noise
- Temperature
- Leaks

#### vibration_level

**What it is**: Mechanical vibration assessment  
**Measurement**: Usually with vibration meter

**Levels**:
**Normal**: < 0.2 in/sec RMS

- Smooth operation
- No concerning vibration
- Typical measurement: 0.05-0.15 in/sec

**Elevated**: 0.2-0.4 in/sec RMS

- Noticeable vibration
- Investigate cause
- Monitor closely
- Schedule corrective action

**Excessive**: > 0.4 in/sec RMS

- Severe vibration felt
- Immediate action required
- Risk of mechanical failure
- May need shutdown

**Common causes of elevated vibration**:

```
- Imbalanced fan
- Worn bearings
- Loose mounting bolts
- Misaligned shaft
- Damaged fan blades
```

#### noise_level

**What it is**: Audible noise assessment  
**Measured**: Decibels (dB) or subjective

**Levels**:
**Normal**: 75-85 dBA

- Typical cooling tower operation
- Consistent sound
- No unusual noises

**Elevated**: 85-95 dBA

- Louder than normal
- Some unusual sounds
- Investigate cause

**Excessive**: > 95 dBA

- Very loud
- Grinding, squealing, or rattling
- Immediate concern
- Bearing failure likely

#### leaks_detected

**What it is**: Any water leaking from tower  
**Types of leaks**:

```
Basin leaks:
- Cracks in concrete
- Failed joints
- Corrosion holes

Piping leaks:
- Connection failures
- Valve leaks
- Pipe corrosion

Spray leaks:
- Mist escaping enclosure
- Drift eliminator failure
```

**Why critical**:

- Water waste
- Structural damage
- Ice hazard (winter)
- Legionella risk (stagnant puddles)

#### corrosion_level

**What it is**: Extent of metal deterioration  
**Assessed on**: Frame, basin, piping, components

**Levels**:
**None**: No visible corrosion

- Protective coatings intact
- Metal surfaces clean

**Minor**: Surface rust only

- <10% of surface area
- No structural impact
- Monitor and treat

**Moderate**: Noticeable corrosion

- 10-30% of surface area
- Some pitting
- Treatment and coating needed
- Monitor structural integrity

**Severe**: Extensive corrosion

- > 30% of surface area
- Deep pitting or holes
- Structural concern
- Component replacement needed

#### scaling_level

**What it is**: Mineral deposit buildup  
**Locations**: Fill media, basin, piping, nozzles

**Levels**:
**None**: No visible scale

- Water treatment effective
- Clean surfaces

**Minor**: Light coating

- <3mm thick
- Does not restrict flow
- Increase water treatment

**Moderate**: Noticeable buildup

- 3-10mm thick
- Some restriction
- Cleaning needed soon

**Severe**: Heavy deposits

- > 10mm thick
- Significant restriction
- Immediate cleaning required
- Performance degraded >15%

#### biological_growth

**What it is**: Biofilm, algae, slime assessment  
**Types**:

- Biofilm (bacterial slime layer)
- Algae (green growth)
- Fungus

**Levels**:
**None**: No visible growth

- Biocide program effective
- Clean surfaces

**Minor**: Slight film/discoloration

- <10% coverage
- Increase biocide

**Moderate**: Visible growth

- 10-40% coverage
- Slippery surfaces
- Biofilm established

**Severe**: Heavy growth

- > 40% coverage
- Thick slime layers
- Green algae visible
- System shock treatment needed

#### overall_rating

**What it is**: Inspector's judgment of tower condition  
**Scale**: 1-10

**Rating guide**:

```
10: Perfect - brand new condition
9: Excellent - well-maintained, no issues
8: Good - minor wear, properly maintained
7: Good - some maintenance needed
6: Fair - several issues noted
5: Fair - maintenance backlog evident
4: Poor - significant issues
3: Poor - multiple serious problems
2: Critical - major failures imminent
1: Critical - unsafe operation
```

**Use**: Quick overall assessment for management

#### findings

**What it is**: Narrative summary of inspection  
**Includes**:

- Key observations
- Specific issues found
- Measurements taken
- Photos referenced

**Examples**:

```
Excellent tower:
"All components in excellent condition; no issues noted; 
tower operating at design parameters"

Average tower:
"Moderate scaling on fill media; fan showing normal wear; 
basin has minor surface rust; approach temp 1.5°F above design"

Poor tower:
"Critical scaling throughout; excessive vibration detected 
(0.45 in/sec); basin leak at southeast corner; legionella 
detected in water sample; immediate action required"
```

#### recommendations

**What it is**: Specific action items  
**Format**: Priority-ordered list  
**Time-based**: Immediate, short-term, long-term

**Example recommendations**:

```
Immediate (0-7 days):
- "Repair basin leak at SE corner"
- "Replace excessively vibrating fan bearing"
- "Treat for legionella contamination"

Short-term (30 days):
- "Clean fill media to remove scaling"
- "Balance fan assembly"
- "Increase biocide dosing"

Long-term (90 days):
- "Consider fill media replacement in 6 months"
- "Plan for basin coating renewal"
- "Upgrade to VFD for energy savings"
```

---

## 6. analysis_result.py - Analysis Outputs

### Purpose

System-generated analysis combining all data sources.

### Complete Attribute List

| Attribute                      | Type     | Description                    | Example                |
|--------------------------------|----------|--------------------------------|------------------------|
| **analysis_date**              | datetime | When analysis was run          | `2024-12-18T10:30:00Z` |
| **tower_id**                   | string   | Tower analyzed                 | `CT-001`               |
| **tower_name**                 | string   | Tower name                     | `Main Plant Tower A`   |
| **client_name**                | string   | Client name                    | `ABC Manufacturing`    |
| **priority_score**             | float    | Urgency score (0-100)          | `75.5`                 |
| **priority_level**             | string   | Urgency category               | `High`                 |
| **response_time_hours**        | integer  | Required response time         | `24`                   |
| **safety_score**               | float    | Safety component (0-40)        | `30.0`                 |
| **performance_score**          | float    | Performance component (0-30)   | `25.5`                 |
| **maintenance_score**          | float    | Maintenance component (0-20)   | `15.0`                 |
| **client_impact_score**        | float    | Client impact component (0-10) | `5.0`                  |
| **issues**                     | list     | Detected problems              | (see below)            |
| **immediate_actions**          | list     | Do today                       | (see below)            |
| **short_term_actions**         | list     | Do this week/month             | (see below)            |
| **preventive_measures**        | list     | Prevent future issues          | (see below)            |
| **predicted_days_to_critical** | integer  | Days until critical failure    | `15`                   |
| **efficiency_current**         | float    | Current efficiency             | `88.5%`                |
| **efficiency_design**          | float    | Design efficiency              | `92.0%`                |

### Detailed Attribute Explanations

#### priority_score

**What it is**: Calculated urgency metric (0-100)  
**Formula**:

```
priority_score = 
  safety_score (0-40) +
  performance_score (0-30) +
  maintenance_score (0-20) +
  client_impact_score (0-10)
```

**Score ranges**:

```
0-19: Good - Well maintained
20-39: Low - Routine monitoring
40-59: Medium - Schedule maintenance
60-79: High - Attention needed (7 days)
80-100: Critical - Immediate action (0-24 hours)
```

#### priority_level

**What it is**: Text classification of urgency

**Categories**:

```
Good (0-19):
- No immediate concerns
- Continue monitoring
- Routine maintenance schedule

Low (20-39):
- Minor issues
- Schedule routine maintenance
- No urgency

Medium (40-59):
- Some concerns
- Schedule maintenance within 30 days
- Monitor closely

High (60-79):
- Significant issues
- Address within 7 days
- May affect performance

Critical (80-100):
- Severe problems
- Immediate action (0-24 hours)
- Safety or major failure risk
```

#### response_time_hours

**What it is**: Maximum time before action required

**Mapping**:

```
Critical (80-100): 0-4 hours
High (60-79): 24 hours
Medium (40-59): 7 days (168 hours)
Low (20-39): 30 days
Good (0-19): Routine schedule
```

#### safety_score (0-40 points)

**What triggers points**:

```
Legionella detected: +40 points (automatic critical)
Excessive vibration: +30 points
Bacteria > 1M CFU/ml: +25 points
Structural damage: +30 points
Major leak: +20 points
pH < 6.0 or > 10.0: +20 points
```

**Example calculation**:

```
Tower with:
- Bacteria count 500,000 CFU/ml: +15 points
- Elevated vibration: +10 points
Total safety_score: 25 points
```

#### performance_score (0-30 points)

**What triggers points**:

```
Approach deviation:
  > 3.0°F: +25 points
  > 2.0°F: +15 points
  > 1.0°F: +10 points

Efficiency:
  < 80%: +25 points
  < 85%: +15 points
  < 90%: +10 points

Range deviation:
  > 3.0°F: +15 points
```

**Example**:

```
Tower with:
- Approach 2.5°F above design: +15 points
- Efficiency 87%: +15 points
Total performance_score: 30 points
```

#### maintenance_score (0-20 points)

**What triggers points**:

```
Emergency maintenance in last 30 days: +20 points
Overdue preventive maintenance: +15 points
Component in Poor condition: +10 points
Scaling Moderate/Severe: +10 points
Multiple corrective repairs: +8 points
```

#### client_impact_score (0-10 points)

**What triggers points**:

```
SLA violation: +10 points
Premium tier client: +5 points
Downtime in last 7 days: +5 points
Client complaint: +8 points
```

#### issues

**What it is**: List of specific problems detected

**Structure**:

```json
{
  "category": "Performance",
  "severity": "High",
  "description": "Approach temperature 2.5°F above design",
  "detected_date": "2024-12-18",
  "metric": "approach_temp",
  "actual_value": 12.5,
  "threshold_value": 10.0
}
```

**Categories**:

- Safety
- Performance
- Water Quality
- Maintenance
- Client Impact

#### immediate_actions

**What it is**: Must-do items for today  
**Examples**:

```
[
  "Inspect and clean fill media for blockage or fouling",
  "Verify fan operation and adjust speed if needed",
  "Increase biocide dosing to 3.0 ppm",
  "Repair basin leak at southeast corner"
]
```

#### short_term_actions

**What it is**: Address this week/month  
**Examples**:

```
[
  "Schedule comprehensive water quality analysis",
  "Replace worn fan belt within 7 days",
  "Clean scale from basin and piping",
  "Balance fan assembly"
]
```

#### preventive_measures

**What it is**: Prevent future problems  
**Examples**:

```
[
  "Implement weekly approach temperature monitoring",
  "Review water treatment program effectiveness",
  "Install vibration monitoring sensor",
  "Upgrade to VFD for better efficiency"
]
```

#### predicted_days_to_critical

**What it is**: Estimated days until failure if no action  
**Calculation**: Based on degradation trends

**Examples**:

```
15 days: Moderate deterioration rate
5 days: Rapid degradation
30+ days: Slow degradation
NULL: No trend detected or excellent condition
```

---

## 📊 Data Relationships Summary

```
PRIMARY DATA (Collected)
├── towers_master → Tower specifications
├── operational_data → Sensor readings
├── water_quality → Lab test results
├── maintenance_history → Work orders
└── inspections → Physical assessments

DERIVED DATA (Calculated)
└── analysis_result → Combines all above data
    ├── Calculates priority_score
    ├── Identifies issues
    └── Generates recommendations
```
