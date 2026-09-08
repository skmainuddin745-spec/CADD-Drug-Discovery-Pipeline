# CADD Drug Discovery — Computer-Aided Drug Design Workflows

> **Molecular docking, molecular dynamics, and QSAR studies on therapeutically relevant protein targets including SARS-CoV-2 Mpro (6LU7), Cytochrome P450, Myoglobin, and peptide-receptor complexes.**

---

## Overview

This repository collects the Computer-Aided Drug Design (CADD) workflow scripts and analysis tools developed during structured learning and original research projects at the **Research Group for Rational Chemistry (RGRC)**.

---

## Projects

---

### 1. Mpro (6LU7) — Covalent Docking Study

**Target:** SARS-CoV-2 Main Protease (PDB: 6LU7)  
**Ligand:** Peptide-mimetic covalent inhibitor  
**Method:** Induced-fit docking (Schrödinger Glide IFD) + MD validation (GROMACS)

```
Workflow:
  Protein prep → Glide grid generation → IFD → MM-GBSA rescoring
  → Top pose → 100 ns MD → RMSD/RMSF/MM-PBSA
```

**Key result:** Top-ranked pose shows stable covalent bond (SG···C distance: 1.85 Å) maintained throughout 100 ns MD.

---

### 2. Cytochrome P450 — Ligand Binding Study

**Target:** CYP3A4 (PDB: 4EY7 derivative)  
**Ligand:** BOAT04000 (therapeutic candidate COMB-00004)  
**Method:** Glide SP/XP docking + Desmond MD

```
Protocol:
  - OPLS3e force field parametrisation
  - 200 ns Desmond NPT production run
  - RDF analysis: heme Fe·ligand coordination
  - RMSD, RMSF, protein-ligand contacts timeline
```

**Key result:** Ligand maintains coordination to Fe(III) heme iron (Fe–N distance: 2.12 ± 0.08 Å).

---

### 3. PEG-400 in Water — 200 ns Desmond MD

**System:** PEG-400 + urea + water (explicit solvent)  
**Engine:** Desmond (Schrödinger)  
**Analysis:** Multi-RDF study (8 pair combinations)

Shell scripts automate RDF calculations:

```bash
# rdf_8.sh — RDF for PEG end group / water oxygen
JOBNAME=rdf_8
$SCHRODINGER/run trj_analysis.py \
  -trj desmond_md_job_3_trj \
  -analysis rdf \
  -asl1 "res. PEG and atom.ptype C2" \
  -asl2 "res. T3P and atom.ptype OT" \
  -r1 0.1 -r2 1.5 -dr 0.01 \
  -o ${JOBNAME}.dat
```

**Output:** 8 RDF profiles characterising PEG–water, PEG–urea, and urea–water interactions.

---

### 4. 6MOJ — Peptide Receptor Complex MD

**Target:** PDB: 6MOJ (GPCR-peptide complex)  
**Method:** GROMACS MD, AMBER99SB-ILDN force field  
**Analysis:** Protein-peptide binding energy (MM-PBSA), H-bond occupancy

---

## Analysis Scripts

| File | Description |
|------|-------------|
| `rdf_*.sh` | Desmond RDF analysis submission scripts (8 RDF pairs) |
| `*.html` | Desmond/Glide HTML reports (docking scores, contact maps) |

---

## CADD Toolbox

| Tool | Version | Purpose |
|------|---------|---------|
| Schrödinger Suite | 2021+ | Protein prep, docking, Desmond MD |
| GROMACS | 2022 | Production MD |
| AMBER | 20 | MM-PBSA energy decomposition |
| AutoDock Vina | 1.2 | Fast flexible docking |
| PyMOL | 2.5 | Structural visualisation |
| LigPlus | 2.1 | 2D protein-ligand interaction maps |
| Avogadro / MarvinSketch | — | Ligand drawing, protonation |
| Open Babel | 3.1 | File format conversion |

---

## Molecular Dynamics Parameters

```
Ensemble:     NPT (T = 300 K, P = 1 bar)
Thermostat:   V-rescale (τ = 0.1 ps)
Barostat:     Parrinello-Rahman (τ = 2 ps)
Electrostatics: PME (rc = 1.2 nm)
VdW:          Lennard-Jones (rc = 1.2 nm)
Timestep:     2 fs
Output:       500 ps (coordinates), 10 ps (energy)
```

---

## References

1. Jin et al., *Science* **2020**, 368 — Mpro structure (6LU7)
2. Pettersen et al., *J. Comput. Chem.* **2021** — UCSF ChimeraX
3. Schrödinger, LLC — Desmond, Glide, Maestro
4. GROMACS 2022 documentation

---

*CADD · Molecular Docking · Molecular Dynamics · Drug Discovery · Bioinformatics*
