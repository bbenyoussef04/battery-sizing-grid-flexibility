# Strategic Battery Sizing to Maximise Grid Flexibility: The Doppelschwand Case Study

Course project for *Optimization in Energy Systems* at ETH Zurich, Power Systems Laboratory, Spring Semester 2026. Group: Benhallam Idriss, Ben Youssef Belkis, Cukic Tijana.

## Overview

A linear optimisation model (Python, pandapower, Gurobi) that computes the feasible flexibility operating region (FFOR) at the point of common coupling of a 400 V residential low-voltage grid in Doppelschwand (LU), a 71-bus radial network. The FFOR is the set of active and reactive power deviations the grid can accept while respecting device and grid constraints. Its area is the flexibility metric.

The device count is scaled from the 2030 baseline (3 BESS, 10 PV) to 6 BESS and 20 PV systems (185.6 kWp). The project asks how battery sizing and operational constraints can be optimised to maximise the FFOR.

## Key Results

- **Operational change first:** widening the SoC window from [0.40, 0.80] to [0.00, 1.00] gives about 30% more flexibility at no hardware cost. Beyond roughly [0.20, 0.90] the gains are small and not monotonic, so narrower limits still keep a useful safety margin for the battery.
- **Energy capacity has a ceiling:** scaling capacity beyond 4× (about 616 kWh total) adds nothing. The inverter rating becomes the bottleneck, so extra storage only pays off if the inverters are also upgraded.
- **Inverter power is the lever for reactive flexibility:** scaling inverter ratings grows the Q-axis width from 187 to 447 kvar at 4×, while the P-axis does not change at all. Beyond about 3×, SoC bounds limit the reactive range, so the practical investment ceiling is about 3×.
- **Unit placement matters:** when upgrading a single BESS, BESS 4 (bus 62) and BESS 2 (bus 32) give the largest flexibility gain per added kW. Network position matters more than inverter size.
- **Planning implication:** active and reactive flexibility need different investments. Active flexibility comes from SoC management and energy capacity; reactive flexibility comes from inverter rating. The right investment depends on which grid service the DSO needs.

## Repository Structure

- `OiESProject.ipynb`: full analysis. Model and constraints are in section 6; Scenario 1 is in section 7, Scenario 3 in section 8 and Scenario 2 in section 9.
- `1001-1_0_3_grid.xlsx`: grid model
- `BESS_allocation_LV.xlsx`: BESS allocation
- `LV_P_installed.csv`: installed PV capacity per node
- `environment.yml`: conda environment
- `FFOR_*.png`, `ffor_area_soc_bounds.png`: saved figures

## Data

- Grid topology: Swiss-PDGs dataset (Oneto et al., 2025)
- PV and BESS data: SwissDN-DERs dataset (Zapparoli et al., 2025)

`LV_generation` and `LV_std` (~1.1 GB each) are not included because of their size. Download them from [Future Deployment and Flexibility of Distributed Energy Resources in the Distribution Grids of Switzerland](https://zenodo.org/records/16272961) and place them in the project folder before running the notebook.

## Installation

```bash
conda env create -f environment.yml
conda activate swiss_grid
```

Open `OiESProject.ipynb` in Jupyter and run all cells. Gurobi needs a valid licence. Free academic licences are available for ETH students.

Key versions: Python 3.11, pandapower 3.4.0, numpy 1.26.4, pandas 2.3.3, gurobipy 13.0.1.

## Team Contributions

- **Benhallam Idriss:** Scenario 1, SoC bounds
- **Cukic Tijana:** Scenario 2, battery energy capacity
- **Ben Youssef Belkis:** Scenario 3, inverter power limit, including the per-BESS upgrade ranking. Also model definition, constraints and interpretation of results.
