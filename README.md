# Optimal Urban Network Design: A\* and MST Approach for POI Connectivity

**Authors:**  
Luisa M. G. Mathias  
Viviane Stephane P. Novo  

**Course:**  
Algorithms and Data Structures II (DCA3702)  
Computer Engineering – Universidade Federal do Rio Grande do Norte (UFRN)

---

## Objective

This project aims to optimize the infrastructure layout required to connect a set of **Points of Interest (POIs)** in urban environments.
It uses a methodology that combines two essential graph algorithms to ensure minimum total distance and complete connectivity:

  * **A\* Search Algorithm** to find the shortest path between every pair of POIs along **real road network segments**.
  * **Kruskal's Algorithm (MST)** to select the minimum total length of these paths required to connect all POIs without creating cycles.

---

## Problem Description

The core challenge is to determine the minimum length of infrastructure (in kilometers) required to connect $N$ critical POIs within a city using its existing road network. 
This problem is modeled as finding the **Minimum Spanning Tree (MST)** on a **complete graph** where the vertices are the POIs, and the edge weights are the shortest route distances calculated by the $\mathbf{A^*}$ algorithm.

The analysis uses data from **9 Northeast Brazilian capital cities** to compare the efficiency of network optimization across different urban topologies.

---

## Methodology and Experimental Setup

The experiment followed a multi-step pipeline to transform geographic data into an optimized network layout.

### 🔹 Data Generation

  * **Road Network Graphs:** The **OSMnx** library was used to download the **`'drive'`** network type (road graph) for each city. The graphs were immediately **projected to UTM** coordinates using `ox.project_graph` to ensure accurate distance calculation in meters/kilometers.
  * **POI Selection:** For the comparative analysis, **50 random POIs** (`n_pois = 50`) were selected in each city to guarantee a standard sample size for the MST calculation.

### 🔹 Algorithm Pipeline

1.  **A\* Pathfinding:** For every pair of the 50 POIs, the shortest path was computed using the $\mathbf{A^*}$ algorithm, utilizing the **Euclidean distance** on the projected graph as the heuristic. The resulting distance (route length) was stored as the **edge weight**.
2.  **Complete POI Graph:** A new complete graph was constructed where the 50 POIs were the vertices, and the edges were weighted by the calculated $\mathbf{A^*}$ distance between them.
3.  **MST Calculation:** **Kruskal's Algorithm** (via NetworkX) was applied to the complete POI graph to find the MST, yielding the minimum necessary connections.
4.  **Real Network Reconstruction:** The actual road segments (routes) corresponding to the MST edges were retrieved, and their lengths were summed to determine the **Real Total Length of the MST Network**.

-----

## Experimental Configuration

| Parameter | Value |
| :--- | :--- |
| **Cities Compared** | 9 Northeast Capitals (e.g., Natal, Recife, Salvador) |
| **Network Type** | `'drive'` (directed multi-graph) |
| **Number of POIs** | 50 (Randomly sampled) |
| **Graph Projection** | UTM (for metric accuracy) |
| **Shortest Path** | $\text{A}^{*}$ using $\mathbf{length}$ as weight |
| **Heuristic** | Euclidean / Great-Circle distance (admissible) |
| **Spanning Tree** | **Minimum Spanning Tree (MST)** using Kruskal's algorithm |
| **Libraries** | OSMnx, NetworkX, Pandas, Matplotlib |

-----

## Experimental Results

The table below consolidates the key metrics from the analysis of 9 major cities, all using **50 randomly selected POIs**.

| Cidade | N POIs | N. Arestas MST | Comprimento MST (km) | Comprimento Real (km) | Média km/POI |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Maceió | 50 | 49 | 115.70 | 103.58 | 2.314 |
| Salvador | 50 | 49 | 131.82 | 99.02 | 2.636 |
| Fortaleza | 50 | 49 | 116.39 | 90.78 | 2.327 |
| São Luís | 50 | 49 | 129.62 | 99.87 | 2.592 |
| João Pessoa | 50 | 49 | 83.65 | 67.99 | 1.673 |
| Recife | 50 | 49 | 101.15 | 79.87 | 2.023 |
| Teresina | 50 | 49 | 169.33 | 147.84 | 3.387 |
| **Natal** | **50** | **49** | **80.76** | **64.55** | **1.615** |
| Aracaju | 50 | 49 | 78.74 | 59.59 | 1.575 |

-----

## Visualization

The final visualization for each city shows the base road network (gray) with the optimized infrastructure layout (red), which is the union of the $\text{A}^{*}$ routes selected by the MST.

---

## Results Analysis

The comparative analysis reveals distinct patterns of spatial efficiency, reflecting local **urban topology and planning**.

  - **Highest Required Length (Teresina):** **Teresina** shows the largest total length and the highest **Mean km/POI** ($\approx 3.39 \text{ km/POI}$). This suggests the POIs are more dispersed,
    partly due to the city's **inland position** and the **Rio Parnaíba acting as a geographic barrier**, necessitating longer A\* routes for connectivity.
  - **Moderate Lengths (Salvador, São Luís):** **Salvador** and **São Luís** also exhibit high averages ($\approx 2.6 \text{ km/POI}$) due to **large territorial extension** and factors like
  - **hilly topography** (Salvador) or **linear development** along the coast. **Lowest Required Length (Natal, Aracaju, João Pessoa):** Cities like **Natal**, **Aracaju**, and **João Pessoa** have
    the lowest Mean $\text{km/POI}$ ($\approx 1.6 \text{ km/POI}$). This implies a **denser, more compact road network** in their central areas, facilitating shorter $\text{A}^*$ paths between randomly distributed POIs.

### Limitations of the $\text{A}^* + \text{MST}$ Method

The $\text{A}^* + \text{MST}$ approach provides a **static, minimum-cost blueprint for connectivity**. However, it has key limitations:

  * It ignores **dynamic variables** like traffic, rush hours, average speed, and one-way restrictions that affect real travel time.
  * The resulting network only guarantees the minimal infrastructure base required to link the POIs; **it does not guarantee the globally most efficient sequential route**
    for visiting all 50 POIs (which is the classic **Traveling Salesman Problem - TSP**).

---

## How to Run

```bash
# Clone the repository
git clone https://github.com/SEU-USUARIO/astar-mst-urban-optimization.git
cd astar-mst-urban-optimization

# (Optional) Create and activate a virtual environment
# python -m venv venv
# source venv/bin/activate      # Linux/Mac
# venv\Scripts\activate         # Windows

# Install dependencies (requires osmnx, networkx, pandas, matplotlib)
pip install osmnx networkx pandas matplotlib

# Run the notebook with the full analysis
jupyter notebook U2\ -\ A_\ +\ MST.ipynb
```

---

## Video Presentation

[ **Watch the project demonstration video**]( )  

---

## References

- Notebook-base I: [week07/Astar.ipnyb](https://github.com/ivanovitchm/datastructure/blob/main/lessons/week07/Astar.ipynb)
- Notebook-base II: [week08/kruskal_natal.ipynb](https://github.com/ivanovitchm/datastructure/blob/main/lessons/week08/kruskal_natal.ipynb)
- 

---




