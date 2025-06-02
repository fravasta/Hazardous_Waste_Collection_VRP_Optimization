# Optimization of Hazardous Waste collection in the Province of Brescia
using a capacitated, multi-vehicle and periodic Vehicle Routin Problem

Tools used: Python pyomo, OpenstreetMap, Overpass API, Excel sheets (to organize data into tables)

## Bi-objective Strategy
The geographical scope is the province of Brescia with its 14 hospital facilities. The goal is double: 
1. Minimize the total collection cost (in terms of travel time) for hospital waste over a weekly horizon, while respecting vehicle capacity, specific frequency constraints and constraints related to the nature of the waste.
2. Minimize the risks during the carriage.
The first goal will be the core of the objective function, while the second will be handled outside the VRP model, during the preprocessing of data. 

## Waste Categories
- A. Non-toxic, Non-flammable, Non-corrosive Chemical Waste EWC/CER: 18 01 07
- B. Toxic, Flammable and/or Corrosive Chemical Waste EWC/CER: 18 01 06 – collected only if the total quantity for the day exceeds 200 kg and cannot coexist with other hazardous waste types within the same vehicle.
- C. Infectious Waste EWC/CER: 18 01 03
- D. Special Waste (Including Anatomical Parts and Ashes) EWC/CER: 18 01 02 - cannot coexist with other hazardous waste types within the same vehicle. 
- E. General Healthcare Waste EWC/CER: 18 01 04


## Proposed Methodology
The methodology is divided into three main parts:

**Part 1: Data Estimation and Preprocessing**
Construction of an evidence-based synthetic dataset. The assumption is that waste is generated only from occupied hospital beds and surgical procedures performed daily. 
At the end of the estimation process we obtained
- `weekly_demands`: a dictionary mapping each day of the week to the waste demand per hospital node for each waste type
- `vehicle_data`: available vehicles and their capacities, per waste type
- `cost_matrix`: matrix representing the transportation cost (distance) between nodes

**Part 2: Geospatial Data Collection and Road Network Weighting**
- Downloaded data from **OpenStreetMap** using **Overpass API**, including:
  - Road network geometry (nodes and ways)
  - Sensitive facilities (e.g., educational facilities, green areas and public gardens)
  - Road attributes (e.g., `highway` type, `surface`, and `access`)
- Assigned an estimated **travel time** to each road segment based on road length and speed limits
- Applied a **risk-adjustment factor** to each segment based on:
  - Proximity to sensitive areas (e.g., schools, parks)
  - Road type and safety levels
- Result: a **risk-adjusted travel time matrix** used as proxy for cost and as input for routing optimization

**Part 3: VRP Model Definition and Resolution to minimize total risk-adjusted travel time**
The model iterates over the 7 days of the week and for each day of the week:
1. Extract daily demands from `weekly_demands`
2. For each waste type:
   - Solve a Vehicle Routing Problem (VRP) with capacity constraints
   - For waste type **B**:
     - If the total demand < 200 kg → collection is skipped
     - The waste is accumulated and reconsidered the next day

Model features include:
  - Vehicle capacity constraints
  - Route continuity and flow conservation
  - Start and end at depot
  - Special logic for type B: collected only when demand exceeds threshold


**Expected Outcomes**
- Realistic and risk-sensitive hospital waste collection routes
- Adaptive scheduling based on dynamic daily demand
- Weekly summary of total travel distance, collections performed, and unmet demand


