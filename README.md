# CUAFP instances and results

Data accompanying the paper *"Strategic Coopetition in Hyperconnected Urban Parcel Logistics Networks: A Partition Function Form Game under Demand Externalities"* (manuscript under review).

The repository contains the data of all seven instances and, for every experiment reported in the paper, the coalition worths $v(S,\Pi)$ and the game outcomes. No code is included: the models are fully specified in the paper (demand allocation, Section 4.1; service network design (SND) model, Section 4.3; recursive core and recursive nucleolus, Section 3.3), and all parameter values are listed below and in Appendix A of the paper. No confidential operator data are included.

## Instances

| Code | Hub network | Hubs | LSPs | Hub assignment | OD demand file |
|---|---|---|---|---|---|
| G1C | Topology 1 | 20 | 3 | `Network_Topology_1_Complementary.csv` | `parisCityDemand.csv` |
| G1A | Topology 1 | 20 | 3 | `Network_Topology_1_Asymmetric.csv` | `parisCityDemand.csv` |
| G2C | Topology 2 | 20 | 3 | `Network_Topology_2_Complementary.csv` | `parisCityDemand.csv` |
| G2X | Topology 2 | 20 | 3 | `Network_Topology_2_Competitive.csv` | `parisCityDemand.csv` |
| G3X | Topology 3 | 20 | 3 | `Network_Topology_3_Competitive.csv` | `parisCityDemand.csv` |
| G4C | Topology 4 | 30 | 4 | `Network_Topology_4_Complementary.csv` | `parisCityDemand_Large.csv` |
| G4X | Topology 4 | 30 | 4 | `Network_Topology_4_Competitive.csv` | `parisCityDemand_Large.csv` |

C = complementary (LSPs serve distinct territories), A = asymmetric, X = competitive (coverage areas overlap). Every instance is used under two market settings:

- **Fixed**: only the OD pairs that at least one LSP can reach on its own within the access-time limit are served.
- **Expanded**: OD pairs that an alliance brings within reach by pooling hubs are also served.

## Folder structure

| Folder | Content |
|---|---|
| `1_instances/` | Hubs, hub assignments, demand zones, OD demand |
| `2_demand_allocation/<instance>/Abar<15,20,25>_<Fixed,Expanded>/` | Output of the accessibility-driven demand allocation (Section 4.1), one file per partition, for each access-time limit $\bar{A}$ used in the paper |
| `3_results/` | Coalition worths and game outcomes of every experiment |

## 1_instances

| File | Columns |
|---|---|
| `Network_Topology_<t>.csv` | `HubID`, `Lat`, `Lon`: the hubs of topology $t$ |
| `Network_Topology_<t>_<assignment>.csv` | `HubID`, `Lat`, `Lon`, `Actor`: the LSP operating each hub |
| `ZoneLocation.csv` | zone index, `lat`, `lon`, `Zone`: demand zones (customer locations) |
| `parisCityDemand.csv`, `parisCityDemand_Large.csv` | `commodity_id`, `origin`, `dest` (zone indices), `Volume` (parcels per operating window), `stratum` (distance stratum used for sampling); 500 OD pairs (about 20,000 parcels) and 748 OD pairs (about 30,000 parcels) |

## 2_demand_allocation

Partitions are written as lists of coalitions, e.g. `[(1,3),(2,)]` = LSPs 1 and 3 allied, LSP 2 alone.

- `Demand_<instance>_<partition>.csv`: `Coalition`, `Origin_Hub`, `Destination_Hub`, `Service_Level` (lead time $\tau_k$, hours), `Volume` (parcels), `Origin_ODs` (zone-level OD pairs aggregated into this hub pair). The volume a coalition captures is the sum of `Volume` over its rows.
- `OutsideOption_<instance>_<partition>.csv`: OD pairs not captured by any coalition in that partition. The file is omitted when every OD pair is captured. Under the **Fixed** setting there is a single such file, for the partition into singletons: the served OD pairs are those that at least one LSP reaches on its own, and every other partition reallocates exactly these OD pairs among its coalitions, so no further OD pair is left out.

## 3_results

| File | Content |
|---|---|
| `coalition_worths.csv` | One row per (experiment, scenario, instance, market setting, partition, coalition): captured volume, revenue (€3 per parcel), dispatch cost, overflow cost (main model) or congestion cost (second model), total cost, and worth $v(S,\Pi)$ = revenue − total cost (€) |
| `game_outcomes.csv` | One row per (experiment, scenario, instance, market setting): partition selected by the R-nucleolus, payoff of each LSP (€), whether the R-core is non-empty, least-core value $\varepsilon$ (€ and % of the grand coalition's worth; 0 when the R-core is non-empty) |
| `comparison_ladder_baseline.csv` | Comparison ladder of Section 5.3 (baseline experiment): rung 0 (grand coalition; Shapley value and pre-nucleolus of the characteristic function in which outsiders act alone, MPW value; `blocked` = some coalition can obtain more by breaking away), rung 1 (R-nucleolus under the outsiders-alone, optimistic and pessimistic characteristic functions), rung 2 (R-nucleolus of the partition function) |
| `dispatch_factor_outcomes.csv` | Appendix F: outcomes when the dispatch cost is charged at $r f$ instead of $2f$ dispatches per arc, for $r \in$ {1, 1.25, 1.5, 1.75, 2}, with the network designs of the baseline. The corresponding worths follow from the baseline rows of `coalition_worths.csv` (see below) |

The worths of Appendix F are obtained from the baseline rows as

$$\mathrm{worth}_r = \mathrm{revenue} - \frac{r}{2}\,\mathrm{dispatch\ cost} - \mathrm{overflow\ cost}.$$

Scenario columns: `access_time_min` ($\bar{A}$, minutes), `overflow_cost_eur` ($c^{\mathrm{over}}$, € per parcel), `hub_capacity` ($\mathrm{Cap}_h$, parcels per window), `rho` ($\rho = \mathrm{Cap}_h / V_{\mathrm{peak}}$, capacity sweep only).

| Experiment (`experiment`) | Section of the paper | Scenarios |
|---|---|---|
| `baseline` | 5.2, 5.3; Appendices B, C | $\bar{A}$ = 20 min, $c^{\mathrm{over}}$ = €1, $\mathrm{Cap}_h$ = 5,000 |
| `capacity_sweep` | 5.4; Appendix D | $\rho \in$ {2, 1, 0.5, 0.25} |
| `overflow_cost` | Appendix G | $c^{\mathrm{over}} \in$ {€0.5, €2, €50} |
| `access_time` | Appendix G | $\bar{A} \in$ {15, 25} min |
| `second_model` | Appendix E | second model (uniform arrivals, hub congestion), baseline instances and settings |

## Parameter values (main model)

| Parameter | Value |
|---|---|
| Operating window $H$ | 8 h |
| Lead-time requirement $\tau_k$ | 4 h, all segments |
| Access-time limit $\bar{A}$ | 20 min (baseline) |
| Customer access speed | 10 km/h (great-circle distance) |
| Arc travel speed | 25 km/h (great-circle distance); arcs of at most 30 min |
| Hub processing time $t_{\mathrm{hub}}$ | 20 min per arc |
| Vehicle capacity $\kappa$ | 35 parcels |
| Cost per dispatch $c_a$ | €20/h × (travel time of $a$ + $t_{\mathrm{hub}}$) |
| Frequency levels $\Theta_a$ | {0, 2, 3, 4, 6, 8, 16, 32, 48, 96, 192, 240} per window |
| Revenue $r_k$ | €3 per parcel |
| Hub capacity $\mathrm{Cap}_h$ | 5,000 parcels per window (baseline) |
| Overflow cost $c^{\mathrm{over}}$ | €1 per parcel (baseline) |
| Minimum outbound-dwell budget (path pre-screening) | 5 min per arc |
| Path limits per segment | 5 / 8 / 12 / 15 for coalitions of 1 / 2 / 3 / 4 LSPs (paths of sub-coalitions retained); at most five hubs per path |

The second model (Appendix E) keeps these values and adds hub congestion with $c_{\mathrm{hub}}$ = €28 per vehicle-hour, $C_s^2 = 1$ and a service rate set so that a hub reaches utilization one at 5,000 parcels per window.

## How to reproduce

1. **Game outcomes from the worths (seconds):** for a given experiment, scenario, instance and market setting, read the rows of `coalition_worths.csv` as the partition function $v(S,\Pi)$ and apply the recursive core and recursive nucleolus as defined in Section 3.3 of the paper. The result should match `game_outcomes.csv`.
2. **Worths from the instance data:** allocate demand as in Section 4.1 (or use `2_demand_allocation/`), solve the SND model of Section 4.3 for every coalition $S$ of every partition $\Pi$ with the parameters above, and compute $v(S,\Pi) = 3 \times \text{captured volume} - \text{cost}$. Worths were obtained with a relative optimality gap tolerance of 2% (Section 5.1), so recomputed worths may differ within that tolerance.

## License and citation

The data are released under the Creative Commons Attribution 4.0 International License (CC BY 4.0; see `LICENSE`). If you use them, please cite the paper above.