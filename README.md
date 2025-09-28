# Zaragoza Urban Mobility Planning Tool

This project builds an **interactive urban mobility planning platform** for Zaragoza, Spain.  
It combines **cycling infrastructure**, **bus routes**, **tram routes**, and spatial layers of **walking and cycling infrastructure gaps** into a single Folium map.  

In addition to the multimodal map, the project includes an **emissions analysis module**, where district-level emissions data is processed, aggregated, and visualised through charts and spatial overlays. Together, these modules create a powerful exploratory tool for mobility and sustainability planning in Zaragoza.  

---
<p align="center">
  <img src="https://github.com/user-attachments/assets/7be2d9c3-d508-40be-b989-4d8599e5491f" alt="Screenshot 1" width="33%"/>
  <img src="https://github.com/user-attachments/assets/ba22856c-fa5f-41ce-a8f1-e3e3fce33a0f" alt="Screenshot 2" width="33%"/>
  <img src="https://github.com/user-attachments/assets/fdaba80c-bd36-454f-bd96-3a5bf5f873c4" alt="Screenshot 3" width="33%"/>
</p>





## 🔍 Overview

The repository provides:

- **Interactive Folium Map**:  
  - Base maps (OpenStreetMap, CartoDB, Stamen).  
  - Layers for Zaragoza’s **bike lanes**, **bus routes (GTFS)**, and **tram routes (GTFS)**.  
  - **Walking and cycling network gap analysis** using OpenStreetMap data (OSMnx).  
  - Toggle controls to enable/disable infrastructure layers.



https://github.com/user-attachments/assets/d1d73fd3-81ce-4e1e-9c47-0ed39f86cdf1



- **Emissions Analysis Module**:  
  - District-level aggregation of transport emissions.  
  - Coverage statistics for walking/cycling networks.  
  - Visualisations: bar charts, stacked bars, pie charts, scatter plots.  
  - Combined 2×2 grid export of visualisations.  
  - Folium map overlay with district-level emissions and legend.

<img width="3018" height="1580" alt="image" src="https://github.com/user-attachments/assets/ba22856c-fa5f-41ce-a8f1-e3e3fce33a0f" />


<img width="3018" height="1580" alt="image" src="https://github.com/user-attachments/assets/fdaba80c-bd36-454f-bd96-3a5bf5f873c4" />

- **Extensions / Side Modules**:  
  - Accessibility: stop coverage and centroids.  
  - Legend customisation and district label testing.  
  - Export-ready multipanel graphics.  

---

## 📊 Aim

The main purpose of this project is to create a **planning support tool** that helps stakeholders and researchers explore:  
- Where Zaragoza’s transport infrastructure is concentrated.  
- Where **gaps** exist in walking and cycling networks.  
- How emissions vary spatially across districts.  
- How multimodal integration (bus, tram, cycling, walking) can inform sustainable mobility planning.

- ---

## 🚀 Features

### 1. Main Interactive Map
- **Basemaps**: OpenStreetMap, CartoDB Positron, Stamen Toner for different visual styles.  
- **Cycling Infrastructure**: Shapefiles from Zaragoza Open Data, styled in blue.  
- **Bus Routes**: Extracted from GTFS feed (`routes.txt`, `shapes.txt`) → drawn as orange polylines.  
- **Tram Routes**: GTFS tram routes parsed → drawn as purple polylines.  
- **Walking & Cycling Gaps**:  
  - Detected using **OSMnx** street graph extraction.  
  - Compared available walking/cycling edges against coverage radii of stops and key destinations.  
  - Highlighted in **red (walking gaps)** and **green (cycling gaps)**.  
- **Interactivity**:  
  - Popups for metadata (`route_id`, `route_long_name`, etc. from GTFS).  
  - Layer control for toggling visibility of infrastructure types.  
  - Custom legend for map readability.  

**Finding**: Walking and cycling gaps often coincided with **lower-density areas** and were **negatively correlated with bus stop density**. Districts with more transit coverage showed fewer walking/cycling accessibility gaps.  

---

### 2. Emissions Analysis Module
- **District Aggregation**: Joined emissions CSV with GeoJSON boundaries (`district_id` join).  
- **Coverage Metrics**: Cycling/walking infrastructure km per district divided by district area.  
  - Formula:  
    ```python
    coverage_ratio = infra_length_km / district_area_km2
    ```
- **Charts**:  
  - **Bar chart**: emissions per district.  
  - **Stacked bar**: transport vs non-transport contributions.  
  - **Pie chart**: modal split of transport-related emissions.  
  - **Scatter plot**:  
    - *X-axis*: emissions per capita.  
    - *Y-axis*: cycling/walking infrastructure coverage.  
    - *Formula for correlation*:  
      ```python
      correlation = df['emissions_pc'].corr(df['coverage_ratio'])
      ```
    - Result: **negative correlation** → higher cycling/walking coverage linked to lower emissions.  
- **Composite Output**:  
  - Multi-panel (2×2) layout created with Matplotlib.  
  - District emissions choropleth overlaid on Folium map.  

**Finding**: Districts with higher cycling and walking infrastructure coverage tended to show **lower per-capita emissions**. This highlighted the **potential emissions benefit of active travel infrastructure investment**.  

---

### 3. Accessibility & Side Modules
- **Accessibility Analysis**:  
  - Measured coverage of **hospitals, universities, and shopping centres** within 800m (walk) and 2.5km (cycle).  
  - Formula:  
    ```python
    accessibility = (covered_destinations / total_destinations) * 100
    ```
  - Findings: Accessibility gaps strongest at **suburban edges**, especially for universities and hospitals.  

- **Centrality Measures**:  
  - Graph-based metrics applied to road/cycle/walk networks using NetworkX:  
    - **Betweenness Centrality**:  
      ```python
      nx.betweenness_centrality(G, weight="length")
      ```  
      → Identified network bottlenecks.  
    - **Closeness Centrality**:  
      ```python
      nx.closeness_centrality(G, distance="length")
      ```  
      → Highlighted most accessible hubs.  
    - **Degree Centrality**:  
      ```python
      nx.degree_centrality(G)
      ```  
      → Showed connectivity density of nodes.  

  - Findings: High-centrality nodes often coincided with **tram interchanges** and **major road junctions**, reinforcing their importance for multimodal access.  

- **Export Options**:  
  - Static PNGs (matplotlib).  
  - Interactive Folium HTML maps with legends and popups.  

---

## 📚 Tools & Libraries

This project integrates **transport, GIS, and data science libraries**:  

- **Folium** → Interactive maps, popups, basemaps, legends.  
- **OSMnx** → Extracting and analysing walking/cycling/driving networks from OpenStreetMap.  
- **GeoPandas** → Reading shapefiles, GeoJSONs, and performing spatial joins.  
- **Pandas** → Data wrangling (GTFS parsing, emissions data aggregation).  
- **Matplotlib** → Generating bar charts, pie charts, scatter plots, and multi-panel outputs.  
- **Shapely** → Geometry operations (buffering for accessibility radii).  
- **NetworkX** → Graph-theoretical centrality calculations on networks.  
- **GTFS Parsing (CSV)** → Extracting routes, trips, and shapes for bus/tram layers.  

---

## 📂 Data Sources

This project relies on **open and official datasets**:  

- **Cycling Mobility Data**  
  - Source: [Zaragoza Open Data Portal](https://www.zaragoza.es/sede/portal/conoce-explora-zgz/servicio/indicadores/7c218d20-a88e-4e1c-b11e-562632531ee5)  
  - License: [Municipal Open Data Licence](https://www.zaragoza.es/sede/portal/aviso-legal#condiciones)  

- **Public Transport (Bus & Tram)**  
  - Format: GTFS feed.  
  - Includes: routes, stops, trips, and schedules.  

- **Street Networks**  
  - Source: [OpenStreetMap](https://www.openstreetmap.org/)  
  - Accessed via [OSMnx](https://osmnx.readthedocs.io/).  

- **Emissions Data**  
  - District-level dataset (transport and total emissions).  
  - Aggregated for bar, pie, and scatter analyses.  

---

## ⚙️ Installation

Clone this repository and install required libraries:  

```bash
git clone https://github.com/yourusername/zaragoza-transport-map.git
cd zaragoza-transport-map
pip install -r requirements.txt


folium
osmnx
geopandas
pandas
matplotlib
shapely
networkx

