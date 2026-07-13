# Renewable Energy Private Equity Investment: Wind Turbine EDA

## Problem Statement
A private equity firm focused on renewable energy wants to acquire U.S. wind farms, but needs to know which turbine specifications and geographic locations are linked to the strongest historical performance. This project analyzes turbine data across U.S. Census Regions to identify performance benchmarks and niche investment opportunities.

---

## Executive Summary

This project analyzes over 66,000 U.S. wind turbines from the U.S. Wind Turbine Database (USWTDB), combined with state-level wind speed data, to help a private equity firm identify the best regions and turbine specifications for acquiring wind farm assets. Since real, turbine-level Net Generation (MWh) data wasn't available, we built an estimated Net Generation proxy using each project's rated capacity combined with an estimated capacity factor scaled to each state's wind strength (ranging 20%-45%). This let us rank regions and turbine configurations by estimated output while still reflecting real differences in wind resource quality across locations.

The analysis found that the **Midwest and South Census Regions together account for about 75% of total estimated Net Generation**, making them the clear priority for acquisition targeting. The South showed the strongest average performance per project, while the Midwest had the largest overall turbine count. We also built a benchmark profile for high-performing assets (rated capacity ~2,378 kW, hub height ~86.5m, rotor diameter ~111.6m) and identified several niche, low-count turbine models — including the Nordex AW132/3300 and Vestas V136-3.7 — that significantly outperform the fleet average despite being uncommon.

Our recommendation is to prioritize acquisition sourcing in the Midwest and South, use the benchmark profile as a screening filter for candidate assets, and investigate the identified niche models further as potentially undervalued opportunities. Because our performance metric is an estimate rather than metered generation data, we recommend validating these findings against real plant-level generation data (e.g., EIA-923) before finalizing any investment decisions.

---

## File Directory

```
├── README.md                              <- This file
├── Code/
│   └── wind_turbine_eda.ipynb             <- Full analysis notebook (cleaning, EDA, findings)
├── Data/
│   ├── wind-turbines.csv                  <- Main dataset (USWTDB)
│   ├── Wind Turbine Data Dictionary.pdf    <- Official data dictionary for the main dataset
│   └── extra/                              <- Supplementary state-level datasets
│       ├── average_electricity_bills.csv
│       ├── average_electricity_rates.csv
│       └── windiest-states-in-the-us_-2025.csv
└── Presentation/
    └── EDA_Project.pdf                    <- Presentation slides
```

---

## Data & Data Dictionary

**Data Source:** The main dataset is the [U.S. Wind Turbine Database (USWTDB)](https://emp.lbl.gov/publications/us-wind-turbine-database-files), maintained by USGS and Lawrence Berkeley National Laboratory, containing structural and location data for over 70,000 U.S. wind turbines. This was supplemented with state-level wind speed/power, electricity rate, and electricity bill datasets provided as part of the course project materials.

| Feature / Column | Data Type | Source | Description |
|:---|:---|:---|:---|
| case_id | integer | Original (USWTDB) | Unique identifier for each turbine |
| t_state | string | Original (USWTDB) | State abbreviation where the turbine is located |
| t_county | string | Original (USWTDB) | County where the turbine is located |
| t_fips | string | Original (USWTDB) | State and county FIPS code |
| p_name | string | Original (USWTDB) | Name of the wind power project |
| p_year | integer | Original (USWTDB) | Year the project became operational |
| p_tnum | integer | Original (USWTDB) | Number of turbines in the project |
| p_cap | float | Original (USWTDB) | Cumulative project capacity, in megawatts (MW) |
| t_manu | string | Original (USWTDB), standardized | Turbine manufacturer name (Siemens/Gamesa/Goldwind variants merged) |
| t_model | string | Original (USWTDB) | Turbine model name |
| t_cap | integer | Original (USWTDB) | Rated capacity of a single turbine, in kilowatts (kW) |
| t_hh | float | Original (USWTDB) | Turbine hub height, in meters |
| t_rd | float | Original (USWTDB) | Turbine rotor diameter, in meters |
| t_rsa | float | Original (USWTDB) | Turbine rotor swept area, in square meters |
| t_ttlh | float | Original (USWTDB) | Total turbine height (ground to blade tip), in meters |
| retrofit | integer | Original (USWTDB) | 1 if the turbine has been retrofitted, 0 if not |
| retrofit_year | float | Original (USWTDB), cleaned | Year of retrofit; filled with 0 where no retrofit occurred |
| t_conf_atr | integer | Original (USWTDB) | Confidence level (1-3) in the turbine's attribute data |
| t_conf_loc | integer | Original (USWTDB) | Confidence level (1-3) in the turbine's location data |
| xlong | float | Original (USWTDB) | Longitude of the turbine |
| ylat | float | Original (USWTDB) | Latitude of the turbine |
| MeanWindPower328ft | integer | Engineered (merged from windiest_states) | Mean wind power potential at 328ft for the turbine's state |
| capacity_factor_est | float | Engineered | Estimated capacity factor (0.20-0.45), scaled from state wind power potential |
| net_gen_proxy | float | Engineered | Estimated Net Generation (MWh) = p_cap × 8760 × capacity_factor_est |
| census_region | string | Engineered | US Census Region (Northeast/Midwest/South/West) derived from t_state |

---

## Important Visualizations

- **Total Estimated Net Generation by Census Region** (bar chart): Shows the Midwest and South far ahead of the West and Northeast in total estimated output.
- **Turbine Capacity vs. Rotor Diameter** (scatter plot): Shows a strong, consistent positive relationship — larger rotor diameters are reliably paired with higher rated capacities.

*(See the full notebook in `Code/wind_turbine_eda.ipynb` for both charts with complete formatting and interpretation.)*

---

## Conclusions & Recommendations

- **Prioritize the Midwest and South regions** for acquisition sourcing — together they account for ~75% of total estimated output, with the South showing stronger average performance per project.
- **Use the benchmark profile as a screening filter**: target turbines near 2,378 kW rated capacity, 86.5m hub height, and 111.6m rotor diameter, the profile of the top 10% historical performers.
- **Investigate niche models further**: models like the Nordex AW132/3300 and Vestas V136-3.7 significantly outperform the fleet average despite low unit counts, and may represent undervalued opportunities — though their small sample sizes warrant further verification.
- **Deprioritize the Northeast** under current data, given its consistently weaker performance across every measure.

## Areas for Further Research

- Validate the estimated Net Generation proxy against real plant-level generation data (e.g., EIA-923) before using these findings for investment decisions.
- The `net_gen_proxy` calculation applies project-level capacity (`p_cap`) at the turbine level, which likely inflates totals for larger multi-turbine projects. Recalculating with per-turbine capacity (`t_cap`) would give a more accurate estimate.
- Investigate the niche turbine models further, since their strong performance is based on very small sample sizes (as few as 3 units).
- Integrate the collected electricity rate and bill data to explore whether strong-performing regions also show favorable electricity pricing dynamics.

## Sources

- U.S. Geological Survey (USGS) & Lawrence Berkeley National Laboratory (LBNL), *The U.S. Wind Turbine Database (USWTDB)*, v5.3. Available at: https://gtech.usgs.gov/uwwtd/
- State-level wind speed/power, electricity rate, and electricity bill datasets, provided as part of the course project materials.
- U.S. Census Bureau, *Geographic Terms and Concepts – Census Regions and Divisions*, for standardized state-to-region mapping.
