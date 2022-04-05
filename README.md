# Cyberattack Detecting Models

## About the project
It aims to implement two unsupervised machine learning models, Cluster-based Local Outlier Factors and Isolation Forest, to identify malicious attacks. 

## Dataset 
A collection of traffic flow (NetFlow) data is used. The dataset is collected from from 01/12/12 03:24:33 to 10/12/12 23:24:32 and contains botnet traffic and normal traffic. Training and test dataset for unsupervised models consist of 14 features; timestamp, duration, protocol, source IP, source port, direction, destination IP, destination port, state, source type service, destination type service, total packets, transmitted bytes from both sides and transmitted bytes from source. In addition to the 14 features, the other datasets include true labels.

| Training Dataset | Valid Dataset| Test Dataset|
| ---------- | - | - |
| 13,882,035 | 940,062 | 1,053,845 |

## Identified Attack Scenarios
Several abnormal traffic patterns were observed in the test dataset from Splunk. Firstly, each traffic of UDP and ICMP protocol take up 21% and 39% in the total traffic flow. In general, UDP volume contributes less than 9% while less than 1% of ICMP volume occurs [1]. Second, port 53 was used the most through UDP and its traffic significantly increased for a short time. Besides, the traffic volume via ICMP protocol soared regularly with a similar pattern over time. Lastly, there was frequent volume change via port 25 which is the most often used in TCP protocol. According to the observed pattern, the test data could include port scan attacks, ICMP flood attack in Distributed Denial of Service (DDoS) and spamming. Therefore, I focuse on these attacks to implement machine learning models and detect them.

## Feature Generation Methods 
| No. | Method | Applied Techniques (in order)|
| ---------- | - | - |
| 1 | A | A1. PCA (optimal dimension = 3)<br>A2. Embedded Selection (Information Entropy) |
| 2 | A+B | B. Aggregation by time-based clustering <br>A1. PCA (optimal dimension = 5)<br>A2. Embedded Selection(Information Entropy) |
| 3 | A+B+C | B. Aggregation of features by time-based clustering <br> C. Pearson's correlation coefficient-based filtering <br>A1. PCA (optimal dimension = 4)<br> A2. Embedded Selection (Information Entropy)|

*Standard Scaler and Normalizer are applied

## Detecting Algorithms
Two well-known anomaly detection algorithms, Cluster-based Local Outlier Factor (CBLOF) and Isolation Forest (IF), are considered for classifiers.
| No. | Model | Tuned Params |
| ---------- | - | - |
| 1 | CBLOF | n_clusrers = 8 <br> contamination = 0.1 |
| 2 | IF | n_estimators = 100 <br> max_samples = 0.25 <br> contamination = 0.1 |

Total 6 combinated models (3 Feature Generation Methods * 2 Detecting Algorithms) are examined to compare detected attacks. 

## Version
Pandas 1.3.1<br>
Numpy 1.19.5<br>
Matplotlib 3.4.2<br>

## References
[1] D. Lee, B. E. Carpenter and N. Brownlee. (2010). Observations of UDP to TCP Ratio and Port Numbers. 2010 Fifth International Conference on Internet Monitoring and Protection, Barcelona, pp. 99-104, doi: 10.1109/ICIMP.2010.20.
