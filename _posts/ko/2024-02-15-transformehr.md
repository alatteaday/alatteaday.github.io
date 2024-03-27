---
layout: post
title:  "[Paper] TransformEHR: transformer-based encoder-decoder generative model to enhance prediction of disease outcomes using electronic health records (2023)"
categories: ['Biomedical AI']
lang: ko
tags: bio ehr transformer
---

Yang, Zhichao, et al. "TransformEHR: transformer-based encoder-decoder generative model to enhance prediction of disease outcomes using electronic health records." *Nature Communications* 14.1 (2023): 7857.

[paper link](https://www.nature.com/articles/s41467-023-43715-z)



# Points

1. New pre-training objective: predicting all diseases or outcomes of a future visit

   * helps the model uncover the complex interrelations among different diseases and outcomes

2. **TransformEHR**, the generative encoder-decoder framework to predict patients' ICD codes using their longitudinal EHRs

3. Validation of the generalizability using both internal and external datasets

   * demonstrated a strong transfer learning capability of the model

   * could be great with limited data and computing resources

     

# Background

* Longitudinal electronic health records (EHRs) have been successfully used to predict clinical diseases or outcomes (congestive heart failure, sepsis mortality, mechanical ventilation, septic shock, diabetes, PTSD, etc.)
* With the availability of large cohorts and computational resources, deep learning (DL) based models outperform traditional machine learning (ML) models (Med-BERT, BEHRT, BRLTM, etc.)
* The existing pre-training tasks were limited in predicting a fraction of ICD codes within each visit :arrow_right: A novel pre-training strategy, which predicts the complete set of diseases and outcomes within a visit, might improve clinical predictive modeling 



# Method

## Data

*VHA: Veterans Health Administration, the largest integrated healthcare system in the US, providing care at 1,321 healthcare facilities

Pre-training data: around 6M patients who received care from more than 1,200 facilities of the US VHA 

<img src="https://github.com/alatteaday/alatteaday.github.io/blob/gh-pages/_images/2024-02-15-transformehr/table1.png?raw=true" style="zoom: 67%;" />

* Two common and uncommon disease/outcome agnostic prediction (DOAP) datasets

  * ICD-10CM codes with more than a 2% prevalence ratio for common dataset
  * Those with a 0.04%-0.05% prevalence ratio for uncommon dataset

* Non-VHA dataset: from MIMIC-IV dataset (29,482)

  * Only selected objects with ICD-10CM records to match the cohorts from VHA

    

## Longitudinal EHRs

<img src="https://github.com/alatteaday/alatteaday.github.io/blob/gh-pages/_images/2024-02-15-transformehr/fig1.png?raw=true" alt="image-20240306100731535" style="zoom:67%;" />

* Include demographic information (gender, age, race, and marital status) and ICD-10CM codes as predictors

* Group ICD codes at the visit level

* Order the codes by priority, where the primary diagnosis is typically given the highest priority

* Form multiple visits as a time-stamped input of a sequence by date of visit

  

## Embeddings

![KakaoTalk_Image_2024-03-13-09-14-52_007](file:///Users/jiyun/Documents/Gitlog/alatteaday.github.io/_images/2024-02-15-transformehr/KakaoTalk_Image_2024-03-13-09-14-52_007.png)<img src="https://github.com/alatteaday/alatteaday.github.io/blob/gh-pages/_images/2024-02-15-transformehr/fig2.png?raw=true" alt="image-20240306103928325"  />

![image-20240306110913978](https://github.com/alatteaday/alatteaday.github.io/blob/gh-pages/_images/2024-02-15-transformehr/code_embeds1.png?raw=true)

Multi-level embeddings: visit embeddings + time embeddings + code embeddings

* Time embeddings: embed days difference as relative time information by getting the difference between a certain visit and the last visit in the EHR
  * Includes the date of each visit to integrate temporal information, not only sequential order
  * Date is important as the importance of predictor in a visit can vary over time




## Model Architecture

Encoder-decoder transformer-based architecture

<img src="https://github.com/alatteaday/alatteaday.github.io/blob/gh-pages/_images/2024-02-15-transformehr/fig3.png?raw=true" alt="image-20240306114845710" style="zoom:80%;" />

* Encoder: performs cross-attention, unlikely BERT, over representations and assigns an attention weight for each representation
  * Cross-attention is implemented by masking the complete set of ICD codes of a future visit as shown in Fig. 2b
* Decoder: generates ICD codes of the masked future visit with the weighted representations from the encoder
  * Generates the codes following the order of code priority within a visit



# Evaluation

Metrics: PPV (precision), AUROC, AUPRC

Baseline models: logistic regression, LSTM, BERT without pre-training, BERT with pre-training    <span style= "color:silver;"> # what's the objective when pre-training BERT? MLM or the objective proposed in this paper? </span>

![image-20240306175748572](https://github.com/alatteaday/alatteaday.github.io/blob/gh-pages/_images/2024-02-15-transformehr/table2_3_4.png?raw=true)

## Pre-training

Task: Disease or outcome agnostic prediction (DOAP); Predicting the ICD codes of a patient's future visit based on longitudinal information up to the current visit

**Ablation study**

1. Visit masking vs. code (part of visit) masking for an encoder-decoder model

   :arrow_right: visit masking performed better; pre-training of all diseases outperform traditional pre-training objective (2.52-2.96% in AUROC)

2. Encoder-decoder vs. encoder-only (BERT) on DOAP	

   :arrow_right: ​encoder-decoder outperformed; 0.74-1.16% in AUROC      # the possibility if the parameter size affected this result? 

3. Time embeddings O vs. X

   :arrow_right: the model with the time embeddings outperformed moderately; 0.43 in AUROC	

   	* days difference is more effective than specific date as the embeddings 



## Fine-tuning

Tasks: the pancreatic cancer onset prediction (Table 3) and intentional self-harm prediction in patients with PTSD (Table 4) 

* TransformEHR outperforms on the both tasks
* AUPRC was consistent when using different set of demographics

* Results with all visits was better than with recent few(five) visits
* In generalizability evaluation, 
  * when testing with internal dataset which is included data from VHA facilities not used for pre-training, there's no statistical difference in AUPRC on the intentional self-harm prediction task among PTSD
