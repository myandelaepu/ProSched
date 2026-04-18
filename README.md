# ProSched: Intelligent HPC Scheduling
ProSched: Closing the Loop Between Architecture-Aware Profiling and Intelligent HPC Scheduling
This repository contains the complete artifact reproduction code for the ProSched paper. All results, tables, and figures can be reproduced from publicly available ALCF job traces.

================================================================================

QUICK START
One-Click Reproduction (Recommended):

1. Go to Google Colab: https://colab.research.google.com
2. File -> Upload notebook -> Select prosched_reproduce.ipynb
3. Runtime -> Run all
4. Wait 5-10 minutes
5. All tables and figures generated automatically
Local Reproduction:
git clone https://github.com/ANONYMOUS/ProSched.git
cd ProSched
pip install -r requirements.txt
jupyter notebook prosched_reproduce.ipynb

================================================================================

WHAT THIS REPRODUCES

Tables:

Table I: Co-scheduling interference analysis (workload pair patterns)
Table II: Cluster specifications and job statistics (4 clusters)
Table III: Main results: 11 schedulers x 6 metrics (mean +/- std over 10 runs)
Table IV: Ablation study: component contributions to performance
Table V: NUMA sensitivity analysis (ProSched vs. HARMONIC)

Figures:

Figure 2: Main results bar chart (slowdown, energy, utilization, interference)
Figure 3: Scheduling latency scalability (64 to 262K nodes)
Figure 4: Energy consumption and thermal event analysis
Supplemental: Workload characterization (runtime, node, wait time distributions)

Key Claims Verified:

- ProSched achieves 30%+ slowdown reduction vs. HARMONIC
- ProSched achieves 25%+ energy reduction vs. HARMONIC
- ProSched achieves 38%+ interference reduction vs. HARMONIC
- NUMA-safe placements rise from 41% to 94%
- Scheduling latency under 50ms at 262K nodes

================================================================================

DATASET

ALCF Job Traces (Public, No Registration Required)

Cluster: Polaris
Time Period: 2024
Jobs: 241,772
Key Columns: RUNTIME_SECONDS, NODES_USED, WALLTIME_SECONDS

Cluster: Mira
Time Period: 2019
Jobs: 52,154
Key Columns: ELIGIBLE_WAIT_SECONDS, QUEUE_NAME

Cluster: Cooley
Time Period: 2019
Jobs: 95,678
Key Columns: EXIT_STATUS, CORES_USED

Cluster: Aurora
Time Period: 2025
Jobs: 735,560
Key Columns: GPUS_REQUESTED, SCIENCE_FIELD

Total raw: 1,125,164 jobs
Filtered: 389,620 jobs (exit_status=0, runtime>0, nodes>=1)

Automatic Download:

The notebook automatically downloads all four CSV.gz files from:
https://reports.alcf.anl.gov/data/index.html

No registration or authentication required.

================================================================================

REPRODUCTION INSTRUCTIONS

Method 1: Google Colab (Easiest)

Step 1: Download the notebook file (prosched_reproduce.ipynb) from this repository

Step 2: Go to https://colab.research.google.com

Step 3: File -> Upload notebook -> Select the downloaded file

Step 4: Runtime -> Run all (or press Ctrl+F9)

Step 5: Results appear in the prosched_results/ folder (left sidebar)

Colab Tips:
- Use GPU runtime: Runtime -> Change runtime type -> T4 GPU (faster)
- Results persist for the session; download before closing
- Total runtime: 5-10 minutes on CPU, 3-5 minutes on GPU

Method 2: Local Jupyter

Step 1: Clone the repository
git clone https://github.com/ANONYMOUS/ProSched.git
cd ProSched

Step 2: Create virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

Step 3: Install dependencies
pip install -r requirements.txt

Step 4: Run the notebook
jupyter notebook prosched_reproduce.ipynb

Method 3: Command Line (Headless)

jupyter nbconvert --to notebook --execute prosched_reproduce.ipynb --output prosched_reproduce_executed.ipynb

================================================================================

EXPECTED RESULTS

After running the notebook, you should see output similar to:

ProSched vs. HARMONIC:
  Slowdown improvement:     32.6%  (paper target: ~32.6%)
  Energy improvement:       28.1%  (paper target: ~28.1%)
  Utilisation improvement:  +4.4pp (paper target: ~+4.4pp)
  Interference reduction:   41.5%  (paper target: ~41.5%)

Statistical Significance:
  ProSched vs. FCFS:        p=0.0002 ***
  ProSched vs. HARMONIC:    p=0.0008 ***
  ProSched vs. GNN-RL:      p=0.0012 **

Note: Results may vary by +/-5% due to random subsampling. All 10 runs should preserve the relative ordering of baselines.

================================================================================

REPOSITORY STRUCTURE

ProSched/
├── prosched_reproduce.ipynb    # Main reproduction notebook
├── README.md                    # This file
├── requirements.txt             # Python dependencies
├── .gitignore                   # Git ignore rules
├── prosched_results/            # Generated at runtime
│   ├── table2_cluster_stats.csv
│   ├── table3_main_results.csv
│   ├── table4_ablation.csv
│   ├── table5_numa.csv
│   ├── table1_interference_analysis.csv
│   ├── fig2_main_results.pdf
│   ├── fig2_main_results.png
│   ├── fig3_scalability.pdf
│   ├── fig4_energy_thermal.pdf
│   └── workload_characterisation.pdf
└── data/                        # Optional: place downloaded CSVs here

================================================================================

REQUIREMENTS

Python Dependencies:

pandas>=2.2.0
numpy>=1.26.0
matplotlib>=3.8.0
seaborn>=0.13.0
scikit-learn>=1.4.0
scipy>=1.12.0
jupyter>=1.0.0

Install with: pip install -r requirements.txt

Hardware Requirements:

Colab (free): 12 GB RAM, 5 GB disk, GPU optional, 5-10 min
Local (min): 8 GB RAM, 2 GB disk, no GPU needed, 10-15 min
Local (full): 16 GB RAM, 5 GB disk, GPU recommended, 5-8 min

Software Requirements:

- OS: Linux, macOS, or Windows (WSL2 recommended for Windows)
- Python: 3.10 or higher
- Browser: Any modern browser for Colab

================================================================================

SCHEDULER IMPLEMENTATIONS

The notebook includes all 11 scheduling algorithms from the paper:

Classical Schedulers:
- FCFS (First-Come-First-Served)
- SJF (Shortest Job First)
- EASY Backfilling

ML-based Schedulers:
- DeepRM
- Decima
- DRAS
- MRSch
- GARLSched
- GNN-RL

Advanced Schedulers:
- HARMONIC
- ProSched (full implementation)

Each scheduler is implemented as a trace-driven simulator with the same interface.

================================================================================

OUTPUT FILES DESCRIPTION

CSV Files (Tables):

table2_cluster_stats.csv - Job counts, median runtime, nodes, slowdown per cluster
table3_main_results.csv - All 11 schedulers x slowdown, energy, utilization, interference
table4_ablation.csv - ProSched variants (no GNN, no uncertainty, etc.)
table5_numa.csv - NUMA-safe percentage comparison
table1_interference_analysis.csv - Workload pair interference patterns

Figure Files:

PDF format - Publication-quality vector graphics
PNG format - Preview images for quick viewing

================================================================================

TROUBLESHOOTING

Common Issues and Solutions:

Issue: "File not found" for datasets
Solution: Notebook auto-downloads; check your internet connection

Issue: Memory error in Colab
Solution: Reduce SUBSAMPLE parameter (line ~180) from 0.30 to 0.15

Issue: Results don't match paper exactly
Solution: Expected +/-5% variation; increase N_RUNS or SUBSAMPLE

Issue: Colab disconnects
Solution: Use free GPU runtime; results save to prosched_results/

Issue: Slow execution
Solution: Enable GPU: Runtime -> Change runtime type -> T4 GPU

Verifying Correct Execution:

Run this check after notebook completion:

import pandas as pd
df = pd.read_csv('prosched_results/table3_main_results.csv')
ps_row = df[df['System'] == 'ProSched']
print(f"ProSched slowdown: {ps_row['sl_raw'].values[0]:.2f}")

Expected output: 1.26 +/- 0.11

================================================================================

CITATION

If you use this code or reproduce these results, please cite:

@inproceedings{prosched2026,
  title={ProSched: Closing the Loop Between Architecture-Aware Profiling and Intelligent HPC Scheduling},
  author={Anonymous for Double-Blind Review},
  booktitle={Proceedings of the International Conference for High Performance Computing, Networking, Storage, and Analysis (SC26)},
  year={2026}
}

Note: Author information will be updated upon publication.

================================================================================

LICENSE

This project is licensed under the MIT License.

================================================================================

ACKNOWLEDGMENTS

- ALCF for providing public job trace data
- The open-source scientific computing community

================================================================================

CONTACT

Questions? Open an issue on this repository.

Status: Complete artifact - reproduces all paper results
