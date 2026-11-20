# Computational checks for truck-multi-drone delivery

Companion notebook for **Algorithms for Collaborative Truck-Multi-Drone Delivery in the En-Route Model**, by Jerry Mathew Oommen and Kelin Luo, University at Buffalo.

The notebook [computational_checks.ipynb](computational_checks.ipynb) reproduces the paper's numerical examples. It prints delivery coordinates, launch windows, and schedule traces, and plots the compared schedules. All instance data are included in the notebook; no external dataset is required.

## Examples

| Example | Instance | Expected result |
| --- | --- | --- |
| Maximum coverage | Nine requests, two drones | Sequential Greedy serves five requests; two feasible schedules serve all nine, giving ratio `5/9`. |
| Minimum fleet size | Five requests, optimum fleet size two | Sequential Greedy uses four drones, attaining the integer bound at these parameters and ratio `2`. |
| Case 1, Section 3.1 | Two requests | Both orders are feasible, but the order by increasing x-coordinate finishes at `0.279361`, later than `0.264241` for the reversed order. |
| Case 2A, Appendix A | Three requests | An unrestricted order serves all three; the restriction to increasing x-coordinate permits only two. |
| Case 2B, Appendix A | Four requests | Both orders of the middle pair are feasible, but putting them in increasing x-coordinate delays completion and misses the final launch window. Unrestricted coverage is four; coverage in increasing x-coordinate is three. |

The notebook also checks that all three counterexample instances are proper: no launch window contains another, and no delivery location lies in another delivery's launch triangle.

## Run the notebook

The computational cells have been checked with Python **3.12.14**, pandas **2.2.3**, Matplotlib **3.11.2**, and IPython **9.18.1**.

With Python 3.12 installed, run the following on macOS or Linux:

```bash
git clone https://github.com/JerryMathew07/Truck_Drone_Paper.git
cd Truck_Drone_Paper
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install jupyterlab ipykernel pandas==2.2.3 matplotlib==3.11.2 ipython==9.18.1
python -m jupyterlab computational_checks.ipynb
```

On Windows, create the environment with `py -3.12 -m venv .venv` and activate it in PowerShell with `.\.venv\Scripts\Activate.ps1`, then use the same installation and launch commands.

Select the environment's Python kernel and run all cells from top to bottom. A successful run executes nine code cells and produces five `PASS` messages, tables, and five schedule plots. Assertions stop execution if an expected check fails. Saved outputs are also included in the notebook.

## Model and calculations

The truck starts at time zero and moves along the positive x-axis at unit speed. Each drone has speed `v > 1` and flight range `R`. Each flight serves one delivery request before returning to the moving truck; recharge and handling times are zero.

The notebook implements launch-window and return-time calculations, earliest-return Greedy, Sequential Greedy, fixed-order feasibility checks, and properness checks. The checks use floating-point arithmetic with `TOL = 1e-10`; displayed tables are rounded. The paper supplies the formal proofs of the approximation guarantees.

The launch-window geometry and single-drone Greedy algorithm follow Krizanc et al., [The En Route Truck-Drone Delivery Problem](https://arxiv.org/abs/2402.00829).
