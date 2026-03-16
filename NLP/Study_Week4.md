# 17. BERT

## 17.1 NLP에서의 사전 훈련(Pre-training)

### 17.1.1 사전 훈련된 워드 임베딩
- 워드 임베딩 방법론
  1. 임베딩 층을 랜덤 초기화하여 처음부터 학습하는 방법
  2. 방대한 데이터로 임베딩 알고리즘으로 사전에 학습된 임베딩 벡터들을 가져와 사용하는 방법
     - 태스크에 사용하기 위한 데이터가 적은 경우, 사전 훈련된 임베딩을 사용하면 성능 향상이 기대됨
  3. 예: Word2Vec, FastText, GloVe 등
- 위 두 방법 모두 하나의 단어가 하나의 벡터값으로 맵핑되므로, 문맥을 고려하지 못하여 다의어나 동음이의어를 구분하지 못함
- 위 한계를 사전 훈련된 언어 모델을 사용함으로서 극복

### 17.1.2 사전 훈련된 언어 모델

#### LSTM 언어 모델

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/8ece3f1d-7fd0-42cc-aea5-49ab8decd745" />

- LSTM 언어 모델을 학습하고 나서 학습한 LSTM을 텍스트 분류에 추가하는 학습 방법
- 주어진 텍스트로부터 이전 단어들로부터 다음 단어를 예측하도록 학습
- 기본적으로 별도의 레이블이 부착되지 않은 텍스트 데이터로 학습 가능
- 학습 전 사람이 별도로 레이블을 지정해줄 필요가 없음

#### ELMO

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a3b78e9a-fb24-45d8-8428-d43b20873bf7" />

- 순방향 언어 모델과 역방향 언어 모델을 각각 따로 학습
- 위 과정으로 사전 학습된 언어 모델로부터 임베딩 값을 얻음
- 문맥에 따라 임베딩 벡터값이 달라지므로, 다의어를 구분할 수 없었던 문제를 해결

#### Trm(Transformer)

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/5796e602-abb8-4aef-b5cd-43b0c73a1269" />

- 트랜스포머의 디코더는 순차적으로 이전 단어들로부터 다음 단어를 예측

#### 양방향 언어 모델

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f1b61d42-5990-42e9-bef7-8819c3f73b85" />

- 기존 언어 모델을 양방향 구조 구현하는 경우는 거의 없응
  - 이미 예측해야하는 단어를 역방향 언어 모델을 통해 미리 관측한 셈이기 때문
  - 즉, '예측'은 순방향이어야 함
- 양방향 언어모델의 대안으로 ELMo에서 순방향과 역방향이라는 두개의 단방향 언어 모델을 따로 학습

### 17.1.3 마스크드 언어 모델(Masked Language Model)
- 마스킹: 원래의 단어가 무엇이었는지 모르게 함
- 순서
  1. 입력 텍스트 단어 집합의 15%의 단어를 랜덤으로 마스킹
  2. 인공 신경망에게 마스킹된 단어들을 예측하도록 함
 

## 17.2 BERT(Bidirectional Encoder Representations from Transformers)
