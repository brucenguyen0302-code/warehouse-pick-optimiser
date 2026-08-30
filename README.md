# Warehouse Pick-Path Optimiser

Route optimisation for order picking in a real footwear warehouse.

A picker collects every item in a picking wave from shelves across the warehouse floor and
returns to the packing station. Racking blocks direct movement, so travel is restricted to
17 horizontal aisles and 3 vertical corridors. The question is which order to visit the
stops in.

**A\* search** computes true walking distance between shelf positions on the aisle network.
**Simulated Annealing** and a **Genetic Algorithm** then compete to find the shortest
visiting order. A\* is the cost model, not one of the compared techniques.

## The warehouse

| | |
|---|---|
| Storage locations | 2,292 across 4 rack levels |
| Distinct floor stops | 466 |
| Aisles / corridors | 17 horizontal, 3 vertical |
| Longest walk | 208 m |
| Routable picking waves | 5,858 (8–24 stops each) |

## Method

- Pick locations are mapped to the aisle position a picker stands in to reach them.
- A\* on the aisle network gives exact walking distances, verified against an independently
  written Dijkstra implementation.
- Distances are precomputed into a 466 × 466 matrix so route evaluation is array lookups.
- Waves are split chronologically at 1 July 2023: earlier waves tune parameters, later
  waves are held out entirely as unseen test data.
- Both techniques receive identical objective-evaluation budgets, counted rather than
  assumed.

## Baselines

Results are measured against the warehouse's own picking order, not against a random one.
That choice matters: the company's existing order is already 76% better than random, so
comparing against random would inflate any reported improvement roughly fourfold.

| Baseline | Gap above the true optimum |
|---|---|
| Random order | +76.0% |
| Listed order (the company's) | +17.1% |
| Nearest neighbour | +4.6% |

Exact optima come from Held-Karp dynamic programming on waves small enough to solve
exactly, giving ground truth rather than only relative comparisons.

## Data

Not included in this repository.

> de Assis, R. F. et al. *Order Picking Dataset from a Warehouse of a Footwear
> Manufacturing Company.* Mendeley Data, V1.
> https://doi.org/10.17632/pf2w725pw3.1

Download it and upload these five files to a folder in Google Drive:

```
Storage_Location.csv
Support_Points_Navigation.csv
Picking_Wave.csv
Customer_Order.csv
Product.csv
```

The notebook searches Drive for `Storage_Location.csv` and uses whichever folder contains
it, so the folder name does not matter.

## Running it

Open `notebooks/Nguyen_LeBinh_AT1_1_notebook.ipynb` in Google Colab and run all cells.

## Status

Built for 42172 Introduction to AI, University of Technology Sydney.
