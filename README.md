# Astock: A New Dataset and Automated Stock Trading based on Stock-specific News Analyzing Model

This repository provides codes for [](http://)

## Abstract
Natural Language Processing(NLP) demonstrates a great potential to support financial decision-making by analyzing the text from social media or news outlets. In this work, we build a platform to study the NLP-aided stock auto-trading algorithms systematically. In contrast to the previous work, our platform is characterized by three features: (1) We provide financial news for each specific stock. (2) We provide various stock factors for each stock. (3) We evaluate performance from more financial-relevant metrics. Such a design allows us to develop and evaluate NLP-aided stock auto-trading algorithms in a more realistic setting. In addition to designing an evaluation platform and dataset collection, we also made a technical contribution by proposing a system to automatically learn a good feature representation from various input information. The key to our algorithm is a method called semantic role labeling Pooling (SRLP), which leverages Semantic Role Labeling (SRL) to create a compact representation of each news paragraph. Based on SRLP, we further incorporate other stock factors to make the final prediction. In addition, we propose a self-supervised learning strategy based on SRLP to enhance the out-of-distribution generalization performance of our system. Through our experimental study,  we show that the proposed method achieves outstanding performance and outperforms all the baselines' annualized rate of return as well as the maximum drawdown of the CSI300 index and XIN9 index on real trading. Our Astock dataset and code are available at https://github.com/JinanZou/Astock.

<img src=figs/model_structure.jpg width=800>

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

## Installation:
```shell

```
## Reference
