# Optimization of Hazardous Waste collection in the Province of Brescia
using a capacitated, multi-vehicle and periodic Vehicle Routin Problem.

Tools used: Python pyomo, Overpass API, Excel sheets (to organize data into tables)

## Bi-objective Strategy
Our goal is double
1. Minimize the total collection cost (in terms of travel time) for hospital waste over a weekly horizon, while respecting vehicle capacity, specific frequency constraints and constraints related to the nature of the waste.
2. Minimize the risks during the carriage.
The first goal will be the core of the objective function, while the second will be handled outside the VRP model, during the preprocessing of data. 

## Waste Categories
- **ACE**: General medical waste (collected daily)
- **D**: Hazardous medical waste (collected daily)
- **B**: Chemical/special waste – collected **only if the total quantity for the day exceeds 200 kg**

## Proposed Methodology
The methodology is divided into three main parts:

**Part 1: Training a forecasting model on Real Data**
- 
**Part 2: Augmenting the Dataset with Synthetic Data and Training a New LSTM Model**

**Part 3: Comparison and Analysis**

**Expected Outcomes**
