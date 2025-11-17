#  Drop-Test Pipeline for Selection Analysis

Automated pipeline for testing **robustness in gene-level selection analyses** by dropping one or more foreground taxa from FASTA alignments, pruning corresponding phylogenetic trees, and re-running **HyPhy** (BUSTED or RELAX models).  

This workflow validates whether signals of **intensified or relaxed selection** remain significant after removing SPECIES OF INTEREST — providing a quantitative check on how much statistical signal is derived from PERVASIVE selection across the tree vs selection predicted in your foreground taxa. Interpretation: A negatige result indicates selection is not pervasive across the tree when focal species are removed (which indicates a robust *real* signal of selection in focal species. 

---

<details>
<summary>🧭 <b>Pipeline Overview</b></summary>

| Step | Purpose | Input | Output |
|------|----------|--------|---------|
| **1. File Selection** | Copy selected FASTA files into a working directory. | `DropTestGenes.txt` (list of target genes) | New directory with genes flagged as P < 0.05 by BUSTED/RELAX |
| **2. Foreground Drop** | Remove specified taxa from alignments. | FASTA + taxa list | Cleaned FASTAs in new subdirectory |
| **3. Tree Pruning** | Rewrite tree files to match dropped FASTAs. | Master tree + taxa list | Pruned trees matching new alignments |
| **4. Re-analysis** | Run **HyPhy BUSTED (HF)** on FASTA/tree pairs. | Updated FASTA + tree files | Refined selection-model results |

</details>

---

<details>
<summary>⚙️ <b>Step-by-Step Instructions</b></summary>

### **STEP 1 – Select and Copy Input Files**

Scripts: `GetFiles.sh`, `DropTestGenes.txt`  

- `DropTestGenes.txt` → one gene ID or filename prefix per line.  
- These should correspond to genes with **P < 0.05** from prior BUSTED or RELAX analyses.  
- The script copies matching files into a new working directory `DropTestFiles/`.

---

### **STEP 2 – Drop Foreground Taxa from FASTA Files**

Scripts: `DropSpecies_fastafiles.sh`, `DropSpecies_fastafiles.py`  

- Provide a plain-text list of taxa to remove (e.g., `ForegroundTaxa.txt`).  
- The Python script removes those sequences, verifies alignment integrity, and writes results to `Dropped_FASTAs/`.  
- Generates logs summarizing taxa removed and sequence counts.

---

### **STEP 3 – Rewrite Matching Tree Files**

Scripts: `WriteTrees.sh`, `writeTrees.py`  

- Requires a **master tree file** containing all taxa.  
- Automatically prunes the tree to match each new FASTA alignment.  
- Outputs are stored in `Dropped_Trees/`.

---

### **STEP 4 – Re-run HyPhy BUSTED (HF Mode)**

Script: `dropBUSTED_HF.sh`  

- Loops over each FASTA/tree pair and executes **HyPhy BUSTED** in high-throughput (HF) mode.  
- Produces JSON result files summarizing likelihood-ratio tests for selection.  
- Compare these to the original results to assess robustness.

</details>

---

<details>
<summary>🗂️ <b>Example Directory Layout</b></summary>
project/
│
├── DropTestGenes.txt
├── ForegroundTaxa.txt
├── GetFiles.sh
├── DropSpecies_fastafiles.sh
├── DropSpecies_fastafiles.py
├── WriteTrees.sh
├── writeTrees.py
├── dropBUSTED_HF.sh
│
├── DropTestFiles/
│ ├── GeneA.fasta
│ ├── GeneB.fasta
│ └── ...
│
├── Dropped_FASTAs/
│ ├── GeneA_dropped.fasta
│ └── ...
│
└── Dropped_Trees/
├── GeneA_dropped.tree
└── ...

<summary>💡 <b>Best Practices</b></summary>

- Foreground taxa names **must match** FASTA headers exactly.  
- Keep an unmodified copy of your **master tree** for reproducibility.  
- Recommended: run BUSTED jobs as a **SLURM array** for efficiency.  
- Scripts are modular — substitute or extend individual steps as needed.  
- Each script writes logs for transparency and debugging.  

</details>

---

<details>
<summary>🧠 <b>Conceptual Summary (for Non-Biologists)</b></summary>

This pipeline functions as a **sensitivity analysis** for evolutionary models.  
By selectively removing species and re-running model fits, we test whether observed signals of adaptive evolution are:

- **Robust biological phenomena**, or  
- **Artifacts of model structure or taxon sampling.**

Data-science parallels:  
- Dropping features to test feature importance.  
- Running A/B tests on subsets to confirm generalization.  

</details>

---

<details>
<summary>📚 <b>Dependencies</b></summary>

- Python ≥ 3.8  
- HyPhy (BUSTED / RELAX modules)  
- Standard UNIX utilities (`bash`, `awk`, `sed`)  
- SLURM or similar HPC scheduler (recommended)  

</details>

---

<details>
<summary>✍️ <b>Author</b></summary>

**Danielle H. Drabeck, Ph.D.**  
University of Minnesota – Ecology, Evolution & Behavior  
📧 drabe004@umn.edu | 🐙 [github.com/drabe004](https://github.com/drabe004)

</details>
