# PDVRPTW-with-Gurobi

## Overview

This project implements a **Pickup and Delivery Vehicle Routing Problem with Time Windows (PDVRPTW)** solver using **Gurobi** optimization engine. The model handles simultaneous pickup and delivery operations with time window constraints, capacity limits, and multiple vehicles.

## Key Features

- Multi-vehicle support with customizable vehicle count
- Time window constraints for each node (earliest/latest service time)
- Capacity constraints with pickup and delivery quantities
- Pickup-before-delivery precedence constraints
- Visualized route planning with directional arrows
- Scenario comparison: 1-vehicle, 2-vehicle, and multi-vehicle configurations

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

$$ \text{Minimize} \sum_{i} \sum_{j} \sum_{k} c_{ij} \cdot x_{ijk} $$

### Constraints

1. Each node is visited exactly once
2. Pickup and delivery paired nodes are served by the same vehicle
3. Each vehicle departs from its unique start point
4. Flow conservation: arriving vehicles must depart
5. Each vehicle returns to its unique end point
6. Time feasibility: arrival time + service + travel ≤ next arrival
7. Load feasibility: load updates after pickup/delivery
8. Pickup before delivery: pickup node must be visited before its paired delivery
9. Total route time limit per vehicle
10. Time window constraints: arrivals must fall within [earliest, latest]
11. Capacity constraints: load must stay within [0, vehicle capacity]

## Scenarios

- Case 1: 1 Vehicle, 2 Orders
- Case 2: 2 Vehicles, 2 Orders
- Case 3: 3 Vehicles, 4 Orders

## Visualization

The solver generates route maps with:
- Color-coded vehicle paths
- Directional arrows indicating travel direction
- Different node types (start points, pickup points, delivery points, end points)

## Dependencies

- Python 3.7+
- Gurobi
- NumPy
- Matplotlib
