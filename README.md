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

For Experiment 1, Librispeech-360h is adapted to six Libri-Adapt [Mathur et al., 2020] tasks, which are a combination of three accents (United States [us], India [in] and Great Britian [gb]) and two microphones (USB [u], Matrix [m]). These six tasks are merged in one training set, over which the model does one pass (since the paper considers **online** CL). During training, however, the utterances are sorted by task and by speaker. Table below contains the number of utterances per training set:

task  | dataset | #utterances 
------------- | ------------- | :-------------: 
lib | Librispeech | 104.0k
libapt | Libri-Adapt | 91.6k

### Experiment 2

For Experiment 2, the initial task is again Librispeech-360h [Panayotov et. al]. This task is first adapted to three accents from the Mozilla Common Voice English dataset (7.0 release - July 2021) [Ardila et al., 2020]. These accents are: Australia (aus), India (ind) and Ireland (ire). Next, it is adapted to a subset from the TEDLIUM-3 dataset [Hernandez et al., 2018]. The tasks are again merged in one large training set over which the model does again one pass. During training, utterances are again sorted by task and by speaker. The utterances for the training sets are given below:

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


## models

The model directory contains information regarding the models used in the experiments. In the conf directory, one can find the configuration files for (training) the models as well as for testing. Under training, for each experiment, one can find:
* a directory named 'initial_model' with the training configuration file for the model trained (using labeled data) on task $T_0$.
* a directory named 'ocl' with the training configuration file for the supervised OCL methods.
* a directory named 'uocl' with the training configuration file for the unsupervised OCL methods.

Note that in our ESPnet implementation of ER and AOS, $\lambda$ was named $alpha$. 

In addition to the model configuration files, the models directory contains SentencePiece model [Kudo and Richardson, 2018] used as output tokens for the ASR model. 

## statistical significance

Statistical significance of the results was tested by computing the number of errors per utterance [Strik et al., 2000] for each method and comparing two methods with Wilcoxon sign-rank test. Three levels of significance were considered: 0.05 (\*), 0.01 (\*\*) and 0.001 (\*\*\*).

Below are the results from the statistical significance tests. Note that a green color means that the method corresponding to the row was significantly better than the method corresponding to the column with the given signifcance level, while a red color means that the former was worse than the latter and a yellow color that there was no significant difference between the two.  

### Experiment 1

Table below shows the statistical significances for all (OCL and UOCL) methods:

![Significance Experiment 1](https://github.com/StevenVdEeckt/unsupervised-ocl-for-asr/blob/main/significance_testing_Experiment1_Results.png)


### Experiment 2

Table below shows the statistical significances for all (OCL and UOCL) methods:

![Significance Experiment 2](https://github.com/StevenVdEeckt/unsupervised-ocl-for-asr/blob/main/significance_testing_Experiment2_Results.png)

Note that significance results for the ablation study are also available. 

## references

[Ardila et al., 2020] Ardila et al., “Common voice: A massively-multilingual speech corpus,” in Proceedings of the 12th Conference on Language Resources and Evaluation (LREC 2020), 2020, pp. 4211–4215.

[Hernandez et al., 2018] F. Hernandez et al., “Ted-lium 3: Twice as much data and corpus repartition for experiments on speaker adaptation,” in Speech and Computer, 2018, pp. 198–208.

[Kudo and Richardson, 2018] T. Kudo and J. Richardson, “SentencePiece: A simple and language independent subword tokenizer and detokenizer for neural text processing,” in Proceedings of the 2018 Conference on Empirical Methods in Natural Language Processing: System Demonstrations. Association for Computational Linguistics, 2018, pp. 66–71.

[Mathur et al., 2020] A. Mathur et al., “Libri-adapt: a new speech dataset for unsupervised domain adaptation,” 2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), pp. 7439–7443, 2020.

[Panayotov et. al] V. Panayotov et. al, "Librispeech: An ASR corpus based on public domain audio books," 2015 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), 2015, pp. 5206-5210.


[Strik et al., 2000] Helmer Strik, Catia Cucchiarini, and Judith M. Kessens, “Comparing the recognition performance of csrs: in search of an adequate metric and statistical significance test,” in INTERSPEECH, 2000.

[Vander Eeckt and Van hamme, 2022] S. Vander Eeckt and H. Van hamme, “Continual learning for monolingual end-to-end automatic speech recognition,” Proceedings EUSIPCO 2022, 2022.

[Vander Eeckt and Van hamme, 2023] S. Vander Eeckt and H. Van hamme, “Rehearsal-free online continual learning for automatic speech recognition,” in Proc. Interspeech 2023, 2023.

[Wanatabe et al., 2018] S. Watanabe et al., “ESPnet: End-to-end speech processing toolkit,” in Proceedings of Interspeech, 2018, pp. 2207–2211.









