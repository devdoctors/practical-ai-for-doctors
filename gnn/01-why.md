# Practical GNN for Doctors
## Why? GNN?

### 구성
[Practical AI for Doctors](https://github.com/devdoctors/practical-ai-for-doctors)
- [<제 1강> Practical GNN for Doctors](https://github.com/devdoctors/practical-ai-for-doctors/tree/main/gnn)
  1. **Why? GNN?**
  2. [What is the GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/02-what.md)
  3. [How do I implement GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/03-how.ipynb)
  4. [Where do I use GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/04-where.md)



### 들어가며

인공지능에 관심을 가지고 계신 분들 중 대부분은 CNN이나 RNN에 대해 들어보셨을 것입니다.
하지만 GNN(Graph Neural Network)에 대해 들어보신 분은 상대적으로 적을 것 같습니다.
만약 들어보았다고 하더라도 GNN에 관심을 가지고 공부를 하거나, 직접 사용을 해 보신 분들은 많지 않으실 것으로 생각되는데요,
그래서 GNN에 대해 가장 먼저 다뤄보고 싶은 이야기는 바로 GNN이 *왜?* 의사에게 필요한지에 대해서 입니다.
아마도 연구경험이 많은 의사선생님들에게 필요성을 강조하기에는 좋은 논문을 소개하는 방법이 가장 쉽게 와닿을 것 같아서 이번 시간에는 아래 논문에 대해 간략히 리뷰를 해 보고자 합니다.

Stokes et al., [A Deep Learning Approach to Antibiotic Discovery](https://www.sciencedirect.com/science/article/pii/S0092867420301021) *Cell* (2020).

### 인공지능과 신약개발
유명한 과학 저널 Cell에 2020년 게재된 이 논문은 제목에서 알 수 있다시피 신약(항생제)개발에 딥러닝을 적용한 결과를 보고하는 것을 그 주제로 하고 있습니다.
이 연구에서 사용된 딥러닝 모델이 바로 GNN입니다.

![gnn-01-01](/assets/gnn-01-01.png)
_딥러닝(GNN)을 사용한 신약개발_

GNN이 강점으로 갖는 그래프 구조 데이터의 처리 과정이나 그 이론적 배경에 대해서는 추후에 좀 더 깊게 들어가 보는 것으로 하고, 이번 시간에는 먼저 이 논문을 훑어보는 것으로 하겠습니다.

#### 배경
다양한 항생제에 내성을 갖는 균주가 늘어남에 따라, 이에 대응할 수 있는 새 항생제의 필요성은 날로 증가하고 있습니다.
반면에 새로운 항생제를 찾는 것은 계속 어려워지고 있습니다.
우리가 접근할 수 있는 화학적 공간 (Chemical space)는 빠르게 넓어지고 있고, 기존의 항생제 개발 방법은 이미 알려진 화합물을 다시 발견하는 문제에 봉착하였기 때문입니다.
이러한 한계를 극복하기 위해, 데이터를 기반으로 신약을 개발하고자 하는 시도가 많이 이루어지고 있는데, 이는 사실 새로운 것은 아닙니다.
다만 최근 머신러닝과 인공지능의 급격한 발전은 이러한 시도가 성공에 다다를 수 있는 가능성을 크게 높여주었고, 이 연구는 그 높아진 가능성을 어느정도 보여주었다는 점에서 그 가치가 인정되었다고 보여집니다.
저자들은 딥러닝 방법 중 GNN 구조의 하나인 ChemProp을 이용해 방대한 공개된(Public) 화합물 라이브러리로부터 *E.coli*에 효과를 보이는 항생제 후보 물질들을 추렸고, 추려진 후보 물질들로부터 실제로 균의 증식을 억제하는 효과가 있는 물질을 발견할 수 있었습니다.

#### 방법
현실적인 문제 해결을 위한 딥러닝의 적용이 성공하기 위해서는 몇 가지 중요하게 고려되어야 할 점이 있습니다.
1. 목표가 현실적인가?
2. 학습 데이터가 잘 전처리 되어있고 샘플수가 충분한가?
3. 모델은 충분히 강건(Robust)한가?

이 연구는 현실적인 문제 해결(항생제 발견)에 대한 연구이며, 위 세 가지 조건이 충분히 고려되었다고 볼 수 있습니다.
이제 위 세 가지에 대해서 이 연구가 택한 방향들을 간략히 짚어보겠습니다.

##### 1. 목표
본 연구에서는 화합물 구조(원자속성 + 결합속성 + 분자속성)를 GNN에 넣었을 때 *E. coli*의 억제 여부를 예측하고자 하였습니다.
연구자들은 학습된 모델이 완벽히 일반화(Generalize, 학습에 쓰이지 않은 새 데이터를 보았을 때 올바른 예측을 하는 정도)되지 않았을 점을 고려하여, GNN에서 예측된 점수를 곧바로 사용하지 않고, **후보 물질을 스크리닝**하는 목적으로 사용하였습니다.
전체 6,111개의 후보 물질들로부터 높은 점수가 예측된 순서대로 상위 99개의 물질 스크리닝되었고, 해당 물질들은 추가적인 실험(Growth inhibition assay)과 전문 지식(Domain knowledge)을 기반으로 하여 최종 물질인 c-Jun N-terminal kinase inhibitor SU3327 (Halicin)이 선정되었습니다.
여기에서 중요하게 생각해야 할 점은, '*딥러닝이 새로운 항생제를 찾아줄 수 있구나!*'라기보다는, '*딥러닝이 후보 물질을 스크리닝하는 정도에는 적용될 수 있겠구나!*' 라는 점입니다.
아마도 저자들의 의학-공학에 대한 전문 지식과 경험이 잘 융합되어 현실적인 목표로 실험을 진행한 것이 이 연구가 좋은 결과를 낼 수 있었던 시작점이 아닐까 싶습니다.

##### 2. 데이터
본 연구에는 학습 데이터의 *샘플수*와 *전처리*에 대해서도 현실적인 고려가 녹아있습니다.
저자들은 학습 데이터의 숫자를 확보하기 위해 두 개의 데이터베이스로부터 총 2,560개(1,760+800)의 화합물 구조를 얻었고, 이를 곧바로 적용하지 않고 중복 물질을 제거하는 전처리 과정을 거쳐 최종적으로 2,335개의 화합물을 모델 학습에 사용하였습니다.
또한 저자들은 학습데이터의 화합물 구조로부터 얻어질 수 있는 정보(원자속성 + 결합속성)만으로는 최종 예측이 충분히 가능하지 않을 것을 고려하여 RDKit으로부터 얻어진 분자수준의 속성을 혼합하여 최종 예측을 하는 방식을 적용합니다.
물론 2,335개의 데이터가 아주 충분한 숫자인지 여부는 경험적으로 알 수 있는 사항으로 미리 예측하기 쉽지는 않지만, 중요한 점은 데이터의 샘플수가 충분하고, 전처리가 잘 되어 일관된 분포(i.i.d)를 가지고, 모델에 적당한 정보가 입력되어야 원하는 결과를 얻을 수 있다는 점입니다.

##### 3. 모델
본 연구에서는 모델의 선택으로 GNN의 구조 중 하나인 [ChemProp](https://pubs.acs.org/doi/10.1021/acs.jcim.9b00237)을 사용하였습니다.
ChemProp은 Spatial GNN의 대표격 구조인 [Message Passing Neural Network (MPNN)](https://arxiv.org/abs/1704.01212), [structure2vec](https://arxiv.org/abs/1603.05629) (본 연구에서는 Directed MPNN, D-MPPN으로 칭함)을 기반으로 화합물의 표상(Representation)을 추출할 수 있도록 응용된 구조입니다.
MPNN, D-MPNN은 많은 분야에 적용사례가 보고되어있어 그 강건함이 상당부분 입증된 모델들입니다.
저자들은 추가적으로 안정적인 결과를 얻기 위해 다수의 동일한 구조 모델의 앙상블을 적용합니다 (앙상블에 대해서는 기회가 된다면 나중에 따로 다루도록 하겠습니다.).
이처럼 딥러닝을 문제해결에 응용하기 위해서는 가장 최신의 모델을 사용하는 것 보다는 성능이 일반적으로 잘 나온다고 알려진 모델을 먼저 고려하는 것이 유리한 경우가 많습니다.
강건한 모델인지 판단할 때 저는 개인적으로 해당 모델이 처음 제안된 페이퍼를 찾아서 그 연구가 몇번이나 인용이 되었는지 확인하는 편입니다.
아주 정확한 방법은 아니겠지만, 강건한 모델들은 여러 응용분야에 적용이 되고 새 모델들이 제안될 때 기준점(Baseline)으로 사용되는 경우가 많기 때문입니다.

위에 나열된 연구의 방법에 대해 요약을 하자면, **2,335개의 화합물을 기반으로 / ChemProp이라는 GNN모델을 학습한 뒤 / 6,111개의 후보 물질로부터 상위 99개를 추리고, 추가 실험을 통해 최종적으로 Halicin이라는 물질을 고려할 수 있게 되었다**고 보시면 될 것 같습니다.

#### 결론
우리의 관심사는 GNN이기 때문에 결론에 대해서는 짧게 다루도록 하겠습니다.
결론은 저자들이 진행한 방법으로 발견한 Halicin이 실험실 세팅과 동물 모델에서 항균효과가 입증이 되었다는 것입니다.

![gnn-01-02](/assets/gnn-01-02.png)
_Halicin의 항균효과 실험_

Halicin은 과거에 당뇨병 치료 후보물질로 고려가 되었지만, 효과가 충분히 입증되지 않아 실제 치료제 적용으로 이어지지는 못하였습니다.
당시에는 Halicin의 항생제로서의 가능성이 인지되지 못했지만, 이 연구를 통해 해당 물질의 새로운 가능성을 발견할 수 있었다는 점이 중요하다고 볼 수 있겠습니다.
또한 저자들은 추가적으로 두 개의 화합물 데이터베이스(WuXi: 9,997화합물 / ZINC15: ~15억 화합물)로부터 학습/예측을 다시 진행하였고 GNN으로부터 스크리닝된 6,820개의 구조 중 기존 항생제와 유사도(Tanimoto similarity)가 적은 화합물을 골라 23개의 새로운 후보 물질을 선별하였습니다.
이 23개의 후보 물질 중 8개는 이어지는 실험에서 실제 항균효과를 보이는 고무적인 결과가 확인되었습니다.


### 마무리하며
논문에 대한 리뷰는 간략하지만 이정도에서 마무리를 하고자 합니다.
머신러닝을 이용한 신약개발 자체에 대해 관심이 있으신 분들은 원문을 읽어보시는 것도 큰 의미가 있을 것으로 생각이 됩니다.
이번 시간에 저희가 목표로 했던 내용은 **GNN이라는 딥러닝 방법이 존재하는데, 의사들도 이것에 대해 알면 도움이 될 수 있다**라는 느낌을 얻어가는 것인 만큼 이에 충분히 동기 부여가 되었기를 바랍니다.
추가적으로, 위에 말씀드린 성공적인 딥러닝 적용을 위한 세 가지 고려사항(목표, 데이터, 모델)은 꼭 GNN이 아니더라도 반드시 기억하시는 것을 권유드리고 싶습니다.
현실적인 목표를 세우는 것이나, 데이터 숫자가 충분한지를 예상하는 것은 딥러닝에 대한 경험이 축적되기 전까지는 직관적으로 알기 어려운 부분이 있습니다.
그래도 이어지는 GNN에 대한 강의를 통해 GNN 모델이 어떻게 작동하는지 이해하고 다뤄보아서 실제 연구 방향 설정시에 다른 분야 전문가들과 원활한 융합/소통이 가능해진다면 이러한 문제들을 더 효율적으로 접근할 수 있을 것이라고 확신합니다.

#### [다음: What is the GNN?](https://github.com/devdoctors/practical-ai-for-doctors/blob/main/gnn/02-what.md)
