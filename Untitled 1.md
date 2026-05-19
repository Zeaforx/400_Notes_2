## 3.1 Analysis of the Current System

Current Kubernetes cost optimization strategies on GCP generally follow two paths. Some teams use standard Horizontal Pod Autoscalers (HPA) and Cluster Autoscalers (CA) strictly with On-Demand nodes to guarantee stability. Other teams deploy Spot VMs without specific configurations to reduce compute costs. The Baseline model in this study uses only On-Demand nodes, representing maximum stability at the highest infrastructure cost. The Standard Spot model uses Spot instances with default settings. It lacks specialized taints or affinity rules to handle node preemption.

## 3.2 Problems of the Current System

The On-Demand model generates high operational costs and provisions nodes inefficiently. The standard Spot implementation lowers costs but creates severe reliability problems during preemption. Unoptimized Spot interruptions cause immediate downtime and return HTTP 500 errors before Kubernetes can replace the dead node. Neither model coordinates cost and reliability natively. This forces organizations to build custom forecasting algorithms, which drives up the Total Cost of Ownership (TCO).

## 3.3 Analysis of the Proposed System

This research designs a hybrid dual-pool architecture on GKE. Pool 1 contains Spot VMs. We taint this pool with "node-type=spot:NoSchedule" so random workloads do not schedule there accidentally. Pool 2 contains On-Demand nodes to act as a reliable fallback. The core scheduling logic relies on Node Affinity with a "preferredDuringScheduling" rule. This forces pods to prioritize the cheaper Spot nodes. The system only uses the On-Demand pool when Spot capacity runs out. Taints keep non-experiment workloads completely off the test nodes.

![Diagram of Kubernetes Architecture][image3]

Figure 3.1: Sequence Diagram of the Proposed System

## 3.4 Methodology

### 3.4.1 Implementation Strategy

We provision the GKE infrastructure using Terraform to keep the environment consistent. We containerize and deploy a Golang-based microservice that simulates a CPU-intensive task. This application contains a graceful shutdown mechanism. It listens for operating system signals and waits up to 15 seconds after receiving a termination signal. This allows active requests to finish processing. If the shutdown takes longer than 15 seconds, the system forcefully terminates the process. We configure the HPA to scale out when CPU usage passes 70%. The CA provisions new nodes whenever pods enter a pending state.

### 3.4.2 Experimental Procedure

The experiment runs in sequential phases across the Baseline, Standard Spot, and Hybrid Model groups.

1. Phase 1 establishes the baseline for performance and cost by applying steady-state and burst traffic profiles.
    
2. Phase 2 measures the fallout from unoptimized Spot interruptions. We force failures on standard Spot VMs using the "gcloud compute instances simulate-maintenance-event" command.
    
3. Phase 3 tests the Hybrid Model's failover logic. We trigger the same maintenance simulation to preempt Spot nodes while the system handles active traffic.
    
    1. We run this simulation 20 times ($N=20$) to account for network variance.
        
    2. We verify that the system immediately evicts pods from dying Spot nodes, reschedules them to the On-Demand pool, and moves them back to Spot nodes when capacity returns.
        

### 3.4.3 Metrics for Evaluation

We use specific metrics to compare the testing groups.

**Cost per 1,000 Requests (**$C_{1k}$**):** This normalizes cost efficiency across varying traffic volumes.

$$C_{1k} = \frac{P_{node} \cdot T_{active}}{R_{total}}$$

Here, $P_{node}$ is the per-second billing rate of the active nodes, $T_{active}$ is the duration of activity, and $R_{total}$ is the total count of HTTP 200 OK responses.

**Reliability (Error Rate %):** This measures the percentage of failed requests against total requests during the test window. The target for the Hybrid model is an error rate below 1%.

$$ErrorRate = \frac{R_{5xx}}{R_{total}} \times 100$$

**Failover Latency (P95):** We measure the 95th percentile latency of requests served during the specific "Failover Window."

**Recovery Time:** We measure the total time from the injection of the maintenance signal until the cluster returns to a fully healthy state.

## 3.5 Data Collection

We collect data systematically to compare the performance and cost-efficiency of the testing groups.

### 3.5.1 Method of data collection

We use Prometheus to collect performance metrics like CPU utilization, request latency, and failover timing. We source financial data from the GCP Billing Catalog to calculate the specific compute cost per unit of work. We use Apache JMeter to generate traffic and record request-level metrics.

## 3.6 Choice of Programming Language and Technology

We selected Go (Golang) for the microservice because of its fast startup time. This ensures our failover latency measurements reflect the underlying infrastructure performance, not application boot delay. We chose Terraform for infrastructure provisioning to guarantee replicability across all cluster variations. We chose Google Kubernetes Engine (GKE) as the deployment environment because it directly integrates with GCP Spot virtual machines and provides native autoscaling tools.