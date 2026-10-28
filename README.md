# CUAFP instances

Data accompanying the paper *"Strategic Coopetition in Hyperconnected Urban Parcel Logistics Networks: A Partition Function Form Game under Demand Externalities"*: the seven instances, their demand allocation, and the results of Section 5.2. The models and all parameter values are given in the paper (Sections 3–5 and Appendix A). No code and no confidential operator data are included.

## Instances

| Code | Hubs | LSPs | Hub assignment | OD demand |
|---|---|---|---|---|
| G1C | 20 | 3 | `Network_Topology_1_Complementary.csv` | `parisCityDemand.csv` |
| G1A | 20 | 3 | `Network_Topology_1_Asymmetric.csv` | `parisCityDemand.csv` |
| G2C | 20 | 3 | `Network_Topology_2_Complementary.csv` | `parisCityDemand.csv` |
| G2X | 20 | 3 | `Network_Topology_2_Competitive.csv` | `parisCityDemand.csv` |
| G3X | 20 | 3 | `Network_Topology_3_Competitive.csv` | `parisCityDemand.csv` |
| G4C | 30 | 4 | `Network_Topology_4_Complementary.csv` | `parisCityDemand_Large.csv` |
| G4X | 30 | 4 | `Network_Topology_4_Competitive.csv` | `parisCityDemand_Large.csv` |

C = complementary, A = asymmetric, X = competitive. Each instance is solved under a fixed and an expanded market setting (Section 5.1).

## Files

- `1_instances/`: hub coordinates and the LSP operating each hub (`HubID`, `Lat`, `Lon`, `Actor`), demand zones (`ZoneLocation.csv`), and OD demand (`origin`, `dest`, `Volume` in parcels).
- `2_demand_allocation/<instance>/Abar20_<Fixed,Expanded>/`: demand captured by each coalition in each partition (Section 4.1). Partitions are written as `[(1,3),(2,)]` (LSPs 1 and 3 allied, LSP 2 alone). `OutsideOption_*` files list unserved OD pairs; under the fixed market only the singleton partition has one, since all partitions serve the same customers.
- `3_results/coalition_worths.csv`: worth of every coalition in every partition (Section 5.2): captured volume, revenue, dispatch and overflow costs, and worth (€).
- `3_results/game_outcomes.csv`: for each instance and market setting, the R-nucleolus partition and payoffs (€), whether the R-core is non-empty, and the least-core value (€ and % of the grand coalition's worth).

## License

CC BY 4.0 (see `LICENSE`). Please cite the paper above if you use the data.
