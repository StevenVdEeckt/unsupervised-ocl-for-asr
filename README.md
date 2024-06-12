# Unsupervised Online Continual Learning for Automatic Speech Recognition
Supplementary material to the paper "Unsupervised Online Continual Learning for Automatic Speech Recognition", accepted for Interspeech 2024. 

To cite the paper, use:
```
@inproceedings{vandereeckt_interspeech2024,
  author = {Vander Eeckt, Steven and Van hamme, Hugo},  
  title = {Unsupervised Online Continual Learning for Automatic Speech Recognition},
  booktitle = {Proc. Interspeech 2024},
  year = {2024},  
}
```

## ESPnet

ESPnet [Wanatabe et al., 2018] is used for all experiments. More specifically, we use ESPnet2.

## data

The data folder contains information regarding the data of the two experiments. More specifically, it contains a list of utterances per dataset. 

### Experiment 1

For Experiment 1, Librispeech-360h is adapted to six Libri-Adapt tasks, which are a combination of three accents (United States [us], India [in] and Great Britian [gb]) and two microphones (USB [u], Matrix [m]). These six tasks are merged in one training set, over which the model does one pass (since the paper considers **online** CL). During training, however, the utterances are sorted by task and by speaker. Table below contains the number of utterances per training set:

task  | dataset | #utterances 
------------- | ------------- | :-------------: 
lib | Librispeech | 104.0k
libapt | Libri-Adapt | 91.6k

### Experiment 2

For Experiment 2, the initial task is again Librispeech-360h. This task is first adapted to three accents from the Common Voice English dataset (7.0 release - July 2021). These accents are: Australia (aus), India (ind) and Ireland (ire). Next, it is adapted to a subset from the TEDLIUM dataset. The tasks are again merged in one large training set over which the model does again one pass. During training, utterances are again sorted by task and by speaker. The utterances for the training sets are given below:

task  | dataset | #utterances 
------------- | ------------- | :-------------: 
lib | Librispeech | 104.0k
cv_ted | Common Voice + TEDLIUM | 236.2k

## hyper-parameters

To determine the hyperparameters, a small 'test experiment' was used for both experiments. The data for these test experiments is unused (for the actual experiment) data from Libri-Adapt and Common Voice English. Table below gives an overview of which and how much data was used per experiment: 

experiment  | dataset  | subset  | #utterances 
------------- | ------------- | ------------- | :-------------: 
Experiment 1 | Libri-Adapt | gb-Nexus6, in-Respeaker |  18.2k
Experiment 2 | Common Voice (English) | Accent: United States | 20.0k

To determine the optimal value for a hyper-parameter, we try four different values and choose the value that results in the lowest Avg. WER, where Avg. WER for the 'test experiment' is computed as: $Avg.WER=(2*WER_{initial\_task}+WER_{test})/3$ (Since the 'test' task is so small, forgetting is expected to be less of an issue in the 'test experiment' than in the 'real experiment'. To make sure we still select a hyper-parameter value that overcomes catastrophic forgetting sufficiently, we give $WER_{initial\_task}$ of initial task us a higher weight).  The hyper-parameter value with the lowest Avg. WER is considered the optimal one.

We optimized the hyper-parameters for each method only on supervised OCL. The same values were then used in the unsupervised OCL methods. 

For ER, we consider the implementation of [Vander Eeckt and Van hamme, 2022]. For AOS, we only optimized the parameter $\tau$, and took the default values for $\tau_2$ and $\lambda$, to limit the number of parameter to tune and since in [Vander Eeckt and Van hamme, 2023] $\tau$ seemed to be the most crucial parameter. 

### Experiment 1

The table below shows the hyper-parameters optimized for each method, as well as the values tested, the default value and the optimal value. Between square brackets for the two last columns is the $Avg.WER$ for the given setting on the test experiment. 

method  | hyper-parameters | values tested | default | optimal
------------- | ------------- | ------------- | ------------- | ------------- 
ER | $\lambda$ | $(0.1, 0.2, 0.3, 0.5)$ | $0.1$ $[7.36]$ | $0.5$ $[6.84]$
AOS | $\tau$, $\tau_2$, $\lambda$ | $(1, 5, 10, 20)$, $(1)$, $(0.1)$ | $1, 1, 0.1$ $[8.01]$ | $10, 1, 0.1$ $[7.23]$

### Experiment 2

The table below shows the hyper-parameters optimized for each method, as well as the values tested, the default value and the optimal value. Between square brackets for the two last columns is the $Avg.WER$ for the given setting on the test experiment. 

method  | hyper-parameters | values tested | default | optimal
------------- | ------------- | ------------- | ------------- | ------------- 
ER | $\lambda$ | $(0.1, 0.2, 0.3, 0.5)$ | $0.1$ $[9.40]$ | $0.1$ $[9.40]$
AOS | $\tau$, $\tau_2$, $\lambda$ | $(1, 5, 10, 20)$, $(1)$, $(0.1)$ | $1, 1, 0.1$ $[10.09]$ | $20, 1, 0.1$ $[9.35]$








