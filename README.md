# Practical AI for Doctors

### 구성
**Practical AI for Doctors**
- [<제 1강> Practical GNN for Doctors](https://github.com/devdoctors/practical-ai-for-doctors/tree/main/gnn)
  1. [Why? GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/01-why.md)
  2. [What is the GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/02-what.md)
  3. [How do I implement GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/03-how.ipynb)
  4. [Where do I use GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/04-where.md)


### 소개

안녕하세요.
저는 DevDoctors라는 의료인을 위한 기술-소통 플랫폼에서 AI를 주제로 운영을 맡게된 김병훈이라고 합니다.
먼저 이 자료를 접하는 여러분들과 소통할 수 있게 되어 매우 반갑고 기쁩니다.
이 자료는 일차적으로 실전에서 의사 선생님들이 AI를 적용할 수 있는데 도움이 되는 것을 목표로 만들어졌습니다.
하지만 AI를 제가 강의한다는 개념보다는, 제가 알고있는 지식을 공유하고 이에 대해 질의/토의하는 과정에서 서로의 실력을 키우고 관심사에 대해 공유를 하는 것이 이 자료를 작성하게 된 더 궁극적인 목표입니다.

저는 2017년부터 2021년까지 4년간 의료영상 신호처리와 인공지능을 주제로 KAIST 바이오및뇌공학과에서 공부를 하여 공학박사 학위를 취득하였습니다.
저보다 AI에 대해 깊이 이해하고 계실 선생님들 앞에서 강의자료를 올린다는 것이 부끄럽지만, 앞서 말씀드린대로 강의를 한다는 느낌 보다는 소통의 계기가 되었으면 하는 바람으로 글을 시작해보겠습니다.


### 주제
강의는 AI(혹은 딥러닝)의 대표적인 모델들을 다루는 것을 큰 틀로 합니다.
현재로서는 아래와 같은 모델들을 다루는 것을 생각하고 있습니다.

1. Graph Neural Network (GNN)
2. Transformers
3. Convolution Neural Network (CNN)

GNN을 처음으로 다루는 이유는, 의료도메인에서 그래프 구조의 데이터를 접하게 되는 경우가 생각보다 흔하지만, 이를 인공지능으로 처리할 수 있는 GNN에 대한 자료는 상대적으로 적다고 생각이 되었기 때문입니다.


Transformer는 Self-attention을 기반으로 한 인공지능 모델로, 자연어처리에서 괄목할 성과를 내었습니다. 최근에는 자연어처리뿐 아닌 컴퓨터비젼을 포함한 제반 분야에서 그 활용도가 폭발적으로 증가하고 있습니다. 이에 의료분야에서도 Transformer를 기반으로 한 좋은 성과들이 나올 것으로 예상이 되어 이에 대해 다루어 보고자 합니다.

CNN을 다루는 이유는, U-Net과 같은 강력한 CNN 기반 모델들이 의료영상 딥러닝에서 안정적이고 훌륭한 성능을 낼 수 있음이 여러 좋은 논문들로 입증된 바가 있기 때문입니다. 다만 GNN에 비해 상대적으로 관련 자료를 찾기 쉬워 다른 자료를 추가로 참고하시는 것도 도움이 될 것으로 생각됩니다.

### 구성
각 주제(모델)에 대한 강의는 아래와 같은 구성으로 진행을 하고자 합니다.

1. Why: 왜 해당 모델에 대해 알아야 하는지에 대해 대표적인 논문을 간략히 검토하여 공부할 의욕을 끌어올려봅니다.
2. What: 해당 모델이 무엇인지 그 기반을 이루고 있는 이론과 기술에 대해 간략히 검토하여 우리가 배우는 대상에 대해 이해를 해 봅니다.
3. How: 모델이 왜 필요한지, 그리고 무엇을 기반으로 하는지 다루었다면 실제로 그 모델을 코드로 구현하여 간단한 실험을 해 보는 과정을 함께 해 봅니다.
4. Where/When: 지금까지 공부한 모델을 언제, 어디에서 사용할 수 있을지 논의를 해 봅니다. 이에 대해서는 *1. Why* 에서 다룬 대표 논문보다는, 최신 연구들을 통해 아이디어를 다루어 보겠습니다.


### 선행

본 과정은 대학 초기과정 수학(선형대수, 확률통계, 미적분)과, 기본적인 코딩에 대한 지식이 있는 것을 전제로 진행이 됩니다.
제가 공부하면서 도움이 되었던 기초 책들 몇 권은 아래와 같습니다.

- Anton, Busby. Contemporary linear algebra: 선형대수의 기본적인 책이지만, 중요한 내용들을 쉽고 짜임새있게 설명해두었습니다.
- Stewart. Calculus: 역시나 미적분의 기본적인 책입니다. 고등학교 교과과정과 일부 겹치지만 편미분, 중적분, 최적화와 관련된 내용은 도움이 될 수 있습니다.
- Roussas. An introduction to Probability and Statistical Inference: 확률, 통계의 기초를 다루면서 추론에 대한 내용이 포함되어 머신러닝 기초를 쌓는데 도움이 됩니다.

코딩은 파이썬 기초에 대해 알고 계시면 도움이 될 것입니다.
딥러닝 라이브러리는 [PyTorch](https://pytorch.org/)를 기본적으로 사용할 예정이며 가능하다면 PyTorch에서 제공하는 기초 [튜토리얼](https://pytorch.org/tutorials/)을 한 번 훑으신 뒤에 강의를 따라오시기를 추천드립니다.


### 문의
문의는 [DevDoctors 슬랙 채널](https://devdoctors.slack.com/)으로 해 주시기를 부탁드립니다.
잘못된 점에 대한 지적, 이해가 되지 않는 부분 등 어떠한 내용이던 환영합니다!


#### [다음: <제 1강> Practical GNN for Doctors](https://github.com/devdoctors/practical-ai-for-doctors/tree/main/gnn)
