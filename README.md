## BomScroller

A time-based BOM navigation tool based on ERP data.


---

## Overview

BomScroller is a local web tool for exploring how a Bill of Materials (BOM) evolves over time.

It does **not** compare static version snapshots.  
Instead, it reconstructs BOMs from a **date-driven row model** and compares them visually.

The goal is simple:

> Understand *what changed, where,* and *when* — in a way that matches how BOMs actually behave in real systems.

---
## Example

<img width="1850" height="831" alt="image" src="https://github.com/user-attachments/assets/eadfdfa1-7d4f-4ca8-915a-e97d245935e4" />

---

## How a BOM is presented

For each version:

1. Take its `from_date`
2. Get all active rows at that date
3. Group by base article (e.g. `550412`)
4. Keep only the latest revision (`-03` over `-02`)
5. Sort by `item_nr`

**Result:** one clean BOM per version

---


## Coloring logic

| Relation                  | Color   |
|--------------------------|--------|
| Change Left vs Center    | Blue   |
| Change Center vs Right   | Purple |

---

## What this tool is

- A visual diff tool for BOM evolution  
- Based on date-effective data  
- Designed for human understanding  

---

## Running locally

```bash
pip install flask
python app.py
```

Then open:

```
http://127.0.0.1:5000
```

---
## How it compare

[bom-diff](https://github.com/ttran-tech/bom-diff)
→ Upload and compare two BOM files (from Excel)

Bom Scroller (this project)
→ Scroll through BOM versions over time (from ERP data)

---

## Project status

This is a concept prototype.

The focus is on:

- data model correctness  
- visual clarity  
- realistic BOM behavior  

Not production readiness.
