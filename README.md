# PDBe-KB Shape Benchmark Dataset

This repository contains a **benchmark dataset and accompanying notebook** developed in the context of **PDBe-KB complex aggregation** to support **computational resource estimation** for 3D shape-matching workflows.

The benchmark is intended to characterise **computational scaling regimes**, not biological accuracy.

---

## Dataset Scope

The benchmark includes:

- **Protein-only macromolecular complexes**
- All components mapped to **UniProt**
- Complexes represented by **multiple biological assemblies**
- Molecular weight defined as the **median assembly molecular weight (kDa)**

RNA-containing and mixed-polymer complexes are excluded to ensure consistent size estimates.

---

## Input Data

The input is a CSV file containing **one row per complex**.  
The file may contain additional metadata columns, but **only the following columns are used** to construct the benchmark:

```
complex_id
num_assemblies
median_mw_kda
```

All other columns are ignored by the benchmark construction workflow.

---

## Stratification Strategy

The benchmark is constructed using **two variables only**:

- **Number of assemblies per complex**, a proxy for pairwise workload
- **Median molecular weight**, a proxy for shape size and memory footprint

### Assembly count bins (`A_BIN`)

| Bin | Assemblies |
|-----|------------|
| A1  | 3–4        |
| A2  | 5–8        |
| A3  | 9–20       |
| A4  | 21–100     |
| A5  | >100       |

### Molecular weight bins (`MW_BIN`)

| Bin | Median MW (kDa) |
|-----|-----------------|
| MW1 | <60             |
| MW2 | 60–120          |
| MW3 | 120–300         |
| MW4 | >300            |

---

## Benchmark Definition

Each complex is assigned to an `(A_BIN × MW_BIN)` cell.  
For each populated cell:

- complexes are sorted by `median_mw_kda`
- the **median complex** is selected (upper median for even counts)

All assemblies belonging to each selected complex are included.

This yields a compact, representative benchmark spanning multiple computational regimes while avoiding pathological outliers.

---

## Intended Use

This dataset is intended for:

- estimating SLURM memory and CPU requirements
- analysing scaling behaviour of shape-matching pipelines
- identifying computational stress regimes in PDBe-KB complexes

It is **not** intended for benchmarking biological or functional similarity.

---

## Notes

- All molecular weights are expressed in **kDa**
- Bin definitions may be revisited if the underlying PDBe-KB complex distribution changes
- Extreme bins are retained explicitly to support stress-testing scenarios
