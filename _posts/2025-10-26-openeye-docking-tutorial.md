---
layout: single
title: "OpenEye FRED Docking Tutorial Workflow"
date: 2025-10-26
excerpt: "A concise, step-by-step tutorial for performing virtual screening with OpenEye's FRED docking software, covering receptor and ligand preparation using PDB and Omega2."
tags: [openeye, fred, docking, virtual screening, molecular modeling, bioinformatics]
---

## 1. Prepare the Receptor

1. **Download a PDB file with a bound ligand**
    - Obtain the target protein structure from RCSB PDB (https://**www.rcsb.org**).
    - Make sure the structure contains a co-crystallized ligand (HETATM records).
2. **Generate the receptor file (`.oedu`)**
    - Use the **make_receptor** GUI application from OpenEye (currently GUI-only).

### Output

- `rec_site1.oedu`: the receptor definition file required by FRED for docking.

## 2. Prepare the Ligands

1. **Combine and compress SDF files**
    
    ```bash
    cat sdf_folder/*.sdf | gzip > enamine_all.sdf.gz
    ```
    
    - Merges all `.sdf` files in a folder into a single compressed file.
2. **Generate 3D conformations**
    
    ```bash
    omega2 -in enamine_all.sdf.gz -out enamine_all_3d.oeb.gz
    ```
    
    - `omega2` generates 3D conformers for all ligands.
    - The `.oeb.gz` output is directly compatible with FRED docking.

### Output

- `enamine_all_3d.oeb.gz`: the ligand database with 3D conformers.

---

## 3. Run Docking (FRED)

### Basic Command

```bash
fred -mpi_np 4 \
     -receptor rec_site1.oedu \
     -dbase enamine_all_3d.oeb.gz \
     -docked_molecule_file enamine_docked.sdf.gz
```

### Parameter Explanation

- `mpi_np 4`: runs with 4 parallel processes (MPI cores).
- `receptor`: input receptor file (`.oedu`).
- `dbase`: input ligand database (`.oeb.gz`).
- `docked_molecule_file`: output file for docked poses.
- *(Optional)* `hitlist_size 100`: limit to top 100 results.
- *(Optional)* `conftest none`: disable conformer filtering.

### Output

- `enamine_docked.sdf.gz`: contains docked poses and scores.
