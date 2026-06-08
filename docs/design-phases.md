# Ontology Design Phases

> From raw tables to unified knowledge graph — a three-phase methodology

---

## P0: Metadata Standardization

### Phase Goal
Every table, every column cataloged with: name, type, null rate, encoding, join keys, known traps.

### Output
```
metadata/
├── enterprises.yaml      ← 216 columns, 9,140 rows
├── parks.yaml             ← spatial, output, innovation tags
├── innovation.yaml        ← 3,404 carriers, types, levels
├── land_use.yaml          ← 45,000 plots, M/C/R classification
├── benchmark.yaml         ← cross-district comparison data
└── README.md              ← index + trap quick-reference
```

### Critical Discovery: SRID Camouflage
```
ST_SRID(geom) = 4326 (claimed)
Actual coords:  496089, 2509810 (EPSG:4547 projected)

→ Every spatial join returns 0 rows until fixed
→ Fix once at ontology layer, never again
```

---

## P1: Materialized Views

### Phase Goal
Pre-join the most common query patterns. One table = one insight.

### View Inventory

| View | Rows | Question Answered |
|------|:---:|------|
| `enterprise_full` | 9K | "Tell me everything about this company" |
| `park_full` | 2K | "Which park is most efficient?" |
| `street_industry` | 400 | "What clusters in each street?" |
| `street_lq` | 350 | "Where is industry X specialized?" |
| `cluster_health` | 32 | "Which cluster is thriving?" |
| `billion_tier` | 1.5K | "Who are the billion-yuan players?" |
| `street_competitiveness` | 10 | "Rank all streets" |
| `carrier_efficiency` | 19 | "Does innovation density = output?" |
| `mland_performance` | 10 | "Industrial land efficiency by street" |
| `park_fusion_score` | 840 | "Production-living integration" |

### Design Principle
Each view answers exactly ONE question from the 32-theme research framework. No more, no less.

---

## P2: Analysis Templates

### Phase Goal
For each of the 32 research themes, create a reusable SQL template with documented parameters.

### Template Structure
```sql
-- Template: Cluster Health Diagnosis
-- Parameters: <cluster_name>, <street>
-- Returns: health_score (0-100), enterprise_count, total_output, srdi_count

SELECT cluster_name,
       ROUND((leader_score * 0.4 + density_score * 0.3 + innovation_score * 0.3) * 100, 1) as health,
       enterprise_count, total_output_yi, srdi_count
FROM ontology.cluster_health
WHERE cluster_name = '<cluster_name>' AND street = '<street>';
```

### Coverage Status
- **A: Cluster Diagnosis** (6/6 ✅)
- **B: Enterprise Profiling** (5/5 ✅)
- **C: Spatial Performance** (5/5 ✅)
- **D: Supply Chain** (3/3 ✅)
- **E: Innovation Ecosystem** (3/3 ✅)
- **F: Benchmarking** (2/4) — missing relocation + policy impact
- **G: Forecasting** (1/3) — missing scenario + satellite

25/32 research themes covered → **78.1% coverage**

---

*Next: [The 9 Data Domains →](data-domains.md)*
