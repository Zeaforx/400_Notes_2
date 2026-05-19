Abstract
AI-generated

0%

Mixed

0%

Human-written

100%

chapter 1
AI-generated

8%

Mixed

5%

Human-written

88%

chapter 2
AI-generated

8%

Mixed

1%

Human-written

91%

chapter 3
AI-generated

3%

Mixed

17%

Human-written

80%

chapter 4
AI-generated

0%

Mixed

0%

Human-written

100%

chapter 5 
AI-generated

2%

Mixed

0%

Human-written

98%

appendix
AI-generated

2%

Mixed

16%

Human-written

82%








I apply
123, kasu running
123, kase chesss

123 kasu

Apply(SSN, cName, date, major, hobby)
FD:  {SSN, cName} → date       (a student applies to each college on one date)
MVD: {SSN, cName, date} ↠ major (majors and hobbies are independent)

R₁ = SSN, cName, date
$R_2$ = SSN, cName, major, hobby

R₁ = SSN, cName, date, major 
$R_2$ = SSN, cName, date, hobby


We tested three Kubernetes configurations on GKE in the africa-south1 region: an On-Demand baseline (Group A), a Standard Spot configuration (Group B), and the proposed Hybrid dual-pool model. All tests used e2-small instances with the HPA capped at 10 replicas. Cost figures were recomputed from Prometheus node-ready uptime against the GCP Billing Catalog list price for the region: $0.018429 per hour for on-demand nodes and $0.005213 per hour for Spot nodes, a 71.7 percent Spot discount. Table 5.1 consolidates the Hybrid model's headline results.

**Table 5.1: Headline findings — Hybrid Spot + On-Demand Fallback architecture**

|Metric|Hybrid Result|Context|
|---|---|---|
|Hourly compute cost reduction|36.8% lower|$0.0342/hr vs $0.0541/hr On-Demand|
|Mean preemption recovery time|8.5 s|62% faster than Standard Spot (22.33 s)|
|Preemption error rate|0.40%|Below 1% SLO; under 1.45× the load|
|Preemption iterations completed|10 / 10|Standard Spot lost 1 of 10|
|Steady-state error rate|0.00%|Standard Spot recorded 0.18% (33 / 18,083)|
|Burst P95 latency|6,272 ms|On-Demand: 3,713 ms; Standard Spot: 4,434 ms|

All results achieved using only native Kubernetes primitives (Node Affinity, Taints, Tolerations, HPA, Cluster Autoscaler) — no custom controllers.

Standard Spot reduced normalised hourly cost by 71.0 percent relative to On-Demand, but the Hybrid captured roughly half of that discount while retaining a permanent on-demand safety net. Fallback pool size proved to be the dominant cost lever: an earlier Hybrid generation with two on-demand fallback nodes saved only 3.3 percent. Reducing the maximum to one shifted the on-demand share of node-hours from roughly half to one quarter, lifting savings to 36.8 percent. Per-request cost followed the same ordering but compressed the gap with On-Demand to 9.2 percent, because the Hybrid served fewer total requests across a longer phase window.

Recovery behaviour separated the two Spot configurations most clearly. The Hybrid's pre-warmed fallback removed the dependency on GCP provisioning a replacement node, producing a tight 7-to-11-second recovery band (σ = 1.57 s) against Standard Spot's 17-to-34-second range (σ = 5.21 s). A new finding in steady-state telemetry reinforced the pattern: Group B logged 33 failures before any deliberate preemption, most consistent with an uncontrolled GCP-initiated reclamation, while the Hybrid recorded zero steady-state errors over the same period.

Latency was the trade-off. Group A led at every percentile under both steady and burst load. The Hybrid sat highest, with a burst P95 of 6,272 ms and P99 of 8,587 ms compared with Group A's 3,713 ms and 5,928 ms respectively, because roughly 75 percent of its pods ran on shared-core Spot nodes carrying higher CPU contention. During active preemption the Hybrid's P99 stayed below 2.5 seconds, an acceptable overhead for the cost and reliability gains documented above.

The autoscaling result was bounded by the design. Group A scaled from two to ten replicas in 120 seconds. Groups B and Hybrid entered Phase 1 pre-warmed at ten replicas, so cross-group scale-out times are not directly comparable. All three configurations recorded a 0.00 percent burst error rate, confirming that matching the HPA ceiling to the sustainable concurrency on the available hardware eliminates the warm-up errors seen in the previous experimental generation.

The reliability results assume a stateless workload. The CPU-burn microservice handles every request independently and stores no shared data in memory. Pod eviction is therefore a scheduling event rather than a data-loss event, and the measured recovery window covers only Kubernetes rescheduling and pod readiness delays.