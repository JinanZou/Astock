# Astock: A New Dataset and Automated Stock Trading based on Stock-specific News Analyzing Model

This repository provides codes for a dataset and automated stock trading based on stock-specific news analyzing model(https://aclanthology.org/2022.finnlp-1.24/).
This paper has been accepted by FinNLP 2022 from EMNLP.


## Abstract
Natural Language Processing(NLP) demonstrates a great potential to support financial decision-making by analyzing the text from social media or news outlets. In this work, we build a platform to study the NLP-aided stock auto-trading algorithms systematically. In contrast to the previous work, our platform is characterized by three features: (1) We provide financial news for each specific stock. (2) We provide various stock factors for each stock. (3) We evaluate performance from more financial-relevant metrics. Such a design allows us to develop and evaluate NLP-aided stock auto-trading algorithms in a more realistic setting. In addition to designing an evaluation platform and dataset collection, we also made a technical contribution by proposing a system to automatically learn a good feature representation from various input information. The key to our algorithm is a method called semantic role labeling Pooling (SRLP), which leverages Semantic Role Labeling (SRL) to create a compact representation of each news paragraph. Based on SRLP, we further incorporate other stock factors to make the final prediction. In addition, we propose a self-supervised learning strategy based on SRLP to enhance the out-of-distribution generalization performance of our system. Through our experimental study,  we show that the proposed method achieves outstanding performance and outperforms all the baselines' annualized rate of return as well as the maximum drawdown of the CSI300 index and XIN9 index on real trading. Our Astock dataset and code are available at https://github.com/JinanZou/Astock.


## Contents 
- <a href="#Abstract">Abstract</a><br>
- <a href="#Dataset">Dataset</a><br>
- <a href="#Requirements">Requirements</a><br>
- <a href="#Structure of the Model">Structure of the Model</a><br>
- <a href="#Reference">Reference</a><br>
- <a href="#Simulated Trading Strategy">Simulated Trading Strategy</a><br>


## Dataset:
Sample from the dataset

| CODE | NAME | DATE | CREATED_DATE | TITLE | TITLE_TRANSLATED | READ | LABEL | PRICE_CHG(%) | 
| ---------- | :-----------:  | :-----------: | ---------- | :-----------:  | :-----------: | ---------- | :-----------:  | :-----------: |
|601991|大唐发电|2021-03-26 21:18:00+08:00|2021-03-26 21:11:00+08:00|大唐发电发布2020年年报，实现营业收入956.14亿元，同比上升0.17%；归属于母公司所有者的净利润30.40亿元，同比上涨约185.25%；基本每股收益0.1017元。|Datang Power released its 2020 annual report and achieved an operating revenue of 95.614 billion yuan, up 0.17% year on year; The net profit attributable to the owners of the parent company was 3.040 billion yuan, up about 185.25% year on year; The basic earnings per share is 0.1017 yuan.|4808633.0|1|0.006920415224913601|

## Requirements:
```shell
Python      : 3.9
numpy       : 1.20.3
pandas      : 1.3.4
torch       : 1.10.0
transformers: 4.7.0
```

## Structure of the Model
(pretrained model can be download from https://drive.google.com/file/d/1llrxRKj0J0af5cVx4gmygYy3Z3JeAhJe/view?usp=sharing)
<img src=figs/model_structure.jpg width=800>

## Simulated Trading Strategy
We design a dynamic trading strategy based on news responses to back-test our models. Generally, we define three types of actions: short, long, and preserve. The stocks from China A-shares can be shortened by short selling, and we assume that it applies to all stocks. When a piece of news is published, an action is automatically triggered according to the result predicted by the models.
Different types of the transaction have different methods to calculate the return rate.
For a "long" transaction, we define its return as $\frac{P_{sell}-P_{buy}}{P_{buy}}\%$. For a "short" transaction, we define its return as $\frac{P_{sell}-P_{buy}}{P_{sell}}\%$( $P$ stands for the price).

Our strategy considers three types of actions: short, preserve, and long. When news is published, the corresponding stock price will surge or plummet abnormally. This strategy is based on the assumption that the stock price will return to the normal interval after surging or plummeting. The trading actions will be triggered according to the result predicted by our model. Specifically, if a news piece's predicted result is a downtrend, the long action will be triggered, and if the result is an uptrend, the short action will be triggered. Otherwise, the preserve action will be triggered. The position will be closed the day after the next trading day. To make the strategy flexible to deal with the different situations, a dynamic weight is introduced to control the position of each transaction under different actions, especially for the actions of long and short. The weight is determined by the sum of the latest three days' return rates from the same type of action. The position for action $a_i$ is $P_{i}$ defined as Equation \ref{eq.eval_w} and the process is shown in the Figure

Where $r_{ij}$ is the average return rate of the $j$-th latest day for a type of action $a_i$, $a_i \in \{a_1,a_2,a_3\}$. $N_{i}$ is the number of transactions for an action in a trading day. $C$ is the hyper-parameter to adjust the flexibility,  $p$ is the prediction probability of a piece of news. Figure describes the progress of the trading transaction from an action triggered by the news to the transaction finalized. 

Since taking preserve action cannot make a profit, we introduce a hypothetical return rate to calculate the weight of preserve. Specifically, if the $t$-th latest day return rates of long action and short action are both negative, we assume that the return rate of the preserve action is the opposite average of short return and long return. Otherwise, the return rate of preserve action is set to 0.

For each transaction, the commission fee is set to 0.13\%, and the position is opened by the price when the news is published. The position will be closed if a -10\% loss occurs.



$$
    P_{i}  = \frac{p_iC^{\sum_{t=0}^{3}r_{ij}}}{N_i\sum_{i\in{A}}C^{\sum_{j=0}^{3}r_{ij}}}
$$

<img src=figs/process_of_each_transaction.png width=800>

## Implementation Details

 In our proposed model, we set the number of heads and layers of the transformer blocker to 1 and 2, respectively.  The weights of $L_{SSL}$ and $L_{CE}$ are set to 0.8 ($\alpha$) and 0.2, and we also conduct the comparison study by choosing different alpha. We use AdamW as the optimizer and train the model for 20 epochs with a batch size of 16 and a fixed learning rate of 1e-5. In addition, two epochs of linear warming up are used.

## Bibtex Citation 
@inproceedings{zou-etal-2022-astock,
    title = "Astock: A New Dataset and Automated Stock Trading based on Stock-specific News Analyzing Model",
    author = "Zou, Jinan  and
      Cao, Haiyao  and
      Liu, Lingqiao  and
      Lin, Yuhao  and
      Abbasnejad, Ehsan  and
      Shi, Javen Qinfeng",
    booktitle = "Proceedings of the Fourth Workshop on Financial Technology and Natural Language Processing (FinNLP)",
    month = dec,
    year = "2022",
    address = "Abu Dhabi, United Arab Emirates (Hybrid)",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2022.finnlp-1.24",
    pages = "178--186",
    abstract = "Natural Language Processing (NLP) demonstrates a great potential to support financial decision-making by analyzing the text from social media or news outlets. In this work, we build a platform to study the NLP-aided stock auto-trading algorithms systematically. In contrast to the previous work, our platform is characterized by three features: (1) We provide financial news for each specific stock. (2) We provide various stock factors for each stock. (3) We evaluate performance from more financial-relevant metrics. Such a design allows us to develop and evaluate NLP-aided stock auto-trading algorithms in a more realistic setting. In addition to designing an evaluation platform and dataset collection, we also made a technical contribution by proposing a system to automatically learn a good feature representation from various input information. The key to our algorithm is a method called semantic role labeling Pooling (SRLP), which leverages Semantic Role Labeling (SRL) to create a compact representation of each news paragraph. Based on SRLP, we further incorporate other stock factors to make the final prediction. In addition, we propose a self-supervised learning strategy based on SRLP to enhance the out-of-distribution generalization performance of our system. Through our experimental study, we show that the proposed method achieves better performance and outperforms all the baselines{'} annualized rate of return as well as the maximum drawdown of the CSI300 index and XIN9 index on real trading. Our Astock dataset and code are available at https://github.com/JinanZou/Astock.",
}

