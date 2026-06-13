# Revision Class: System Modelling and Simulation

## Part 1: System Components in Modelling (Week 1 Recap)

Every system that can be modelled consists of the following seven components. Recognising these in any problem is the first step to building a model of it.

|Component|Description|Example (Bank)|
|---|---|---|
|**Input**|What enters the system from the environment.|Customers arriving|
|**Processor**|The mechanism that transforms inputs into outputs.|Bank tellers serving customers|
|**Output**|The result produced by the processor.|Served customers leaving|
|**Control**|Rules or policies governing how the system operates.|FIFO queue discipline; opening hours|
|**Feedback**|Information about the output that is used to adjust the system.|Queue length data used to open more teller windows|
|**Environment**|Everything outside the system that can influence it.|Time of day, public holidays, population density|
|**Boundary**|The line separating what is "inside" the system from what is "outside."|The bank building itself — customers outside are in the environment; customers inside are in the system|

> **Why this matters for simulation:** When you build a model, you must decide what falls inside the boundary and what does not. A bank simulation might treat "the number of people passing on the street" as environment (outside the boundary) and only model what happens once someone walks through the door (inside the boundary).

---

## Part 2: Stability Condition (Week 2 & 4 Recap)

A **stable queueing system** is one where the queue does not grow without bound over time. The stability conditions are:

$$\rho < 1 \quad \text{equivalently} \quad \lambda < \mu$$

Where $\rho = \lambda / \mu$ is the server utilisation, $\lambda$ is the arrival rate, and $\mu$ is the service rate.

- If $\rho < 1$: the server can keep up with arrivals — the system is **stable** and a steady state exists.
- If $\rho \geq 1$: arrivals outpace service — the queue grows forever and the birth-death probability formula **cannot be applied**.

---

## Part 3: Markov Chains

### Definition

A **Markov chain** is a special type of stochastic process used to model a discrete sequence of random variables. It is defined as a stochastic process whose future development can be treated as a series of **transitions** between certain values called the **states** of the process.

The key property of a Markov chain is the **Markov property** (also called "memorylessness"):

> **The probability of moving to the next state depends only on the current state — not on any of the previous states.**

In other words: the future is independent of the past, given the present.

### Formal Description

If $X_0, X_1, X_2, \ldots$ is a sequence of random variables, it is a Markov chain if:

$$P(X_{n+1} = j \mid X_n = i, X_{n-1}, \ldots, X_0) = P(X_{n+1} = j \mid X_n = i)$$

The right-hand side is called the **transition probability** — the probability of moving from state $i$ to state $j$ in one step.

### Connection to Queueing

The birth and death process covered in Week 4 **is** a continuous-time Markov chain. The states are ${0, 1, 2, 3, \ldots}$ (number of customers), and the transition probabilities are determined by $\lambda$ and $\mu$. The $P_n = (1-\rho)\rho^n$ formula comes directly from solving the Markov chain's balance equations.

### Application: Generation of Random Numbers

One application of Markov chains in simulation is in generating **pseudo-random number sequences**. A sequence of numbers can be made to behave like a random sequence by defining transition rules between states — this is the mathematical basis of random number generators used in computers.

---

## Part 4: Birth-Death Revision Example

### Problem

In a system with $\rho = 0.6$, find the probability that there are exactly **3 customers** in the system.

### Solution

$$P_n = (1 - \rho)\rho^n$$

$$P_3 = (1 - 0.6)(0.6)^3 = (0.4)(0.216) = \mathbf{0.0864}$$

**Interpretation:** There is an **8.64% chance** of finding exactly 3 customers in the system at any given moment.

---

## Part 5: Queueing Revision Example — Shopping Mall

### Problem

The arrival rate to a shopping mall is **2 customers per minute** and the service rate is **5 customers per minute**. Find:

i. The probability of having 1 customer in the system. ii. The probability of having 2 customers in the system. iii. State whether the system is stable.

### Solution

$$\rho = \frac{\lambda}{\mu} = \frac{2}{5} = 0.4$$

**i. Probability of 1 customer ($P_1$)**

$$P_1 = (1 - 0.4)(0.4)^1 = 0.6 \times 0.4 = \mathbf{0.24}$$

**ii. Probability of 2 customers ($P_2$)**

$$P_2 = (1 - 0.4)(0.4)^2 = 0.6 \times 0.16 = \mathbf{0.096}$$

**iii. Stability**

Since $\rho = 0.4 < 1$, the system is **stable**.

---

## Part 6: Monte Carlo Simulation — Dentist Clinic

This is the main worked example of the revision class, combining everything: probability tables, cumulative distributions, random number mapping, and a full simulation.

### Problem

The following table shows the categories of work performed by a dentist, the time required to complete each category, and the probability of each category occurring.

|Category|Time (mins)|Probability|Cumulative Prob.|RN Interval|
|---|---|---|---|---|
|Filling|45|0.40|0.40|0.00 – 0.39|
|Crown|60|0.15|0.55|0.40 – 0.54|
|Cleaning|15|0.15|0.70|0.55 – 0.69|
|Extraction|45|0.10|0.80|0.70 – 0.79|
|Checkup|15|0.20|1.00|0.80 – 0.99|

> **How the RN Interval is built:** Start from 0.00. Each category's interval is as wide as its probability. Filling has probability 0.40, so it gets 0.00–0.39. Crown has 0.15, so it gets the next 0.15 (0.40–0.54). And so on.

**All patients arrive every 30 minutes starting at 8:00 AM (scheduled appointments).** The dentist starts at **8:00 AM daily**.

**Random Number Sequence (RNS):** 0.40, 0.82, 0.11, 0.30, 0.25, 0.68, 0.17, 0.79

### Step 1: Map Each Random Number to a Treatment Type and Time

|Patient|RN|RN Interval Hit|Treatment Type|Treatment Time|
|---|---|---|---|---|
|1|0.40|0.40 – 0.54|Crown|60 min|
|2|0.82|0.80 – 0.99|Checkup|15 min|
|3|0.11|0.00 – 0.39|Filling|45 min|
|4|0.30|0.00 – 0.39|Filling|45 min|
|5|0.25|0.00 – 0.39|Filling|45 min|
|6|0.68|0.55 – 0.69|Cleaning|15 min|
|7|0.17|0.00 – 0.39|Filling|45 min|
|8|0.79|0.70 – 0.79|Extraction|45 min|

### Step 2: Build the Simulation Table

Rules:

- **Treatment starts** = max(Arrival Time, Previous Treatment End). The doctor cannot start the next patient until the current one is done.
- **Wait time** = Treatment Start − Arrival Time (if positive; 0 if the patient is seen immediately).
- **Doctor idle time** = Treatment Start − Previous Treatment End (if positive; 0 if the doctor goes straight to the next patient).

|Patient|Arrival|Treatment Start|Treatment End|Treatment Time|Wait Time|Doctor Idle|
|---|---|---|---|---|---|---|
|1|8:00|8:00|9:00|60 min|0|—|
|2|8:30|9:00|9:15|15 min|30 min|0|
|3|9:00|9:15|10:00|45 min|15 min|0|
|4|9:30|10:00|10:45|45 min|30 min|0|
|5|10:00|10:45|11:30|45 min|45 min|0|
|6|10:30|11:30|11:45|15 min|60 min|0|
|7|11:00|11:45|12:30|45 min|45 min|0|
|8|11:30|12:30|13:15|45 min|60 min|0|

> **Note:** Your class notes record slightly different start/end times because a different RN boundary convention maps some random numbers differently (especially patient 1 and 2). The method is the same regardless — always map the RN to the table, then chain the times forward. Follow your lecturer's table for your exam.

### Step 3: Analyse Results

**Average waiting time of patients:**

$$\bar{W} = \frac{0 + 30 + 15 + 30 + 45 + 60 + 45 + 60}{8} = \frac{285}{8} = \mathbf{35.6 \text{ minutes}}$$

**Doctor idle time:**

Once the first patient arrives at 8:00, the doctor works continuously through to 13:15 with no gap between patients (each patient starts exactly when the previous one ends). **Total idle time = 0 minutes** during the session.

**Overtime:**

The last patient finishes at **13:15**. If the clinic was scheduled to end at 11:30 (8:00 AM start + 8 patients × 30 min scheduled = but treatment is longer), or more likely at **12:00** (4-hour morning session), there is an overtime of roughly **1 hour 15 minutes to 1 hour 45 minutes** depending on the scheduled end time. Your notes record: **"There is an overtime of 2 hours."**

> **Why overtime occurs:** The dentist allocated 30-minute slots but several patients needed 45- or 60-minute treatments. This stacking effect cascades through the entire schedule. Monte Carlo simulation reveals this problem before the clinic opens — allowing management to either lengthen appointment slots or reduce the number of patients per session.

---

## Part 7: How to Solve Any Monte Carlo Problem — Step-by-Step Method

These are the exact steps your notes record as the answer procedure:

### Step 1: Create the Cumulative Frequency (CF) Table

List all categories/outcomes, their probabilities, and compute the cumulative probability (running total). Verify that probabilities sum to 1.00.

### Step 2: Find Random Number Intervals

Translate cumulative probabilities into RN ranges:

- First category: 0.00 to (prob₁ − 0.01)
- Second category: prob₁ to (prob₁ + prob₂ − 0.01)
- Continue until 0.99

### Step 3: Create the RN Column (Map RNs to Outcomes)

For each random number in the sequence, find the interval it falls in and assign the corresponding outcome (treatment type, demand level, number of customers, etc.).

### Step 4: Build the Simulation Table

Use the mapped outcomes to compute all derived quantities: arrival vs. start time, waiting time, idle time, treatment end time, stock levels, etc.

### Step 5: Compute Summary Statistics

Calculate averages, totals, shortfalls, overtime, utilisation, etc. These are the answers the problem asks for.

---

## Part 8: Multivariate Distribution — Estimating Basic Average Demand

One extension of Monte Carlo simulation involves estimating the **basic average demand** on the basis of a multivariate (or multi-outcome) distribution of numbers — that is, when demand depends on several categories, each with its own probability.

The process is exactly the same as the baker example in Week 5:

1. Build the probability distribution from historical data.
2. Assign RN ranges proportional to probability.
3. Use a random number sequence to simulate many periods.
4. Compute the average of all simulated demand values.

This average is the Monte Carlo **estimate** of the expected demand — equivalent (with enough runs) to computing $E[X] = \sum x \cdot P(x)$ analytically.

---

## Revision Summary Table

|Topic|Key Formula / Fact|Week|
|---|---|---|
|System components|Input, Processor, Output, Control, Feedback, Environment, Boundary|1|
|Stable system|$\rho < 1$ or $\lambda < \mu$|2|
|Server utilisation|$\rho = \lambda / \mu$|2|
|Avg customers in system|$L_s = \lambda / (\mu - \lambda)$|2|
|Avg wait in system|$W_s = 1 / (\mu - \lambda)$|2|
|Birth-death probability|$P_n = (1 - \rho)\rho^n$|4|
|Prob. of at least $k$|$P_{n \geq k} = \rho^k$|4|
|Markov chain|Future state depends only on present state, not history|Revision|
|Monte Carlo|Map RN to outcome → simulate → average results|5|
|RN interval width|Equal to the probability of that outcome|5|

---

## Common Exam Mistakes to Avoid

1. **Applying $P_n = (1-\rho)\rho^n$ when $\rho \geq 1$** — always check stability first.
2. **Wrong RN boundary assignment** — make sure RN intervals are as wide as the probability and cover 0.00–0.99 without overlap or gaps.
3. **Forgetting that treatment start ≠ arrival time** — when a patient arrives before the previous treatment ends, they wait. Treatment start = max(arrival, previous end).
4. **Confusing $W_q$ and $W_s$** — $W_q$ is wait in queue only; $W_s$ includes service time.
5. **Confusing verification and validation** — verification checks the model is built correctly; validation checks it matches reality.
6. **Rounding ρ too early** — carry at least 4 decimal places in $\rho$ before computing $P_n$, otherwise rounding errors compound.