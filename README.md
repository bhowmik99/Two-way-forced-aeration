# Two-Way Forced Aeration Composting Simulation Using Python

## Overview

This project implements a **Python-based kinetic model for solid waste composting under two-way forced aeration**. The model simulates the effect of periodically reversing airflow direction between **upflow and downflow** conditions within a layered composting system.

The model was developed based on the mathematical framework of forced-aeration composting and was used to investigate the behavior of key composting parameters, including **temperature, moisture content, oxygen concentration, and volatile solids degradation**.

The project was developed as a computational extension of the previously established one-way forced aeration model by **Bari and Koenig (2012)**.

---

## Research Objective

The main objectives of this project were to:

* Develop a **programming-based kinetic model** for two-way forced aeration composting.
* Simulate composting behavior across multiple vertical layers.
* Implement **dynamic reversal of airflow direction** based on a user-defined cycle duration.
* Track changes in temperature, moisture, oxygen, and volatile solids with time.
* Compare two-way aeration with the previously developed **one-way forced aeration model**.
* Investigate how different airflow reversal periods affect composting performance.

---

## How Two-Way Aeration Works

In conventional one-way forced aeration, air continuously moves through the compost in a single direction. This can create differences in temperature, oxygen availability, and moisture between the lower and upper layers.

In this model, the airflow direction is periodically reversed:

```text
Upflow phase
     ↑
 Layer 6
 Layer 5
 Layer 4
 Layer 3
 Layer 2
 Layer 1
     ↑
   Air Inlet
```

After the specified cycle duration:

```text
   Air Inlet
     ↓
 Layer 6
 Layer 5
 Layer 4
 Layer 3
 Layer 2
 Layer 1
     ↓
Downflow phase
```

Therefore, a layer that previously experienced the beginning of the airflow path becomes part of the opposite flow path after reversal. This allows the model to investigate whether alternating airflow produces more uniform composting conditions.

---

## Model Structure

The composting system is represented as **six vertical layers**.

For every simulation hour, the model:

1. Determines the current airflow direction.
2. Establishes the corresponding layer calculation order.
3. Calculates the composting behavior of each layer.
4. Uses information from the previous layer in the airflow path.
5. Updates the relevant composting variables.
6. Stores the results in arrays.
7. Repeats the process for the complete simulation period.

The airflow direction is controlled using the cycle duration:

```python
if (hour // cycle_hours) % 2 == 0:
    layer_order = list(range(layers))
else:
    layer_order = list(range(layers - 1, -1, -1))
```

Thus, the model dynamically switches between:

**Layer 1 → Layer 2 → ... → Layer 6**

and

**Layer 6 → Layer 5 → ... → Layer 1**

---

## Model Inputs

The model allows the composting process to be simulated using parameters such as:

| Parameter                | Example Value |
| ------------------------ | ------------: |
| Initial compost mass     |        750 kg |
| Number of layers         |             6 |
| Simulation duration      |       28 days |
| Time step                |        1 hour |
| Initial temperature      |         25 °C |
| Initial moisture content |         58.9% |
| Fixed solids             |            4% |
| Volatile solids          |           48% |
| Airflow rate             |     5 m³/m²·h |
| Relative humidity        |          0.75 |
| Aeration cycle           |      Variable |

The model also incorporates parameters associated with heat generation, specific heat capacity, heat transfer, and reaction kinetics.

---

## Computational Method

The model begins by defining the initial composting and environmental parameters. Preliminary calculations determine quantities such as:

* Dry air mass
* Initial water mass
* Initial solid masses
* Initial volatile solid mass
* Initial mass of each compost layer
* Total simulation hours
* Vapor inflow

Arrays are then initialized to store **layer-wise and time-dependent simulation results**.

A nested computational loop processes the simulation hour by hour and layer by layer. The calculation sequence changes according to the airflow direction.

For upward airflow, the calculation proceeds from the lower layer toward the upper layer. When the airflow reverses, the calculation proceeds from the upper layer toward the lower layer.

This approach preserves the dependency between adjacent compost layers while representing the changing direction of forced aeration.

---

## Simulated Parameters

The model tracks the major physical and biological variables governing the composting process:

### Temperature

The model tracks temperature changes in each compost layer resulting from biological heat generation and heat transfer associated with aeration.

### Moisture

Water content is updated throughout the simulation to represent moisture loss associated with the composting and aeration process.

### Oxygen

Oxygen concentration is calculated layer by layer according to airflow direction and oxygen consumption during biological degradation.

### Volatile Solids

Volatile solids are tracked as an indicator of organic matter degradation during composting.

## Key Results

Under the investigated conditions, the two-way forced aeration model produced a lower final compost mass than the one-way model.

| Parameter                               | One-Way Aeration | Two-Way Aeration |
| --------------------------------------- | ---------------: | ---------------: |
| Initial mass                            |           750 kg |           750 kg |
| Final compost mass                      |         357.9 kg |     **332.2 kg** |
| Final total solids                      |         240.5 kg |         236.9 kg |
| Final fixed solids                      |          12.3 kg |          12.3 kg |
| Final non-volatile biodegradable solids |         148.0 kg |         148.0 kg |
| Final volatile solids                   |          80.6 kg |      **76.6 kg** |
| Final water content                     |         117.4 kg |      **95.3 kg** |

The layer-wise analysis showed comparable behavior between opposite layers, supporting the expected effect of airflow reversal on the distribution of composting conditions.

## Python Tools Used

The project uses Python for numerical computation, data handling, and visualization.

**Core tools:**

* Python
* NumPy
* Matplotlib
* Jupyter Notebook

The computational implementation relies primarily on **arrays, loops, conditional logic, mathematical calculations, and plotting** to reproduce the composting model.

---


## Reference

The kinetic framework underlying the forced-aeration composting model is based on:

> Bari, Q. H., & Koenig, A. (2012). Application of a simplified mathematical model to estimate the effect of forced aeration on composting in a closed system. *Waste Management, 32*(11), 2037–2045. https://doi.org/10.1016/j.wasman.2012.01.014

---
