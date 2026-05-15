# Queueing System

A way of accessing people working for you. Queueing theory is also termed waiting line theory. It owes its development to A.K. Erlang in 1903. Erlang took up the problem of congestion of telephone traffic. During those days, operators were unable to handle the calls the moment they were made, resulting in delayed calls.

After WWII, the problem of congestion of telephone traffic was extended to other general problems involving queues and waiting lines. A queue is a waiting line for service to be rendered to customers. All types of businesses like industries, schools, hospitals, banks, cafeterias, libraries, petrol pumps, etc., all have queueing problems.

The service of customers in a queue is based on arrival. When a service rate or arrival rates are not defined, queues are likely to develop, and at that instance, the service facility remains idle. Every queue involves wastage of time and also results in wastage of revenue. Therefore, a queue problem is essentially a problem of balancing the cost of waiting in a queue against the cause of idle time for a service facility in the queue.

In any system, queueing problems arise either because:

1. There is too much demand on a service station.
    
2. There is too little demand, and as a result, there is too much idle service time or too many services.
    

## Major Constituents of a Queue System

There are 3 major constituents of a queueing system:

1. **Customer unit:** The people arriving to get the normal service (e.g., persons, machines, vehicles, goods, parts).
    
2. **Queue / waiting line:** The number of customers waiting to get serviced.
    
3. **Serving channel:** The process or facility which is providing services to the customers. This may be a single or multi-channel.
    

**System Diagram:**

Arrival of customers $\rightarrow$  

$$Queue / Waiting Line$$

$\rightarrow$  

$$Service Facility (Channels 1, 2)$$

$\rightarrow$ Departures (Customers)

## Key Characteristics of Queue Theory

1. Arrivals occur randomly at an average rate ($\lambda$). $\lambda$ (lambda) = arrival rate.
    
2. Service is provided at an average rate ($\mu$). $\mu$ (mu) = service rate.
    
3. Queue discipline is FIFO (First In, First Out).
    
4. Infinite queue capacity (no limit).
    
5. Population size is assumed to be infinite.
    

For a queue system to be stable, let $\lambda < \mu$.

## Performance Measures

### Common Performance Measures in Modelling

**a. Server Utilization (**$\rho$**)**

$$\rho = \frac{\lambda}{\mu}$$

**Example 1:**

If the arrival rate $\lambda = 4$ customers per hour and the service rate $\mu = 6$ customers per hour, calculate the server utilization.

$$\lambda = \frac{4}{60} \text{ customers per minute}$$$$\mu = \frac{6}{60} \text{ customers per minute}$$$$\rho = \frac{\lambda}{\mu} = \frac{4/60}{6/60} = \frac{4}{6} = 0.66 = 66\%$$

On average, the channel is performing maximally at 66%.

**b. Average Number of Customers in a System (**$L_s$**)**

$$L_s = \frac{\lambda}{\mu - \lambda}$$

Using the previous example:

$$L_s = \frac{4/60}{6/60 - 4/60} = \frac{4/60}{2/60} = \frac{4}{2} = 2 \text{ customers}$$

**c. Average Number of Customers in a Queue (**$L_q$**)**

$$L_q = \frac{\lambda^2}{\mu(\mu - \lambda)}$$

Using the previous example:

$$L_q = \frac{4^2}{6(6 - 4)} = \frac{16}{6(2)} = \frac{16}{12} = 1.33 \text{ customers}$$

**d. Average Waiting Time in the System (**$W_s$**)**

$$W_s = \frac{1}{\mu - \lambda}$$

Using the previous example:

$$W_s = \frac{1}{6 - 4} = \frac{1}{2} = 0.5 \text{ hours} = 30 \text{ minutes}$$

This means the total time from waiting to complete service is 30 minutes.

**e. Average Waiting Time in the Queue (**$W_q$**)**

$$W_q = \frac{\lambda}{\mu(\mu - \lambda)} \quad \text{OR} \quad W_q = \frac{L_q}{\lambda}$$

Using the previous example:

$$W_q = \frac{4}{6(6 - 4)} = \frac{4}{12} = \frac{1}{3} \approx 0.33 \text{ hours} = 20 \text{ minutes}$$

$W_q$ is the waiting time in the queue before the beginning of service.

**f. Average Service Time (**$S_t$**)**

$$S_t = W_s - W_q$$$$S_t = 30 - 20 = 10 \text{ minutes}$$

## Example 2

Cars arrive at a petrol pump having a petrol unit with an average of 10 cars per hour. The service times have an exponential distribution with a mean of 3 minutes. Find:

a. The average number of cars in the system.

b. The average queue length.

c. The average waiting time in the queue.

d. Average waiting time in the system.

**Hints:**

$\lambda = 10$ cars per hour.

Mean service time $= 3$ minutes.

$\mu = \frac{1}{3 \text{ mins}} = \frac{60}{3} = 20$ cars per hour.

**Solutions:**

**a. Average number of cars in the system (**$L_s$**)**

$$L_s = \frac{\lambda}{\mu - \lambda} = \frac{10}{20 - 10} = \frac{10}{10} = 1 \text{ car}$$

**b. Average queue length (**$L_q$**)**

$$L_q = \frac{\lambda^2}{\mu(\mu - \lambda)} = \frac{10^2}{20(20 - 10)} = \frac{100}{200} = 0.5 \approx 1 \text{ car}$$

**c. Average waiting time in the queue (**$W_q$**)**

$$W_q = \frac{\lambda}{\mu(\mu - \lambda)} = \frac{10}{20(20 - 10)} = \frac{10}{200} = 0.05 \text{ hours} = 3 \text{ minutes}$$

OR

$$W_q = \frac{L_q}{\lambda} = \frac{0.5}{10} = 0.05 \text{ hours} = 3 \text{ minutes}$$

**d. Average waiting time in the system (**$W_s$**)**

$$W_s = \frac{1}{\mu - \lambda} = \frac{1}{20 - 10} = \frac{1}{10} = 0.1 \text{ hours} = 6 \text{ minutes}$$

## Elements of a Queueing System

1. **Queueing Input / Arrival Distribution (Calling Population):** This represents the pattern in which the number of customers are arriving at the service. The rate at which customers are arriving for the service facility per unit of time is called the arrival rate. In case the arrivals are random, you use probability to describe the time between arrivals. Random arrivals are best described by the Poisson distribution, and the mean arrival rate is represented by lambda ($\lambda$).
    
2. **Service Distribution:** Represents the pattern in which the number of customers are leaving the service centers. The rate at which customers leave service centers per unit of time is called the service rate. If service times are distributed randomly, then the exponential probability distribution applies, and the mean service rate is represented by the symbol mu ($\mu$).
    
3. **Service Mechanism:** In the queue, there may be a single or multiple service channels. In a single service channel, customers form a single queue in line and get serviced by one unit.
    
4. **Queue Discipline:** This is the rule by which customers are selected from the queue to get serviced. Service discipline is also called the order of service. Sometimes, it is FIFO (First In, First Out).
    
5. **Maximum Number of Customers Allowed in a Queue System:** This depends on the nature of the system. In some systems, the total number of customers allowed can be finite, while in others it is infinite.
    

## Customer Behaviour

In order to study queues, you must consider customer behaviour.

- **Balking:** When a customer leaves a queue because of a very long wait time, the customer balked.
    
- **Reneging:** If a customer leaves the queue in between, the customer reneged.
    
- **Jockeying:** When there are 3 or more queues and customers switch frequently, the customer is jockeying.
    

## Example 3

A TV technician finds the time spent on his job has an exponential distribution with a mean rate of 30 minutes. If he repairs sets in the order in which they arrive and if the arrival of a TV set is approximately at an average rate of 10 sets per 8 hours. Find the technician's expected idle time and the average time a TV spends in the system.

**Hints:**

$\lambda = \frac{10}{8} \text{ sets per hour}$

$\mu = \frac{1}{30 \text{ minutes}} = \frac{1}{0.5 \text{ hours}} = 2 \text{ sets per hour}$  

**Server Utilization (**$\rho$**):**

$$\rho = \frac{\lambda}{\mu} = \frac{10/8}{2} = \frac{10}{16} = \frac{5}{8}$$

**Probability of Idle Time (**$P_0$**):**

$$P_0 = 1 - \frac{\lambda}{\mu} = 1 - \frac{5}{8} = \frac{3}{8} = 0.375 = 37.5\%$$

The total expected idle time for the technician in an 8-hour day is:

$$0.375 \times 8 \text{ hours} = 3 \text{ hours}$$

**Average Time a TV Spends in the System (**$W_s$**):**

$$W_s = \frac{1}{\mu - \lambda} = \frac{1}{2 - \frac{10}{8}} = \frac{1}{2 - 1.25} = \frac{1}{0.75} = \frac{4}{3} \text{ hours} = 1 \text{ hour } 20 \text{ minutes}$$