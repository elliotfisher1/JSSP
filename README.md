# JSSP
# Job Scheduler ACO

**Comprehensive Ant‑Colony Optimisation for Single‑Machine Sequencing**

This repository implements a world‑class job scheduler using Ant‑Colony Optimisation (ACO). It sequences a list of manufacturing jobs on a single machine, balancing change‑over costs, due‑date pressure, and profitability.

---

## 🚀 Features

* **Static Change‑Over Penalties**

  * Raw material swaps (`raw material`)
  * Tooling changes (`material`)
  * Inner diameter adjustments (`ID`)
* **Dynamic Due‑Date Penalties**

  * 5‑working‑day buffer, linear ramp, quadratic overdue cap
  * Business‑day awareness via NumPy’s `busday_count`
* **Profitability Heuristic Toggle**

  * Off / favour high‑profit (£ per minute) / favour low‑profit
  * Exponent (`ATTR_EXP`) to adjust pull strength
* **Full Exports & Visuals**

  * **Best Schedule CSV**: simple sequence output
  * **Detailed Schedule CSV**: start time, finish time, lateness per job
  * **Single‑Row Gantt Chart**: visual timeline of jobs back‑to‑back
  * **Convergence Plot**: best cost vs. iteration
  * **Pheromone Heatmap**: final pheromone levels across job pairs

---

## 🛠️ Prerequisites

* Python 3.8+
* Install dependencies:

  ```bash
  pip install numpy pandas matplotlib
  ```
* Your CSV (`vrp01001.csv`) must include **exactly** these columns (case‑sensitive):
  `id`, `quantity`, `material`, `raw material`, `ID`, `time_minutes` (per piece), `price_gbp` (per job), `DUE DATE` (format `DD/MM/YYYY`)

---

## 🔧 Installation & Setup

1. **Clone** this repository:

   ```bash
   git clone <repo-url>  
   cd job-scheduler-aco  
   ```
2. **Edit** the top‑of‑file toggles in `scheduler.py` to suit your environment:

   ```python
   CSV_FILE        = '~/Desktop/vrp01001.csv'  
   BEST_SCHED_CSV  = '~/Desktop/best_schedule.csv'  
   # ... other directory paths  
   ```
3. **Adjust penalties & heuristics** in the script header:

   ```python
   RAW_MATERIAL_PENALTY = 1000.0  
   MATERIAL_PENALTY     = 100.0  
   ID_PENALTY           = 50.0  
   DUE_BUFFER_WORKDAYS  = 5  
   PROFIT_MODE          = 1  # 0=off,1=high-profit,2=low-profit  
   ATTR_EXP             = 1.0  
   # ... ACO hyper‐parameters  
   ```

---

## ▶️ Usage

Run the scheduler:

```bash
python scheduler.py  
```

This will generate:

* `best_schedule.csv`
* `detailed_schedule.csv`
* `schedule_gantt.png`
* `convergence.png`
* `pheromone_heatmap.png`

---

## 📂 Output Files Overview

| File                    | Contents                                                                |
| ----------------------- | ----------------------------------------------------------------------- |
| `best_schedule.csv`     | Sequence index + job fields                                             |
| `detailed_schedule.csv` | Sequence, ID, start‑time, finish‑time, elapsed, due date, lateness days |
| `schedule_gantt.png`    | Single‑row Gantt of job timeline                                        |
| `convergence.png`       | ACO cost convergence over iterations                                    |
| `pheromone_heatmap.png` | Final pheromone matrix heatmap                                          |

---

## 📈 Where to Take It Further

1. **Multi‑Machine Scheduling**

   * Cluster jobs by tool family or resource, then balance loads.
   * Or encode nodes as `(machine, job)` for assignment + sequencing.

2. **Interactive Dashboard**

   * Streamlit or Panel: upload CSV, real‑time sliders for penalties, live Gantt & convergence.

3. **Parameter Auto‑Tuning**

   * Integrate Optuna/Hyperopt to optimize `ALPHA, BETA, RHO, ATTR_EXP` on historical data.

4. **Smart Calendars**

   * Use holiday/shift calendars via `pandas.tseries.offsets`.

5. **Hybrid Meta‑Heuristics**

   * Seed a GA with ACO tours, or apply local search (2‑opt) each generation.

6. **Performance Boosts**

   * JIT‑compile core loops with Numba.
   * Offload to Rust/Cython for very large job sets.

7. **Explainability & Analytics**

   * Pareto chart of penalty contributions (raw vs. tooling vs. ID vs. lateness).
   * Shapley‑value analysis on penalty factors.

8. **Testing & CI**

   * Unit tests for penalty functions, heuristic builder, and deterministic samples.
   * GitHub Actions pipeline for smoke tests on a sample CSV.

---

## 💡 Pro Tips

* **Version Parameter Sets**: keep JSON snapshots alongside git tags.
* **Traceability**: log input, parameters, and summary JSON per run.
* **Modularize**: split penalties, ACO core, and exports into separate modules.
* **Visual Sanity Checks**: review Gantt after tweaks—visual errors show faster than CSV diffs.

---

## 🧪 Testing Checklist

* [ ] `compute_changeover_penalty` sums correctly for known job pairs.
* [ ] `days_until_due_workdays` skips weekends properly.
* [ ] `due_date_penalty` is zero above buffer, linear inside buffer, quadratic overdue.
* [ ] `job_attractiveness` uses `time_minutes * quantity`.
* [ ] Single‑row Gantt shows sequential jobs with no overlap.

---

## 📄 License

Released under the MIT License. See `LICENSE` for details.

---

**Happy scheduling!**
