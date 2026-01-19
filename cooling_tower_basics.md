# Cooling Tower Basics

*(Water Flow, Fans, Wet-Bulb, Bleed & Makeup Explained)*
![Image](https://www.saracoolingtower.com/update/blog/how-cooling-towers-work/how-does-a-thermal-power-plant-work.jpg?utm_source=chatgpt.com)

---

## 1. What a Cooling Tower Does (Big Picture)
A cooling tower removes **heat from water** using **evaporation**.
**Simple loop:**

1. Hot water comes from chillers or process equipment
2. Water is cooled inside the tower
3. Cool water is sent back to absorb more heat
4. The cycle repeats continuously

Cooling towers are common in:

* Data centers
* Hospitals
* Large commercial buildings
* Industrial plants

---

## 2. Water Flow Inside a Cooling Tower

### Step-by-step

1. **Hot water inlet**

   * Water enters the tower at the top
   * This water contains unwanted heat

2. **Distribution deck / spray nozzles**

   * Water is spread evenly
   * Maximizes contact with air

3. **Fill (heat-exchange media)**

   * Plastic sheets or structured surfaces
   * Slows water and increases surface area

4. **Cold water basin**

   * Cooled water collects at the bottom
   * Pumps send it back to chillers/process

---

## 3. Role of the Fan (Airflow)

### What the fan does

* Pulls air **upward** through the tower
* Removes warm, moist air
* Brings in fresh ambient air

### Why airflow matters

* No airflow → no evaporation
* Poor airflow → rising cold-water temperature
* Fan issues show up as:

  * High fan speed
  * Poor cooling performance
  * Increased vibration

---

## 4. Wet-Bulb Temperature (Critical Concept)

### Simple explanation

* **Wet-bulb temperature** is the *lowest temperature air can reach through evaporation*
* It depends on:

  * Air temperature
  * Humidity

### Golden rule

> **A cooling tower can NEVER cool water below the wet-bulb temperature**

### Why this matters

* Two identical towers behave differently in dry vs humid climates
* Performance must always be evaluated relative to wet-bulb

---

## 5. Approach and Range (How Performance Is Measured)

![Approach and range](images/approach_and_range.png)

### Definitions

| Term         | Formula                | Meaning          |
| ------------ | ---------------------- | ---------------- |
| **Range**    | Hot water − Cold water | Heat removed     |
| **Approach** | Cold water − Wet-bulb  | Tower efficiency |

### Interpretation

* **Low approach (5–7°F)** → Healthy tower
* **Rising approach** → Fouling, scaling, airflow issues
* **High approach** → Immediate attention

---

## 6. Makeup Water Tank (Why Fresh Water Is Needed)

### What is makeup water?

Makeup water is **fresh water added** to the cooling tower system.

### Why it’s required

Water is lost through:

* **Evaporation** (intentional cooling)
* **Drift** (small droplets carried out)
* **Bleed / blowdown** (intentional discharge)

Without makeup water:

* Basin level drops
* Pumps can run dry
* System fails

### Typical makeup sources

* City water
* Treated well water
* Reclaimed water (with controls)

---

## 7. Bleed / Blowdown (Why Water Is Discharged)


### What is bleed (blowdown)?

Bleed is **intentional draining of water** from the tower basin.

### Why it is necessary

As water evaporates:

* Minerals stay behind
* Water becomes more concentrated
* High mineral content causes:

  * Scale
  * Corrosion
  * Reduced heat transfer

Bleed removes concentrated water and keeps chemistry safe.

---

## 8. Makeup + Bleed Working Together

### Balance concept

* **Evaporation** → concentrates minerals
* **Bleed** → removes concentrated water
* **Makeup** → replaces with fresh water

This balance is measured by:

* **Conductivity**
* **Cycles of concentration**

### What happens when balance is wrong

| Problem          | Result                       |
| ---------------- | ---------------------------- |
| Too little bleed | Scaling, fouling             |
| Too much bleed   | Water waste, high cost       |
| Makeup failure   | Low basin level, pump damage |

---

## 9. How This Shows Up in Your Data

| Physical system    | Data signal           |
| ------------------ | --------------------- |
| Evaporation        | Rising conductivity   |
| Makeup failure     | Falling basin level   |
| Bleed stuck closed | Conductivity spikes   |
| Bleed stuck open   | Excessive makeup flow |
| Scaling            | Rising approach       |

---

## 10. Key Takeaways

* Cooling towers cool water using **evaporation**
* **Wet-bulb temperature sets the cooling limit**
* **Approach is the most important performance metric**
* **Makeup water adds fresh water**
* **Bleed removes concentrated water**
* Chemistry control protects equipment and health
