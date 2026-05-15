# Week 1: Introduction to System Modelling and Simulation

## What is a System?

Before discussing models, it helps to know what we are modelling. A **system** is a collection of interacting components that work together to achieve a defined purpose. Examples:

- A bank with tellers, customers, and queues.
- A power grid with generators, transmission lines, and consumers.
- A traffic network with roads, vehicles, and traffic lights.
- A hospital with doctors, patients, beds, and equipment.

To study how a system behaves under different conditions, we often build a **model** of it instead of experimenting on the real system directly. This is because real systems are usually:

- **Too expensive** to experiment with (you cannot keep crashing real planes to study aviation safety).
- **Too dangerous** (you cannot test nuclear reactor failure on the actual reactor).
- **Too slow** (studying climate change over 100 years).
- **Not yet built** (a proposed factory, a new road network, a system still in design).

This is the main reason we associate modelling with system study.

---

## Definition of a Model

A model is a **simplified representation of a real system** that captures the features relevant to the questions we want to answer. Key points to remember:

- A model is a collection of relevant information used to represent a system.
- It helps the system analyst understand the functionality of a system and communicate it to customers and clients.
- It is a simplified version of a system at a particular point in time and space, intended to promote understanding of the real system.
- A model is anything a system can be applied to in order to answer questions about the system.
- It is a pattern, representation, or description designed to show the structure or working of an object, system, or concept.

> **Real-world analogy:** A map is a model of a city. It does not show every tree or pothole — it only shows what is relevant for navigation (roads, landmarks, distances). A globe is a model of the Earth. Both are useful because they are *simpler* than the real thing.

### Why Simplify?

A model should not be so complex that it becomes impossible to understand. If a model contains every detail of the real system, you might as well use the real system itself. The art of modelling lies in deciding **what to include and what to leave out**.

---

## Definition of Modelling

**Modelling** is the process of organising knowledge about a given system — that is, the activity of creating or developing a model.

If a *model* is the noun (the finished product), *modelling* is the verb (the process of building it).

---

## Processes of Modelling

Building a useful model is a structured activity. It follows six main stages:

### 1. Problem Formulation

Clearly state the problem the model is meant to solve. Without a defined problem, the model has no direction.

> **Example:** "We want to reduce average customer waiting time at a bank from 15 minutes to under 5 minutes." This is a well-formulated problem.

### 2. Data Collection and Analysis

Real data is needed to make a model realistic. Common collection methods include:

- **Questionnaires** — written sets of questions distributed to respondents.
- **Interviews** — direct one-on-one conversations.
- **Observations** — watching the system operate without interfering (e.g., timing how long customers wait in a queue).
- **Experiments** — controlled tests where conditions are varied deliberately.

### 3. Simulation Model Development

This is where the actual model is built, typically as a computer program or mathematical formulation.

### 4. Model Verification, Validation, and Calibration

These three terms are often confused, but they answer different questions:

- **Verification** — *"Are we building the model right?"* That is, does the code/math correctly implement what we intended? (Programming correctness.)
- **Validation** — *"Are we building the right model?"* That is, does the model actually represent the real system? (Real-world accuracy.)
- **Calibration** — adjusting model parameters so its output matches observed real data. For instance, if students $x$ and $y$ represent entities in a school simulation, calibration tunes how they behave so the model matches actual school records.

> **Quick way to remember:**
> Verification = "Did I do the math right?"
> Validation = "Did I do the right math?"

### 5. Input Data Analysis

Analyse the data that will feed into the model. Are the arrival rates Poisson-distributed? Are service times exponential? This step ensures the inputs are realistic.

### 6. Sensitivity Estimation

Check how sensitive the model's output is to changes in its inputs. If a tiny change in input causes huge output swings, the model may be unreliable. If the output barely changes when inputs vary, the model is robust.

---

## Types of Models

Models can be classified along two main axes:

1. **Deterministic vs. Stochastic** — based on whether randomness is involved.
2. **Static vs. Dynamic** — based on whether time plays a role.

---

### Deterministic vs. Stochastic Models

#### Deterministic Models

A deterministic model produces **exactly the same output every time** for the same set of inputs. There is no randomness involved. The output is fully determined by the inputs and the relationships between them.

> **Example:** Newton's equation $F = ma$. Given the same mass and acceleration, you will always get the same force. No randomness.

Deterministic models can take a lot of computer time to evaluate when the system is complex, but the result is reproducible and exact.

#### Stochastic Models

A stochastic model involves **at least one random input component**, so the output is also random. Each time you run the model, you may get a slightly different answer. The output only **estimates** the true characteristics of the system.

> **Example:** Predicting how many customers will arrive at a bank tomorrow. You cannot say "exactly 247." You can only describe the probability distribution of possible arrivals.

Because of randomness, stochastic models have two key implications:

1. They have one or more random inputs.
2. You need to **repeat the experiment many times** (this is called running multiple *replications*) to average out the randomness and get reliable estimates.

This need for repetition can sometimes cast doubt on results if too few runs are made, which is why confidence intervals are reported in stochastic studies.

---

### Static vs. Dynamic Models

#### Static Models

A static model represents the system at a **single point in time**. Time plays no role — the state does not evolve.

> **Example:** A snapshot of how much money is in each account in a bank right now. It tells you the state at one moment, but not how balances change throughout the day.

#### Dynamic Models

A dynamic model shows how the system **changes over time**. The state evolves as time progresses.

> **Example:** A traffic simulation showing how cars move through an intersection over a one-hour period. Time is central to the model.

---

### Summary of Mathematical Model Classification

| Type | Defining Feature |
|------|------------------|
| **Deterministic** | Input and output variables are fixed values (no randomness). |
| **Stochastic** | At least one input or output variable is probabilistic. |
| **Static** | Time is not taken into account. |
| **Dynamic** | Time of variables is taken into account. |

A model can be a combination — for example, a *dynamic stochastic* model (a bank queue simulation that runs over time and has random arrivals) or a *static deterministic* model (a one-shot calculation with fixed inputs).

---

## Worked Example: Power Generation in Nigeria

Given that Nigeria currently generates 5,801 MW, calculate how many years it will take to reach 24-hour electricity supply (assumed to require 33,000 MW), given that the average annual increase in generation is 11.87 MW per year.

### Variables

- Currently Generated MW (CGM) = 5,801 MW
- Projected MW needed (PMW) = 33,000 MW
- Average MW added per year (AMW) = 11.87 MW

### Formula

$$\text{Number of years} = \frac{\text{PMW} - \text{CGM}}{\text{AMW}}$$

### Calculation

$$\text{Number of years} = \frac{33{,}000 - 5{,}801}{11.87} = \frac{27{,}199}{11.87} \approx 2{,}291 \text{ years}$$

### Interpretation

At the current pace of expansion (only 11.87 MW added per year), Nigeria would need approximately **2,291 years** to reach 33,000 MW. This is a classic example of a **deterministic, static model** — there is no randomness, and time is captured only as a single output number rather than as an evolving variable. The result also exposes how unrealistic linear assumptions can be over long horizons; a more realistic model would use a dynamic growth rate.

---

## Model Validity

An important issue in modelling is **model validity** — how well the model represents the real system for the purpose it was built. A model may be valid for one purpose and invalid for another.

> **Example:** A flat-Earth model is perfectly valid for drawing a small city map but invalid for plotting intercontinental flights.

A model intended for a simulation study is typically a mathematical model developed with the help of simulation software (such as Python with SimPy, Arena, AnyLogic, or MATLAB).

---

# Simulation

## What is Simulation?

A **simulation** of a system is the **operation of a model of the system**. Once you have a model, you can run it under different conditions to see how the real system would respond — without touching the real system itself.

Simulation lets you:

- Reconfigure how the system operates.
- Experiment with different input data.
- Test "what if" scenarios safely.
- Evaluate performance of an existing, partially built, or proposed system under different configurations over long periods of time.

> **Real-world examples:**
> - **Flight simulators** train pilots without using real aircraft.
> - **SimCity** simulates city planning decisions.
> - **Hospital simulations** test whether adding two more nurses would reduce emergency-room waiting times.
> - **Crash test simulations** check vehicle safety before any physical prototype is built.

## Relationship Between Modelling and Simulation

Simulation comes **after** modelling. You first build the model (the description of the system), and then you simulate it (run it to observe behaviour). Without a model, there is nothing to simulate.

$$\text{System} \xrightarrow{\text{modelling}} \text{Model} \xrightarrow{\text{simulation}} \text{Insights about the system}$$

In this course, the simulation software being used is **Python** — which is well suited for this purpose due to libraries like SimPy, NumPy, SciPy, and Matplotlib.
