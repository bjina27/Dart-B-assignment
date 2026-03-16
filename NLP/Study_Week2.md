# 9. 워드 임베딩

## 9.1 워드 임베딩(Word Embedding)
워드 임베딩은 단어를 벡터로 표현하는 방법으로, 단어를 밀집 표현으로 변환함

### 9.1.1 희소표현(Sparse Representation)

#### 원-핫 인코딩
- 원-핫 인코딩을 통해서 나온 원-핫 벡터들은 표현하고자 하는 단어의 인덱스 값만 1이고, 나머지 인덱스는 전부 0으로 표현됨
- 벡터 또는 행렬의 값의 대부분이 0으로 표현되는 방법을 희소 표현이라 함
- 원-핫 벡터는 희소 벡터임

#### 희소 벡터의 문제점
- 단어의 개수가 늘어나면 벡터의 차원이 한없이 커짐
- 심지어 단어의 인덱스에 해당되는 부분만 1이고 나머지는 전부 0의 값을 가져야 함
- 예시: 강아지 = [ 0 0 0 1 0 0 0 ... 0]
  - 강아지랑 단어의 인덱스가 4라면
  - n개의 단어 중 강아지 단어를 제외한 n-1개는 모두 0으로 표현됨
- 결론적으로, 공간적 낭비가 심함
- DTM도 희소 표현의 일종

### 9.1.2 밀집 표현(Dense Representation)
- 희소 표현과 반대되는 표현
- 벡터의 차원을 단어 집합의 크기로 상정하지 않음
- 사용자가 설정한 값으로 모든 단어의 벡터 표현의 차원을 맞춤
- 0과 1만 가진 값이 아닌 실수값을 가지게 됨
- 만약 밀집 표현을 사용하고 사용자가 밀집 표현의 차원을 128로 설정한다면, 모든 단어의 벡터 표현의 차원은 128로 바꾸면서 모든 값이 실수가 됨
  - 예시: 강아지 = [ 0.2 1.8 -2.1 ... 3.0]
    - 위의 실수 값들은 128차원이라는 거대한 의미 공간에서 강아지가 위치한 좌표
    - 첫번째 축(x1)에서 강아지가 0.2만큼 위치
    - 세번째 축(x3)에서 강아지가 -2.1만큼 위치
  - 차원 수 = 벡터의 길이(좌표축 개수)
  - 차원을 128로 정하면, 모든 단어가 128개의 숫자로 표현

### 9.1.3 워드 임베딩(Word Embedding)
- 워드 임베딩: 단어를 밀집 벡터의 형태로 표현하는 방법
- 워드 임베딩 과정을 통해 나온 밀집 벡터 = 임베딩 벡터
- 워드 임베딩 방법론: LSA, Word2Vec, FastText, Glove 등
- 케라스 제공 도구: Embedding()
  - 위 방법론을 사용하지는 않음
  - 단어를 랜덤한 값을 가지는 밀집 벡터로 변환한 뒤
  - 인공 신경망의 가중치를 학습하는 것과 같은 방식으로 단어 벡터를 학습하는 방법을 사용

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/13fc4987-3ef4-4691-aafa-c319a5cedbb4" />


## 9.2 워드투벡터(Word2Vec)
단어 벡터 간 유의미한 유사도를 반영할 수 있도록 단어의 의미를 수치화하는 방법

### 9.2.1 희소 표현(Sparse Representation)
- 원-핫 인코딩과 같은 희소 표현은 각 단어 벡터 간 유의미한 유사성을 표현할 수 없음
- 분산 표현(distributed representation)
  - 희소표현의 대안
  - 단어의 의미를 다차원 공간에 벡터화하는 방법
  - 워드 임베딩: 분산 표현을 아용하여 단어 간 의미적 유사성을 벡터화하는 작업

### 9.2.2 분산 표현(distributed representation)
- 분포 가설이라는 가정 하에 만들어진 표현법
  - 가정: **비슷한 문맥에서 등장하는 단어들은 비슷한 의미를 가진다**
  - 예: 강아지는 귀엽다, 예쁘다, 애교 등과 함께 등장
  - 비슷한 문맥에서 등장하는 단어 벡터들은 유사한 벡터값을 가짐
- 벡터의 차원이 단어 집합의 크기일 필요가 없으므로 벡터의 차원이 상대적으로 저차원으로 줄어들음
- 예: 강아지 = [0.2 0.3 0.5 0.7 0.2 ... 중략 ... 0.2]
- 저차원에 단어의 의미를 여러 차원에다 분산하여 표현
- 단어 벡터 간 유의미한 유사도 계산 가능
- 대표적 학습 방법: Word2Vec

### 9.2.3 CBOW(Continuous Bag of Words)

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/85f7366f-b427-4276-b8c0-1a52d1967f81" />

- Word2Vec의 학습 방식
- 주변에 있는 단어들을 입력으로 중간에 있는 단어들을 예측하는 방식 
- 예시
  - 예문 : "The fat cat sat on the mat"
  - 목적: ['The', 'fat', 'cat', 'on', 'the', 'mat']으로부터 sat을 예측하는 것
  - 중심 단어: sat
  - 주변 단어: 예측에 사용되는 단어
  - 윈도우: 중심 단어 기준으로 앞뒤 몇 개를 볼지 정하는 범위
    - 윈도우 크기가 n이면 문맥 단어 수는 2n
    - 슬라이딩 윈도우: 윈도우를 옆으로 움직여서, 주변 단어와 중심 단어의 선택을 변경해가며 학습을 위한 데이터셋을 만듦

#### CBOW의 인공 신경망

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/9290c4ec-cfd6-4aae-b388-3c65feab533a" />

- 입력층의 입력으로서 앞뒤로 사용자가 정한 윈도우 크기 범위 안에 있는 주변 단어들의 원-핫 벡터가 들어감
- 출력층에서 예측하고자 하는 중간 단어의 원-핫 벡터가 레이블로서 필요
- Word2Vec은 은닉층이 1개인 얕은 신경망
  - Word2Vec의 은닉층은 활성화 함수가 존재하지 않음
  - 투사층: 룩업 테이블이라는 연산을 담당하는 층

##### 동작 메커니즘

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/3833e039-19be-4ec8-bddf-e2ef0e6079d1" />

- 투사층의 크기 = M: 임베딩하고 난 벡터의 차원
- 입력층과 투사층 사이 가중치 W는 V × M 행렬
- 투사층에서 출력층사이의 가중치 W'는 M × V 행렬
  - V: 단어 집합의 크기
- 위 두 행렬은 동일한 행렬을 전치한 것이 아닌, 서로 다른 행렬
- 인공 신경망 훈련 전에 W와 W'는 랜덤 값을 가짐
- 결국, CBOW는 주변 단어로 중심 단어를 더 정확히 맞추기 위해 계속해서 이 W와 W'를 학습해가는 구조

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/fce21e1e-d604-4a9e-aa51-3a374529c21d" />

- 룩업 테이블 (lookup table)
  - 입력 벡터는 원-핫 벡터
  - i번째 인덱스에 1의 값을 가짐
  - 그 외의 0의 값을 가지는 입력 벡터와 가중치 W 행렬의 곱은 W행렬의 i번째 행을 그대로 읽어오는 것과 동일
  - lookup해온 W의 각 행벡터가 Word2Vec 학습 후에는 각 단어의 M차원의 임베딩 벡터로 간주됨

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/f5fbbd59-d16c-4bf5-a593-5555816d378e" />

- 주변 단어의 원-핫 벡터에 대해 가중치 W가 곱해서 생긴 결과, 벡터들은 투사층에서 만나 이 벡터들의 평균 벡터를 구하게 됨
- 투사층에서 벡터의 평균을 구하는 부분은 CBOW가 Skip-Gram과 다른 차이점

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/110edfdb-4c19-4f37-8625-b78dddf3d103" />

- 구한 평균 벡터는 두번째 가중치 행렬 W'와 곱해짐
  - 곱셈의 결과, 원-핫 벡터들과 차원이 V로 동일한 벡터가 나옴
  - 만약 입력 벡터의 차원이 7이었다면 여기서 나오는 벡터도 동일함
- 위에서 나온 벡터에 소프트맥스 함수를 지나며 벡터의 각 원소들의 값은 0과 1 사이의 실수로, 총 합은 1이 됨
- 다중 클래스 분류 문제를 위한 일종의 스코어 벡터
- 스코어 벡터의 j번째 인덱스가 가진 0과 1사이 값 = j번째 단어가 중심 단어일 확률
- 스코어 벡터의 값은 중심 단어 원-핫 벡터의 값에 가까워져야 함
- 스코어 벡터: $\hat{y}$
- $\hat{y}$과 y(중심 단어의 원-핫 벡터) 이 두 벡터값의 오차를 줄이기 위해 **손실함수와 크로스 엔트로피 함수** 사용
- 크로스 엔트로피 손실 함수
  - <img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/97033305-7201-4d06-a6ee-67c0bf30e09a" />

- 역전파를 수행하면 W와 W'가 학습됨
  - 학습이 다 되면 M차원의 크기를 갖는 W의 행렬의 행을 각 단어의 임베딩 벡터로 사용하거나
  - W와 W' 행렬 두 가지 모두를 갖고 임베딩 벡터를 사용하기도 함

##### 정리
정리하자면, CBOW는 문맥임 단어들을 원-핫 -> 임베딩 -> 평균 -> 소프트맥스 확률 분포로 변환하고, 크로스 엔트로피 손실로 중심 단어를 맞히도록 가중치를 학습하는 신경망 모델임

### 9.2.4 Skip-gram

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/1cbbfd47-548d-428a-97d0-64b2f581f3c1" />

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/96fbf609-9771-41f4-8fdc-1998587b24e7" />


- 중심 단어에서 주변 단어를 예측하는 방식(CBOW와 반대)
- Skip-gram이 CBOW보다 성능이 좋
- 데이터셋: (중심,문맥) 쌍
- 신경망 구조
  - 구조는 CBOW와 거의 같음
  - CBOW와의 차이점
    - 입력은 중심 단어 1개만 들어오기에 투사층에서 평균 낼 필요가 없음
    - 투사층 결과(임베딩 벡터)를 사용해 여러 개의 문맥 단어를 동시에 예측

### 9.2.5 NNLM Vs. Word2Vec

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/fbb464f1-05d3-421a-a023-b8580c72c469" />

#### NNLM(피드 포워드 신경망 언어 모델)
- 단어 벡터 간 유사도를 구할 수 있도록 워드 임베딩의 개념을 도입
-워드 임베딩 자체에 집중하여 NNLM의 느린 학습 속도와 정확도를 개선하여 탄생한 것이 Word2Vec

#### 차이점
- 예측하는 대상이 다름
  - NNLM은 다음 단어를 예측하는 언어 모델이 목적이므로 다음 단어를 예측
  - NNLM은 예측 단어 이전 단어들만을 참고
  - Word2Vec(CBOW)은 워드 임베딩 자체가 목적이므로 다음 단어가 아닌 중심 단어를 예측
  - Word2Vec은 예측 단어의 전후 단어들을 모두 참고
- 구조가 다름
  - Word2Vec은 NNLM에 존재하던 활성화 함수가 있는 은닉층을 제거함
  - Word2Vec은 투사층 다음에 바로 출력층으로 연결되는 구조임
- 추가적으로 사용되는 기법
  - Word2Vec은 계층적 소프트맥스와 네거티브 샘플링 기법 사용
- 연산량
  - NNLM: $(n \times m) + (n \times m \times h) + (h \times V)$
  - Word2Vec: $(n \times m) + (m \times \log(V))$

## 9.3 영어/한국어 Word2Vec 실습
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/17zRqwUsdMY_DDBRkrNC6rWTSFPj_wCD-)

## 9.4 네거티브 샘플링을 이용한 Word2Vec 구현(Skip-Gram with Negative Sampling, SGNS)

### 9.4.1 네거티브 샘플링(Negative Sampling)
- 문제점
  - Word2Vec은 출력층에서 소프트맥스 확률 벡터와 원-핫 벡터의 오차를 구하고
  - 역전파로 임베딩 테이블의 모든 단어 벡터를 업데이트 해야함
  - 단어 집합 크기가 크면 연산이 무거워짐
- 비효율 예시
  - 중심/주변 단어가 '강아지', '고양이', '귀여운'인데
  - '돈가스', '컴퓨터'처럼 관련 없는 단어까지 게속 업데이트하는 것은 비효율적
- 네거티브 샘플링
  - 현재 주변 단어는 긍정(positive)
  - 무작위로 뽑은 주변이 아닌 단어는 부정(negative)로 둠
- 변환
  - 이렇게 해서 전체 단어 집합 대신 작게 줄인 단어 집합을 사용
  - 원래는 다중 클래스 분류 문제였는데, 이를 이진 분류 문제로 변환함
- 효과
  - 모든 단어를 다 갱신할 필요 없이, 일부 단어에만 집중함
  - 연산량이 훨씬 효율적
 
### 9.4.2 네거티브 샘플링 Skip-Gram(Skip-Gram with Negative Sampling, SGNS)
- 기존 Skip-gram
  - 입력: 중심 단어
  - 출력: 주변 단어 예측
- SGNS
  - 입력: 중심 단어 + 주변 단어, 쌍으로 입력
  - 출력: 두 단어가 실제오 윈도우 내 이웃 관계인지 여부, 확률
    - 단어 쌍이 positive(1)인지, negative(0)인지
   
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/9db59da2-f9f9-4975-8888-8abcbd16c593" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/5ac97eb2-673c-4301-ae57-2c32062e0200" />

- 데이터셋 변환
  - 중심 단어 = 입력 1
  - 실제 주변 단어 = 입력2, 레이블 = 1
  - 무작위로 뽑은 단어 = 입력2, 레이블 = 0
  - (입력1, 입력2, 레이블) 구조의 데이터셋 생성
 
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/391b2b2b-7c6c-43e6-9225-e750c9f5a43e" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/baf29ad0-996b-4fcb-9340-7970b8d36290" />

- 두 개의 임베딩 테이블
  - 중심 단어용 임베딩 테이블(입력1 룩업)
  - 주변 단어용 임베딩 테이블(입력2 룩업)
  - 두 테이블의 크기는 동일(단어 집합 크기 X 임베딩 차원)
 
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/ea2dce70-d23c-44ae-b0d0-bf1fed3b2aa6" />

- 학습 과정
  - 중심 단어와 주변 단어 각자 임베딩 벡터로 변환
  - 두 벡터의 내적을 모델의 예측값으로 함
  - 레이블(0/1)과 비교해 오차 계산
  - 역전파로 두 임베딩 테이블 업데이트
- 학습 후 활용
  - 학습 완료 후 사용할 수 있는 임베딩 벡터 방식
    - 중심 단어용 테이블만 사용
    - 두 테이블의 벡터를 합해서 사용
    - 두 테이블의 벡터를 연결해서 사

### 9.4.3 ~ 9.4.6
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/17xahzedpVSp_kN16sDtEvwAP4A81GBne)

## 9.5 글로브(GloVe)
- 카운트 기반(LSA)과 예측 기반(Word2Vec)을 모두 활용
- 기존 두 접근법의 단점을 보완하기 위해 제안됨
- Word2Vec만큼 뛰어난 성능을 보임

### 9.5.1 기존 방법론에 대한 비판

#### LSA
- DTM/TF-IDF 같은 단어-문서 행렬(빈도수 기반 전체 통계)을 입력으로 받아 차원 축소(Truncated SVD) 수행
- 장점: 코퍼스 전체 통계 정보를 고려
- 단점: 단어 의미 유추(Analogy task)에서 성능이 떨어짐

#### Word2Vec
- 실제값과 예측값의 오차를 손실 함수로 줄여나가는 예측 기반 학습
- 장점: 단어 간 유추 작업에서 LSA보다 뛰어남
- 단점: 윈도우 크기 내 주변 단어만 고려하므로 코퍼스 전체 통계 정보는 반영하지 못함

#### GloVe
- 위 두 접근법의 한계를 보완하려고 등장
- LSA의 카운트 기반과 Word2Vec의 예측 기반 방식을 모두 활용

### 9.5.2 윈도우 기반 동시 등장 행렬(Window based Co-occurrence Matrix)
- 구성: 행과 열 = 전체 단어 집합의 단어들
- 값: i 단어의 윈도우 내에 k 단어가 등장한 횟수를 i행 k열에 기록
- 윈도우 크기 = N일 때, 좌/우에 존재하는 N개의 단어만 참고
- 윈도우 크기 = 1일 때, 각 단어의 주변 단어 등장 횟수를 집계하여 동시 등장 행렬을 완성함
- 행렬을 전치해도 동일함
  - 이유: i 단어의 윈도우 내 k 단어 등장 횟수 = k 단어의 윈도우 내 i 단어 등장 횟수

### 9.5.3 동시 등장 확률(Co-occurrence Probability)
- 동시 등장 행렬로부터 계산한 조건부 확률
- 중심 단어 = i, 주변 단어 = k
- $P(k|i)$: 특정 단어 i가 등장했을 때 어떤 단어 k가 등장한 횟수를 카운트하여 계산한 조건부 확률
- 단순히 동시 등장 횟수가 아닌, 동시 등장 확률과 그 비율을 보면 단어 의미 관계가 잘 드러남

### 9.5.4 손실함수(Loss function)

#### 용어 정리
- **X** : 동시 등장 행렬 (Co-occurrence Matrix)  
- **X_ij** : 중심 단어 i가 등장했을 때 주변 단어 j가 등장한 횟수  
- **X_i = Σ_j X_ij** : 중심 단어 i가 등장했을 때 모든 주변 단어 등장 횟수의 총합  
- **P_ik = P(k | i) = X_ik / X_i** : 중심 단어 i가 등장했을 때 주변 단어 k가 등장할 조건부 확률  
- **w_i** : 중심 단어 i의 임베딩 벡터  
- **ẇ_k** : 주변 단어 k의 임베딩 벡터 (tilde 표시는 주변단어용)  

#### GloVe의 아이디어
- 한줄 요약: **임베딩된 중심 단어 벡터와 주변 단어 벡터의 내적이 전체 코퍼스에서의 동시 등장 확률이 되도록 만드는 것**
- $w_i^\top \tilde{w}_k \approx \log P_{ik}$

#### 손실 함수 유도 과정
- 임베딩 벡터의 차이와 주변 단어 벡터의 내적으로 확률 비율을 표현
  - $F((w_i - w_j)^\top \tilde{w}_k) = \frac{P_{ik}}{P_{jk}}$
- 내적 결과가 동시 등장 확률을 반영하도록 설정 
  - $\exp(w_i^\top \tilde{w}_k) = P_{ik} = \frac{X_{ik}}{X_i}$
- 내적이 동시 등장 횟수의 로그 값과 연결됨
  - $w_i^\top \tilde{w}_k = \log X_{ik} - \log X_i$
- 편향을 추가하여 대칭성 문제를 해결
  -$w_i^\top \tilde{w}_k + b_i + \tilde{b}_k = \log X_{ik}$
- 모든 단어 쌍에 대해 제곱 오차를 최소화 
  - $J = \sum_{m,n=1}^V \big(w_m^\top \tilde{w}_n + b_m + \tilde{b}_n - \logX_{mn}\big)^2$
  - 예측된 내적값과 실제 로그 동시 등장 수의 차이를 최소화
- 희소 행렬 문제를 완화하기 위해 가중치 함수 f(x) 추가
  - $f(x) = \min \Big(1, \big(\tfrac{x}{x_{\max}}\big)^{3/4}\Big)$
  - 자주 등장하지 않는 단어 쌍은 가중치를 낮추고, 빈도 높은 단어는 충분히 반영
- 최종 손실 함수
  - $J = \sum_{m,n=1}^V f(X_{mn}) \cdot \big(w_m^\top \tilde{w}_n + b_m + \tilde{b}_n - \log X_{mn}\big)^2$
  - 가중치를 포함한 최종 목적 함수: 임베딩이 동시 등장 로그 확률을 잘 근사하도록 학습

### 9.5.5 GloVe 훈련시키기
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/17ml0uPxjhU8iFpFqdZ14AerJ0LrOPLWV)


## 9.6 패스트텍스트(FastText)
- Word2Vec의 확장판
- 차이점
  - Word2Vec: 단어를 쪼개지 않는 독립 단위로 취급
  - FastText: 단어를 내부 단어(subword) 단위의 조합으로 취급 (n-gram 기반)

### 9.6.1 내부 단어(subword)의 학습
```
# n = 3인 경우
<ap, app, ppl, ple, le>
```
- 각 단어를 글자 단위 n-gram으로 분리하여 각각 벡터화 후 합산
- 특별 토큰: <apple>
- 최종 벡터: 위 벡터들의 합
- 실제 사용시 n의 최대/최소 범위 설

### 9.6.2 모르는 단어(Out Of Vocabulary, OOV)에 대한 대응
- Word2Vec, GloVe: OOV 단어는 벡터를 만들 수 없음
- FastText: 학습한 subword 벡터 조합으로 OOV 단어도 벡터 생성 가능
- 예시: birthplace가 학습에 없더라도 birth + place subword 벡터로 유추 가능

### 9.6.3 단어 집합 내 빈도 수가 적었던 단어(Rare Word)에 대한 대응
- Word2Vec: 등장 빈도가 적은 단어는 벡터 품질이 낮음
- FastText : 희귀 단어라도 다른 단어와 n-gram 공유 시 비교적 정확한 벡터 생성 가능

### 9.6.4 실습으로 비교하는 Word2Vec Vs. FastText
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/17g-YLp9ZDx3sbS_ZAzV-263s7KAiTYJO)

### 9.6.5 한국어에서의 FastText

#### 음절단위
```
<자연, 자연어, 연어처, 어처리, 처리>
```
- n=3일 때 '자연어처리' 단어에 대한 n-gram

#### 자모단위
```
분리된 결과 : ㅈ ㅏ _ ㅇ ㅕ ㄴ ㅇ ㅓ _ ㅊ ㅓ _ ㄹ ㅣ _
n-gram을 적용하여 임베딩: < ㅈ ㅏ, ㅈ ㅏ _, ㅏ _ ㅇ, ... 중략>
```
- 자모 단위(초성, 중성, 종성 단위)로 임베딩
- 오타나 노이즈 측면에서 더 강한 임베딩

## 9.7 자모 단위 한국어 FastText 학습하기

## 9.8 사전 훈련된 워드 임베딩(Pre-trained Word Embedding)
- 케라스 임베딩 층(embedding layer)
  - Keras의 Embedding() 층을 사용하여 모델 학습 과정에서 단어를 벡터로 학습하는 방법
- 사전 훈련된 워드 임베딩 (Pre-trained Word Embedding)
  - 위키피디아 같은 방대한 코퍼스에서 Word2Vec, FastText, GloVe 등으로 미리 학습된 임베딩 벡터를 불러와 사용하는 방법

### 9.8.1 케라스 임베딩 층(Keras Embedding layer)
- 케라스는 단어 임베딩을 수행하는 도구로 Embedding()을 제공
- 인공 신경망 구조 관점에서 임베딩 층(embedding layer)을 구현하는 역할

#### 임베딩 층은 룩업 테이블이다.

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/575cc5cd-19e6-4d56-b90f-c5376e7f0ec9" />

- 입력 단어는 정수 인코딩 되어 있어야 함.
- 흐름: 단어 > 고유 정수값 > 임베딩 층 통과 > 밀집 벡터(dense vector)
- 임베딩 층은 입력 정수에 대해 밀집 벡터로 맵핑하고, 이 벡터는 학습 과정에서 갱신됨
- 각 단어는 단어 집합 크기만큼의 행을 가지는 테이블 내 고유한 벡터와 연결
- 즉, 정수를 인덱스로 하는 룩업 테이블에서 임베딩 벡터를 가져오는 것
- 원-핫 벡터 혼동
  - Word2Vec/NNLM 설명에서는 입력을 원-핫 벡터로 가정
  - 하지만 케라스는 원-핫 변환을 거치지 않음
  - 단어를 정수 인코딩까지만 진행 후 임베딩 층에 입력한 후 임베딩 벡터 반환
~~~
케라스 인베딩 층 구현 코드

vocab_size = 20000
output_dim = 128
input_length = 500

v = Embedding(vocab_size, output_dim, input_length=input_length)
~~~
- 인자
  - vocab_size = 텍스트 데이터의 전체 단어 집합의 크기
  - output_dim = 워드 임베딩 후의 임베딩 벡터의 차원
  - input_length = 입력 시퀀스의 길이, 만약 각 샘플의 길이가 500개라면 이 값은 500
- 입력: (number of samples, input_length) 형태의 2D 정수 텐서
  - 각 샘플은 정수 인코딩된 시퀀스
- 출력: (number of samples, input_length, embedding word dimensionality) 형태의 3D 실수 텐서

### 9.8.1 ~ 9.8.2 코드
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/17Y9pEQDQT-terBqcgLfb7PlAfm4g8byi)


## 9.9 엘모(Embeddings from Language Model, ELMo)
- 언어 모델로 하는 임베딩
- 사전 훈련된 언어 모델을 사용

### 9.9.1. ELMo(Embeddings from Language Model)
- 한계: Word2Vec, GloVe 같은 전통적 임베딩은 같은 단어를 항상 동일한 벡터로 표현
  - 예: Bank = [0.2, 0.8, -1.2]
  - Bank Account 와 River Bank 은 서로 다른 의미임에도 동일 벡터 사용
- 같은 표기의 단어라도 문맥에 따라 다르게 임베딩하면 성능 향상 가능
- 문맥을 고려해 임베딩을 수행하는 접근을 문맥을 반영한 워드 임베딩(Contextualized Word Embedding) 이라고 함

### 9.9.2 biLM(Bidirectional Language Model)의 사전 훈련

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/1d6d48cb-e8ef-47cb-91f6-862a2483d8d0" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/d2b391ba-4b34-4d0d-84b9-edcb2cf7c2d5" />

- RNN 언어 모델
  - 단어 단위 입력을 받고, 시점이 지남에 따라 은닉 상태가 업데이트
  - 은닉 상태는 문맥 정보를 점차적으로 반영
- 순방향 RNN뿐 아니라 반대 방향으로 스캔하는 역방향 RNN도 함께 사용
- 양쪽 방향의 언어 모델을 모두 학습하는 것이 biLM
- 기본적으로 다층 구조 (Multi-layer), 은닉층이 최소 2개 이상
- 각 시점의 입력 단어 벡터는 임베딩 층이 아니라 문자 임베딩으로 얻음
- 문자 임베딩은 subword 정보를 활용하듯 단어 변형의 연관성을 포착 가능
- OOV 단어에도 견고함
- 주의점
  - 양방향 RNN: 순방향과 역방향 은닉 상태를 연결하여 다음 층 입력으로 사용
  - biLM: 순방향 언어 모델과 역방향 언어 모델을 별개의 모델로 보고 학습

### 9.9.3 biLM의 활용

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/37796d2d-4e95-4849-aeee-faded020a767" />

- 사전 훈련된 biLM으로부터 입력 문장의 단어를 임베딩
- 단계

<img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/f8950346-2a95-4d99-9ae9-b9c0ff20e7f7" />
<img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/8343822e-f7e6-4565-bf30-18f5c4f23451" />
<img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/76894a56-82c8-47fe-83bc-1cdf646b09fc" />
<img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/d9725b3b-ba0f-4771-9c21-d77c3335ca17" />

  1) 각 층의 출력값을 연결
  2) 각 층의 출력값에 가중치 부여
  3) 가중치가 적용된 층 출력값을 모두 더함 = 가중합
  4) 벡터 크기를 조정하는 스칼ㄹ 매개변수 y를 곱함
- ELMo 표현은 기존 임베딩(GloVe 등)과 함께 사용할 수 있음
- 예: 텍스트 분류 작업
  - GloVe 임베딩 벡터 준비
  - ELMo 표현과 연결(concatenate) 하여 입력으로 사용
- biLM의 가중치는 고정
- 층별 가중치와 스칼라 매개변수 y는 훈련 과정에서 학습됨


## 9.11 문서 벡터를 이용한 추천 시스템(Recommendation System using Document Embedding)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/16l5t-yBSt9H2SKZCCVeuPDiqgt0SZX3q)


## 9.12 문서 임베딩 : 워드 임베딩의 평균(Average Word Embedding)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/16YNgfCgAcA91GBBEKVtC4DR1W5ZL4QCX)


## 9.13 Doc2Vec으로 공시 사업보고서 유사도 계산하기
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/188eGCDytoNoDgMWPJzn-OwDhw7-8hsiA)

## 9.14 실전! 한국어 위키피디아로 Word2Vec 학습하기
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/19Qg8pcbE3PmaeeQ4seU6ogM79aXTV293)


# 12. 태깅 작업(Tagging Task)

## 12.1 케라스를 이용한 태깅 작업 개요(Tagging Task using Keras)

### 12.1.1 훈련 데이터에 대한 이해

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/db687b93-65cf-4be7-be05-63da1b3477e9" />

- 태깅 작업도 텍스트 분류와 마찬가지로 지도 학습에 속함
- 데이터 명명 규칙
  - 입력 단어 데이터: X
  - 레이블(태깅 정보): y
  - 훈련 데이터: X_train, y_train
  - 테스트 데이터: X_test, y_test
- X와 y는 쌍(pair) 관계를 가짐
- 각 샘플에서 X와 y의 길이는 동일
- 모델 입력 전처리
  - 정수 인코딩: 단어와 레이블을 숫자로 변환
  - 패딩: 모든 데이터 길이를 동일하게 맞춤

### 12.1.2 시퀀스 레이블링(Sequence Labeling)
- 입력 시퀀스 X = [x1,x2, ..]에 대해 레이블 시퀀스 y = [y1,y2, ..]을 부여하는 작업
- 태깅 작업은 대표적인 시퀀스 레이블링 작업

### 12.1.3 양방향 LSTM(Bidirectional LSTM)
```
model.add(Bidirectional(LSTM(hidden_units, return_sequences=True)))
```
- 양방향은 기존의 단방향 LSTM()을 Bidirectional() 안에 넣으면 됨
- LSTM의 인자값은 단방향 LSTM을 사용할 때와 동일
- 인자값을 하나를 줄 경우 은닉 상태의 차원을 의미

### 12.1.4 RNN의 다-대-다(Many-to-Many) 문제

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/59fbcb4c-4ff3-4b41-8e93-ef8a51014d5b" />

- RNN 은닉층의 두 가지 출력 방식
  - 모든 시점의 은닉 상태 출력
  - 마지막 시점의 은닉 상태만 출력
- 케라스에서 return_sequences 인자로 설정
  - return_sequences=False: 마지막 시점만 출력
  - return_sequences=True: 모든 시점 출력
- 태깅 작업은 다-대-다(Many-to-Many) 문제이므로 return_sequences=True로 설정하여 모든 은닉 상태의 값을 출력층에 전달


## 12.2 양방향 LSTM를 이용한 품사 태깅(Part-of-speech Tagging using Bi-LSTM)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/15ETIIjMEqL_7q2Hb9HKKRM2UqVCKmwvr)

### 전처리
- NLTK의 treebank 코퍼스를 사용해 품사 태깅된 문장들을 받아옴
- 문장별로 단어에 해당되는 부분과 품사 태깅 정보에 해당되는 부분을 분리
-  zip(): 동일한 개수를 가지는 시퀀스 자료형에서 동일한 순서에 등장하는 원소들끼리 묶어주는 역할
- 단어와 태그 모두 정수 인코딩
- 문장 길이를 동일하게 만들기 위해 패딩(pad_sequences) 사용
- 학습용/테스트용 데이터 비율로 분할

### 모델 셜계
- 양방향 LSTM(Bi-LSTM) 사용
- 손실 함수를 categorical_crossentropy 대신 sparse_categorical_crossentropy를 선택
  - 이유: 레이블에 원-핫 인코딩을 하지 않고 학습을 진행하고자 함
- 학습 후 테스트 정확도 측정
- 샘플 하나를 뽑아 단어별로 실제값 vs 예측값 비교하여 결과 확인

### 추가 설명
- 패딩 길이 결정: 일반적으로는 훈련 데이터 기준으로 하거나, 넉넉히 길이를 잡음
- mask_zero=True 옵션: 패딩 토큰을 무시해서 성능 개선에 도움 가능성
- TensorFlow/Keras 버전에 따라 import 방식 차이가 영향 줄 수 있음
- 예측 확인 시 PAD 값(0)인 단어는 제외해서 출력


## 12.3 개체명 인식(Named Entity Recognition)

### 12.3.1 개체명 인식(Named Entity Recognition)이란?
- 이름을 가진 개체(named entity)를 찾아내고, 해당 단어가 어떤 유형에 속하는지 분류하는 작업
- 코퍼스로부터 어떤 단어가 사람, 장소, 조직 등을 의미하는 단어인지를 찾을 수 있음
```
'유정이는 2018년에 골드만삭스에 입사했다.'를 사람, 조직, 시간에 대해 개채명 인식 수행

유정 - 사람  
2018년 - 시간  
골드만삭스 - 조직
```

### 12.3.2 NLTK를 이용한 개체명 인식(Named Entity Recognition using NTLK)
```
from nltk import word_tokenize, pos_tag, ne_chunk

sentence = "James is working at Disney in London"
# 토큰화 후 품사 태깅
tokenized_sentence = pos_tag(word_tokenize(sentence))
print(tokenized_sentence)

# 개체명 인식
ner_sentence = ne_chunk(tokenized_sentence)
print(ner_sentence)
```
- ne_chunk는 개체명을 태깅하기 위해서 앞서 품사 태깅(pos_tag)이 수행되어야 함


## 12.4 개체명인식의 BIO 표현 이해하기
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/15AD5Y1p4X8KB-gguNO_be5LrA-v8M0B8)

### 12.4.1 BIO 표현
```
영화 제목에 대한 개체명 뽑아내기

해 B
리 I
포 I
터 I
보 O
러 O
가 O
자 O

해 B-movie
리 I-movie
포 I-movie
터 I-movie
보 O
러 O
메 B-theater
가 I-theater
박 I-theater
스 I-theater
가 O
자 O
```
- B(Bigin): 개체명이 시작되는 부분
- I(Inside): 개채명의 내부 부분
- O(Outside): 개체명이 아닌 부분
- 한 종류의 개체에 대해서만 언급하는 것이 아니라 여러 종류의 개체가 있을 수 있음
  - 이 경우 각 개체가 어떤 종류인지도 함께 태깅됨

### 12.4.2 개체명 인식 데이터 이해하기
```
EU NNP B-NP B-ORG
rejects VBZ B-VP O
German JJ B-NP B-MISC
call NN I-NP O
to TO B-VP O
boycott VB I-VP O
British JJ B-NP B-MISC
lamb NN I-NP O
. . O O

Peter NNP B-NP B-PER
Blackburn NNP I-NP I-PER
```
- 데이터 형식: [단어] [품사 태깅] [청크 태깅] [개체명 태깅]
- EU 옆 NNP: 고유 명사 단수형
- rejects 옆 VBZ: 3인칭 단수 동사 현재형
- LOC: location
- ORG: organization
- PER: person
- MISC: miscellaneous
- 11번째 줄 Peter가 나오는 부분 사이에서 10번째 줄이 공란으로 되어있음
  - 이는 9번째 줄에서 문장이 끝나고 11번째 줄에서 새로운 문장이 시작됨을 의미
- 11번째 줄에서는 개체명이 하나의 단어로 끝나지 않았을 때 어떻게 다음 단어로 개체명 인식이 이어지는지를 보여줌


## 12.5 BiLSTM을 이용한 개체명 인식(Named Entity Recognition, NER)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/14z-XjiJqpAjcIlTUJ4CiwJJEdxraCUb4)

### 12.5.1 개체명 인식 데이터에 대한 이해와 전처리
- Pandas의 fillna: Null 값을 가진 행의 바로 앞의 행의 값으로 Null 값을 채우는 작업을 수행
- 훈련을 시키려면 훈련 데이터에서 단어에 해당되는 부분과 개체명 태깅 정보에 해당되는 부분을 분리시켜야 함
- 동일한 개수를 가지는 시퀀스 자료형에서 각 순서에 등장하는 원소들끼리 묶어주는 역할을 하는 zip()을 사용하여 단어와 개체명 태깅 정보를 분리

### 12.5.2 양방향 LSTM을 이용한 개체명 인식
- 다 대 다(Many-to-Many) 구조의 양방향 LSTM
  - return_sequences=True 설정 필수
  - 입력 데이터에 패딩(0)이 많을 경우 Embedding(mask_zero=True)로 0은 연산에서 제외
- TimeDistributed(): 모든 시점에 대해 출력층 적용
- 각 시점마다 개체명 레이블 개수 중 하나를 예측
- 다중 클래스 분류 문제 수행
  - 활성화 함수: 소프트맥스
  - 손실 함수: 크로스 엔트로피

### 12.5.3 F1-score

<img width="100" height="50" alt="image" src="https://github.com/user-attachments/assets/3c8762f6-0d96-4b0a-a1fa-a85dd3f352fa" />

- f1-score란, 정밀도와 재현률로부터 조화 평균을 구한 것
- 정밀도 = TP/(TP+FP)
  - 특정 개체라고 예측한 경우 중에서 실제 특정 개체로 판명되어 예측이 일치한 비율
- 재현률 = TP/(TP+FN)
  - 전체 특정 개체 중에서 실제 특정 개체라고 정답을 맞춘 비  

## 12.6 BiLSTM-CRF를 이용한 개체명 인식
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/14z-XjiJqpAjcIlTUJ4CiwJJEdxraCUb4)

### 12.6.1 CRF(Conditional Random Field)
- 원래 독립적으로 존재하던 모델
- 양방향 LSTM 위에 층으로 추가하여 양방향 LSTM + CRF 모델로 확장
- 예시: 개체명 인식 (BIO 표현, 5가지 태깅)
  - B-Per, I-Per, B-Org, I-Org, O
- 문제점 (CRF 층이 없는 양방향 LSTM 모델)
  - 각 시점에서 독립적으로 개체명 예측
  - BIO 제약 위반 발생
    - 문장의 첫 단어에 I가 등장
    - I-Per이 B-Per 없이 등장
    - I-Org이 B-Org 없이 등장
- CRF 층 추가의 효과
  -활성화 함수의 출력값이 CRF 층으로 입력
  - CRF 층은 레이블 시퀀스 전체를 고려하여 가장 높은 점수의 시퀀스를 예측
  - 훈련 과정에서 다음과 같은 제약사항을 학습
    - 문장의 첫 단어에는 I가 나오지 않음
    - O-I 패턴은 등장하지 않음
    - B-I-I 패턴에서 개체명 일관성 유지 (예: B-Per 뒤에 I-Org 불가)
- 즉, 양방향 LSTM은 입력 단어의 양방향 문맥을 반영하고, CRF는 출력 레이블의 양방향 문맥을 반영함


## 12.7 문자 임베딩(Character Embedding) 활용하기
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/14z-XjiJqpAjcIlTUJ4CiwJJEdxraCUb4)

### 12.7.1 문자 임베딩(Char Embedding)을 위한 전처리
- 문자 단위 정수 인코딩
- 문자 인코딩 - 임베딩 층 통과 - 문자 단위 임베딩 벡터 획득의 과정을 거침

### 12.7.2 BiLSTM-CNN을 이용한 개체명 인식
- 단어를 문자 단위로 토큰화 - 점수 매핑 - 임베딩 층 통과의 과정을 거침
- 합성곱 처리
  - 1D 합성곱 층 입력
  - 그 결과, 하나의 단어 벡터 생성
- 문자 기반 단어 벡터와 워드 임베딩 벡터를 결합
- 모델 구조
  - 입력: 연결된 벡터
  - 출력: TimeDistributed()로 모든 시점에 대해 출력층 적용
  - 양방향 LSTM
- 즉, 문자 정보와 워드 임베딩을 결합하여 양방향 LSTM 모델 입력으로 사용

### 12.7.3 BiLSTM-CNN-CRF
- 문자 임베딩을 사용한 위 모델에 CRF 층까지 추가적으로 사용

### 12.7.4 BiLSTM-BiLSTM-CRF
- 단어를 문자 단위로 토큰화 - 점수 매핑 - 임베딩 층 통과의 과정을 거침
- 양방향 LSTM 처리
  - 다 대 일 구조
  - 출력: 순방향 + 역방향 은닉 상태 연결
    - 이 출력을 단어 벡터로 간주
- 문자 기반 단어 벡터와 워드 임베딩 벡터를 결합
- 결합된 벡터를 개체명 인식을 위한 양방향 LSTM의 입력으로 사용
- CRF 층은 원-핫 인코딩 레이블 미지원하기 때문에 y_train_int 사용
- 즉, 문자 기반 벡터와 워드 임베딩을 결합하여 BiLSTM+CRF 모델 입력으로 사용

