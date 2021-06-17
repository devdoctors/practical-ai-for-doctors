# Practical GNN for Doctors
## Where do I use GNN?

### 구성
[Practical AI for Doctors](https://github.com/devdoctors/practical-ai-for-doctors)
- [<제 1강> Practical GNN for Doctors](https://github.com/devdoctors/practical-ai-for-doctors/tree/main/gnn)
  1. [Why? GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/01-why.md)
  2. [What is the GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/02-what.md)
  3. [How do I implement GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/03-how.ipynb)
  4. **Where do I use GNN?**


### 들어가며

지금까지 GNN의 이론적 배경과 대표적인 GNN 모델인 GCN의 구현에 대한 공부를 해 보았습니다.
GNN자체에 대해 공부를 해 보았다면 이번에는 이러한 GNN이 어디에 적용이 될 수 있는지를 짚어보고자 합니다.


### 그래프 구조 데이터
GNN은 이론적으로 그래프 구조를 갖는 모든 데이터에 적용이 가능합니다.
앞선 시간들에 언급되었던 화합물, 출판물의 인용관계를 포함해서 다음과 같은 데이터들이 그래프 구조를 갖는다고 볼 수 있겠습니다.
- **화합물**: 노드 - 원자 / 엣지 - 결합
- **출판물**: 노드 - 출판물 / 엣지 - 인용여부
- **교통망**: 노드 - 교차로 / 엣지 - 도로여부
- **지식그래프**: 노드 - 정보 / 엣지 - 관계여부
- **사회/물품관계망**: 노드 - 회원, 물품 / 엣지 - 관계(구입,함께구입,친구 등)여부

실제로는 위에 언급되지 않은 그래프 구조의 데이터가 훨씬 더 많이 존재합니다.
다만 위에 언급된 그래프 데이터는 모두 GNN의 적용이 활발히 연구되거나, 산업에서 응용되고 있는 대표적인 데이터들입니다.

### GNN의 적용

위 그래프 구조 데이터에는 GNN이 다음과 같이 적용될 수 있습니다.

- **화합물**: [화합물 전체의 특성을 학습, 예측](https://www.sciencedirect.com/science/article/pii/S0092867420301021)
- **출판물**: [각 출판물의 종류를 학습, 예측](https://www.nature.com/immersive/d41586-019-03165-4/index.html)
- **교통망**: [두 지점간의 소요시간을 학습, 예측](https://deepmind.com/blog/article/traffic-prediction-with-advanced-graph-neural-networks)
- **지식그래프**: [지식그래프기반 추천/질의응답](https://www.amazon.science/blog/building-product-graphs-automatically)
- **사회/물품관계망**: [회원-물품간의 관계를 통한 추천시스템 제공](https://eng.uber.com/uber-eats-graph-learning/)

각 영역의 자세한 학문적/산업적 적용 사례들이 궁금하시다면 링크를 확인해보세요!
저는 개인적으로 뇌가 이루고 있는 네트워크를 그래프 구조 데이터로 치환하여 GNN을 통해 뇌연결성의 표상을 학습하는 것을 연구하고 있습니다 ([논문1](https://arxiv.org/abs/2001.03690), [논문2](https://arxiv.org/abs/2105.13495)).


### Open Graph Benchmark
직접 GNN을 개발하고 연구해보고 싶은데, 무수히 많은 종류의 그래프 구조 데이터들 중 어떤 것으로 실험을 해 볼 수 있을지 고민이 되기 마련입니다.
GNN의 성능을 평가하거나 비교하기 위한 표준을 정립하기 위해 [Open Graph Database (OGB)](https://ogb.stanford.edu/) 가 2020년에 공개되었습니다.
OGB는 그래프 데이터 딥러닝 툴인 [DGL](https://www.dgl.ai/), [PyTorch Geometric](https://pytorch-geometric.readthedocs.io/en/latest/#)과 연동되어 데이터를 쉽게 불러올 수 있고, 노드특성 예측 / 그래프특성 예측 / 엣지특성 예측과 관련된 데이터셋의 표준이 제공되어 있습니다.
최근 공개되는 GNN 연구들은 OGB의 표준을 따르는 경우들이 많아지고 있어 GNN을 구현하고 연구해보고자 하시는 분은 잘 활용하신다면 도움이 될 것으로 생각됩니다.
OGB에 관련한 자세한 사항은 [이 논문](https://arxiv.org/abs/2005.00687)을 참고해보세요.


### 마무리하며
이렇게 GNN과 관련한 기본 사항들을 익혀보았습니다.
GNN은 급변하는 AI/머신러닝 분야속에서도 꾸준히 연구와 적용이 진행되고 있는 모델입니다.
의학과 관련하여서도 중요한 역할을 할 가능성이 있는 모델로 관심을 가져보신다면 재미있는 결과들을 얻으실 수 있을 것이라 생각합니다.
궁금하신 점은 무엇이든 편하게 [DevDoctors 슬랙 채널](https://devdoctors.slack.com/)에 남겨주세요 :)
감사합니다.

#### [이전: How do I implement GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/03-how.ipynb)
