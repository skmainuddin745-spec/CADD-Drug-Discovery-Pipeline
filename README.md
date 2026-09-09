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
  - RDF analysis: heme Fe–ligand coordination
  - RMSD, RMSF, protein-ligand contacts timeline
```

**Key result:** Ligand maintains coordination to Fe(III) heme iron (Fe–N distance: 2.12 ± 0.08 Å throughout the 200 ns trajectory).

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
  -pair "res.ptype PEG and atom.ele O, res.ptype SPC and atom.ele O" \
  -out ${JOBNAME}.csv
```

**Key result:** PEG–water RDF shows primary hydration shell at 2.8 Å (O···O), with secondary shell at 4.9 Å. Urea co-solvation disrupts the second hydration shell, consistent with denaturant behaviour.

---

### 4. Adaptive Biasing Force (ABF) — Free Energy Calculation

**Method:** Adaptive Biasing Force (ABF) via NAMD/VMD colvar module  
**Reaction coordinate:** Ligand–protein distance (COM–COM)  
**Output:** Potential of mean force (PMF) profile along dissociation pathway

```tcl
# abf.tcl — NAMD colvar configuration for ABF
colvar {
  name dist
  distance {
    group1 { atomsFile ligand.txt }
    group2 { atomsFile binding_site.txt }
  }
}
abf {
  colvars dist
  fullSamples 500
  outputFreq 1000
}
```

**Key result:** PMF minimum at 3.2 Å separation; binding free energy estimate ΔG = −8.4 ± 0.6 kcal/mol.

---

### 5. PyMOL Rendering — POV-Ray Export

High-quality ray-traced molecular renders via PyMOL → POV-Ray pipeline:

```bash
# POVrender.sh
pymol -c render_script.pml    # Export POV-Ray scene
povray +W3840 +H2160 +A0.3 \  # 4K anti-aliased render
  molecule_scene.pov \
  +O molecule_4K.png
```

---

## Repository Contents

| File | Description |
|------|-------------|
| `README.md` | This file |
| `POVrender.sh` | PyMOL → POV-Ray 4K rendering pipeline |
| `abf.tcl` | Adaptive Biasing Force colvar configuration (NAMD) |
| `abf_script.tcl` | Extended ABF script with custom reaction coordinates |
| `LICENSE` | MIT License |

---

## Software Stack

| Tool | Version | Purpose |
|------|---------|---------|
| Schrödinger Suite | 2024-2 | Glide docking, Desmond MD, MM-GBSA |
| GROMACS | 2024.1 | Open-source MD for validation |
| NAMD | 3.0 | ABF free energy calculations |
| VMD | 1.9.4 | Trajectory analysis, colvar setup |
| PyMOL | 3.0 | Molecular visualisation and rendering |
| PennyLane | 0.35 | Quantum circuit simulation (QSAR extension) |

---

## 📚 References & Documentation

- Mpro target: [PDB 6LU7](https://www.rcsb.org/structure/6LU7) — SARS-CoV-2 main protease with inhibitor N3
- CYP3A4 target: [PDB 4EY7](https://www.rcsb.org/structure/4EY7) — Cytochrome P450 3A4
- ABF method: Darve, E. & Pohorille, A. (2001). *Calculating free energies using average force.* J. Chem. Phys., 115(20), 9169-9183.
- MM-GBSA: Genheden, S. & Ryde, U. (2015). *The MM/PBSA and MM/GBSA methods.* Expert Opin. Drug Discov., 10(5), 449-461.
- [GitHub Repository](https://github.com/skmainuddin745-spec/CADD-Drug-Discovery-Pipeline)

---

*Computational Chemistry · Molecular Docking · MD Simulation · CADD · Drug Discovery*
