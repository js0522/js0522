
# Orca: A Distributed Serving System for Transformer-Based Generative Models [[Orca](https://www.usenix.org/conference/osdi22/presentation/yu)]


## Introduction

Transformer-based models are increased in parameter size from GPT-1 100 million to GPT-3 nearly 200 billion, such large-scale model emphasized the need for system support. Current Orca system analyzed the model inferencing process which generate outputs in a iteration based scheduling which can pipeline requests to produce multiple outputs. In this way, inference requests do not need to be waiting for the previous output to proceed to the next request. In this multi-iteration characteristic, Orca can add newly arrived requests and batch with current batch of requests to process all together, overall, the throughput at the same latency range is increased about 40 times compared to the original method.
In this report, we proceeded to improve the overall performance of the model in a higher level in our own way, that is how to efficiently allocate resources in VMs. For instance, there are only a light load of requests coming from users, not all workers are active, so a strategy is to free the resource for other activities. On the other hand, if current workers cannot handle all workloads, maybe allocate more resources by deploying more inference models can reduce the tension. We made two assumptions in this experiment that are the amount of workload follows a predictable trend and the computing power in each request is same. Our evaluation on efficiency and latency shows that dynamically allocate extra or less resources can keep the efficiency at nearly 100% while latency stays the same. Compared to static method, the efficiency will reduce 11.4% to 20.8%, and 31.5% to 41.7% of the time maybe experience longer latency in case of sinusoidal trend.
We also tested deployment time and RAM consumption of load a GPT-3 similar model into a workstation that is running RTX 2070 and 16-core CPU, found that the deploy time is under one minute with peak DRAM usage of 19 GB.



#### Paper link: [Orca: A Distributed Serving System for Transformer-Based Generative Models](https://www.usenix.org/system/files/osdi22-yu.pdf)

## File 1: CS6111_Final.ipynb

## File 2: GPT_NEO.ipynb

## Running the experiments


