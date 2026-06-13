# Week 2: Queueing Systems

## What is a Queueing System?

A **queueing system** is any system in which customers (people, jobs, vehicles, data packets, etc.) arrive to receive a service, and may have to wait if the service is not immediately available. It is one of the most common things modelled in real life — almost every business has a queue somewhere.

**Queueing theory** (also called *waiting line theory*) is the mathematical study of these waiting lines. It was developed by the Danish engineer **A. K. Erlang in 1909**, who was studying congestion in telephone exchanges. In those early days, telephone operators could not handle all the calls being made simultaneously, which caused delays. Erlang's mathematical analysis of this problem laid the foundation for modern queueing theory.

After **World War II**, the same techniques were extended beyond telephony to all sorts of waiting-line problems — industries, schools, hospitals, banks, cafeterias, libraries, petrol pumps, and so on.

> **Definition:** A *queue* is a waiting line of customers waiting for a service to be rendered.

### Why Queues Form

Customers in a queue are typically served based on the order of arrival. However, when the **arrival rate** and **service rate** are not well matched, queues develop. Two opposite situations cause inefficiency:

1. **Too much demand** on the service station → customers wait too long (or leave).
2. **Too little demand** → the service facility sits idle, wasting resources.

A queueing problem, therefore, is essentially a **balancing problem**: balancing the cost of customers waiting against the cost of having idle service capacity.

> **Real-world example:** A bank that has 10 tellers when only 2 customers come per hour wastes money on salaries. A bank with 1 teller when 100 customers come per hour loses customers to balking and reneging. The right number lies in between, and queueing theory helps find it.

---

## Major Constituents of a Queue System

Every queueing system has **three core constituents**:

1. **Customer Unit** — the entities arriving to receive service. These can be persons, machines, vehicles, goods, parts, jobs, data packets — anything that needs servicing.
2. **Queue / Waiting Line** — the customers waiting (not yet being served).
3. **Serving Channel** — the facility providing the service. This may be a single server (one teller) or multi-channel (several tellers).

### System Diagram

```
                     ┌──────────────────┐      ┌─────────────────────┐
   Arrivals  ────▶   │  Queue /         │ ───▶ │  Service Facility   │ ────▶  Departures
   (Customers)       │  Waiting Line    │      │  (Channels 1, 2...) │        (Served)
                     └──────────────────┘      └─────────────────────┘
```

The customer enters → waits in the queue → gets served → leaves the system.

---

## Key Characteristics of Queue Theory

The standard assumptions of basic (M/M/1) queueing models are:

1. **Random arrivals** at an average rate $\lambda$ (lambda) — the **arrival rate**, typically following a Poisson distribution.
2. **Random service** at an average rate $\mu$ (mu) — the **service rate**, typically following an exponential distribution.
3. **Queue discipline is FIFO** (First In, First Out) — customers are served in the order they arrived.
4. **Infinite queue capacity** — no limit on how many customers can wait.
5. **Infinite population size** — the pool of potential customers is unlimited.

### Stability Condition

For a queue to be stable (i.e., not grow forever), the service rate must exceed the arrival rate:

$$\lambda < \mu$$

If $\lambda \geq \mu$, the queue will grow without bound — customers arrive faster than they can be served. (Think of a petrol station during fuel scarcity: the queue effectively becomes infinite.)

---

## Performance Measures

Once we know $\lambda$ and $\mu$, we can compute several useful quantities that describe how the queue behaves.

### a. Server Utilisation ($\rho$)

This is the fraction of time the server is busy. It tells you how heavily the service facility is loaded.

$$\rho = \frac{\lambda}{\mu}$$

- $\rho$ close to 0 → server mostly idle.
- $\rho$ close to 1 → server almost always busy, long queues likely.

#### Worked Example 1

If the arrival rate is $\lambda = 4$ customers per hour and the service rate is $\mu = 6$ customers per hour, calculate the server utilisation.

Converting to per-minute (optional, units must just be consistent):

$$\lambda = \frac{4}{60} \text{ cust/min}, \quad \mu = \frac{6}{60} \text{ cust/min}$$

$$\rho = \frac{\lambda}{\mu} = \frac{4/60}{6/60} = \frac{4}{6} \approx 0.667 = 66.7\%$$

**Interpretation:** On average, the server is busy 66.7% of the time and idle for the remaining 33.3%.

---

### b. Average Number of Customers in the System ($L_s$)

This is the average total number of customers present — both those being served *and* those waiting in line.

$$L_s = \frac{\lambda}{\mu - \lambda}$$

Using the same example ($\lambda = 4$, $\mu = 6$):

$$L_s = \frac{4}{6 - 4} = \frac{4}{2} = 2 \text{ customers}$$

**Interpretation:** At any moment, you'd expect about 2 customers in the system.

---

### c. Average Number of Customers in the Queue ($L_q$)

This counts only those *waiting*, excluding the one currently being served.

$$L_q = \frac{\lambda^2}{\mu(\mu - \lambda)}$$

Using the example:

$$L_q = \frac{4^2}{6(6 - 4)} = \frac{16}{12} \approx 1.33 \text{ customers}$$

**Interpretation:** On average, about 1.33 customers are waiting in line.

Note that **$L_s - L_q = \rho$**, since on average $\rho$ of the time someone is being served:
$$2 - 1.33 = 0.67 \approx \rho \;\checkmark$$

---

### d. Average Waiting Time in the System ($W_s$)

This is the average total time a customer spends in the system, from arrival until service is complete.

$$W_s = \frac{1}{\mu - \lambda}$$

Using the example:

$$W_s = \frac{1}{6 - 4} = 0.5 \text{ hours} = 30 \text{ minutes}$$

**Interpretation:** From the moment a customer arrives until they leave (fully served), an average of 30 minutes elapses.

---

### e. Average Waiting Time in the Queue ($W_q$)

This is the time spent *just waiting* — before service even begins.

$$W_q = \frac{\lambda}{\mu(\mu - \lambda)} \quad \text{or equivalently} \quad W_q = \frac{L_q}{\lambda}$$

The second form is known as **Little's Law** (average number in queue = arrival rate × average wait).

Using the example:

$$W_q = \frac{4}{6(6 - 4)} = \frac{4}{12} \approx 0.333 \text{ hours} = 20 \text{ minutes}$$

**Interpretation:** A customer waits about 20 minutes before being served.

---

### f. Average Service Time ($S_t$)

The time it takes the server to serve one customer:

$$S_t = W_s - W_q$$

$$S_t = 30 - 20 = 10 \text{ minutes}$$

Note that $S_t = 1/\mu$ always, so $1/6$ hours = 10 minutes. ✓

---

### Quick Reference Summary

| Symbol | Meaning | Formula |
|--------|---------|---------|
| $\rho$ | Server utilisation | $\lambda / \mu$ |
| $L_s$ | Avg. customers in system | $\lambda / (\mu - \lambda)$ |
| $L_q$ | Avg. customers in queue | $\lambda^2 / [\mu(\mu - \lambda)]$ |
| $W_s$ | Avg. time in system | $1 / (\mu - \lambda)$ |
| $W_q$ | Avg. time in queue | $\lambda / [\mu(\mu - \lambda)]$ |
| $S_t$ | Avg. service time | $W_s - W_q = 1/\mu$ |
| $P_0$ | Probability system is empty | $1 - \rho$ |

---

## Worked Example 2: Petrol Pump

Cars arrive at a petrol pump (with one petrol unit) at an average rate of **10 cars per hour**. The service times follow an exponential distribution with a mean of **3 minutes**. Find:

a. The average number of cars in the system.
b. The average queue length.
c. The average waiting time in the queue.
d. The average waiting time in the system.

### Step 1: Identify $\lambda$ and $\mu$

$$\lambda = 10 \text{ cars/hour}$$

Mean service time = 3 minutes, so the service rate is:

$$\mu = \frac{60 \text{ min}}{3 \text{ min/car}} = 20 \text{ cars/hour}$$

Check stability: $\lambda < \mu$ (10 < 20) ✓

### Step 2: Solve

**a. Average number of cars in the system**

$$L_s = \frac{\lambda}{\mu - \lambda} = \frac{10}{20 - 10} = 1 \text{ car}$$

**b. Average queue length**

$$L_q = \frac{\lambda^2}{\mu(\mu - \lambda)} = \frac{100}{20 \times 10} = 0.5 \text{ cars}$$

**c. Average waiting time in the queue**

$$W_q = \frac{\lambda}{\mu(\mu - \lambda)} = \frac{10}{20 \times 10} = 0.05 \text{ hours} = 3 \text{ minutes}$$

Or using Little's Law: $W_q = L_q / \lambda = 0.5 / 10 = 0.05$ hours ✓

**d. Average waiting time in the system**

$$W_s = \frac{1}{\mu - \lambda} = \frac{1}{10} = 0.1 \text{ hours} = 6 \text{ minutes}$$

---

## Elements of a Queueing System

There are **five elements** that fully describe any queueing system:

### 1. Queueing Input / Arrival Distribution (Calling Population)

The pattern in which customers arrive. The **arrival rate** is the number of customers arriving per unit time. When arrivals are random, we describe inter-arrival times using a probability distribution — most commonly the **Poisson distribution** — with mean arrival rate $\lambda$.

### 2. Service Distribution

The pattern in which customers leave (get served). The **service rate** is the number of customers served per unit time. When service times are random, the **exponential distribution** is typically used, with mean service rate $\mu$.

### 3. Service Mechanism

The structure of the service facility. There can be:

- **Single channel** — one server, one queue (e.g., a small ATM).
- **Multi-channel** — several servers (e.g., bank with multiple tellers).
- **Single-phase** — only one service step.
- **Multi-phase** — service goes through several stages (e.g., a hospital: registration → triage → consultation → pharmacy).

### 4. Queue Discipline

The rule for choosing the next customer to serve. Common disciplines:

- **FIFO** (First In, First Out) — most common; fair and standard.
- **LIFO** (Last In, First Out) — used in stack-like systems.
- **Priority** — VIP customers go first (e.g., emergency triage in a hospital).
- **Random** — random selection (less common).

### 5. Maximum Number of Customers Allowed

Depends on the system. Some systems are **finite** (a barbershop with only 10 chairs) — once full, new customers are turned away. Others are **infinite** (an online support queue can absorb millions).

---

## Customer Behaviour

To model queues realistically, we must account for how customers actually behave when they see a queue. Three important behaviours:

### Balking

A customer who **arrives, sees the queue is too long, and decides not to join at all** is said to have **balked**.

> **Example:** You drive to a barbershop, see a long line through the window, and decide to come back tomorrow. You balked.

### Reneging

A customer who **joins the queue but leaves before being served** has **reneged**.

> **Example:** You join a hospital queue, wait 45 minutes, get frustrated, and walk out. You reneged.

### Jockeying

When there are **multiple parallel queues** and customers **switch between them** trying to find the fastest one, they are **jockeying**.

> **Example:** At a supermarket with several checkout lanes, you switch from lane 3 to lane 5 hoping it moves faster. You're jockeying. (And Murphy's Law guarantees lane 3 then speeds up.)

These behaviours make real queueing systems harder to model than the simple equations suggest — but they are essential to capture realistic outcomes.

---

## Worked Example 3: TV Technician

A TV technician finds that the time he spends on each repair follows an exponential distribution with a mean of **30 minutes** per set. He repairs sets in the order they arrive. TV sets arrive at a rate of **10 sets per 8-hour day**. Find:

a. The technician's expected idle time per day.
b. The average time a TV spends in the system.

### Step 1: Identify $\lambda$ and $\mu$

Arrival rate:

$$\lambda = \frac{10 \text{ sets}}{8 \text{ hours}} = 1.25 \text{ sets/hour}$$

Service rate (one set takes 30 min = 0.5 hr):

$$\mu = \frac{1}{0.5} = 2 \text{ sets/hour}$$

Check stability: $1.25 < 2$ ✓

### Step 2: Server Utilisation

$$\rho = \frac{\lambda}{\mu} = \frac{1.25}{2} = \frac{5}{8} = 0.625$$

The technician is busy 62.5% of the time.

### Step 3: Idle Time

The probability the system is empty (technician idle) is:

$$P_0 = 1 - \rho = 1 - \frac{5}{8} = \frac{3}{8} = 0.375 = 37.5\%$$

In an 8-hour day, the expected idle time is:

$$\text{Idle time} = 0.375 \times 8 = 3 \text{ hours}$$

**Interpretation:** The technician is idle for 3 hours of every 8-hour workday — meaning there is spare capacity that could be used for additional jobs.

### Step 4: Average Time a TV Spends in the System

$$W_s = \frac{1}{\mu - \lambda} = \frac{1}{2 - 1.25} = \frac{1}{0.75} = \frac{4}{3} \text{ hours} \approx 1 \text{ hour } 20 \text{ minutes}$$

**Interpretation:** From the moment a TV arrives until it is fully repaired and returned, about 1 hour and 20 minutes pass on average.

---

## Where Queueing Theory is Used in Practice

To anchor the theory:

- **Telecommunications** — Erlang's original problem; still used for sizing call centres and data networks.
- **Banking** — deciding how many tellers to staff at different times of day.
- **Hospitals** — emergency room triage, bed allocation, ambulance dispatch.
- **Computer systems** — CPU scheduling, web server request handling, print queues.
- **Manufacturing** — production line buffer sizing.
- **Airports** — check-in counters, security lines, runway scheduling.
- **Road traffic** — traffic-light timing, toll-booth design.

In each of these, the same basic question recurs: *how do we balance the cost of waiting against the cost of extra service capacity?* And the same equations help answer it.
