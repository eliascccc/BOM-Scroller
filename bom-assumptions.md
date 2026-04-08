# Core assumptions behind the BOM structure in BOMscroller

BOMscroller is not built on the assumption that a BOM is a completely free and chaotic list of components that changes arbitrarily between versions. Instead, it is based on a common ERP-driven scenario where the same SKU persists over time and its BOM evolves incrementally through revisions.

In this model, a new BOM version does not mean a completely new list. It means that the same SKU continues forward with the same structural layout, while individual rows are added, removed, or changed.

BOMscroller is built around that assumption of structural continuity. Most neighboring versions are expected to share the majority of their row positions, with changes happening locally inside the existing structure rather than through a full rebuild.

This is a fundamental design decision. The project does not attempt to match rows based on content alone. Instead, it assumes that structure is stable and uses it as the primary anchor for comparison.

It assumes that the user wants to compare revisions of the same SKU over time, where structure and ordering already carry meaning from the source system.

---

# Row structure as the backbone of the BOM

The central structural concept in BOMscroller is the row position, represented in the simplified model as `item_grp`.

In real ERP systems, this would typically correspond to a position number or row identifier that is stable within a SKU over time. It is not just a sorting key. It encodes a structural position in the BOM.

In BOMscroller, `item_grp` plays this role. It defines where a component sits in the BOM and provides continuity across revisions. It does not represent the identity of the component itself. Instead, it represents the position or functional slot in the BOM structure.

If a BOM evolves from version 2 to 3 to 4, the structure is expected to persist. Rows are not renumbered from scratch. Instead, new rows are inserted into the existing structure, often using nearby numbers (for example `0201` after `0200`). Old rows are not arbitrarily reused, because traceability is typically important in ERP-driven environments.

This means that a value like `0200` is not just a number. It is a signal of a specific structural location in the BOM over time.

---

# How changes are assumed to behave

BOMscroller assumes that changes to a BOM respect the existing structure.

When new components are introduced, they are usually added near related components. If a new steel plate is added, it will typically appear close to existing steel plates in the numbering sequence. This preserves a meaningful ordering that reflects how the product is organized.

As a result, the ordering of `item_grp` often carries semantic meaning. Even if the exact article number changes, the position in the list still tells the user something about where the change happened in the product structure.

This makes structural comparison more informative than matching based purely on article numbers or descriptions.

---

# What `item_grp` means in this model

In BOMscroller, `item_grp` does not mean “this is the exact same component.”

It means: “this is the same structural position, or the closest available anchor for comparison.”

It is a positional anchor, not an identity key.

If two rows share the same `item_grp`, they are treated as belonging to the same logical position in the BOM. They are compared directly. If the `item` or `qty` differs, the row is marked as changed.

If a row appears in one version but not in another at that position, it is treated as an addition or removal.

The system does not attempt to prove true identity of components across revisions. It uses structure as the primary anchor and then evaluates differences in:

* `item` (exact article, e.g. `550437-03` vs `550612-01`)
* `qty`

A change in either is treated as a meaningful modification.
