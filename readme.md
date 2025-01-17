# Proactive Retrieval-based Chatbots based on Relevant Knowledge and Goals

[![made-with-python](https://img.shields.io/badge/Made%20with-Python-red.svg)](#python)

## Abstract
This repository contains the source code and datasets for the SIGIR 2021 paper [Proactive Retrieval-based Chatbots based on Relevant Knowledge and Goals](http://playbigdata.ruc.edu.cn/dou/publication/2021_SIGIR_Proactive_Dialogue_Short.pdf) by Zhu et al. <br>

A proactive dialogue system has the ability to proactively lead the conversation. Different from the general chatbots which only react to the user, proactive dialogue systems can be used to achieve some goals, e.g., to recommend some items to the user. Background knowledge is essential to enable smooth and natural transitions in dialogue. In this paper, we propose a new multi-task learning framework for retrieval-based knowledge-grounded proactive dialogue. To determine the relevant knowledge to be used, we frame knowledge prediction as a complementary task and use explicit signals to supervise its learning. The final response is selected according to the predicted knowledge, the goal to achieve, and the context. Experimental results show that explicit modeling of knowledge prediction and goal selection can greatly improve the final response selection.  

Authors: Yutao Zhu, Jian-Yun Nie, Kun Zhou, Pan Du, Hao Jiang, Zhicheng Dou

## Ablation Study
![Table](/img/table2.png)

We remove the knowledge ("w/o K.") and goal ("w/o G.") from KPN to see the impact of these modules. Besides, we also test modeling goal as a knowledge triplet as in DuRetrieval ("G. as K.").  From the results shown in Table 2, we observe that: <br>

(1) The performance drops sharply when no knowledge is provided. This is consistent with our assumption that the knowledge is vital in proactive dialogue since it drives the proactive dialogue. <br>

(2) Removing the goal also degrades performance, but the difference is not as large as removing the knowledge. A potential reason is that the entities in the goal usually also appear in the knowledge, so they may be partially modeled even without an explicit goal component. <br>

(3) Using the goal as a knowledge triplet (as in DuRetrieval) is not a good idea, and it is even worse than no using the goal at all.
Our explanation is that the goal plays a different role from the knowledge, thus cannot be simply fused together. Our method that uses the goal to determine the target of the whole dialogue, and the knowledge to create a possible path toward it, is a much more appropriate method. 

## Performance with Different Lambdas
![Table](/img/lambda.png)

We use a hyperparameter lambda to control the influence of the KP loss. We study the effect of the supervision signal by varying the value of lambda. As shown in Figure 3, our model performs best with lambda around 0.3 consistently on the validation set of both DuConv and DuReDial. When lambda=0, the knowledge prediction process becomes implicitly supervised by the global loss function (i.e., the RS loss) as in previous studies, and the corresponding performance drops significantly. This confirms the usefulness of leveraging explicit feedback to train knowledge prediction. On the other hand, a large lambda may force the model to focus too much on the KP task and hurt the performance of the RS task.

## Requirements
I test the code with the following packages. Other versions may also work, but I'm not sure. <br>
- Python 3.5 <br>
- Pytorch 1.3.1 (with GPU support)<br>

## Usage
- Download the data from the [link](https://drive.google.com/drive/folders/1nToVlrgQNJ6NADy7Cv7VpfsCTN4DCx_Q?usp=sharing)
- Unzip the data
- python3 runModel.py

The diarectory structure is:
```
KPN
├── data
│   ├── duconv
│   │   ├── dev.pt
│   │   ├── embeddings.pkl
│   │   ├── test.pt
│   │   └── train.pt
│   └── durecdial
│       ├── dev.pt
│       ├── embeddings.pkl
│       ├── test.pt
│       └── train.pt
├── evaluation.py
├── kpn.py
├── output
│   ├── duconv
│   │   └── model
│   └── durecdial
│       └── model
└── runModel.py
```

## Citations
If you use the code and datasets, please cite the following paper:  
```
@inproceedings{ZhuNZDJD21,
  author    = {Yutao Zhu and
               Jian{-}Yun Nie and
               Kun Zhou and
               Pan Du and
               Hao Jiang and
               Zhicheng Dou},
  editor    = {Fernando Diaz and
               Chirag Shah and
               Torsten Suel and
               Pablo Castells and
               Rosie Jones and
               Tetsuya Sakai},
  title     = {Proactive Retrieval-based Chatbots based on Relevant Knowledge and
               Goals},
  booktitle = {{SIGIR} '21: The 44th International {ACM} {SIGIR} Conference on Research
               and Development in Information Retrieval, Virtual Event, Canada, July
               11-15, 2021},
  pages     = {2000--2004},
  publisher = {{ACM}},
  year      = {2021},
  url       = {https://doi.org/10.1145/3404835.3463011},
  doi       = {10.1145/3404835.3463011},
}
```
