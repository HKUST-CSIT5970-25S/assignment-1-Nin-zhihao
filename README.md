[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)

# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: REN Zhihao
### Student Id: 21072161
### Email: zrenas@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > ### 1. Measurement tool: sysbench
    >
    > ### 2. Instructions
    >
    > Instructions for measuring CPU performance:
    >
    > ```bash
    > sysbench cpu --cpu-max-prime=20000 run
    > ```
    > Parameter settings:
    > `` --cpu-max-prime=20000``: This parameter specifies the maximum number of primes to be computed in the CPU test. This value determines the complexity of the test, `-20000` is a reasonable medium load.
    >
    > Instructions for measuring memory performance:
    >
    > ```bash
    > sysbench memory --memory-block-size=8K --memory-total-size=1G run
    > ```
    > Parameter settings:
    > `` --memory-block-size=8K``: The data block size used for the memory test. 8K is a common choice for most common memory operation loads.
    > `--memory-total-size=1G`: The total amount of memory allocated during the test.
    >
    > ### 3. **Interpretation of measurement results:**
    >
    > #### About the CPU performance:
    >
    > ```bash
    > `total time`: This is the total time in seconds it took to perform the task. The smaller this value is, the more capable your CPU is in performing computational tasks.
    > 
    > `total number of events`: indicates the total number of computational events executed. You can use this value to get an idea of the CPU throughput.
    > 
    > `total time taken by event execution`: the average time taken by each computation event.
    > ```
    >
    > #### About memory performance
    >
    > ```bash
    > `total time`: Indicates the total time in seconds to complete a memory operation. The shorter the test time, the higher the memory bandwidth and access speed.
    > 
    > `total number of events`: indicates the number of memory operations completed.
    > ```

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    

    Answer： 

    1. Relation between CPU performance and number of vCPUs: In this experiment, CPU performance does not improve significantly with increasing vCPUs. t2.micro and t2.medium have almost the same CPU performance, while c5d.large performs worse despite having more vCPUs.
    2. Relation between Memory Performance and Memory Size: Memory performance is significantly better for c5d.large than for t2.micro and t2.medium, suggesting that memory performance may be influenced more by hardware configuration and optimization than just an increase in memory size.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | CPU speed:<br/>    events per second:   894.40<br/><br/>General statistics:<br/>    total time:                          10.0013s<br/>    total number of events:              8947 | General statistics:<br/>    total time:                          2.8025s<br/>    total number of events:              1310720<br/> |
    | `t2.medium`  | CPU speed:<br/>    events per second:   889.67<br/><br/>General statistics:<br/>    total time:                          10.0006s<br/>    total number of events:              8899 | General statistics:<br/>    total time:                          2.8432s<br/>    total number of events:              1310720<br/> |
    | `c5d.large` | CPU speed:<br/>    events per second:   448.99<br/><br/>General statistics:<br/>    total time:                          10.0009s<br/>    total number of events:              4491 | General statistics:<br/>    total time:                          0.6755s<br/>    total number of events:              1310720<br/> |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    Answer:

    1. Overall, there is not much difference in network performance between instances within the same region.
    2. The network performance between instances of the same type is relatively consistent, with TCP bandwidth and RTT showing a relatively stable trend, and c5n.large has the best bandwidth performance.
    3. The network performance varies among different types of instances, and the TCP bandwidth and RTT are affected by the differences in instance types and hardware. 

    | Type                      | TCP b/w (Mbps) | RTT (ms)                                          |
    | ------------------------- | -------------- | ------------------------------------------------- |
    | `t3.medium` - `t3.medium` | 3.41 Gbits/sec | rtt min/avg/max/mdev = 0.350/0.355/0.360/0.003 ms |
    | `m5.large` - `m5.large`   | 3.83 Gbits/sec | rtt min/avg/max/mdev = 0.391/0.398/0.412/0.008 ms |
    | `c5n.large` - `c5n.large` | 4.02 Gbits/sec | rtt min/avg/max/mdev = 0.400/0.405/0.417/0.007 ms |
    | `t3.medium` - `c5n.large` | 3.18 Gbits/sec | rtt min/avg/max/mdev = 0.459/0.466/0.475/0.006 ms |
    | `m5.large` - `c5n.large`  | 3.57 Gbits/sec | rtt min/avg/max/mdev = 0.425/0.435/0.445/0.008 ms |
    | `m5.large` - `t3.medium`  | 4.35 Gbits/sec | rtt min/avg/max/mdev = 0.225/0.237/0.246/0.008 ms |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    Answer:

    1. Network performance within the same region: Within the same region, the TCP bandwidth is very high and the RTT values are very low, indicating that network communication between instances within the same region is very efficient.
    2. Network performance across regions: Network performance across regions is significantly lower than connections in the same region, TCP bandwidth decreases and RTT increases.

    | Connection                | TCP b/w (Mbps) | RTT (ms)                                             |
    | ------------------------- | -------------- | ---------------------------------------------------- |
    | N. Virginia - Oregon      | 32.0 Mbits/sec | rtt min/avg/max/mdev = 58.501/58.516/58.546/0.015 ms |
    | N. Virginia - N. Virginia | 4.43 Gbits/sec | rtt min/avg/max/mdev = 0.212/0.305/0.706/0.179 ms    |
    | Oregon - Oregon           | 4.50 Gbits/sec | rtt min/avg/max/mdev = 0.175/0.274/0.612/0.168 ms    |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
