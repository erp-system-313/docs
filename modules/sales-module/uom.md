# UoM (Unit of Measure) — Odoo Alignment

## Implementation: V29 UoM

**Date:** 2026-05-15
**Migration:** V29__uom.sql
**Commit:** 32a8528

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `uom.category` | `UomCategory` | ✅ Full |
| `uom.uom` | `Uom` | ✅ Full |

## Entity Model

```
UomCategory
├── id
├── name (e.g., "Length", "Weight", "Volume")
└── uoms → [Uom]

Uom
├── id
├── name (e.g., "Meters", "Kilograms")
├── code (e.g., "m", "kg")
├── category_id → UomCategory
├── factor (conversion to reference unit)
├── rounding
├── is_reference (factor = 1)
├── uom_type: UNIT | LENGTH | WEIGHT | VOLUME | TIME | AREA | WORKING_HOURS
└── active
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/uoms/categories` | List all categories |
| GET | `/api/v1/uoms/categories/{id}` | Get category by ID |
| POST | `/api/v1/uoms/categories` | Create category |
| GET | `/api/v1/uoms` | List all active UoMs |
| GET | `/api/v1/uoms/{id}` | Get UoM by ID |
| GET | `/api/v1/uoms/by-category/{categoryId}` | Get UoMs by category |
| GET | `/api/v1/uoms/by-type/{type}` | Get UoMs by type |
| POST | `/api/v1/uoms` | Create UoM |
| PUT | `/api/v1/uoms/{id}` | Update UoM |
| GET | `/api/v1/uoms/convert?quantity=X&fromUomId=Y&toUomId=Z` | Convert quantity |

## Seed Data

Migration V29 creates 7 categories with standard units:

| Category | Reference Unit | Other Units |
|----------|---------------|-------------|
| Unit | Units (1) | Dozens (12) |
| Length | Meters (1) | Kilometers (1000), Feet (0.3048), Inches (0.0254) |
| Weight | Kilograms (1) | Grams (0.001), Pounds (0.453592) |
| Volume | Liters (1) | Milliliters (0.001), Gallons (3.78541) |
| Time | Hours (1) | Days (24), Minutes (0.016667) |
| Area | *(empty)* | — |
| Working Time | Working Hours (1) | Working Days (8) |

## Conversion Example

```
GET /api/v1/uoms/convert?quantity=5&fromUomId=<km_id>&toUomId=<m_id>
→ 5000 (5 km = 5000 m)
```
