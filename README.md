
# Orca: A Distributed Serving System for Transformer-Based Generative Models [[Orca](https://www.usenix.org/conference/osdi22/presentation/yu)]


## Introduction

Transformer-based models are increased in parameter size from GPT-1 100 million to GPT-3 nearly 200 billion, such large-scale model emphasized the need for system support. Current Orca system analyzed the model inferencing process which generate outputs in a iteration based scheduling which can pipeline requests to produce multiple outputs. In this way, inference requests do not need to be waiting for the previous output to proceed to the next request. In this multi-iteration characteristic, Orca can add newly arrived requests and batch with current batch of requests to process all together, overall, the throughput at the same latency range is increased about 40 times compared to the original method.
In this report, we proceeded to improve the overall performance of the model in a higher level in our own way, that is how to efficiently allocate resources in VMs. For instance, there are only a light load of requests coming from users, not all workers are active, so a strategy is to free the resource for other activities. On the other hand, if current workers cannot handle all workloads, maybe allocate more resources by deploying more inference models can reduce the tension. We made two assumptions in this experiment that are the amount of workload follows a predictable trend and the computing power in each request is same. Our evaluation on efficiency and latency shows that dynamically allocate extra or less resources can keep the efficiency at nearly 100% while latency stays the same. Compared to static method, the efficiency will reduce 11.4% to 20.8%, and 31.5% to 41.7% of the time maybe experience longer latency in case of sinusoidal trend.
We also tested deployment time and RAM consumption of load a GPT-3 similar model into a workstation that is running RTX 2070 and 16-core CPU, found that the deploy time is under one minute with peak DRAM usage of 19 GB.



#### Paper link: [Orca: A Distributed Serving System for Transformer-Based Generative Models](https://www.usenix.org/system/files/osdi22-yu.pdf)

## File 1: CS6111_Final.ipynb

NOTE: Run on google collab.

# Data generation:
The generate_data() function uses sine function as basis, we first create time_num*sample_num points uniformly throughout the time_num stamps. Then we apply the values with sine function with random noise. Next, we set the offset_num and scale_num so the range of the data is between 7 to 13 with random noise +or- 10%. If needed, we can tune the time_num, sample_num, offset_num and scale_num for different case. Default we are set the time_num as 24 represents 24 hours in a day, sample_num be 600, scale_num be 3 and offset_num be 10.

# Train-Valid-Test split:

60% training, 20% validation and testing

# NN model implementation:
We use sequential model in Keras because we have exactly one input tensor and one output tenser. Optimizer we used adam, and the loss function we chose MAE. In the model we created, we used a total of seven intermediate layer and tested to have a training loss and validation loss less than 0.1. we used 80 epochs for training and batch size to be 150.

# Plots:
![image](https://user-images.githubusercontent.com/105509461/207440969-39814819-2e1d-4734-96db-8600eb70ef6b.png)
![image](https://user-images.githubusercontent.com/105509461/207440988-5d35a391-9fc7-460b-9a09-da941c97a999.png)
![image](https://user-images.githubusercontent.com/105509461/207440996-07602ae4-955f-42dd-8c64-f28d7f097de3.png)

## File 2: GPT_NEO.ipynb

NOTE: Run on Jupyter lab using anaconda

# Deployment of GPT-3
After previous experiments and analysis, we decided to test the “cost” of deploying a GPT-3 model to account for the time needed to allocate more worker. We found GPT-Neo is an open-source alternative to GPT-3 and is publicly available. GPT-Neo has two versions, 1.3 billion and 2.7 billion parameters. Because GPT-3 Ada also uses 2.7 billion parameters, we decided to test on the larger version. Based on the test, we found deploy 2.7 billion parameters model would take around 45 seconds, DRAM consumption is around 9 GB with peak consumption of 19 GB.

# Model test:

Input: How is the weather?

Output Size: 100

Generated Text: “How is the weather?” And one says, “I heard it’s been a dreadful
night.” In the meantime, the other looks at him and says, “You don’t
know?” So, one is sure to be in the wrong place at the wrong time.

And a man told me a story about a boy who went home for his Christmas
dinner one night. He was hungry, and the cook had put”



