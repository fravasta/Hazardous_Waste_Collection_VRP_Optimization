# Optimization of Hazardous Waste collection in the Province of Brescia
using a capacitated, multi-vehicle and periodic Vehicle Routin Problem.

Tools used: Python pyomo, OpenstreetMap, Overpass API, Excel sheets (to organize data into tables)

## Bi-objective Strategy
Our goal is double
1. Minimize the total collection cost (in terms of travel time) for hospital waste over a weekly horizon, while respecting vehicle capacity, specific frequency constraints and constraints related to the nature of the waste.
2. Minimize the risks during the carriage.
The first goal will be the core of the objective function, while the second will be handled outside the VRP model, during the preprocessing of data. 

## Waste Categories
A. Non-toxic, Non-flammable, Non-corrosive Chemical Waste EWC/CER: 18 01 07
B. Toxic, Flammable and/or Corrosive Chemical Waste EWC/CER: 18 01 06 – collected only if the total quantity for the day exceeds 200 kg and cannot coexist with other hazardous waste types within the same vehicle.
C. Infectious Waste EWC/CER: 18 01 03
D. Special Waste (Including Anatomical Parts and Ashes) EWC/CER: 18 01 02 - cannot coexist with other hazardous waste types within the same vehicle. 
E. General Healthcare Waste EWC/CER: 18 01 04


## Proposed Methodology
The methodology is divided into three main parts:

**Part 1: Data Estimation and Preprocessing**

- `weekly_demands`: a dictionary mapping each day of the week to the waste demand per hospital node for each waste type
- `vehicle_data`: available vehicles and their capacities, per waste type
- `cost_matrix`: matrix representing the transportation cost (distance) between nodes

**Part 2: Geospatial Data Collection and Road Network Weighting**

**Part 3: VRP Model Definition and Resolution**
The model iterates over the 7 days of the week and for each day of the week:
1. Extract daily demands from `weekly_demands`
2. For each waste type:
   - Solve a Vehicle Routing Problem (VRP) with capacity constraints
   - For waste type **B**:
     - If the total demand < 200 kg → collection is skipped
     - The waste is accumulated and reconsidered the next day

**Expected Outcomes**
- **Daily solution**: vehicle routes, ordered nodes, total distance
- **Weekly log**: collected vs. accumulated waste, total distance

- Efficient waste collection routes per category and day
- Cost-effective use of vehicles with minimal redundant trips
- Handling of waste accumulation when demand is low (for type B)
- Weekly summary of total travel distance, collections performed, and unmet demand


