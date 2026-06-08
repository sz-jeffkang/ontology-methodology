# The 9 Data Domains

> A unified data model for enterprise and industrial analysis

---

## Domain Map

```
┌─────────────────────────────────────────────┐
│               Enterprise Core (5)            │
│  registration · financials · tags · spatial  │
├─────────────────────────────────────────────┤
│  Industrial Space (3)    │  Innovation (1)   │
│  parks · buildings · M   │  carriers · labs  │
├──────────────────────────┼───────────────────┤
│  Urban Space (7)         │  Planning (3)     │
│  roads · metro · POI ·   │  detailed plan ·  │
│  land · boundary ·       │  zoning · control │
│  buildings · elevation   │                   │
├──────────────────────────┼───────────────────┤
│  Transportation (5)      │  Special Zones (2)│
│  metro · bus · rail ·    │  Qianhai · border │
│  airport · ports         │                   │
├──────────────────────────┴───────────────────┤
│  Historical/Natural (3)                      │
│  heritage · geology · ecology                │
└─────────────────────────────────────────────┘
```

## Domain Details

### Enterprise Core (5 tables)
- **Enterprise Registration**: legal name, credit code, legal rep, registered capital
- **Financials**: annual revenue, value-added, growth rates, R&D expenditure
- **Tags**: national high-tech, SRDI champion, listed, unicorn, gazelle, foreign/HK-funded
- **Spatial**: geocoded lat/lng, street assignment, park membership
- **Classification**: industry code (4-digit), strategic emerging category, product chain

### Industrial Space (3 tables)
- **Parks**: name, area, spatial boundary, enterprise count, total output
- **Buildings**: height, footprint, usage classification
- **M-Land Performance**: industrial plot output per hectare, FAR, compliance

### Urban Space (7 tables)
- Roads (classified: motorway/trunk/primary), metro stations/stops, POI (shopping/medical/education/restaurants), land use, administrative boundaries, buildings footprint, elevation DEM

### Innovation (1 table)
- **Innovation Carriers**: 3,404 entries — key labs, engineering centers, enterprise tech centers, public service platforms, by type and national/provincial/municipal level

### Transportation (5 tables)
- Metro lines/stations, bus routes/stops, railway stations, airports, ports

### Planning Control (3 tables)
- Detailed land use plans (45K plots with use code, FAR, building density), zoning, regulatory control lines

### Special Zones (2)
- Qianhai cooperation zone boundaries, cross-border integration areas

### Historical/Natural (3)
- Cultural heritage sites, geological survey, ecological protection zones

---

## Join Key Patterns

```
Enterprise ←park_id→ Parks
Enterprise ←street_name→ Boundaries
Enterprise ←spatial ST_Contains→ Detailed Plan
Parks ←spatial ST_DWithin→ Metro (500m buffer)
Parks ←spatial ST_DWithin→ Innovation Carriers (500m buffer)
Innovation ←spatial ST_DWithin→ Enterprise (500m buffer)
```

---

## Schema Convention

```
public.*         ← raw ingested tables
sz.*             ← analysis-ready tables (cleaned, geocoded)
ontology.*       ← materialized views (pre-joined, indexed)
```

---

*Related: [Design Phases →](design-phases.md)*
