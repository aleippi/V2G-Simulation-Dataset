# Developing an Equitable V2G Pricing Strategy: Simulation Dataset

This repository contains a comprehensive simulation dataset designed to evaluate, test, and validate Vehicle-to-Grid (V2G) economic outcomes and pricing strategies. The core purpose of this dataset is to investigate how much profit an electric vehicle (EV) can generate for a company depending on its unique operational and technical characteristics. By analyzing this multi-parameter data, researchers can identify which variables exert the greatest influence on V2G profitability and use these insights to design equitable incentive mechanisms for EV owners.

The dataset acts as a benchmark to verify V2G optimization models and pricing strategies within corporate charging infrastructures.

---

## Methodology & Simulation Framework

The dataset was generated using a deterministic **Mixed-Integer Linear Programming (MILP)** mathematical model. 
* **Core Framework:** The optimization model is based on an adapted version of the **Demand Response Analysis Framework (DRAF)** ([GitHub Repository](https://github.com/DrafProject/draf)), an open-source Python tool designed for environmental and economic analysis of Demand Response.
* **Optimization Solver:** Every simulation scenario was solved to global optimality (MIP gap = 0) using the **Gurobi 13.0 Solver** via its Python interface.
* **Pricing Strategy & Empirical Foundation:** The simulations utilize **real-world corporate facility data** (including historical load profiles and peak pricing) combined with a Real-Time Pricing (RTP) strategy. The electricity market component is based on the official historical hourly prices of the **Germany − GER/LU (bidding zone)** Day-Ahead spot market.
* **Scale:** A total of **over 61,000 discrete simulations** were conducted, spanning a five-year horizon (**2021–2025**).

---

## Dataset Overview & Parameter Space

Each simulation run models the daily commuter cycle of a single EV over an entire calendar year. The simulation replicates a realistic corporate environment where the EV arrives at the company facility every day, remains plugged in during its designated availability window (enabling V2G charging, discharging, and smart grid optimization), and departs for the return home trip.

* **Battery Capacity:** 10 kWh to 180 kWh (evaluated in 10 kWh increments).
* **Arrival State of Charge (SOC):** Categorized into three operational windows:
  * `low`: 20% – 40% (represented numerically as `0.3`)
  * `medium`: 40% – 60% (represented numerically as `0.5`)
  * `high`: 60% – 80% (represented numerically as `0.7`)
* **Charging/Discharging Rate:** Primary analysis focused on 7.2 kW, 11 kW, 22 kW, and 30 kW (additional charging/discharging ratings were included for exploratory testing, e.g., 100 kW, 150 kW, 200 kW, and 250 kW).
* **Daily Availability Windows:** 2h, 4h, 6h, 8h, and 10h per day.
* **Corporate Shift Slots (Availability Windows):**
  * `slot1` (06:00 – 14:00) | Simulation Index: 24 to 56
  * `slot2` (14:00 – 22:00) | Simulation Index: 56 to 88
  * `slot3` (22:00 – 06:00) | Simulation Index: 88 to 96 & 0 to 24 (Cross-day)
  * `slot4` (08:00 – 18:00) | Simulation Index: 32 to 72 (Standard office shift)
* **Target (Free) Charging Increments:** 0%, 5%, 10%, 15%, 20%, 25%, and 30%.
* **Economic Variables:** Multi-year Day-Ahead market spot pricing (GER/LU bidding zone), real corporate peak pricing, and empirical facility load curves (2021–2025).

---

## Data Dictionary (Column Descriptions)

The columns within the dataset (`.xlsx` / `.csv`) are formatted with word-capitalization and underscores (`_`) instead of whitespace characters for improved computational compatibility:

| Column Name | Type | Description | Example Value |
| :--- | :--- | :--- | :--- |
| **ID** | Integer | Unique vehicle/simulation run identifier. | `1` |
| **Year** | Integer | Simulation calendar year (2021–2025). | `2022` |
| **Capacity** | Float | Battery capacity of the EV in kWh. | `20.0` |
| **Charging_Discharging_Rate** | Float | Maximum charging or discharging rate of the vehicle charger in kW. | `7.2` |
| **Slot** | Integer | Shift slot index (1 to 4) mapping the vehicle's presence. | `1` |
| **Time_Period** | Float | Presence duration: Number of hours per day the EV is stationed at the corporate facility and available for V2G operations. | `6.0` |
| **SOC_Arrival** | String | Categorized state of charge scenario upon arrival (`low`, `medium`, `high`). | `medium` |
| **Pct_Charge** | Float | Target state of charge increase required per presence day (e.g., 20.0% means an EV arriving at 50% must reach 70% by departure). | `20.0` |
| **Total_Costs** | Float | Total annual electricity costs for the facility including EV operations. Formulated as: Reference_Cost - Arbitrage_Value + Battery_Degradation_Costs - Peak_Reduction_Savings | `3,228.0` |
| **Peak_Price** | Float | Annual grid operator price per kW of peak load (€/kW). | `150.0` |
| **Peak_Costs** | Float | Calculated peak load costs for the facility (€). | `10,000.0` |
| **Peak_Reduction** | Float | Achieved reduction of the facility's peak load via V2G discharge (kW). | `35.0` |
| **Battery_Degradation_Costs** | Float | Estimated financial equivalent of battery degradation per year (€). | `155.2` |
| **Power_In_Kwh** | Float | Total electrical energy charged into the EV battery over the year. | `14,000.0` |
| **Power_Out_Kwh** | Float | Total electrical energy discharged from the EV back to the grid/facility. | `13,200.0` |
| **Charging_Costs** | Float | Isolated electricity costs directly related to charging the EV (€). | `500.0` |
| **Discharge_Savings** | Float | Financial savings generated by discharging the EV during high-price market periods (€). | `1,000.0` |
| **Arbitrage_Value** | Float | Net financial gains from energy shifting (Discharge_Savings - Charging_Costs) (€). | `500.0` |
| **Reference_Cost** | Float | Baseline energy costs for the corporate facility without any V2G integration (€). | `200,000.0` |
| **Net_Compensation** | Float | Total financial difference between baseline and V2G operational costs (Reference_Cost - Total_Costs) (€). | `314.67` |
| **Profit_Company** | Float | Net economic profit remaining for the corporate facility, calculated as: Net_Compensation + Battery_Degradation_Costs (€). | `1,294.63` |
| **SOC_Num** | Float | The numerical representation of the arrival state of charge profile (0.3 for low, 0.5 for medium, 0.7 for high). | `0.5` |

---

## License

This dataset is made available under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license. You are free to share and adapt the data, provided that appropriate credit is given to the original authors.

## How to Cite

If you utilize this dataset, please use the following citation format:

```text
[Author Name]. (2026). Developing an Equitable V2G Pricing Strategy: Simulation Dataset (Version v1.0.0). Zenodo. [https://doi.org/10.5281/zenodo.XXXXX](https://doi.org/10.5281/zenodo.XXXXX)
