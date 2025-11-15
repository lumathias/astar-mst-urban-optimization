# MST analysis for police station connectivity using A* algorithm

**Authors:**

Luisa M. G. Mathias

Viviane Stephane P. Novo

This project is developed for academic purposes as part of Assignment 2, Unit 2 

**Course:** Algorithms and Data Structures II (DCA3702)

**Major:** Computer Engineering 

**Institution:** Federal University of Rio Grande do Norte (UFRN)

---

## Objective

This project aims to optimize the infrastructure layout required to connect **Police Stations** (amenity: police) in urban environments of Northeast Brazil. Combining two essential graph algorithms to ensure
minimum total distance and complete connectivity:
 * **A\* Search Algorithm:** to find the shortest path between every pair of police stations along **real road network segments**.
 * **Kruskal's Algorithm (MST)** to select the minimum total length of these paths required to connect all police stations without creating cycles.

---

## Problem Description

The core challenge is to determine the minimum length of infrastructure (in kilometers) required to interconnect police stations within a city using its existing road network. This problem is modeled as finding the **Minimum Spanning Tree (MST)** on a **complete graph** where the vertices are the police stations, and the edge weights are the shortest route distances calculated by the A\* algorithm. The analysis uses data from **9 Northeast Brazilian capital cities** to compare the efficiency of network optimization across different urban topologies and police station distributions. Providing a quantitative view of how efficiently distributed and connected urban security infrastructures are within various cities.

---

## Methodology and Experimental Setup
The experiment followed a multi-step pipeline to transform geographic data into an optimized network layout. Implemented using Python and the OSMnx and NetworkX libraries.

### 🔹  Data Acquisition

   * **Road Network Graphs:** The **OSMnx** library was used to download the **`'drive'`** network type (road graph) for each city. The graphs were immediately **projected to UTM**
   coordinates using `ox.project_graph` to ensure accurate distance calculation in meters/kilometers.
   
   * **POI Selection:** Police stations were identified using the ```amenity: police``` tag from OpenStreetMap. The number of stations varied naturally by city based on actual infrastructure.
     - Their geographic coordinates (latitude, longitude) were obtained through:   ```ox.features.features_from_place(place_name, tags=tags)```
     - Each POI was then matched to its nearest node in the street network using:  ``` ox.distance.nearest_nodes(G, X, Y)```
 

### 🔹 Algorithm Pipeline

   1. **A\* Pathfinding:** For every pair of police stations, the shortest path was computed using the **A*** algorithm, utilizing the **Euclidean distance** on the projected graph as the heuristic. The resulting distance (route length) was stored as the **edge weight**.
   
   2. **Complete Police Station Graph:** A new complete graph was constructed where police stations were the vertices, and the edges were weighted by the calculated **A*** distance between them.
   
   3. **MST Calculation:** **Kruskal's Algorithm** (via NetworkX) was applied to the complete graph to find the MST, yielding the minimum necessary connections.
   
   4. **Network Analysis:** The total length of the MST was calculated and compared across all 9 cities.

---

## Experimental Configuration
| Parameter | Value |
| :--- | :--- |
| **Cities Compared** | 9 Northeast Capitals |
| **POI Type** | Police Stations (`amenity: police`) |
| **Network Type** | `'drive'` |
| **Graph Projection** | UTM (for metric accuracy) |
| **Pathfinding Algorithm** | **A*** using `length` as weight |
| **Heuristic** | Euclidean distance (admissible and consistent) |
| **Spanning Tree** | **Minimum Spanning Tree (MST)** using Kruskal's algorithm |
| **Analysis Libraries** | OSMnx, NetworkX, Pandas, Matplotlib |

---

## Experimental Results

The table below consolidates the key metrics from the analysis of police station connectivity
across 9 major cities.

| City | Total MST (km) | Police Stations | Unique Nodes | MST Edges | Avg per Edge (km) | Avg per Station (km) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Maceió | 75.38 | 27 | 27 | 26 | 2.90 | 2.79 |
| Salvador | 139.08 | 83 | 66 | 65 | 2.14 | 1.68 |
| Fortaleza | 132.12 | 79 | 74 | 73 | 1.81 | 1.67 |
| São Luís | 111.57 | 41 | 39 | 38 | 2.94 | 2.72 |
| João Pessoa | 52.29 | 25 | 23 | 22 | 2.38 | 2.09 |
| Recife | 74.51 | 57 | 52 | 51 | 1.46 | 1.31 |
| Teresina | 101.11 | 38 | 35 | 34 | 2.97 | 2.66 |
| **Natal** | **74.81** | **100** | **70** | **69** | **1.08** | **0.75** |
| Aracaju | 64.90 | 36 | 36 | 34 | 1.91 | 1.80 |

### 🔹  Key Statistics
   -  **Total MST across all cities:** 826.77 km
   -  **Average per city:** 91.86 km
   -  **Most efficient:** Natal (0.75 km per station)
   -  **Least efficient:** Teresina (2.66 km per station)

---

## Results Analysis
The comparative analysis reveals distinct patterns of spatial efficiency in police stations distribution, reflecting local urban topology and public security planning. The images can be seen ![here](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issue-3627596341)

### 🔹  Efficiency Highlights
  
  - **![Natal](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issuecomment-3535178653) demonstrates exceptional efficiency:** Despite having the largest number of police stations (100), it achieves the lowest average distance per station (0.75 km). This indicates excellent spatial distribution and urban planning for public security infrastructure.
  
  - **![Recife](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issuecomment-3535192083) shows strong performance:** With 57 stations connected by only 74.51 km, Recife maintains a low average of 1.31 km per station, suggesting dense coverage and efficient road connectivity.
  
  - **![Salvador](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issuecomment-3535193103) faces scalability challenges:** While having 83 stations, Salvador requires the longest total infrastructure (139.08 km). However, its average per-station (1.68 km) remains reasonable, indicating good individual station placement despite the city's large area.


### 🔹  Geographic and Urban Factors

  - **Coastal compact cities** (Natal, Recife, ![João Pessoa](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issuecomment-3535249812)) generally show better efficiency due to denser urban fabric and more concentrated development patterns.
  
  - **Inland and geographically constrained cities** (![Teresina](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issuecomment-3535205507), ![São Luís](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issuecomment-3535204568)) exhibit higher averages per station, potentially due to:
  
  - River barriers (Rio Parnaíba in Teresina)
  
  - More dispersed urban development
  
  - Topographical challenges
  
  - **Large metropolitan areas** (Salvador, ![Fortaleza](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issuecomment-3535173242)) demonstrate the trade-off between comprehensive coverage and infrastructure costs, with higher total distances but reasonable per-station efficiency.


### 🔹  Public Security Implications

  - Cities with **average distance > 2.5 km per station** (Teresina, São Luís, ![Maceió](https://github.com/lumathias/astar-mst-urban-optimization/issues/1#issuecomment-3535173242)) may benefit from the strategic placement of additional stations or infrastructure improvements to reduce response times.
  
  - The **Natal model** (high station count with low connection costs) could serve as a benchmark for urban security planning in other cities.

---

##  Methodological Considerations

### 🔹  Strengths of the A* + MST Approach

  - **Real-world accuracy:** Uses actual road networks rather than straight-line distances
  
  - **Optimality guarantees:** A* with Euclidean heuristic ensures shortest paths; MST provides minimum connecting infrastructure
  
  - **Scalability:** The method efficiently handles cities with up to 100 police stations
  
  - **Comparative framework:** Enables objective comparison across different urban contexts

### 🔹  Limitations and Future Work

  - **Static analysis:** Does not account for dynamic factors like traffic patterns, time of day, or road conditions
  
  - **Single criterion optimization:** Focuses solely on distance minimization, while real-world security planning may prioritize other factors (response time, population density, crime rates)
  
  - **Data quality dependence:** Relies on OpenStreetMap completeness and accuracy for both road networks and police station locations
  
  - **Network resilience:** MST provides minimal connectivity but offers no redundancy; real security networks may require more robust topologies

---

## How to Reproduce

```bash
# Install dependencies
pip install osmnx networkx pandas matplotlib numpy scipy tqdm
# Run the analysis
python police_station_analysis.py
```

### Code Structure

```
project/
├── police_station_analysis.py # Main analysis script
├── results/
│ ├── comparative_analysis.csv
│ └── graphs/
│ ├── mst_maceio.png
│ ├── mst_salvador.png
│ └── ...
└── README.md
```

---

## 🎥 Video Presentation
[**Watch the project demonstration video**]([])

---

## References
- Notebook-based I:
[week07/Astar.ipynb](https://github.com/ivanovitchm/datastructure/blob/main/lessons/week07
/Astar.ipynb)
- Notebook-based II:
[week08/kruskal_natal.ipynb](https://github.com/ivanovitchm/datastructure/blob/main/lessons
/week08/kruskal_natal.ipynb)
- OSMnx Documentation: https://osmnx.readthedocs.io/
- NetworkX Documentation: https://networkx.org/

---
