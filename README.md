# PDVRPTW-with-Gurobi

## Overview

This project implements a **Pickup and Delivery Vehicle Routing Problem with Time Windows (PDVRPTW)** solver using **Gurobi** optimization engine. The model handles simultaneous pickup and delivery operations with time window constraints, capacity limits, and multiple vehicles.

## Model Paradigm

- Multi-vehicle support with customizable vehicle count, each with distinct start and end depots (non-closed loops)
- Paired pickup-delivery nodes & pickup-before-delivery precedence constraints
- Hard time windows: earliest/latest service time for each node
- Identical capacity and speed across all vehicles

## Data Structure

Each node in the dataset contains:

| Column | Description |
|--------|-------------|
| 0 | Node ID |
| 1 | X-coordinate |
| 2 | Y-coordinate |
| 3 | Demand (positive = pickup, negative = delivery) |
| 4 | Earliest service time |
| 5 | Latest service time |
| 6 | Service duration |

## Model Formulation

### Decision Variables

- `x[i,j,k]`: Binary variable, 1 if vehicle k travels from node i to node j
- `Q[i,k]`: Load of vehicle k after visiting node i
- `T[i,k]`: Arrival time of vehicle k at node i

### Objective Function

Minimize total travel distance:

$$ \text{Min} \sum_{i} \sum_{j} \sum_{k} c_{ij} \cdot x_{ijk} $$

### Constraints

1. Each node is served exactly once
2. Each pickup-delivery pair is handled by the same vehicle
3. Each vehicle starts from its own depot
4. Flow conservation: every arrival must depart
5. Each vehicle ends at its own depot
6. Time consistency: arrival + service + travel ≤ next arrival
7. Load updates after each pickup/delivery
8. Pickup must precede delivery for each paired order
9. Total route time cannot exceed vehicle limit
10. Arrival times must fall within node time windows
11. Vehicle load must stay within capacity bounds

## Scenarios

- Case 1: 1 Vehicle, 2 Orders
- Case 2: 2 Vehicles, 2 Orders
- Case 3: 3 Vehicles, 4 Orders

## Visualization

The solver generates route maps with:
- Color-coded vehicle paths
- Directional arrows indicating travel direction
- Different node types (start points, pickup points, delivery points, end points)

## Requirements

- Python 3.11
- Gurobi Optimizer 11.0.3
- NumPy 1.26.4
- Matplotlib 3.9.2
