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

```bash
GetFiles.sh
DropTestGenes.txt
