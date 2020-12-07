<p align="center">
    <br>
    <img src="./pics/banner.png" width="500"/>
    <br>
</p>
<p align="center">
    <a href="https://github.com/ymcui/MacBERT/blob/master/LICENSE">
        <img alt="GitHub" src="https://img.shields.io/github/license/ymcui/MacBERT.svg?color=blue&style=flat-square">
    </a>
</p>

This repository contains the resources in our paper **"Revisiting Pre-trained Models for Chinese Natural Language Processing"**, which will be published in "[Findings of EMNLP](https://2020.emnlp.org)". You can read our camera-ready paper through [ACL Anthology](#) or [arXiv pre-print](https://arxiv.org/abs/2004.13922).


**[Revisiting Pre-trained Models for Chinese Natural Language Processing](https://arxiv.org/abs/2004.13922)**  
*Yiming Cui, Wanxiang Che, Ting Liu, Bing Qin, Shijin Wang, Guoping Hu*


有关MacBERT以外的资源，请访问以下repository：
- Chinese BERT-wwm series: https://github.com/ymcui/Chinese-BERT-wwm
- Chinese ELECTRA: https://github.com/ymcui/Chinese-ELECTRA
- Chinese XLNet: https://github.com/ymcui/Chinese-XLNet

HFL的更多资源 : https://github.com/ymcui/HFL-Anthology


## News
**[Nov 3, 2020] 预训练的MacBERT模型可通过直接下载  [Download](#Download) or [Quick Load](#Quick-Load). 就像使用原始BERT一样使用它  (除了它不能执行原始的MLM ).**

[Sep 15, 2020] Our paper ["Revisiting Pre-Trained Models for Chinese Natural Language Processing"](https://arxiv.org/abs/2004.13922) is accepted to [Findings of EMNLP](https://2020.emnlp.org) as a long paper.


## Guide
| Section | 描述 |
|-|-|
| [Introduction](#Introduction) | 介绍 MacBERT |
| [Download](#Download) | 下载链接 MacBERT |
| [Quick Load](#Quick-Load) | 使用transformers快速加载 [🤗Transformers](https://github.com/huggingface/transformers) |
| [Results](#Results) | 在几个Chinese NLP datasets的结果 |
| [FAQ](#FAQ) | 常见问题|
| [Citation](#Citation) | Citation |


## Introduction
MacBERT是经过改进的BERT，具有新颖的MLM作为校正预训练任务，从而减轻了预训练和微调的差异。 

我们提议不要使用[MASK]token进行masking，因为token不会出现在微调阶段，我们提议使用相似的单词进行masking。 
通过使用基于word2vec(Mikolov et al.,2013)相似度计算的同义词工具包(Wang and Hu，2017)获得相似的单词。 
如果选择一个N-gram进行masked，我们将分别找到相似的单词。 在极少数情况下，当没有相似的单词时，我们会降级以使用随机单词替换。 

这是我们训练前任务的一个样本。
|  | Example       |
| -------------- | ----------------- |
| **Original Sentence**  | we use a language model to predict the probability of the next word. |
|  **MLM** | we use a language [M] to [M] ##di ##ct the pro [M] ##bility of the next word . |
| **Whole word masking**   | we use a language [M] to [M] [M] [M] the [M] [M] [M] of the next word . |
| **N-gram masking** | we use a [M] [M] to [M] [M] [M] the [M] [M] [M] [M] [M] next word . |
| **MLM as correction** | we use a text system to ca ##lc ##ulate the po ##si ##bility of the next word . |

除了新的预训练任务，我们还采用了以下技术。 

- Whole Word Masking (WWM)
- N-gram masking
- Sentence-Order Prediction (SOP)

**请注意，我们的MacBERT可以直接用原始BERT替换，因为主要神经体系结构没有差异**

有关更多技术细节，请检查我们的论文 : [Revisiting Pre-trained Models for Chinese Natural Language Processing](https://arxiv.org/abs/2004.13922)


## Download
我们在TensorFlow 1.x中提供经过预训练的MacBERT模型。 

* **`MacBERT-large, Chinese`**: 24-layer, 1024-hidden, 16-heads, 324M parameters   
* **`MacBERT-base, Chinese`**：12-layer, 768-hidden, 12-heads, 102M parameters   

| Model                                |                         Google Drive                         |                        iFLYTEK Cloud                         | Size |
| :----------------------------------- | :----------------------------------------------------------: | :----------------------------------------------------------: | :--: |
| **`MacBERT-large, Chinese`**    | [TensorFlow](https://drive.google.com/file/d/1lWYxnk1EqTA2Q20_IShxBrCPc5VSDCkT/view?usp=sharing) | [TensorFlow（pw:3Yg3）](http://pan.iflytek.com:80/link/805D743F3826EC4F4EB5C774D34432AE) | 1.2G |
| **`MacBERT-base, Chinese`**     | [TensorFlow](https://drive.google.com/file/d/1aV69OhYzIwj_hn-kO1RiBa-m8QAusQ5b/view?usp=sharing) | [TensorFlow（pw:E2cP）](http://pan.iflytek.com:80/link/CF2A1F9AEBF859650E8956854A994C1B) | 383M |

### PyTorch/TensorFlow2 Version

如果您需要在PyTorch / TensorFlow2中使用这些模型， 

1) 将TensorFlowcheckpoint转换为PyTorch / TensorFlow2 , 使用这个工具 [🤗Transformers](https://github.com/huggingface/transformers)

2) 从https://huggingface.co/hfl下载 
步骤：在上面的页面中选择一个模型→单击模型页面末尾的“列出模型中的所有文件”→从弹出窗口下载bin / json文件。 


## Quick Load
With [Huggingface-Transformers](https://github.com/huggingface/transformers), 上面的模型可以通过以下代码轻松访问和加载。 
```
tokenizer = BertTokenizer.from_pretrained("MODEL_NAME")
model = BertModel.from_pretrained("MODEL_NAME")
```
**注意力：请使用Bert Tokenizer和BertModel加载MacBERT模型。   **

实际模型及其“MODEL_NAME”在下面列出。 

| Original Model | MODEL_NAME                |
| -------------- | ------------------------- |
| MacBERT-large  | hfl/chinese-macbert-large |
| MacBERT-base   | hfl/chinese-macbert-base  |

## Results
我们介绍了MacBERT在以下六个任务上的结果(请阅读我们的论文以获取其他结果)。 
- [**CMRC 2018 (Cui et al., 2019)**：Span-Extraction Machine Reading Comprehension (Simplified Chinese)](https://github.com/ymcui/cmrc2018)
- [**DRCD (Shao et al., 2018)**：Span-Extraction Machine Reading Comprehension (Traditional Chinese)](https://github.com/DRCSolutionService/DRCD)
- [**XNLI (Conneau et al., 2018)**：Natural Langauge Inference](https://github.com/google-research/bert/blob/master/multilingual.md)
- [**ChnSentiCorp**：Sentiment Analysis](https://github.com/pengming617/bert_classification)
- [**LCQMC (Liu et al., 2018)**：Sentence Pair Matching](http://icrc.hitsz.edu.cn/info/1037/1146.htm)
- [**BQ Corpus (Chen et al., 2018)**：Sentence Pair Matching](http://icrc.hitsz.edu.cn/Article/show/175.html)

为了确保结果的稳定性，我们为每个实验运行10次，并报告最高和平均分数(在方括号中)。

### CMRC 2018
[CMRC 2018 dataset](https://github.com/ymcui/cmrc2018) 由HIT和iFLYTEK Research联合实验室发布。 模型应根据给定的段落回答问题 , which is identical to [SQuAD](http://arxiv.org/abs/1606.05250). Evaluation metrics: EM / F1

| Model                     |        Development        |           Test            |         Challenge         | #Params |
| :------------------------ | :-----------------------: | :-----------------------: | :-----------------------: | :-----: |
| BERT-base                 | 65.5 (64.4) / 84.5 (84.0) | 70.0 (68.7) / 87.0 (86.3) | 18.6 (17.0) / 43.3 (41.3) |  102M   |
| BERT-wwm                  | 66.3 (65.0) / 85.6 (84.7) | 70.5 (69.1) / 87.4 (86.7) | 21.0 (19.3) / 47.0 (43.9) |  102M   |
| BERT-wwm-ext              | 67.1 (65.6) / 85.7 (85.0) | 71.4 (70.0) / 87.7 (87.0) | 24.0 (20.0) / 47.3 (44.6) |  102M   |
| RoBERTa-wwm-ext           | 67.4 (66.5) / 87.2 (86.5) | 72.6 (71.4) / 89.4 (88.8) | 26.2 (24.6) / 51.0 (49.1) |  102M   |
| ELECTRA-base          | 68.4 (68.0) / 84.8 (84.6) | 73.1 (72.7) / 87.1 (86.9) | 22.6 (21.7) / 45.0 (43.8) |  102M   |
| **MacBERT-base** | 68.5 (67.3) / 87.9 (87.1) |73.2 (72.4) / 89.5 (89.2)|30.2 (26.4) / 54.0 (52.2)|102M|
| ELECTRA-large         |        69.1 (68.2) / 85.2 (84.5)        |        73.9 (72.8) / 87.1 (86.6)        |        23.0 (21.6) / 44.2 (43.2)        |  324M   |
| RoBERTa-wwm-ext-large | 68.5 (67.6) / 88.4 (87.9) | 74.2 (72.4) / 90.6 (90.0) | 31.5 (30.1) / 60.1 (57.5) |324M|
| **MacBERT-large** | 70.7 (68.6) / 88.9 (88.2) | 74.8 (73.2) / 90.7 (90.1) | 31.9 (29.6) / 60.2 (57.6) | 324M |


### DRCD
[DRCD](https://github.com/DRCKnowledgeTeam/DRCD) 也是Delta研究中心发布的跨度提取机器阅读理解数据集。 繁体中文写成. Evaluation metrics: EM / F1

| Model                     |        Development        |           Test            | #Params |
| :------------------------ | :-----------------------: | :-----------------------: | :-----: |
| BERT-base                 | 83.1 (82.7) / 89.9 (89.6) | 82.2 (81.6) / 89.2 (88.8) |  102M   |
| BERT-wwm                  | 84.3 (83.4) / 90.5 (90.2) | 82.8 (81.8) / 89.7 (89.0) |  102M   |
| BERT-wwm-ext              | 85.0 (84.5) / 91.2 (90.9) | 83.6 (83.0) / 90.4 (89.9) |  102M   |
| RoBERTa-wwm-ext           | 86.6 (85.9) / 92.5 (92.2) | 85.6 (85.2) / 92.0 (91.7) |  102M   |
| ELECTRA-base          | 87.5 (87.0) / 92.5 (92.3) | 86.9 (86.6) / 91.8 (91.7) |  102M   |
| **MacBERT-base** | 89.4 (89.2) / 94.3 (94.1) | 89.5 (88.7) / 93.8 (93.5) | 102M |
| ELECTRA-large         |        88.8 (88.7) / 93.3 (93.2)        |        88.8 (88.2) / 93.6 (93.2)        |  324M   |
| RoBERTa-wwm-ext-large | 89.6 (89.1) / 94.8 (94.4) | 89.6 (88.9) / 94.5 (94.1) |324M|
| **MacBERT-large** | 91.2 (90.8) / 95.6 (95.3) | 91.7 (90.9) / 95.6 (95.3) |324M|


### XNLI
We use [XNLI](https://github.com/google-research/bert/blob/master/multilingual.md) data for testing the NLI task. Evaluation metrics: Accuracy

| Model                     | Development |    Test     | #Params |
| :------------------------ | :---------: | :---------: | :-----: |
| BERT-base                 | 77.8 (77.4) | 77.8 (77.5) |  102M   |
| BERT-wwm                  | 79.0 (78.4) | 78.2 (78.0) |  102M   |
| BERT-wwm-ext              | 79.4 (78.6) | 78.7 (78.3) |  102M   |
| RoBERTa-wwm-ext           | 80.0 (79.2) | 78.8 (78.3) |  102M   |
| ELECTRA-base          | 77.9 (77.0) | 78.4 (77.8) |  102M   |
| **MacBERT-base** | 80.3 (79.7) | 79.3 (78.8) | 102M |
| ELECTRA-large         |    81.5 (80.8)    |    81.0 (80.9)    |  324M   |
| RoBERTa-wwm-ext-large | 82.1 (81.3) | 81.2 (80.6) |324M|
| **MacBERT-large** | 82.4 (81.8) | 81.3 (80.6) |324M|


### ChnSentiCorp
We use [ChnSentiCorp](https://github.com/pengming617/bert_classification) 数据测试情感分析. Evaluation metrics: Accuracy

| Model                     | Development |    Test     | #Params |
| :------------------------ | :---------: | :---------: | :-----: |
| BERT-base                 | 94.7 (94.3) | 95.0 (94.7) |  102M   |
| BERT-wwm                  | 95.1 (94.5) | 95.4 (95.0) |  102M   |
| BERT-wwm-ext              | 95.4 (94.6) | 95.3 (94.7) |  102M   |
| RoBERTa-wwm-ext           | 95.0 (94.6) | 95.6 (94.8) |  102M   |
| ELECTRA-base          | 93.8 (93.0) | 94.5 (93.5) |  102M   |
| **MacBERT-base** | 95.2 (94.8) | 95.6 (94.9) | 102M |
| ELECTRA-large         |    95.2 (94.6)    |    95.3 (94.8)    |  324M   |
| RoBERTa-wwm-ext-large | 95.8 (94.9) | 95.8 (94.9) |324M|
| **MacBERT-large** | 95.7 (95.0) | 95.9 (95.1) | 324M |


### LCQMC
[**LCQMC**](http://icrc.hitsz.edu.cn/info/1037/1146.htm) 是一个句子对匹配数据集，可以看作是二分类任务. Evaluation metrics: Accuracy

| Model                     | Development |    Test     | #Params |
| :------------------------ | :---------: | :---------: | :-----: |
| BERT                      | 89.4 (88.4) | 86.9 (86.4) |  102M   |
| BERT-wwm                  | 89.4 (89.2) | 87.0 (86.8) |  102M   |
| BERT-wwm-ext              | 89.6 (89.2) | 87.1 (86.6) |  102M   |
| RoBERTa-wwm-ext           | 89.0 (88.7) | 86.4 (86.1) |  102M   |
| ELECTRA-base          | 90.2 (89.8) | 87.6 (87.3) |  102M   |
| **MacBERT-base** | 89.5 (89.3) | 87.0 (86.5) | 102M |
| ELECTRA-large         |    90.7 (90.4)    |    87.3 (87.2)    |  324M   |
| RoBERTa-wwm-ext-large | 90.4 (90.0) | 87.0 (86.8) |324M|
| **MacBERT-large** | 90.6 (90.3) | 87.6 (87.1) | 324M |

### BQ Corpus 
[**BQ Corpus**](http://icrc.hitsz.edu.cn/Article/show/175.html) 是一个句子对匹配数据集，可以看作是二分类任务 . Evaluation metrics: Accuracy

| Model                     | Development |    Test     | #Params |
| :------------------------ | :---------: | :---------: | :-----: |
| BERT                      | 86.0 (85.5) | 84.8 (84.6) |  102M   |
| BERT-wwm                  | 86.1 (85.6) | 85.2 (84.9) |  102M   |
| BERT-wwm-ext              | 86.4 (85.5) | 85.3 (84.8) |  102M   |
| RoBERTa-wwm-ext           | 86.0 (85.4) | 85.0 (84.6) |  102M   |
| ELECTRA-base          | 84.8 (84.7) | 84.5 (84.0) |  102M   |
| **MacBERT-base** | 86.0 (85.5) | 85.2 (84.9) | 102M |
| ELECTRA-large         |    86.7 (86.2)    |    85.1 (84.8)    |  324M   |
| RoBERTa-wwm-ext-large | 86.3 (85.7) | 85.8 (84.9) |324M|
| **MacBERT-large** | 86.2 (85.7) | 85.6 (85.0) | 324M |

## FAQ
**Question 1: 你有英文版的MacBERT吗 ?**

A1: 抱歉，我们没有英语版本的预训练的MacBERT。 

**Question 2: How to use MacBERT?**

A2: Use it as if you are using original BERT in the fine-tuning stage (just replace the checkpoint and config files). Also, you can perform further pre-training on our checkpoint with MLM/NSP/SOP objectives. 

**Question 3: 您能提供MacBERT的预训练代码吗 ?**

A3: 抱歉，我们目前无法提供源代码，也许我们会在将来发布它们，但不能保证。 

**Question 4: 如何发布预训练数据 ?**

A4: 我们无权重新分发这些数据，因为这些数据可能会违反法律。 

**Question 5: Will you release pre-trained MacBERT on a larger data?**

A5: Currently, we have no plans on this.

## Citation
If you find our resource or paper is useful, please consider including the following citation in your paper.
- https://arxiv.org/abs/2004.13922
```
@inproceedings{cui-etal-2020-revisiting,
    title = "Revisiting Pre-Trained Models for {C}hinese Natural Language Processing",
    author = "Cui, Yiming  and
      Che, Wanxiang  and
      Liu, Ting  and
      Qin, Bing  and
      Wang, Shijin  and
      Hu, Guoping",
    booktitle = "Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing: Findings",
    month = nov,
    year = "2020",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://www.aclweb.org/anthology/2020.findings-emnlp.58",
    pages = "657--668",
}
```

## Acknowledgment
The first author would like to thank [Google TensorFlow Research Cloud (TFRC) Program](https://www.tensorflow.org/tfrc).

## Issues
Before you submit an issue:

- **You are advised to read [FAQ](#FAQ) first before you submit an issue.** 
- Repetitive and irrelevant issues will be ignored and closed by [stable-bot](stale · GitHub Marketplace). Thank you for your understanding and support.
- We cannot acommodate EVERY request, and thus please bare in mind that there is no guarantee that your request will be met.
- Always be polite when you submit an issue.

