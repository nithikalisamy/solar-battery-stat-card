
# ⚡ K‑Flow Card for Home Assistant

Animated energy flow dashboard card – solar, battery, grid, home – real‑time, no token needed.

Standard .<img width="1132" height="1603" alt="standard png" src="https://github.com/user-attachments/assets/ed8d07d1-044f-455d-abec-8995f4764342" />

Multi Pv(4pv and arc flow updated, dowload "k-flow-pv4.js" if solar arc and value flow not showing).<img width="1110" height="1624" alt="multi-pv png" src="https://github.com/user-attachments/assets/3ba1e191-add4-47d3-9b0f-0a52902651cb" />
Dual Battery <img width="1102" height="1620" alt="kflowdual" src="https://github.com/user-attachments/assets/40f80d43-7fd7-447c-842a-58f16d03a37b" />




---

## 🛠 Installation

1. Download the desired variant (see below) and rename it to `k-flow-card.js`.
2. Place it in your `config/www` folder.
3. Add as a **module** resource:  
   **Settings → Dashboards → Resources → + Add Resource**  
   URL: `/local/k-flow-card.js`  
   Type: **JavaScript Module**
4. Reload the dashboard.

**HACS custom repository** – add `https://github.com/thekhan1122/k-flow-card` (no `.git`) in **Frontend** custom repositories.

---

## 📋 Basic Configuration

```yaml
type: custom:k-flow-card
pv1_power: sensor.goodwe_pv1_power
pv2_power: sensor.goodwe_pv2_power
pv_total_power: sensor.goodwe_pv_power
grid_active_power: sensor.goodwe_active_power
grid_import_energy: sensor.goodwe_today_energy_import
consump: sensor.goodwe_house_consumption
today_pv: sensor.goodwe_today_s_pv_generation
today_batt_chg: sensor.goodwe_today_battery_charge
today_load: sensor.goodwe_today_load
battery_soc: sensor.jk_soc
battery_power: sensor.jk_power
battery_current: sensor.jk_current
battery_voltage: sensor.jk_voltage
battery_temp1: sensor.jk_temp1
battery_temp2: sensor.jk_temp2
battery_mos: sensor.jk_mos
battery_min_cell: sensor.jk_cellmin
battery_max_cell: sensor.jk_cellmax
battery_rem_cap: sensor.jk_remain
goodwe_battery_soc: sensor.goodwe_battery_state_of_charge
goodwe_battery_curr: sensor.goodwe_battery_current
inv_temp: sensor.goodwe_inverter_temperature_module
batt_dis: sensor.goodwe_today_battery_discharge
```
---

🧩 Card Variants

The repository contains three versions. Choose the one that fits your setup, rename it to k-flow-card.js, and replace the file.

Variant	File	Description

Standard	k-flow-card.js
Single battery, PV1/PV2. Uses pv_total_power or sums PV1+PV2 if missing.

Dual Battery	k-flow-card-dual.js	
Same as standard, but with two battery compartments in one shell. Second battery is optional – no entities = hides.

Multi PV (4 strings)	k-flow-card-pv4.js	
Single battery, supports PV1–PV4. Auto‑calculates total PV when pv_total_power is missing/zero.

All variants use the same card type custom:k-flow-card – no YAML changes needed when switching files.

🔋 Dual Battery (optional)

To use the dual‑battery variant (k-flow-card-dual.js), add these optional entities to your card config:

---

```yaml
battery2_soc: sensor.battery2_soc
battery2_power: sensor.battery2_power
battery2_current: sensor.battery2_current
battery2_voltage: sensor.battery2_voltage
battery2_temp1: sensor.battery2_temp1
battery2_temp2: sensor.battery2_temp2
battery2_mos: sensor.battery2_mos
battery2_min_cell: sensor.battery2_min_cell
battery2_max_cell: sensor.battery2_max_cell
battery2_rem_cap: sensor.battery2_remain
battery2_batt_dis: sensor.battery2_discharge
battery2_batt_chg: sensor.battery2_charge   # optional
```

All second‑battery fields are optional. If left empty, the second compartment is hidden and the card works like the standard version.

---
☀️ Multi PV (up to 4 strings)

If your inverter only reports separate PV strings, use k-flow-card-pv4.js. It supports PV3 and PV4 in addition to the usual PV1/PV2.

Add the extra entities:
---

```yaml
pv3_power: sensor.goodwe_pv3_power
pv4_power: sensor.goodwe_pv4_power
```
---

How total PV works:

If pv_total_power is present and returns a value greater than 0, it is used directly.

If missing, unavailable, or zero, the card automatically sums all available strings: pv1 + pv2 + pv3 + pv4.

The arc label, PV wave, and block bar always reflect the correct total.

🎨 Colour Logic & Animations

Power bar (Pwr)

< 50 W → grey

Charging → blue

Discharging → yellow‑orange‑red gradient depending on power level

Flow line speeds

Idle (< 10 W) → hidden

Active → duration = max(0.5, 3.0 – (min(|watts|,8000)/8000)*2.5) seconds

Battery fill → colour‑coded by SOC: red (≤20%), orange (≤40%), green (≤75%), cyan (>75%).

Sun arc wave → speed and density depend on PV power; colour stays yellow‑gold.

Grid lines always red; Home line changes colour according to the dominant source (grid = orange, battery = orange, PV = yellow).

🖼️ Custom Icons


The card uses two PNG icons for the Grid and Home nodes.
Place them in /config/www/ with these exact names:

grid-icon.png

home-icon.png

If you don’t provide them, the icons will be blank. You can use any 110×110 px PNG (even a simple coloured square with a letter).
The icons glow dynamically when power flows to/from the corresponding node.

🐞 Troubleshooting

Symptom	Solution

“Custom element doesn’t exist”
Check resource URL is /local/k-flow-card.js and type is JavaScript Module.

All values show “--” or NaN	   The sensor IDs don’t match your system –      verify them in the YAML.

PV arc label doesn’t match PV1+PV2	  The arc uses pv_total_power. If it’s wrong, use the multi‑PV variant or sum strings with a template sensor.

Grid / Home icons missing	     Place grid-icon.png and home-icon.png in /config/www/.

Animations not playing	The browser tab must be active; some browsers pause SVG animations in background tabs.

Buttons (relay, grid, zigbee) not working	This card doesn’t handle switches – use the companion k‑sensors‑card for controls.

Dual battery not showing	Make sure you’re using k-flow-card-dual.js and that battery2_soc is set.
