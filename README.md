# EV_battery_analytics_ml
### Electric Vehicle Battery Analytics and Machine Learning

## Project Overview

This project focuses on developing predictive models for State of Health (SoH) and Remaining Useful Life (RUL) estimation of Lithium-ion batteries using machine learning techniques. The work utilizes an accelerated life testing dataset containing battery performance data under various loading conditions.

## Team

- **Rushikesh Rajesh Kaduskar** - MSc Automation & Robotics, TU Dortmund University  
  GitHub: [@rushi-k-op](https://github.com/rushi-k-op)

- **Raghavi Dhaundiyal** - MSc Agriculture and Food Economics, University of Bonn  
  GitHub: [@raavi-dh](https://github.com/raavi-dh)

## Dataset Description

### Source Dataset
An accelerated Life Testing Dataset for Lithium-Ion Batteries with Constant and Variable Loading Conditions (Fricke et al., 2023)

The dataset is organized into three primary categories representing different battery aging scenarios:

1. **Regular ALT Batteries** (`regular_alt_batteries/`)  
   Battery packs cycled at consistent load levels or load ranges throughout their operational lifetime.

2. **Recommissioned Batteries** (`recommissioned_batteries/`)  
   Battery packs subjected to different load levels at various life stages, simulating real-world usage variability.

3. **Second Life Batteries** (`second_life_batteries/`)  
   Battery packs cycled at constant current during their second-life operational phase.

Each battery pack has a dedicated CSV file for continuous data logging, identified by its respective battery pack number.

### Data Structure

#### Temporal and Operational Data
- **start_time**: Cycle start timestamp (format: mm/dd/yyyy hh:mm:ss), typically representing ~24-hour intervals
- **relative_time**: Continuous time measurement from cycle initiation to battery failure (seconds)
- **mode**: Operational state indicator
  - `1`: Charging
  - `0`: Idle/standby (without load)
  - `-1`: Discharging (with load)

#### Electrical Measurements
- **voltage_charger**: Battery pack voltage measured immediately after connection to charging board (Volts)
- **voltage_load**: Battery pack voltage measured on load board during discharge (Volts)
- **current_load**: Discharge current measured via current sense resistors (Amperes)

#### Thermal Measurements
- **temperature_battery**: Surface temperature at battery cell electrodes (°C)
- **temperature_mosfet**: Load board MOSFET temperature for safety monitoring (°C)
- **temperature_resistor**: Current sense resistor temperature for safety monitoring (°C)

#### Mission Classification
- **mission_type**: Discharge profile identifier
  - `0`: Reference discharge (constant current at 2.5A)
  - `1`: Regular mission profile

**Note**: Voltage load, current load, and temperature measurements (MOSFET and resistor) are only recorded when the battery is connected to the load board during active discharge missions.

## Initial Understanding and Approach

### Key Relationships

The fundamental relationship governing battery capacity is expressed as:
Capacity (Ah) = Current (A) × Time (h)

This integral relationship forms the basis for capacity estimation from the time-series current measurements available in the dataset.

### Feature Engineering Considerations

#### Voltage-Related Features
- **Voltage curves and derivative features**: Analysis of charging/discharging voltage profiles
- **Voltage spikes**: Anomalous voltage behavior during state transitions
- **Voltage differentiation**: Rate of voltage change (dV/dt) as an indicator of degradation

Integration of current over time provides instantaneous capacity measurements:
Capacity = ∫ I(t) dt

#### Temperature Effects
Cold temperature conditions are known to reduce the effective capacity of Lithium-ion batteries. Thermal features may serve as critical indicators for capacity fade and degradation mechanisms.

#### Cycle-Based Features
- **Discharge rate**: Relationship between discharge current and capacity degradation
- **Battery age**: Cumulative cycle count and calendar aging effects

### Problem Formulation

#### Primary Objectives

1. **State of Health (SoH) Estimation**  
   Quantify the current maximum capacity of the battery relative to its original rated capacity:
SoH (%) = (Current Maximum Capacity / Original Rated Capacity) × 100

2. **Remaining Useful Life (RUL) Prediction**  
   Estimate the number of charge-discharge cycles remaining until the battery reaches a critical threshold (typically 80% of original capacity or another predefined end-of-life criterion).

## Methodology (In Progress)

### Data Preprocessing
- Handling multi-modal operational states (charge, discharge, idle)
- Time-series segmentation by cycle boundaries
- Feature extraction from voltage and current profiles

### Model Development
Machine learning and deep learning approaches for time-series prediction and regression tasks targeting SoH and RUL estimation.

### Evaluation Metrics
Appropriate metrics for regression and prognostics performance assessment will be defined as the project progresses.

## References
```bibtex
@misc{2023_alt_dataset_fricke_et_al,
    Author = {Kajetan Fricke and Renato G. Nascimento and Felipe A. C. Viana},
    Month = {July},
    Publisher = {nasa-data@lists.arc.nasa.gov},
    Title = {An accelerated Life Testing Dataset for Lithium-Ion Batteries with Constant and Variable Loading Conditions},
    Version = {0.0.1},
    Year = {2023}
}
