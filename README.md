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

For Experiment 1, Librispeech-360h is adapted to six Libri-Adapt tasks, which are a combination of three accents (United States [us], India [in] and Great Britian [gb]) and two microphones (USB [u], Matrix [m]). These six tasks are merged in one training set, over which the model does one pass (since the paper considers **online** CL). During training, however, the utterances are sorted by task and by speaker. 

For Experiment 2, the initial task is again Librispeech-360h. This task is first adapted to three accents from the Common Voice English dataset (7.0 release - July 2021). Next, it is adapted to a subset from the TEDLIUM dataset. The tasks are again merged in one large training set over which the model does again one pass. During training, utterances are again sorted by task and by speaker. 

