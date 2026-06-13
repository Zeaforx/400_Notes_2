# The Birth and Death Process

Most queueing processes consider arrival and departure on the basis of a birth and death process. Birth refers to the arrival of new customers. Death refers to the departure of served customers.

If the state of the system at time $t$ (where time $\ge 0$) is given by $C(t)$, $C(t)$ is the number of customers in the queueing system at time $t$. The birth and death process describes the behaviour of change of state with an increase in time $t$ with the help of probability distribution.

## Assumptions in Birth and Death Process

1. **Arrivals:** The probability distribution of the remaining time until the next event occurs (i.e. the next arrival or birth occurs) is exponential with parameters $\lambda_n$ ($n = 0, 1, 2, 3 \dots n$) where $n = C(t)$.
    
2. **Departures:** The probability distribution of the remaining time until the next event is completed with parameter $\mu_n$ ($n = 0, 1, 2, 3 \dots n$) where $n = C(t)$.
    
3. **Independence:** The random variable representing the birth process and the random variable representing the death process both are mutually independent.
    

**State Diagram Concept:**

You can visualize this process as a series of states (0, 1, 2, 3...). Forward arrows show the mean arrival rate ($\lambda_0, \lambda_1, \lambda_2 \dots$), moving the system to a higher state. Backward arrows show the mean departure rates ($\mu_1, \mu_2 \dots$), returning the system to a lower state. These rates are mutually independent.

## Birth-Death Process Probabilities

To find the probability of having exactly $n$ customers in the system ($P_n$), use the following formula:

$$P_n = (1 - \rho)\rho^n$$

### Example 1

In a bank operation, if the arrival rate is 4 customers per hour and the departure rate is 9 customers per hour, find the probability that exactly 3 customers are in the system. State whether the system is stable or not.

**Given:**

$\lambda = 4$

$\mu = 9$

$\rho = \frac{4}{9} \approx 0.44$  

**Calculation:**

$$P_3 = (1 - 0.44) \times 0.44^3$$$$P_3 = 0.56 \times 0.085184 \approx 0.048$$

_Because_ $\rho$ _(0.44) is less than 1, the system is stable._

### Example 2

If the arrival rate is 2 customers per minute and the departure rate is 5 customers per minute. Find the probability of having:

a) 0 customers

b) 1 customer

c) 2 customers

d) State whether or not the system is stable.

**Given:**

$\rho = \frac{2}{5} = 0.4 = 40\%$  

**Calculations:**

**a) 0 customers (**$P_0$**)**

$$P_0 = (1 - 0.4) \times 0.4^0 = 0.6 \times 1 = 0.6$$

**b) 1 customer (**$P_1$**)**

$$P_1 = (1 - 0.4) \times 0.4^1 = 0.6 \times 0.4 = 0.24$$

**c) 2 customers (**$P_2$**)**

$$P_2 = (1 - 0.4) \times 0.4^2 = 0.6 \times 0.16 = 0.096$$

**d) Stability**

_The system is stable because_ $\rho = 0.4$_, which is less than 1._

_(Self-check from notes: If arrival rate is 10 c/hr and departure rate is 8 c/hr, check stability rate._ $\rho = \frac{10}{8} = 1.25$_. Since_ $\rho > 1$_, the system is not stable.)_

### Example 3

A TV repairer receives 5 jobs per hour. He serves 7 customers/hr. Find the probability that:

a) The system is empty.

b) There are exactly 2 jobs in the system.

c) There are at least 3 jobs in the system.

**Given:**

$\rho = \frac{5}{7} \approx 0.7143$  

**Calculations:**

**a) The system is empty (**$P_0$**)**

$$P_0 = (1 - 0.7143) \times 0.7143^0 = 0.2857$$

**b) Exactly 2 jobs (**$P_2$**)**

$$P_2 = (1 - 0.7143) \times 0.7143^2$$$$P_2 = 0.2857 \times 0.5102 \approx 0.1458$$

**c) At least 3 jobs (**$P_{n \ge 3}$**)**

To find the probability of 3 or more jobs, calculate 1 minus the probability of having 0, 1, or 2 jobs.

$$P_{n \ge 3} = 1 - (P_0 + P_1 + P_2)$$

First, find $P_1$:

$$P_1 = (1 - 0.7143) \times 0.7143^1 = 0.2041$$

Now calculate the total:

$$P_0 = 0.2857$$$$P_1 = 0.2041$$$$P_2 = 0.1458$$$$P_{n \ge 3} = 1 - (0.2857 + 0.2041 + 0.1458)$$$$P_{n \ge 3} = 1 - 0.6356 = 0.3644$$