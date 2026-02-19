# Storage–Yield–Reliability Curves (Reservoir Design)

This project generates **Storage–Yield–Reliability** curves for a reservoir using a historical monthly inflow time series (Set 1).

The input data are monthly river discharges (m³/s). They are converted to monthly inflow volumes (m³/month), then a reservoir simulation is performed to estimate **time-based reliability**. For each storage capacity $K$, the code finds the **maximum constant yield** $Y$ that satisfies a target reliability level.

---

## Method Overview

### 1) Data
- File: `WRSAM-Data.xls`
- Sheet: `Headflow`
- Monthly columns (in order):  
  `MEH, ABA, AZA, DEY, BAH, ESF, FAR, ORD, KHO, TIR, MOR, SHA`

---

### 2) Unit Conversion (m³/s → m³/month)

Each monthly discharge $Q$ (m³/s) is converted to monthly volume $V$ (m³/month):

$$
V = Q \times (\text{seconds in month})
$$

Persian calendar month lengths are assumed:

- Mehr to Bahman: 30 days  
- Esfand: 29 days  
- Farvardin to Shahrivar: 31 days  

---

### 3) Reservoir Mass Balance (Monthly Simulation)

For each month:

Available water:

$$
A_t = S_t + V_t
$$

Release (yield supplied):

$$
R_t = \min(Y, A_t)
$$

Updated storage:

$$
S_{t+1} = \min(K, A_t - R_t)
$$

Where:

- $S_t$ = storage (m³)  
- $K$ = storage capacity (m³)  
- $Y$ = constant yield (m³/month)  
- $V_t$ = inflow volume during month $t$  

---

### 4) Reliability Definition (Time-Based)

Time-based reliability is computed over months:

$$
R = 1 - \frac{N_f}{N}
$$

Where:

- $N_f$ = number of failure months  
- $N$ = total number of months  

A month is considered a **failure** if the reservoir cannot fully supply $Y$:

$$
R_t < Y
$$

---

### 5) Reducing Initial Condition Effects

To reduce dependence on the initial storage condition:

- The inflow record is repeated **3 times**
- Reliability is evaluated only on the **middle cycle**
- The reservoir starts full ($S = K$)

---

### 6) Finding Maximum Yield for a Given Reliability

For each storage capacity $K$, the maximum yield $Y$ meeting the target reliability is found using the **bisection method**.

---

## Reliability Levels

The code generates curves for:

- 100%
- 95%
- 90%
- 80%

---

## Output

A plot is produced where:

- **X-axis:** Storage Capacity, $K$ (m³)  
- **Y-axis:** Constant Yield, $Y$ (m³/month)  

Each curve corresponds to a different reliability level.

---

## Requirements

Install the required libraries:

```bash
pip install pandas numpy matplotlib xlrd openpyxl
