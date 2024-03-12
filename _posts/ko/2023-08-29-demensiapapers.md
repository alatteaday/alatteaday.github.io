---
layout: post
title:  "[Study] 알츠하이머 치매의 ATN 바이오마커 간 관계 정리"
categories: ['Biomedical AI']
lang: ko
tags: bio
sub-title: "Docker image 및 container와 관련하여 자주 사용되는 명령어를 정리해봅니다."
---


## AI 전공자의 알츠하이머 치매 관련 Brain Imaging 논문 스터디

Amyloid Beta(A), Tau(T), Neurodegeneration(N)과 관련된 Alzheimer's Disease(AD) 기전에 대해 이해하기 위하여 다음의 논문들을 읽고 정리한 내용입니다. 기반 지식이 없어 시각 자료와 사전을 찾아가며 읽었습니다. pdf는 찾아본 이미지와 필기한 내용이 담긴 논문 파일입니다.

Ittner, Lars M., and Jürgen Götz. "Amyloid-β and tau—a toxic pas de deux in Alzheimer's disease." *Nature Reviews Neuroscience* 12.2 (2011): 67-72. [link](https://www.nature.com/articles/nrn2967) [pdf](/Users/jiyun/Documents/nrn2967.pdf)

Vogel, Jacob W., et al. "Four distinct trajectories of tau deposition identified in Alzheimer’s disease." *Nature medicine* 27.5 (2021): 871-881. [link](https://www.nature.com/articles/s41591-021-01309-6) [pdf](/Users/jiyun/Documents/s41591-021-01309-6.pdf)

Lee, Wha Jin, et al. "Regional Aβ-tau interactions promote onset and acceleration of Alzheimer’s disease tau spreading." *Neuron*110.12 (2022): 1932-1943. [link](https://pubmed.ncbi.nlm.nih.gov/35443153/) [pdf](/Users/jiyun/Documents/Regional Aβ-tau interactions promote onset and acceleration of Alzheimer’s disease tau spreading.pdf)



-----



Amyloid Beta(A)는 뉴런에 의해 생성되는 Amyloid Precursor Protein(APP)이 프로테아제에 의해 4부분으로 나눠질 때 생기는 펩타이드 중 하나로, 

뉴런 근처에 존재하여 기능 장애를 야기하는 것으로 알려졌다. A의 침착은 Alzheimer’s Disease(AD) 발병 10-20년 전부터 이뤄진다.

- A는 dimers, oligomers, fibrils 등에 이어 plaque를 형성한다. A가 어느 형태에서 toxicity를 갖기 시작하는지는 확실하지 않다. 항 아밀로이드 치매 치료제는 이 plaque의 감소와 증식 및 생성 방지를 목적으로 한다. 

- A의 toxicity는 postsynaptic compartment, 즉 dendrite(somatodendritic region)를 주 대상으로 하여 작용하고, 특정 수용체의 속성에 따라 세포막을 통해 간접적으로 뉴런에 영향을 끼칠 수 있다. 대표적인 특정 수용체로 NMDAR이 있다.  



Tau(T)는 신경 세포에서 microtubule과 결합하는 단백질로, 주로 axon에 존재하여 microtubule의 안정화 및 axonal transition을 조절하는 역할을 한다. 

정상 상태의 뉴런의 dendrite에도 소량 존재한다.

T는 A에 의해 과인산화되고(hyperphosphorylated Tau), 과인산화된 T는 Neurofibrillary Tangle(NFT)를 형성한다. 

- T의 과인산화는 microtubule 형성을 방해하여 뉴런의 기능을 방해한다.
- NFT는 Somatodendritic region에서 많이 관찰된다. T의 level이 높아지면 T가 dendrite에서 많이 관찰된다. 



Dendrite에서 T는 그곳에 위치한 여러 단백질과 상호작용하여 결과적으로 뉴런이 A의 toxicity에 약해지게 만든다.

- T가 인산화되면 Tyrosine protein kinasen FYN과 강하게 작용한다. 과인산화된 T가 dendrite에서 증가함에 따라 FYN도 Soma에서 증가한다.
- FYN은 NMDAR을 인산화한다. 인산화된 NMDAR은 Postsynaptic Density Protein 95(PDS95)와 상호작용한다.
- 이것의 결과로 NMDAR의 excitotoxicity가 나타난다(흥분독성상태). 수용체의 excitotoxicity로 A의 toxicity에 뉴런이 민감해지게 된다.  


  
결과적으로 A와 T는 뉴런을 약화시키는 데에 있어 서로 시너지를 갖는다. A는 T의 과인산화를 촉진하고, 과인산화된 T는 뉴런이 A의 toxicity에 약해지게 만든다. 

이 시스템에서 A와 T는 세포의 다른 부분(각각 Complex I, Complex IV)에 악영향을 끼쳐 미토콘드리아 호흡을 방해하고, 결국 Neurodegeneration(N)을 야기한다.

따라서 A의 침착과 T의 전파는 AD의 중요한 요인이다. 


  
T의 전파 양상은 Braak Staging System으로 체계화된 바 있다.

- Transentorhinal cortex → medial and basal temporal lobe → neocortical associative regions → unimodal sensory and motor cortex

그런데 이 system에 부합하지 않는 전파 양상 또한 관찰되었다. T의 전파 양상을 병의 진행과 뇌 영역의 시공간적 기준으로 분류하여 4가지 subtype으로 정의할 수 있다. 

- S1 limbic (Braak system), S2 MTL, S3 posterior, S4 Lateral Temporal 



침착된 A는 T의 전파에 영향을 준다. 

- A는 heteromodal association cortex에 침착되고, T의 전파는 entorhinal cortex(EC)에서 시작되어 점차 뇌 전반으로 퍼진다. ( ← Braak system; S1 type ? ) 
- Remote Interation: A와 T가 같은 영역에 있지 않은 상태에서, 먼저 A가 연결된 뉴런을 통해서 EC 영역에 있는 T에 영향을 준다. A의 영향으로 T는 점차 주위 영역으로 확산된다.
- Local Interaction: T가 A와 직접적으로 접촉되어 있는 뉴런에 전파되어 만남으로서 T의 전파가 가속화된다(acceleration). 해당 뇌 영역은 Internal Temporal Gyrus(ITG)이다 (propagation hub).
- T 전파의 acceleration이 진행되면 뇌 전반에서 A와 T의 상호작용이 일어나게 되어 N과 AD의 악화를 막기 어렵다. 



A와 T의 PET 데이터와 MRI 데이터를 병의 진행에 따라 살펴보면

- A의 침착되는 정도는 뇌 전반에 걸쳐 점점 심해질 것이고
- T는 뇌의 특정 부분에서 시작하여 점차 확산되는 양상으로 관찰되고
- T의 슈퍼 전파가 관찰된 이후 MRI 상 전반적인 뇌 위축(N)의 정도가 심하게 나타날 것이다.



