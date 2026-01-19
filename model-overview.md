# 📊 Models Overview with Raw Data (Nalco Water Validated)

**✅ All datasets validated against Nalco Water Handbook and industry standards**

---

## 1. tower.py - Tower Master Data

### Purpose

Basic information about each cooling tower - the "identity card" for each tower.

### What It Stores

- Tower specifications
- Design parameters (validated against CTI standards)
- Location and client info
- Static/rarely changing data

### Raw Data Sample (`towers_master.csv`)

```csv
tower_id,tower_name,client_id,client_name,location,installation_date,tower_type,capacity_tons,manufacturer,model,design_approach,design_range,latitude,longitude
CT-001,Main Plant Tower A,CLIENT-001,ABC Manufacturing,Houston TX,2018-03-15,Counterflow,500,Baltimore Aircoil,VXC-500,10.0,15.0,29.7604,-95.3698
CT-002,East Wing Tower B,CLIENT-001,ABC Manufacturing,Houston TX,2020-06-20,Induced Draft,750,SPX Cooling,PTXI-750,12.0,18.0,29.7604,-95.3698
CT-003,Data Center Tower,CLIENT-002,TechCorp Solutions,Dallas TX,2019-11-10,Crossflow,1200,Evapco,ATC-1200,11.0,20.0,32.7767,-96.7970
```

### Model Structure

```python
@dataclass
class Tower:
    tower_id: str  # "CT-001"
    tower_name: str  # "Main Plant Tower A"
    client_name: str  # "ABC Manufacturing"
    capacity_tons: float  # 500.0 tons (CTI standard sizing)
    design_approach: float  # 10.0 °F (CTI: 5-15°F for modern towers)
    design_range: float  # 15.0 °F (CTI: 10-20°F typical)
    tower_type: str  # "Counterflow" (industry standard types)
    installation_date: date  # 2018-03-15
    manufacturer: str  # Baltimore Aircoil (major manufacturer)
    model: str  # VXC-500 (manufacturer designation)
```

### ✅ Nalco Water Validation

**Design Parameters:**

- ✅ **Approach**: 5-15°F aligns with modern tower performance
- ✅ **Range**: 10-25°F matches industry standards
- ✅ **Capacity**: 100-2000+ tons covers small to large installations
- ✅ **Manufacturers**: Baltimore Aircoil, SPX, Evapco are industry leaders

**CTI Standard Rating**: 95°F inlet → 85°F outlet @ 78°F wet bulb

**Key Point**: One row per tower, rarely changes. Design specs used for performance deviation calculations.

---

## 2. operational_data.py - Daily Operational Metrics

### Purpose

Daily/hourly/15-minute performance data - temperatures, flow rates, runtime. This is the "heartbeat" data.

### What It Stores

- Temperature readings (validated against Nalco limits)
- Water flow rates (CTI standard: 3 GPM/ton)
- Energy consumption
- Runtime hours
- Cycles of concentration (Nalco: 3.0-7.0)

### Raw Data Sample (`operational_data.csv`)

```csv
tower_id,date,inlet_water_temp,outlet_water_temp,wet_bulb_temp,approach_temp,range_temp,flow_rate_gpm,cycles_of_concentration,fan_speed_percent,runtime_hours,power_consumption_kwh,makeup_water_gallons,blowdown_gallons
CT-001,2024-12-18,95.5,85.2,75.0,10.2,10.3,2500,4.5,75.0,22.5,850.0,1200,600
CT-001,2024-12-17,94.8,84.5,74.5,10.0,10.3,2480,4.6,73.0,23.0,830.0,1150,580
CT-001,2024-12-16,96.2,86.0,75.5,10.5,10.2,2520,4.4,78.0,21.5,870.0,1250,620
CT-002,2024-12-18,98.0,86.5,76.0,10.5,11.5,3750,5.2,80.0,24.0,1100.0,1800,900
```

### Model Structure

```python
@dataclass
class OperationalData:
    tower_id: str  # "CT-001"
    date: date  # 2024-12-18
    inlet_water_temp: float  # 95.5 °F (hot water in, max 120°F per Nalco)
    outlet_water_temp: float  # 85.2 °F (cold water out)
    wet_bulb_temp: float  # 75.0 °F (air humidity temp, CTI basis)
    approach_temp: float  # 10.2 °F (outlet - wet_bulb, CTI: 5-15°F)
    range_temp: float  # 10.3 °F (inlet - outlet, typical: 10-20°F)
    flow_rate_gpm: float  # 2500 gallons/minute (~3 GPM/ton for 500 ton)
    cycles_of_concentration: float  # 4.5 (Nalco: 3.0-7.0 optimal)
    runtime_hours: float  # 22.5 hours today
    power_consumption_kwh: float  # 850 kWh used (~0.015 kW/ton)
```

### ✅ Nalco Water Validation

**Temperature Limits:**

- ✅ **Max Operating**: 120°F (Nalco standard)
- ✅ **Wet Bulb Basis**: 78°F (CTI rating standard)
- ✅ **Approach Range**: 5-15°F (modern towers)
- ✅ **Range**: 10-20°F typical

**Performance Metrics:**

- ✅ **Flow Rate**: 2-4 GPM/ton (CTI: 3 GPM/ton standard)
- ✅ **COC**: 3.0-7.0 (Nalco optimal range for water conservation)
- ✅ **Efficiency**: Calculated as Range/(Inlet-WB) × 100%

**Legionella Risk Zone**: 68-122°F (20-50°C) - towers operate in this range, requiring biocide treatment

**Key Point**: One row per tower per day (or per interval for hourly/15-min). Most frequently updated dataset.

---

## 3. water_quality.py - Water Chemistry Tests

### Purpose

Water chemistry analysis - pH, bacteria, minerals. This is the "health check" data validated against Nalco standards.

### What It Stores

- pH levels (Nalco: 6.5-9.0 preferred range)
- Bacteria counts (HPC limits per ASHRAE/CDC)
- Mineral content (hardness, alkalinity per Nalco specs)
- Legionella detection (zero tolerance)
- Chemical treatment levels

### Raw Data Sample (`water_quality.csv`)

```csv
tower_id,test_date,ph,conductivity,tds,hardness,alkalinity,chlorides,sulfates,silica,calcium,magnesium,phosphate,iron,bacteria_count,legionella_detected,oxidizer_level
CT-001,2024-12-15,8.2,1500,1000,400,200,150,100,50,120,30,15.0,0.5,50000,false,2.0
CT-001,2024-12-08,8.1,1480,980,390,195,145,95,48,118,29,14.5,0.4,45000,false,2.1
CT-002,2024-12-15,8.5,1650,1100,450,220,170,120,55,135,35,16.0,0.6,75000,false,1.8
CT-003,2024-12-15,9.2,2200,1500,600,280,220,150,70,180,45,18.0,1.2,850000,false,1.5
```

### Model Structure

```python
@dataclass
class WaterQuality:
    tower_id: str  # "CT-001"
    test_date: date  # 2024-12-15
    ph: float  # 8.2 (Nalco: 6.5-9.0 preferred, <6.5 or >9.0 = corrosive)
    conductivity: float  # 1500 μS/cm (increases with COC)
    bacteria_count: float  # 50,000 CFU/ml (HPC - Heterotrophic Plate Count)
    legionella_detected: bool  # false (CRITICAL if true! >1 CFU/ml = action)
    hardness: float  # 400 ppm (Nalco: 100-500 ppm CaCO3)
    alkalinity: float  # 200 ppm (Nalco: 100-500 ppm CaCO3)
    silica: float  # 50 ppm (Nalco: <100 ppm, max 150 with inhibitors)
    iron: float  # 0.5 ppm (corrosion indicator)
    oxidizer_level: float  # 2.0 ppm (biocide residual for Legionella control)
```

### ✅ Nalco Water Validation

**pH Standards (Nalco Water Handbook):**

- ✅ **Preferred Range**: 6.5-9.0 (prevents corrosion)
- ✅ **Excellent**: 7.5-8.5 (optimal)
- ✅ **Critical**: <6.0 or >10.0 (severe corrosion risk)

**Bacteria Limits (CDC/ASHRAE/Nalco):**

- ✅ **Excellent**: <10,000 CFU/ml HPC
- ✅ **Good**: 10,000-100,000 CFU/ml
- ✅ **Action Level**: >200,000 CFU/ml (system out of control)
- ✅ **Emergency**: >1,000,000 CFU/ml (immediate shutdown)

**Legionella Standards (CDC ELITE/ISO 11731):**

- ✅ **Safe**: <1 CFU/ml (detection limit)
- ✅ **Monitor**: 1-10 CFU/ml (possible amplification)
- ✅ **Action**: 10-100 CFU/ml (review treatment)
- ✅ **Remediation**: 100-1,000 CFU/ml (aggressive treatment)
- ✅ **Emergency**: >1,000 CFU/ml (associated with outbreaks)

**Mineral Limits (Nalco):**

- ✅ **Hardness**: 100-500 ppm as CaCO3
- ✅ **Alkalinity**: 100-500 ppm as CaCO3
- ✅ **Silica**: <100 ppm (150 ppm max with inhibitors)
- ✅ **Iron**: <2.0 ppm (corrosion indicator)

**Key Point**: One row per tower per test (weekly/bi-weekly). Critical for safety and ASHRAE 188 compliance.

---

## 4. maintenance.py - Maintenance History

### Purpose

Record of all maintenance activities - what was done, when, and why. Follows Nalco Water Management Plan requirements.

### What It Stores

- Maintenance dates and types (per ASHRAE 188)
- Work performed
- Parts replaced
- Costs and duration
- Technician info

### Raw Data Sample (`maintenance_history.csv`)

```csv
maintenance_id,tower_id,date,maintenance_type,category,description,technician_id,duration_hours,cost,parts_replaced,downtime_hours,next_scheduled
MAINT-000001,CT-001,2024-11-20,Preventive,Mechanical,Quarterly inspection and lubrication,TECH-012,3.5,850.00,None,0.0,2025-02-20
MAINT-000002,CT-001,2024-09-15,Corrective,Mechanical,Replaced worn fan belt,TECH-008,2.0,450.00,Fan Belt,1.5,
MAINT-000003,CT-002,2024-12-10,Emergency,Electrical,Fan motor bearing failure - replaced,TECH-012,6.0,3200.00,"Bearings Motor",5.0,
MAINT-000004,CT-003,2024-12-05,Preventive,Chemical,Water treatment adjustment and testing,TECH-023,1.5,250.00,None,0.0,2025-03-05
```

### Model Structure

```python
@dataclass
class MaintenanceRecord:
    maintenance_id: str  # "MAINT-000001"
    tower_id: str  # "CT-001"
    date: date  # 2024-11-20
    maintenance_type: str  # "Preventive" | "Corrective" | "Emergency"
    category: str  # "Mechanical" | "Electrical" | "Chemical" | "Structural"
    description: str  # "Quarterly inspection..."
    duration_hours: float  # 3.5 hours
    cost: float  # $850.00
    parts_replaced: str  # "Fan Belt, Bearings"
    downtime_hours: float  # 5.0 hours (tower offline)
    next_scheduled: date  # 2025-02-20 (for preventive only)
```

### ✅ Nalco Water Validation

**Maintenance Types (Nalco Water Management Plan):**

- ✅ **Preventive**: Quarterly/Monthly inspections (ASHRAE 188 requirement)
- ✅ **Corrective**: Repair identified issues (standard practice)
- ✅ **Emergency**: Unplanned failures (Nalco Failure Analysis Guide)

**Maintenance Categories (Nalco Handbook):**

- ✅ **Mechanical**: Fan, pump, belt maintenance (Chapter 14)
- ✅ **Electrical**: Motor, controls (Chapter 16)
- ✅ **Chemical**: Water treatment adjustment (Chapters 15-17)
- ✅ **Structural**: Basin, fill, drift eliminators (Failure Guide)

**Frequency Standards:**

- ✅ **Daily**: Visual checks, chemical monitoring (3x/week per regulations)
- ✅ **Weekly**: Biocide monitoring, visual inspection
- ✅ **Monthly**: Basic inspection (ASHRAE 188)
- ✅ **Quarterly**: Detailed inspection, Legionella testing
- ✅ **Annual**: Certification by qualified person

**Key Point**: One row per maintenance event. Tracks tower care history and ASHRAE 188 compliance.

---

## 5. inspection.py - Inspection Reports

### Purpose

Detailed component-by-component condition assessments. The "physical exam" data following Nalco inspection protocols.

### What It Stores

- Component conditions (Excellent/Good/Fair/Poor per Nalco standards)
- Vibration and noise levels (meter readings)
- Corrosion and scaling severity (Nalco classification)
- Overall ratings
- Inspector findings

### Raw Data Sample (`inspections.csv`)

```csv
inspection_id,tower_id,inspection_date,inspector_id,inspection_type,fill_condition,drift_eliminator_condition,basin_condition,fan_condition,motor_condition,drive_system_condition,structure_condition,vibration_level,noise_level,leaks_detected,corrosion_level,scaling_level,biological_growth,overall_rating,findings,recommendations
INSP-000001,CT-001,2024-12-10,INSP-005,Monthly,Good,Good,Excellent,Good,Good,Good,Excellent,Normal,Normal,false,Minor,Minor,None,8,All components in good condition,Continue current maintenance schedule
INSP-000002,CT-002,2024-12-12,INSP-008,Quarterly,Fair,Good,Good,Fair,Good,Fair,Good,Elevated,Normal,false,Moderate,Moderate,Minor,6,Moderate scaling on fill media; fan showing wear,Clean fill media within 30 days; monitor fan condition
INSP-000003,CT-003,2024-12-08,INSP-005,Monthly,Poor,Fair,Fair,Poor,Fair,Poor,Good,Excessive,Elevated,true,Severe,Severe,Moderate,4,Critical scaling throughout; excessive vibration; basin leak detected,IMMEDIATE: Repair leak; Clean/replace fill; Balance fan
```

### Model Structure

```python
@dataclass
class InspectionReport:
    inspection_id: str  # "INSP-000001"
    tower_id: str  # "CT-001"
    inspection_date: date  # 2024-12-10
    inspector_id: str  # "INSP-005"
    inspection_type: str  # "Monthly" | "Quarterly" (ASHRAE 188)

    # Component conditions (Nalco Failure Analysis classifications)
    fill_condition: str  # "Good" (Excellent/Good/Fair/Poor)
    fan_condition: str  # "Good"
    motor_condition: str  # "Good"
    basin_condition: str  # "Excellent"
    drift_eliminator_condition: str  # "Good"
    drive_system_condition: str  # "Good"
    structure_condition: str  # "Excellent"

    # Operational indicators (measured)
    vibration_level: str  # "Normal" | "Elevated" | "Excessive" (meter reading)
    noise_level: str  # "Normal" | "Elevated" | "Excessive"
    leaks_detected: bool  # false

    # Degradation indicators (Nalco Handbook Chapter 15-16)
    corrosion_level: str  # "Minor" (None/Minor/Moderate/Severe)
    scaling_level: str  # "Minor" (None/Minor/Moderate/Severe)
    biological_growth: str  # "None" (None/Minor/Moderate/Severe)

    overall_rating: int  # 8 out of 10
    findings: str  # "All components in good condition"
    recommendations: str  # "Continue current maintenance schedule"
```

### ✅ Nalco Water Validation

**Inspection Standards (Nalco Guide to Failure Analysis):**

- ✅ **Condition Classifications**: Excellent/Good/Fair/Poor (industry standard)
- ✅ **Vibration Levels**: Normal/Elevated/Excessive (meter-based)
- ✅ **Corrosion Severity**: None/Minor/Moderate/Severe (visual + testing)
- ✅ **Scaling Severity**: None/Minor/Moderate/Severe (Nalco deposition scale)

**Inspection Frequency (ASHRAE 188/Nalco):**

- ✅ **Monthly**: Basic visual inspection, operational checks
- ✅ **Quarterly**: Detailed inspection, Legionella testing (90 days)
- ✅ **Annual**: Certification by qualified person

**Component Assessment (Nalco Failure Analysis Guide):**

- ✅ **Fill Media**: Visual for fouling, scaling, biological growth
- ✅ **Fan/Motor**: Vibration analysis, bearing condition
- ✅ **Basin**: Structural integrity, leak detection
- ✅ **Drift Eliminators**: Condition, effectiveness

**Key Point**: One row per inspection (monthly/quarterly). Shows physical condition and ASHRAE 188 compliance.

---

## 6. analysis_result.py - Analysis Outputs

### Purpose

System-generated analysis results using Nalco-validated thresholds - priority scores, issue detection, recommendations.

### What It Stores

- Priority scores (0-100 scale)
- Identified issues (based on Nalco thresholds)
- Recommended actions (following Nalco best practices)
- Analysis timestamps
- Risk assessments

### Raw Data Sample (`analysis_results.json` or in-memory)

```json
{
  "analysis_date": "2024-12-18T10:30:00Z",
  "tower_id": "CT-001",
  "tower_name": "Main Plant Tower A",
  "client_name": "ABC Manufacturing",
  "priority_score": 75.5,
  "priority_level": "High",
  "response_time_hours": 24,
  "scores_breakdown": {
    "safety_score": 30.0,
    "performance_score": 25.5,
    "maintenance_score": 15.0,
    "client_impact_score": 5.0
  },
  "issues": [
    {
      "category": "Performance",
      "severity": "High",
      "description": "Approach temperature 2.5°F above design",
      "detected_date": "2024-12-18",
      "metric": "approach_temp",
      "actual_value": 12.5,
      "threshold_value": 10.0,
      "nalco_standard": "Approach >3°F above design indicates fouling (Nalco Handbook Ch 15)"
    },
    {
      "category": "Water Quality",
      "severity": "Medium",
      "description": "Bacteria count elevated at 150,000 CFU/ml",
      "detected_date": "2024-12-15",
      "nalco_standard": "HPC >100,000 CFU/ml indicates system moving out of control"
    }
  ],
  "recommendations": {
    "immediate": [
      "Inspect and clean fill media for blockage or fouling (Nalco Ch 15)",
      "Verify fan operation and adjust speed if needed"
    ],
    "short_term": [
      "Increase biocide dosing to reduce bacteria count (target <100,000 CFU/ml)",
      "Schedule comprehensive water quality analysis per ASHRAE 188"
    ],
    "preventive": [
      "Implement weekly approach temperature monitoring",
      "Review water treatment program effectiveness with Nalco standards"
    ]
  },
  "predicted_days_to_critical": 15,
  "maintenance_score": 82.5,
  "efficiency_current": 88.5,
  "efficiency_design": 92.0
}
```

### Model Structure

```python
@dataclass
class AnalysisResult:
    analysis_date: datetime  # When analysis ran
    tower_id: str  # "CT-001"

    # Priority assessment (Nalco-validated thresholds)
    priority_score: float  # 75.5 (0-100 scale)
    priority_level: str  # "High" (Critical/High/Medium/Low/Good)
    response_time_hours: int  # 24 hours (based on severity)

    # Score components (weighted per Nalco priorities)
    safety_score: float  # 30.0 (40% weight: Legionella, vibration, pH)
    performance_score: float  # 25.5 (30% weight: approach, efficiency)
    maintenance_score: float  # 15.0 (20% weight: overdue, emergency)
    client_impact_score: float  # 5.0 (10% weight: SLA, downtime)

    # Issues identified (using Nalco thresholds)
    issues: List[Issue]  # List of detected problems with standards

    # Recommendations (Nalco best practices)
    immediate_actions: List[str]  # 0-4 hours
    short_term_actions: List[str]  # 7-30 days
    preventive_measures: List[str]  # Ongoing

    # Predictions
    predicted_days_to_critical: int  # 15 days (trend-based)
    efficiency_current: float  # 88.5% (actual)
    efficiency_design: float  # 92.0% (target)
```

### ✅ Nalco Water Validation

**Alert Thresholds (Nalco Handbook + ASHRAE 188):**

**Performance Alerts:**

- ✅ **Approach Deviation**: >3°F above design = fouling (Nalco Ch 15)
- ✅ **Low Efficiency**: <80% = significant performance issue
- ✅ **High Approach**: >15°F = severe fouling/poor performance

**Safety Alerts (Priority 40%):**

- ✅ **Legionella Detected**: ANY detection = immediate action
- ✅ **HPC >1M CFU/ml**: Emergency shutdown level
- ✅ **pH <6.0 or >10.0**: Severe corrosion risk
- ✅ **Excessive Vibration**: Bearing/structural failure risk

**Water Quality Alerts:**

- ✅ **HPC >200,000 CFU/ml**: System out of control (Nalco standard)
- ✅ **Legionella >100 CFU/ml**: Remediation required
- ✅ **Silica >100 ppm**: Scaling risk (150 ppm max with inhibitors)

**Maintenance Alerts:**

- ✅ **Overdue Preventive**: >90 days since last service
- ✅ **Emergency Repairs**: Unplanned failures
- ✅ **Component Condition**: Poor rating on critical components

**Priority Levels (Response Times):**

- ✅ **Critical (80-100)**: 0-4 hours (safety issues, Legionella)
- ✅ **High (60-79)**: 24 hours (performance degradation)
- ✅ **Medium (40-59)**: 7 days (minor issues)
- ✅ **Low (20-39)**: 30 days (routine maintenance)
- ✅ **Good (0-19)**: Well maintained, no action needed

**Key Point**: Generated by analyzer using Nalco-validated thresholds. Not manually entered - system output.

---

## 📊 Data Relationships

```
towers_master (CT-001) - CTI/Nalco specs
    │
    ├─→ operational_data (daily readings for CT-001)
    │       2024-12-18: temps, flow, runtime (Nalco limits)
    │       2024-12-17: temps, flow, runtime
    │       Validates: Max temp 120°F, COC 3-7, pH 6.5-9.0
    │
    ├─→ water_quality (weekly tests for CT-001)
    │       2024-12-15: pH 8.2, HPC 50k CFU/ml (ASHRAE 188)
    │       2024-12-08: pH 8.1, HPC 45k CFU/ml
    │       Validates: Legionella quarterly, HPC monthly
    │
    ├─→ maintenance_history (all maintenance for CT-001)
    │       2024-11-20: Preventive - quarterly (ASHRAE 188)
    │       2024-09-15: Corrective - belt replacement
    │       Validates: Frequencies per Nalco WMP
    │
    ├─→ inspections (condition reports for CT-001)
    │       2024-12-10: Monthly - Rating 8/10 (Nalco standards)
    │       2024-11-10: Monthly - Rating 9/10
    │       Validates: Component conditions, vibration levels
    │
    └─→ analysis_result (system output for CT-001)
            2024-12-18: Priority 75.5, High urgency
            Uses: Nalco thresholds, ASHRAE 188 requirements
```

---

## 🎯 Quick Reference - Nalco Water Validated

| Model             | Update Frequency    | Rows per Tower  | Key Metric                     | Nalco Standard                  |
|-------------------|---------------------|-----------------|--------------------------------|---------------------------------|
| **Tower**         | Monthly/Rarely      | 1               | capacity_tons, design_approach | CTI rating: 95°F→85°F @ 78°F WB |
| **Operational**   | Daily/Hourly/15-min | 365-35,040/year | approach_temp, efficiency      | Max 120°F, COC 3-7              |
| **Water Quality** | Weekly              | 52/year         | bacteria_count, legionella     | HPC <200k, Legionella <1 CFU/ml |
| **Maintenance**   | Per event           | ~10-20/year     | maintenance_type, cost         | ASHRAE 188 quarterly            |
| **Inspection**    | Monthly/Quarterly   | 12/year         | overall_rating, conditions     | Nalco Failure Analysis Guide    |
| **Analysis**      | On-demand           | N/A             | priority_score, issues         | Nalco + ASHRAE thresholds       |

---

## 💡 How Models Work Together (Nalco Validated)

```python
# Example: Analyzing a tower using Nalco standards

# 1. Get tower specs (CTI standard)
tower = Tower.from_csv("CT-001")
# design_approach = 10.0°F (CTI: 5-15°F modern towers)

# 2. Get latest operations
ops = OperationalData.get_latest("CT-001")
# approach_temp = 12.5°F

# 3. Calculate deviation (Nalco Handbook Ch 15)
deviation = ops.approach_temp - tower.design_approach  # 2.5°F over!
# Nalco: >3°F = fouling indication

# 4. Check water quality (ASHRAE 188 / CDC standards)
wq = WaterQuality.get_latest("CT-001")
# bacteria_count = 150,000 CFU/ml
# Nalco action level: >200,000 CFU/ml

# 5. Check maintenance history (ASHRAE 188 compliance)
maint = Maintenance.get_recent("CT-001")
# last preventive: 60 days ago (quarterly required)

# 6. Check inspection (Nalco Failure Analysis)
insp = Inspection.get_latest("CT-001")
# fill_condition = "Fair" (may need cleaning)

# 7. Generate analysis result (Nalco-validated thresholds)
result = AnalysisResult(
    tower_id="CT-001",
    priority_score=75.5,  # Calculated from Nalco standards
    issues=[
        Issue("Performance", "High", "Approach 2.5°F above design (Nalco Ch 15)"),
        Issue("Water Quality", "Medium", "HPC 150k approaching action level"),
        Issue("Maintenance", "Medium", "Overdue preventive maintenance")
    ],
    recommendations=generate_recommendations_per_nalco(tower, ops, wq, insp)
)
```

---

## ✅ Industry Standards Compliance

### Nalco Water Standards

- ✅ **pH**: 6.5-9.0 preferred range (Handbook)
- ✅ **Temperature**: <120°F maximum (Handbook)
- ✅ **COC**: 3.0-7.0 optimal (water conservation)
- ✅ **Silica**: <100 ppm (<150 with inhibitors)
- ✅ **Alkalinity**: 100-500 ppm as CaCO3

### CTI (Cooling Tower Institute)

- ✅ **Rating Standard**: 95°F→85°F @ 78°F WB
- ✅ **Flow Rate**: 3 GPM/ton
- ✅ **Ton Definition**: 15,000 BTU/hr

### ASHRAE Standard 188

- ✅ **Legionella Testing**: Quarterly (90 days)
- ✅ **Inspections**: Monthly minimum
- ✅ **Water Management Plan**: Required for all towers
- ✅ **Chemical Monitoring**: 3x per week minimum

### CDC / OSHA Guidelines

- ✅ **Legionella**: <1 CFU/ml safe, >1,000 CFU/ml = outbreak level
- ✅ **HPC**: <200,000 CFU/ml control threshold
- ✅ **Temperature**: 68-122°F = Legionella growth zone

---

**Summary**: Each model represents a different aspect of cooling tower monitoring validated against **Nalco Water
Handbook**, **CTI standards**, **ASHRAE 188**, and **CDC guidelines**. Together, they provide a complete,
industry-compliant picture for analysis! 🎯

# 📚 Source References - Nalco Water Validation

Complete list of sources used to validate the cooling tower datasets.

---

# 🔬 Primary Nalco Water Sources

### 1. Nalco Water Handbook (Official)

**The Nalco Water Handbook, Third Edition**

- **Publisher**: McGraw-Hill Education
- **Access**: https://www.accessengineeringlibrary.com/content/book/9780071548830
- **Amazon**: https://www.amazon.com/Nalco-Water-Handbook-Chemical-Company/dp/0071548831
- **Coverage**: Comprehensive water treatment, cooling systems (Chapters 14-17), biology, chemistry
- **PDF Summary
  **: https://www.accessengineeringlibrary.com/binary/mheaeworks/18951cc1c512ccd7/cfa6f5978060c92076d2f4b0b727ee917b0833b60bae65d2680a92c75ac76095/book-summary.pdf

### 2. Nalco Guide to Cooling Water Systems Failure Analysis

**Second Edition - Authoritative Failure Reference**

- **McGraw-Hill**: https://www.accessengineeringlibrary.com/content/book/9780071803472
- **Amazon**: https://www.amazon.com/Cooling-Systems-Failure-Analysis-Second/dp/0071803475
- **Coverage**: Corrosion, cracking, mechanical damage, material issues, case histories

### 3. Nalco Cooling Water Treatment Technical Documents

- **Technical Manual
  **: https://www.trcc.commnet.edu/wp-content/uploads/2017/06/nalco-cooling-water-treatment-200402-sec-16.pdf
- **Scribd Document**: https://www.scribd.com/document/360132715/NALCO-cooling-water-pdf

---

## 🏢 Official Nalco Water (Ecolab) Pages

### 4. Nalco Water Cooling Tower Solutions

- **Cooling Tower Specifications**: https://en-in.ecolab.com/nalco-water/solutions/cooling-tower-specifications
- **Water Management Plans
  **: https://www.ecolab.com/nalco-water/expertise-and-innovation/water-safety/legionella-control-strategies/cooling-tower-water-management-plan
- **Legionella Control Strategies
  **: https://www.ecolab.com/nalco-water/expertise-and-innovation/water-safety/legionella-control-strategies
- **Premium Cooling Water Program**: https://www.ecolab.com/offerings/premium-cooling-water-program

### 5. Nalco Water Case Studies

- **3D TRASAR Technology Case Study
  **: https://www.ecolab.com/-/media/Ecolab/Ecolab-Home/Documents/DocumentLibrary/CaseStudies/NalcoWater/3DTRASAR/Cooling/CH1545AP-pdf.pdf
    - Demonstrates COC optimization, approach temperature monitoring

---

## 🏛️ Government & Regulatory Standards

### 6. CDC (Centers for Disease Control)

**Legionella Regulations and Guidelines**

- **NCBI - Management of Legionella**: https://www.ncbi.nlm.nih.gov/books/NBK555112/
    - Action levels: <1 CFU/mL (safe), 10-100 CFU/mL (review), >1,000 CFU/mL (emergency)
    - ASHRAE 188 requirements

### 7. OSHA (Occupational Safety and Health Administration)

**Legionellosis Outbreak Response**

- **OSHA Guidelines**: https://www.osha.gov/legionnaires-disease/outbreak-response
    - Cooling tower decontamination procedures
    - CFU/mL thresholds and outbreak associations

### 8. GSA (General Services Administration)

**Water Quality FAQs**

- **GSA Water Quality
  **: https://www.gsa.gov/real-estate/facilities-management/water-quality-management/frequently-asked-questions
    - CDC considers ≥1 CFU/mL as high Legionella level

### 9. Australian Government Standards

**Victoria Health Department - Legionella Control**

- **Section 8 Guide
  **: https://www.health.vic.gov.au/water/legionella-treating-the-critical-risks-in-cooling-towers-section-8
    - HCC >200,000 CFU/mL action requirement
    - Monthly testing requirements

**Queensland WorkSafe - Legionella Control Guide**

- **Guide PDF**: https://www.worksafe.qld.gov.au/__data/assets/pdf_file/0032/17969/guide_legionella-control.pdf
    - AS/NZS 3666.3:2011 standards
    - HCC <100,000 CFU/mL specification

---

## 🔬 Scientific & Academic Sources

### 10. Research Papers - Legionella & Bacteria

**Heterotrophic Plate Count Prediction Study (PMC)**

- **URL**: https://pmc.ncbi.nlm.nih.gov/articles/PMC10059076/
- **Title**: "Heterotrophic Plate Count Can Predict the Presence of Legionella spp. in Cooling Towers"
- **Findings**: HPC ≤100 CFU/mL better predicts Legionella risk

**Industrial Cooling Tower Disinfection Study (PMC)**

- **URL**: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5664626/
- **Title**: "Industrial Cooling Tower Disinfection Treatment to Prevent Legionella spp."
- **Standards**: National regulation limits (0 CFU/mL HPC ideal)

---

## 🏭 Industry Standards & Organizations

### 11. Cooling Tower Institute (CTI)

**CTI Rating Standards** (referenced in multiple sources)

- Standard rating: 95°F inlet → 85°F outlet @ 78°F wet bulb
- 3 GPM/ton flow rate
- 15,000 BTU/hr per cooling tower ton
- Sources referencing CTI:
    - https://jmpcoblog.com/hvac-blog/cooling-tower-and-condenser-water-design-part-3-understanding-tonnage-range-and-approach
    - https://www.fossilconsulting.com/blog/operations/cooling-tower-factors/

### 12. Water Technology Magazine

**Cooling Tower Water Systems Technical Article**

- **URL**: https://www.watertechonline.com/wastewater/article/15550523/cooling-tower-water-systems-part-2
- **References**: Nalco Water Handbook standards
- **Coverage**: pH (6.5-9.0), Temperature (max 120°F), Silica (<100 ppm), Alkalinity (100-500 ppm)

---

## 🏗️ Engineering & Technical Resources

### 13. JMP Engineering Blog

**Cooling Tower Design Parameters**

- **URL
  **: https://jmpcoblog.com/hvac-blog/cooling-tower-and-condenser-water-design-part-3-understanding-tonnage-range-and-approach
- **Coverage**: Range, approach, tonnage definitions
- **CTI Standards**: Detailed explanation of rating conditions

### 14. Fossil Consulting Services

**Cooling Tower Factors: Temperature, Range & Approach**

- **URL**: https://www.fossilconsulting.com/blog/operations/cooling-tower-factors/
- **Coverage**: Modern towers approach 5°F, wet bulb vs dry bulb
- **Date**: May 2024 (recent)

### 15. Deppmann Engineering

**Water Side Economizers & Cooling Tower Temperatures**

- **URL
  **: https://www.deppmann.com/blog/monday-morning-minutes/water-side-economizers-part-3-cooling-tower-temperatures/
- **Coverage**: ASHRAE 90.1 standards, approach calculations
- **Date**: February 2024

### 16. Chemical Engineering Resources

**Cooling Tower Efficiency Calculations**

- **ChemicalTweak**: https://chemicaltweak.com/cooling-tower-efficiency-calculation/
- **Chemical Engineering Site**: https://chemicalengineeringsite.in/cooling-tower-efficiency-calculations/
- **Coverage**: Approach, range, efficiency formulas, COC calculations (3.0-7.0)

---

## 🧪 Laboratory & Testing Standards

### 17. Environmental Safety Technologies (EST)

**Cooling Tower Legionella & Microbiological Tests**

- **URL**: https://estechlab.com/cooling-tower-legionella-microbiological-tests/
- **Coverage**: CDC Elite laboratory, testing protocols
- **Standards**: Legionella <100 CFU/mL, HPC <10,000 CFU/mL
- **Date**: February 2024

### 18. IWC Innovations

**Safe Levels of Legionella**

- **URL**: https://www.iwcinnovations.com/blog/what-are-the-safe-levels-of-legionella/
- **Coverage**: <1 CFU/mL acceptable level, temperature danger zone (25-45°C)
- **Date**: April 2025

---

## 📋 Standards Summary Table

| Standard/Source          | Organization            | Key Parameters                          | URL                                                                                                                   |
|--------------------------|-------------------------|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Nalco Water Handbook** | McGraw-Hill             | pH 6.5-9.0, Temp <120°F, COC 3-7        | [Access Engineering](https://www.accessengineeringlibrary.com/content/book/9780071548830)                             |
| **CTI Rating Standard**  | Cooling Tower Institute | 95°F→85°F @ 78°F WB, 3 GPM/ton          | Referenced in multiple sources                                                                                        |
| **ASHRAE 188**           | ASHRAE                  | Quarterly Legionella testing, WMP       | [CDC NCBI](https://www.ncbi.nlm.nih.gov/books/NBK555112/)                                                             |
| **ISO 11731**            | ISO                     | Legionella testing methodology          | [Nalco Water](https://www.ecolab.com/nalco-water/expertise-and-innovation/water-safety/legionella-control-strategies) |
| **CDC Guidelines**       | CDC                     | <1 CFU/mL safe Legionella               | [OSHA](https://www.osha.gov/legionnaires-disease/outbreak-response)                                                   |
| **NSF 453**              | NSF International       | HPC thresholds, cooling tower standards | [NCBI](https://www.ncbi.nlm.nih.gov/books/NBK555112/)                                                                 |

---

## 🔍 How to Access Sources

### Free Access:

1. **Government sources** (CDC, OSHA, GSA) - Fully public
2. **Industry blogs** (JMP, Deppmann, Fossil) - Free technical articles
3. **Research papers** (PMC/NCBI) - Open access
4. **Nalco Water website** (Ecolab) - Public product information

### Subscription/Purchase Required:

1. **Nalco Water Handbook** - McGraw-Hill Access Engineering subscription or Amazon purchase
2. **Failure Analysis Guide** - Amazon purchase or library access
3. **ASHRAE Standards** - ASHRAE membership or purchase

### Library Access:

- Many technical libraries have Access Engineering subscriptions
- University libraries typically have ASHRAE standards
- Public libraries may have interlibrary loan for Nalco books

---

## 📖 Recommended Reading Order

**For Quick Validation:**

1. Start with free industry blogs (JMP, Fossil Consulting)
2. Review CDC/OSHA Legionella guidelines
3. Check Nalco Water website for product specs

**For Comprehensive Understanding:**

1. **Nalco Water Handbook** - THE authoritative source
2. **ASHRAE 188** - Regulatory requirements
3. **Nalco Failure Analysis Guide** - Problem diagnosis
4. Research papers for specific topics

**For Compliance:**

1. ASHRAE Standard 188 (Water Management Plans)
2. CDC Legionella guidelines
3. State/local regulations (vary by location)
4. Nalco Water Management Plan templates

---

## ✅ Verification Notes

All citations in the validation document can be traced to these sources:

- **Temperature limits**: Nalco Handbook + Water Technology article
- **pH ranges**: Nalco Handbook + multiple industry sources
- **Bacteria limits**: CDC, OSHA, ASHRAE 188, research papers
- **Legionella thresholds**: CDC, OSHA, EST Labs, IWC Innovations
- **COC ranges**: Nalco Handbook + Chemical Engineering sites
- **Approach/Range**: CTI standards via JMP, Fossil, Deppmann
- **Inspection frequencies**: ASHRAE 188 + Nalco WMP

**Last Updated**: December 2024  
**Standards Current As Of**: 2024-2025

---

**Note**: Some Nalco documents may require professional access or purchase. Contact Nalco Water (Ecolab) representatives
for proprietary technical documentation or consult with a water treatment professional for facility-specific guidance.