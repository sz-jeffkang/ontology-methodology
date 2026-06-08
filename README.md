# Enterprise Ontology Methodology

<p align="center">
  <img src="https://img.shields.io/badge/Data%20Domains-9-blue" alt="9 Domains">
  <img src="https://img.shields.io/badge/Views-25-green" alt="25 Views">
  <img src="https://img.shields.io/badge/License-MIT-yellow" alt="MIT">
</p>

> **A proven approach for designing enterprise data ontologies — from raw tables to unified knowledge graphs. 9 domains, 253 tables, 25 materialized views.**

---

## The Problem

Government and enterprise data comes in silos: enterprise registration in one system, GIS coordinates in another, tax records in a third. When analysts need "all enterprises within 500m of a metro station with >10% revenue growth," they spend 80% of their time just finding and joining the right tables.

An ontology solves this: a unified semantic layer where `enterprise_full` already has location, financials, innovation tags, and industry classification — one table, one query.

---

## The Three-Phase Methodology

### P0: Metadata Standardization
Catalog every table, every column. Document data types, null rates, encoding traps, and join keys.

### P1: Materialized Views
Create denormalized views that pre-join the most common query patterns:
```
enterprise_full (9K rows)  → joins enterprise base + GIS + tags + financials
park_full (2K rows)        → joins parks + enterprises + innovation carriers + metro
street_competitiveness (10) → aggregation ready for dashboards
```

### P2: Analysis Templates
For each research theme, create a reusable SQL template with documented parameters:
```sql
-- Template: Cluster Health Diagnosis
SELECT cluster_name,
       ROUND(health_score::numeric, 1) as score,
       enterprise_count, total_output,
       srdi_count
FROM ontology.cluster_health
WHERE street = %s
ORDER BY score DESC;
```

---

## The 9 Data Domains

| Domain | Tables | Key Views |
|--------|--------|-----------|
| **Enterprise** | 5 core | `enterprise_full` (9K rows) |
| **Industrial Space** | 3 | `park_full` (2K parks) |
| **Urban Space** | 7 | `mland_performance` |
| **Transportation** | 5 | `metro_coverage` |
| **Innovation** | 1 | `carrier_efficiency` |
| **Planning Control** | 3 | `detailed_plan_analysis` |
| **Housing** | 1 | — |
| **Qianhai Zone** | 2 | `qianhai_fusion` |
| **Historical/Natural** | 3 | — |

---

## Key Insight: SRID Camouflage

One of our most painful discoveries: `ST_SRID(geom)` returns `4326`, but the actual coordinates are EPSG:4547 (CGCS2000 projected). Every spatial join returned 0 rows until we diagnosed this. The ontology layer fixes this once, so no analyst ever hits it again.

---

## License

MIT. Build your ontology on ours.

<p align="center">
  <sub>9 domains · 253 tables · 25 views · 1 unified query layer</sub>
</p>
