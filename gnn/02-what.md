# Practical GNN for Doctors
## What is the GNN?

### 구성
[Practical AI for Doctors](https://github.com/devdoctors/practical-ai-for-doctors)
- [<제 1강> Practical GNN for Doctors](https://github.com/devdoctors/practical-ai-for-doctors/tree/main/gnn)
  1. [Why? GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/01-why.md)
  2. **What is the GNN?**
  3. [How do I implement GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/03-how.ipynb)
  4. [Where do I use GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/04-where.md)


### 들어가며

지난 시간에는 GNN을 이용한 신약개발 연구 논문을 간략히 훑어 GNN에 대해 왜 알아두면 좋을지를 다루어 보았습니다.
이어서 이번 시간에는 GNN이 *무엇*인지에 대해 알아보고자 합니다.
GNN은 Graph Neural Network의 약자로, 그래프 구조의 데이터를 뜻하는 Graph와 인공신경망을 뜻하는 Neural Network가 합쳐져 있습니다.
이름에서 직관적으로 알 수 있듯이 GNN은 그래프 구조의 데이터에 적용이 가능한 인공신경망 구조를 통틀어 이야기합니다.
먼저 그래프 구조의 데이터가 무엇을 뜻하는지에 대해 알아보고, 그 뒤에 인공신경망 구조가 어떻게 그래프 구조의 데이터에 작용할 수 있는지에 대해서 알아보도록 하겠습니다.


### 그래프 구조 데이터
그래프 구조를 갖는 데이터란, 정점(Node/Vertex; 편의상 *노드*라 칭하겠습니다)과 이를 연결하는 간선(Edge; 편의상 *엣지*라 칭하겠습니다)로 이루어진 데이터를 뜻합니다.
![gnn-02-01](/assets/gnn-02-smallgraph.png)
_그래프 구조의 예시_
위 그림에서는 네 개의 노드(1,2,3,4)와, 이를 일부 연결하는 네 개의 엣지가 존재하여 하나의 그래프를 구성합니다.
위 그림의 그래프를 G라고 하였을 때, 그래프 G는 노드의 집합 V와 엣지의 집합 E의 쌍으로 정의될 수 있습니다. (G=(V,E))
V는 그래프를 구성하는 모든 노드들의 집합입니다. 위 그래프에는 1,2,3,4로 명시된 노드가 있으니 V={1,2,3,4}입니다.
E는 그래프를 구성하는 모든 엣지들의 집합입니다. 엣지는 연결이 존재하는 두 노드의 집합으로 다시 정의될 수 있어서 위 그래프에서는 E={{1,2}, {1,3}, {2,3}, {3,4}}로 볼 수 있겠습니다.
V와 E만 정의된다면 그래프의 기본적인 정의는 끝이 났습니다. 하지만 추가적으로 그래프 데이터를 다룰 때 유용한 인접 행렬(Adjacency matrix) A를 정의하고 넘어가겠습니다.
인접행렬은 행렬의 일종으로, 행의 순서와 열의 순서가 각각 노드의 순서에 대응됩니다.
예시 그래프에서는 노드가 네 개가 있었으므로, 행과 열이 각각 4개인 하나의 행렬로 인접행렬 A가 정의됩니다.
노드 1과 노드 2 사이에는 연결이 있으므로, 인접행렬 A의 1행 2열은 연결이 존재함을 의미하는 '1'이 성분으로 들어가고, 노드 1과 노드 4 사이에는 연결이 없으므로, 인접행렬 A의 1행 4열은 연결이 존재하지 않음을 의미하는 '0'이 성분으로 들어가게 됩니다.
이렇게 1과 0으로 구성된 인접행렬 A는 그래프의 엣지 E에 대한 정보를 모두 포함할 수 있게 됩니다.

앞에서는 노드가 네 개인 매우 작은 그래프를 예시로 들었지만, 현실세계의 데이터에서는 매우 큰 네트워크로 이루어진 그래프를 쉽게 접할 수 있습니다.
예를 들어 Pubmed에 올라온 논문 한 편이 노드 하나에 해당하고, 서로의 인용관계가 엣지에 대응되는 Pubmed 데이터셋은, GNN의 성능을 평가하고 비교하는 벤치마크 데이터셋 중 하나입니다.
Pubmed 데이터셋은 19,717개의 노드와 88,651개의 엣지로 이루어져 있습니다. 앞의 예시 그래프에 비하면 훨씬 크다고 볼 수 있겠지만, 산업에서 다루는 그래프 크기를 고려하였을 때 아주 크다고 보기도 어렵기는 합니다.

추가적으로 앞선 예시의 그래프와 다르게 실제 다루게 될 그래프 데이터에 대해 알아야 할 점 하나는, 각 노드에는 해당 노드를 지칭하는 인덱스(예시의 경우 V의 1,2,3,4)뿐 아니라 그 해당 노드의 특징(Node feature)이 벡터화되어 함께 주어지는 경우가 대부분이라는 점입니다.
Pubmed 데이터셋에서는 19,717개의 노드 각각에 대해 노드의 특징이 대응되는 길이 500의 벡터가 함께 주어집니다.
이제 이러한 그래프 구조 데이터를 어떻게 뉴럴네트워크로 학습할 수 있는지 알아보도록 하겠습니다.


### 그래프 뉴럴네트워크 (GNN)
GNN은 주어진 그래프에 대하여 노드분류(Node classification), 그래프분류(Graph classification), 엣지분류(Edge classification)등의 태스크를 수행할 수 있는 뉴럴네트워크를 구조를 의미합니다.
어떻게 GNN은 주어진 그래프 구조를 이해하여 분류문제를 해결할 수 있을까요?
다음 그림의 과정을 하나씩 쫓아가며 이해해 봅시다.
그림은 저의 [이전 논문](https://www.frontiersin.org/articles/10.3389/fnins.2020.00630/full) 중 GNN구조의 하나인 GIN (Graph Isomorphism Network)의 연산 과정을 설명하는 부분에서 발췌하였습니다.

![gnn-02-process](/assets/gnn-02-process.png)
_GNN(GIN)의 연산 과정_
#### A. Input graph
GNN은 노드 특징벡터로부터 시작합니다.
앞서 보았던 네 개 노드의 예시 그래프와 같은 구조의 그래프가 있습니다.
노드 특징벡터는 각 노드가 갖는 특징들을 일반적으로 [원-핫 인코딩](https://en.wikipedia.org/wiki/One-hot)하여 주어지게 됩니다.
여기에서는 총 네 가지 노드 특징이 가능한 원-핫 벡터가 쓰였고, 1번 노드는 [1,0,0,0]의 특징, 2번 노드는 [0,1,0,0]의 특징, 3번 노드는 [0,0,1,0]의 특징, 마지막 4번 노드는 [0,0,0,1]의 특징을 갖는 경우를 예로 들고 있습니다. (노드 개수와 노드 특징벡터의 길이가 여기에서는 4로 같지만, 꼭 같을 필요는 없습니다.)

#### B. AGGREGATE/COMBINE
AGGREGATE와 COMBINE은 GNN의 가장 핵심이 되는 부분으로 CNN으로 따지면 [컨볼루션(Convolution)](https://en.wikipedia.org/wiki/Convolution)에 해당되는 부분입니다.
이 과정에서는 각 노드를 기준으로 연결이 존재하는 주변 노드들의 특징을 종합하게 되어 Message-passing이라고도 불립니다.
앞서 CNN의 컨볼루션과 비슷한 과정이라고 말씀을 드렸는데, 컨볼루션의 경우에는 k x k 크기의 커널을 한 칸씩 옮기며 주변 성분(Element)와의 국소적인 특징을 종합한다면, GNN의 AGGREGATE와 COMBINE은 각 노드를 돌아다니며 연결이 존재하는 이웃 노드들의 특징을 종합하는 것으로 볼 수 있습니다.
이렇게 주변의 특징을 종합하는 방법은 (i) 주변 노드를 Convolution하는 Spectral한 방법과 (ii) 주변 노드의 특징에 대해 AGGREGATE, COMBINE함수를 정의하여 메시지를 주고받는 Spatial한 방법이 존재하는데, 둘의 이론적 시작점은 다르지만 결국 비슷한 연산을 하는 것으로 볼 수 있기 때문에 Spatial한 방법의 GNN을 기준으로 앞으로 설명을 드리겠습니다.
![convolutions](/assets/gnn-02-convolutions.jpg)
_(a) CNN의 컨볼루션, (b) GNN의 컨볼루션 (Spectral), (c) GNN의 AGGREGATE/COMBINE (Spatial)_

AGGREGATE는 i번째 노드의 이웃 노드들에 대한 함수입니다.
이웃 노드란, 그래프에서 특정 노드와 직접적인 연결성(엣지)가 존재하는 모든 노드들의 집합을 의미합니다.
즉 우리가 x에 대한 함수 f가 y로 대응될 때 y=f(x)처럼 표현할 수 있듯이, i번째 노드 n_i의 이웃을 뜻하는 N(n_i)에 대해서 함수 AGGREGATE가 a_i로 대응된다고 하면 a_i = AGGREGATE(N(n_i))라고 표현을 할 수 있겠습니다.
이 AGGREGATE된 결과인 a_i는 i번째 노드의 이웃들을 입력으로 받았기 때문에 각 노드와 연결성이 있는 노드들의 특징이 녹아들어가게 됩니다.
사실 AGGREGATE라고 함수를 표현하였지만, 일반적인 GNN들에서 이 AGGREGATE함수는 평균을 내는 연산이거나, 합을 구하는 연산 등의 아주 단순한 연산인 경우가 대부분입니다.
다시 말하면 a_i라는 결과물은 n_i의 이웃 노드들의 특징 벡터를 전부 평균을 내거나 합을 내서 얻어진 벡터라는 것이지요.

i번째 노드의 이웃들에 대한 특징을 종합하여 a_i를 얻었다면, 이제는 i번째 노드인 n_i 자체의 특징도 고려가 되어야 하겠습니다.
이를 시행하는 함수가 바로 COMBINE입니다.
COMBINE은 a_i와 n_i를 입력으로, 한 차례 특징이 추출된 다음 층(Layer)의 노드 특징 n_i를 얻어내는 과정, 즉 n_i (다음층) = COMBINE(a_i, n_i) 로 볼 수 있습니다. 이렇게 다음층의 n_i가 얻어지면, 이 노드특징이 GNN의 입력으로 사용될 수 있겠지요.
COMBINE 함수 또한 보통은 합, 평균 등으로 정의가 됩니다. 다만 이웃 노드의 특징이 녹아있는 a_i와 타겟 노드의 특징을 표현하는 n_i간에 약간의 차이를 두는 경우도 있습니다.
예를 들어 앞선 GIN의 연산 과정 그림에서는 n_i에만 약간의 값의 차이를 주는 (1+e)를 곱하는 과정이 COMBINE에 있습니다.
이렇게 a_i와 n_i의 특징을 종합하고 나면 드디어 특징 추출을 위해 뉴럴네트워크가 사용됩니다.

#### C. MLP / D. ReLU
앞서 COMBINE과정에서 a_i와 n_i의 특징을 함께 종합한 벡터를 이제 뉴럴네트워크를 통해 특징 추출을 하게 됩니다.
뉴럴네트워크는 보통 Multi-layer perceptron (MLP)로도 알려져 있는 Fully-connected 층을 사용하고, 비선형적인 특징 추출이 가능하도록 ReLU와 같은 활성함수를 각 층에 배치합니다.
사실 이 MLP를 적용하는 과정도 B.에서 말씀드린 COMBINE 함수의 일부로 보는 것이 엄밀히는 맞습니다.
다만 GNN에서 *뉴럴네트워크*가 어느 부분에 적용되는지 강조하기 위해 따로 섹션을 두어서 설명을 드리게 되었습니다.
이렇게 MLP를 통과한 각 노드의 특징벡터는 다시 다음 층으로 입력될 준비가 되었습니다.
이러한 연산이 반복하여 쌓아올려진 것이 바로 심층(Deep)적인 GNN이라고 이해하시면 좋을 것 같습니다.


### 대표적인 그래프 뉴럴네트워크 모델: GCN
이번 섹션에서는 여러 GNN 구조 중 가장 대표적인 모델로 꼽을 수 있는 [그래프 컨볼루션 네트워크 (Graph Convolutional Network: GCN)](https://arxiv.org/abs/1609.02907)를 설명드리도록 하겠습니다.
또한 다음 시간에는 실제로 이 GCN구조를 구현하고, PubMed데이터를 이용하여 간단한 노드분류 실험을 진행해 볼 계획입니다.
이전 섹션에서 함께 GNN의 연산 과정을 훑어 보았는데, 결국 GNN은 AGGREGATE와 COMBINE함수가 어떻게 정의되느냐에 따라 그 구조가 구별된다고 볼 수 있겠습니다.
GCN은 AGGREGATE와 COMBINE함수가 평균(Average)을 내는 것으로 정의가 되고, 뉴럴네트워크는 1개의 층의 Fully-connected layer, 그리고 활성함수는 ReLU를 사용하는 구조입니다.
즉, 각 노드 특징들은 연결된 이웃노드들의 특징들과 함께 평균이 되고 (AGGREGATE/COMBINE) 이 과정을 통해 종합된 특징은 Fully-connected 층과 ReLU를 통과하여 하나의 GCN 층이 구성되고, 원문에서는 두 개의 GCN층을 사용하는 구조가 제안되었습니다.

GCN은 2016년 9월 arXiv에 공개된 뒤, 글을 작성하는 시점인 2021년 4월 기준으로 7500회 이상 인용이 되었습니다.
이는 GCN의 이론적인 통찰뿐만이 아니라 다양한 데이터와 목적에 대해 안정적으로 성능이 확보되는 강건함이 반영된 결과라고 볼 수 있겠습니다.
GNN에 대해 조금 더 이해하고 싶은 분은 [GCN 논문 원문](https://arxiv.org/abs/1609.02907)을 한 번 읽어보시는 것을 추천드립니다.


#### [이전: Why? GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/01-why.md)
#### [다음: How do I implement GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/03-how.ipynb)
