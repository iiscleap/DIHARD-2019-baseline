## DIHARD-2019-baseline

#### This repository contains the trained models and instructions to reproduce the [_JHU Kaldi_](https://github.com/kaldi-asr/kaldi/tree/master/egs/dihard_2018/v2) x-vector based Speaker Diarization implementation (called v2 hereon), which is closely based on the JHU's DIHARD 2018 submission, as explained in the paper ['_Diarization is Hard: Some Experiences and Lessons Learned for the JHU Team in the Inaugural DIHARD Challenge_'](http://www.danielpovey.com/files/2018_interspeech_dihard.pdf).

---- 

### Prerequisites
**1.** Kaldi\
**2.** Development and evaluation datasets of DIHARD 2018


### Steps to reproduce v2
**1.** Traverse to the directory of choice (called \<k\> here after) and clone the Kaldi repository using the following command
```
git clone https://github.com/kaldi-asr/kaldi.git 
```
**2.** Copy the run_notrain.sh file into the ```/<k>/kaldi-master/egs/dihard_2018/v2``` directory
```
cp /<mod>/run_notrain.sh /<k>/kaldi-master/egs/dihard_2018/v2
```

**3.** Preparing the DIHARD dataset to use them in Kaldi. As in stage 0 of run.sh in v2 direcrtory, run the following two commands
```
local/make_dihard_2018_dev.sh <path of development data of DIHARD> data/dihard_2018_dev
local/make_dihard_2018_eval.sh <path of development data of DIHARD> data/dihard_2018_eval
```
       
**4.** Run stage 1 of run_notrain.sh. This stage creates MFCCs and cepstral mean normalized MFCCs and saves them separately on disk.

  
**5.** change directory to ```/<k>/kaldi-master/egs/dihard_2018/v2``` . Create a directory called exp/xvector_nnet_1a   
``` 
mkdir -p exp/xvector_nnet_1a
```
       
**6.** Select a directory to clone this repository (say \<mod\>) and execute the following command.
```
git clone https://github.com/iiscleap/DIHARD-2019-baseline.git
```
       
**7.** Copy the final.raw, max_chunk_size, min_chunk_size and extract.config files in <mod> to the created directory in step 5.
 ```
 cp /<mod>/{final.raw, max_chunk_size, min_chunk_size,extract.config} /{k}/kaldi-master/egs/dihard_2018/v2/exp/xvector_nnet_1a
 ```

**8.** Run stage 9 of run_notrain.sh. This stage creates 512 dimension x-vectors for development and evaluation datasets of DIHARD 2018.

**9.** Copy the trained PLDA model given in this repository as shown below. Now score the x-vectors obtained in stage 8, by executing stage 11 of run_notrain.sh. 
```
cp /<mod>/plda /{k}/kaldi-master/egs/dihard_2018/v2/exp/xvector_nnet_1a/xvectors_dihard_2018_dev/
```

**10.** Now run stage 12 to perform Agglomerative Hierarchical Clustering on the scores obtained in step 9, which outputs the Diarization Error Rate (DER) for both development and evaluation parts of DIHARD 2018.



### Results obtained

| DER                     | Reported      | Reproduced  |
| :-------------:         |:-------------:| :-----:     |
| evaluation dataset      | 26.47         |26.27        |
| development dataset     | 20.03         |20.71        |
