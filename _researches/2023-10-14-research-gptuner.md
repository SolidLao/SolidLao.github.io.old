---
title: "GPTuner"
date: 2023-10-14
priority: 1
excerpt: "GPTuner is a database tuning system that leverages Large Language Model to handle domain knowledge and then enhance knob tuning procedure"
layout: single
author_profile: true
classes: wide
advisors:
  - name: "Prof. Jianguo Wang from Purdue University"
    url: "https://www.cs.purdue.edu/homes/csjgwang/"
  - name: "Prof. Mingjie Tang from Sichuan University"
    url: "http://merlintang.github.io/"
outcome:
  paper_title: "GPTuner: A Manual-Reading Database Tuning System via GPT-Guided Bayesian Optimization"
  authors: 
    - "Jiale Lao"
    - "Yibo Wang"
    - "Yufei Li"
    - "Zhiyuan Chen"
    - "Yunjia Zhang"
    - "Mingjie Tang"
    - "Jianguo Wang"
  paper_state: "Proceedings of Very Large Data Bases Conference (VLDB), Under Revision, 2024."
---

## Advisor

- [Prof. Jianguo Wang](https://www.cs.purdue.edu/homes/csjgwang/) from [Purdue University](https://www.purdue.edu/)
- [Prof. Mingjie Tang](http://merlintang.github.io/) from [Sichuan University](https://www.scu.edu.cn/)

## Publication

GPTuner: A Manual-Reading Database Tuning System via GPT-Guided Bayesian Optimization [\[paper\]](https://arxiv.org/abs/2311.03157) [\[code\]](https://github.com/SolidLao/GPTuner) [\[video\]](https://www.youtube.com/watch?v=Hz5Zck-9TlA)
- **Jiale Lao**, Yibo Wang, Yufei Li, Zhiyuan Chen, Yunjia Zhang, Mingjie Tang, Jianguo Wang
- In submission to VLDB 2024

## Abstract
Modern database management systems (DBMS) expose hundreds of configurable knobs to control system behaviours. Determining the appropriate values for these knobs to improve DBMS performance is a long-standing problem in the database community. As there is an increasing number of knobs to tune and each knob could be in continuous or categorical values, manual tuning becomes impractical. Recently, automatic tuning systems using machine learning methods have shown great potentials. However, existing approaches still incur significant tuning costs or only yields sub-optimal performance. This is because they either ignore the extensive domain knowledge available (e.g., DBMS manuals and forum discussions) and only rely on the runtime feedback of benchmark evaluations to guide the optimization, or they utilize the domain knowledge in a limited way. Hence, we propose GPTuner, a manual-reading database tuning system that leverages domain knowledge extensively and automatically to optimize search space and enhance the runtime feedback-based optimization process. Firstly, we develop a Large Language Model (LLM)-based pipeline to collect and refine heterogeneous knowledge, and propose a prompt ensemble algorithm to unify a structured view of the refined knowledge. Secondly, using the structured knowledge, we (1) design a workload-aware and training-free knob selection strategy, (2) develop a search space optimization technique considering the value range of each knob, and (3) propose a Coarse-to-Fine Bayesian Optimization Framework to explore the optimized space. Finally, we evaluate GPTuner under different benchmarks (TPC-C and TPC-H), metrics (throughput and latency) as well as DBMS (PostgreSQL and MySQL). Compared to the state-of-the-art approaches, GPTuner identifies better configurations in **16x** less time on average. Moreover, GPTuner achieves up to **30%** performance improvement (higher throughput or lower latency) over the **best-performing** alternative.


## System Overview
![GPTuner](/assets/images/gptuner_overview.png)

**GPTuner** is a manual-reading database tuning system to suggest satisfactory knob configurations with reduced tuning costs. The figure above presents the tuning workflow that involves seven steps:
1. 📌 User provides the DBMS to be tuned (e.g., PostgreSQL or MySQL), the target workload, and the optimization objective (e.g., latency or throughput).
2. 📌 GPTuner collects and refines the heterogeneous knowledge from different sources (e.g., GPT-4, DBMS manuals, and web forums) to construct _Tuning Lake_, a collection of DBMS tuning knowledge.
3. 📌 GPTuner unifies the refined tuning knowledge from _Tuning Lake_ into a structured view accessible to machines (e.g., JSON).
4. 📌 GPTuner reduces the search space dimensionality by selecting important knobs to tune (i.e., fewer knobs to tune means fewer dimensions).
5. 📌 GPTuner optimizes the search space in terms of the value range for each knob based on structured knowledge.
6. 📌 GPTuner explores the optimized space via a novel Coarse-to-Fine Bayesian Optimization framework.
7. 📌 Finally, GPTuner identifies satisfactory knob configurations within resource limits (e.g., the maximum optimization time or iterations specified by users). 

## Experimental Result

### Baselines
We compare GPTuner with state-of-the-art methods both using or not using natural language knowledge as input:
- [DB-BERT SIGMOD'22](https://dl.acm.org/doi/10.1145/3514221.3517843): a DBMS tuning tool that uses BERT to read the manuals and use the gained information to guide Reinforcement Learning(RL)
- SMAC: the best Bayesian Optimiztion (BO)-based method evaluated in an [Experimental Evaluation VLDB'22](https://dl.acm.org/doi/10.14778/3538598.3538604)
- GP: the classic Gassian Process-based BO approach used in [iTuned VLDB'09](https://dl.acm.org/doi/10.14778/1687627.1687767) and [OtterTune SIGMOD'17](https://dl.acm.org/doi/10.1145/3035918.3064029)
- DDPG++: a RL-based tuning method proposed in [CDBTune SIGMOD'19](https://dl.acm.org/doi/10.1145/3299869.3300085) and improved in [Inquiry VLDB'21](https://dl.acm.org/doi/10.14778/3450980.3450992)

### Result on PostgreSQL
We compare GPTuner with baselines on different DBMS (PostgreSQL and MySQL), benchmarks (TPC-H and TPC-C) and metrics (throughput and latency). We present the results on PostgreSQL in this post. For more details, please refer to our paper.

![Result](/assets/images/gptuner_result_postgresql.png)