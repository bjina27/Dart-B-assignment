# 자연어처리와 딥러닝

## 단어 임베딩

### 단어 임베딩
- 자연어처리에서 가장 먼저 해야할 일은 단어 또는 문장을 숫자로 표현하는 것
- 단어주머니(Bag of Words, BoW)
   - 단어를 수치화하는 가장 단순한 방법
   - 학습 없이 말뭉치(corpus)에 나타나는 모든 단어들에 인덱스를 부여하는 것
   - 단어의 개수가 많아질수록 인덱스 크기가 커져서 원-핫 벡터의 크기가 매우 커짐
   - 단어 간의 유사성을 표현할 수 없음
   - 새로운 단어가 등장했을 때 처리할 수 없음

### Word2Vec

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/a622793a-2e42-4270-8bd3-9108499ecd55" />

- 자연어를 학습하여 단어를 숫자 벡터들로 분산 표현
- 원-핫 인코딩으로 희소하게 표현된 고차원 단어군을 신경망을 통해 주성분분석과 같이 저차원의 의미가 포함된 밀집 벡터로 다시 표현하는 것
- 단어들 간 얼마나 유사한지를 코사인 유사도 등으로 계산할 수 있음
- 영국 - 런던 + 서울 = 한국
- 문맥을 고려한 단어들 간 연산을 수행
- CBOW와 Skip-gram 두 가지 방법
  - CBOW
    - 단어의 앞뒤 n개의 인접단어로부터 중앙단어를 예측하는 신경망을 만들고
    - 이 신경망의 가중치 행렬로부터 숫자 벡터를 구하는 방법
    - 학습을 통해 완전연결 신경망의 가중치를 구하는데 이 가중치 행렬의 각 행이 단어의 분산 표현임
  - Skip-gram
    - 중앙의 다른 단어로부터 앞뒤 단어를 예측하는 신경망을 만들고
    - 이 신경망의 가중치 행렬로부터 숫자 벡터를 구하는 방법
  - 다른 방법: GloVe, FastText

## SEq2Seq 모형

### SEq2Seq 모형

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/73b7266a-89f1-4304-825d-4b23a1b7462c" />

1. 입력 문장을 순환 신경망의 은닉상태를 통해 문장의 내용을 압축한 맥락 벡터로 인코딩
2. 그 맥락 벡터를 디코더의 입력으로 사용해서 순차적으로 출력 문장을 생성
3. 인코더에서 순차적으로 읽은 중간 단계의 은닉상태 값은 사용되지 않고, 맥락 벡터인 최종 은닉상태만 디코더에 전달됨

### Seq2Seq 모형의 제약
1. 장기 의존성 문제
   - 은닉상태가 자연어의 복잡한 정보를 담기에는 충분히 크지 않을 때 최종 은닉상태만으로 입력 데이터 전체 문장을 대표하기 어려움
2. 순환 신경망으로 만들어지므로 문장(시퀀스)의 길이가 길어질수록 경사소실 또는 경사폭발 문제가 커짐
   - Seq2Seq 모형은 두 개의 순환신경망으로 구성되어 있고, 디코더에서 발생하는 손실 정보를 바탕으로 역전파를 통해 인코더의 가중치를 학습하므로 문제가 더 커짐
3. 같은 단어라도 위치와 상황에 따라 의미가 달라지는 언어의 특성을 반영하기 어려움
   - 동음이의어: 그녀는 밥을 잘 산다 vs 그녀는 밥을 먹고 잘 산다
  
## 어텐션

### 어텐션의 개념
- 사람을 사진에서 볼 때 한두가지 인물 또는 사물에 집중해서 사진을 봄
- 다른 사람들과 대화할 때 상대방의 말 중 몇 개의 핵심을 집중해서 듣고 상대방의 의도를 이해
- 문장 중 특정 단어를 더 집중하는 모형
- Seq2Seq 모형은 인코더의 전체 문장 정보를 최종 은닉 상태에만 요약하여 디코더에 전달하기 때문에, 문장이 길 때 제대로 변역되지 못함
- 이를 해소하기 위해 디코더에서 출력 단어의 예측 시점마다 입력되는 전체 문장 정보를 다시 검토하여 관련이 높은 입력 문장의 단어에 더 비중을 두어서 출력 단어를 예측

### Dot-product attention

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/38eb055f-efe1-4e0a-8c40-1f7ee962a8aa" />

- 디코더의 세번째 LSTM 셀에서 출력 단어를 예측할 때, 어텐션 메커니즘을 사용하는 모습
- 디코더의 첫번째, 두번째 LSTM 셀은 이미 어텐션 메커니즘을 통해 je와 suis를 예측하는 과정을 거쳤다고 가정
- 디코더의 세번째 LSTM 셀은 출력 단어를 예측하기 위해 인코더의 모든 입력 단어들의 정보를 다시 한번 참고함
- 인코더의 소프트맥스 함수를 통해 나온 결과값은 I, am, a, student 단어 각각이 출력 단어를 예측할 때 얼마나 도움이 되는지의 정도를 수치화한 값
- 빨간 직사각형의 크기: 소프트맥스 함수의 결과값의 크기를 표현
- 직사각형의 크기가 클수록 도움이 되는 정도의 크기가 큼
- 각 입력단어가 디코더의 예측에 도움이 되는 정도를 수치화하여 측정되면 이를 하나의 정보로 담아서 디코더로 전송
- 결과적으로 디코더는 출력 단어를 더 정확하게 예측할 확률이 높아짐

### 어텐션 스코어와 어텐션 가중치 계산

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/f447c50e-d398-4e49-9e8b-d2a44c6c0517" />

- 인코더의 시점별 은닉상태: <img width="70" height="35" alt="image" src="https://github.com/user-attachments/assets/e88f7356-7791-4437-99ce-45738e54418e" />
- 디코더의 현재 시점 t에서의 은닉상태: <img width="12" height="12" alt="image" src="https://github.com/user-attachments/assets/1aa4d494-1a97-4035-b4c6-101064261485" />
- 현재 시점 t에서의 어텐션 스코어: <img width="120" height="30" alt="image" src="https://github.com/user-attachments/assets/ab656097-7b1e-4a02-9874-60bcbbe2b1e6" />
- 어텐션 가중치:
<p align="center">
  <img src="https://github.com/user-attachments/assets/0c879165-b56a-4ab0-83d4-c44a302ca44b" alt="image" width="200" height="70">
</p>

### 어텐션 값 계산

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/8d08ad3f-54d6-4563-9bc5-6c6e1b92c126" />

- 어텐션의 최종 결과값을 얻기 위해 각 인코더의 은닉상태와 어텐션 가중치들을 곱하고 최종적으로 모두 더함

<p align="center">
  <img src="https://github.com/user-attachments/assets/9adea264-1b94-442f-8e8a-bc86d394b6e9" alt="image" width="150" height="80">
</p>

### 어텐션 값과 디코더의 은닉상태 결합

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/b797a1da-b6fd-4021-baab-ddd04e915c11" />

- 디코더의 현재시점 t에서의 은닉상태: $S_t$
- 현재 시점 t에서의 어텐션 값: $a_t$

### 예측 벡터
- $\tilde{S}_t$: 출력층 연산의 입력이 되는 벡터

<p align="center">
  <img width="200" height="54" alt="image" src="https://github.com/user-attachments/assets/729960b3-94b3-4536-8aa9-935ab3a3cbd7" />
</p>

-  $W_c$,  $b_c$: 가중치 행렬, 편의
-  $\hat{y}_t$: 예측벡터

<p align="center">
<img width="200" height="56" alt="image" src="https://github.com/user-attachments/assets/ee30cbfe-c701-4823-8be6-81c6a6b0725d" />
</p>

### 다양한 종류의 어텐션

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/6a5a7392-adfc-41f1-b086-a58a75b97119" />

## 트랜스포머

### 트랜스포머(Transformer)
- Seq2Seq 모형에서 순환신경망을 제거하고 어텐션 메커니즘만 이용한 트랜스포머를 제안
- BERT 를 비롯한 문장 임베딩 모형들의 근간이 됨

### Seq2Seq 모형과 트랜스포머
- Seq2Seq 모형의 병목현상: 다양하고 복잡한 문장들을 맥락 벡터에 담아내기에는 은닉상태의 크기가 부족하여 최종 은닉상태만으로 입력 데이터 전체 문장을 대표하기 어려운 장기의존성 문제
- 어텐션 메커니즘을 이용해 출력 문장의 매 토큰마다 입력 문장의 모든 토큰의 정보를 바탕으로 맥락벡터를 계산하여 입력 문장의 특성을 보다 잘 반영할 수 있게 됨
- 어텐션 메커니즘은 순환신경망에 행렬연산이 추가되어 연산의 부담이 커짐
- 어텐션 메커니즘 기반의 Seq2Seq 모형은 순차적으로 연산할 수 밖에 없는 순환신경망이라는 제약이 있었음
- 위 제약을 극복하기 위해 제안 된 모형이 트랜스포머
- 어텐션 가중치가 입력 토큰과 출력 토근의 조합 간 의존성을 모두 담고 있으므로 전후 토큰들의 의존성을 어텐션 메커니즘으로 표현할 수 있었고, 어텐션 메커니즘만으로 순환신경망을 대체할 수 있음

### 트랜스포머의 구조

<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/9789616d-1c94-4fda-a3a8-94aea66aabf1" />

- 왼쪽의 인코다와 오른쪽읜 디코더로 이루어짐
- 인코더
  - 입력 문장을 구성하는 토큰의 시퀀스들을 받아서 임베딩층을 거쳐 정해진 크기의 벡터로 변형
  - 여기에 위치 인코딩을 더해 토큰의 위치 정보를 가미한 후 N개의 인코더 트랜스포머 블록을 거침
  - 트랜스포머 블록의 입력과 출력의 크기 = (문장의 길이) X (임베딩 크기)
  - N개의 인코더 트랜스포머 블록을 거친 최종 출력은 N개의 디코더 트랜스포머 블록을 거칠 때마다 여러개의 어텐션으로 구성된 멀티-헤드 어텐션의 입력으로 사용됨
  - 위 과정을 통해 입력 문장에서 인코딩된 정보는 디코더로 넘어감
- N개의 디코더 트랜스포어 블록을 통과하면 마지막으로 밀집신경망을 거친 후 소프트맥스 함수를 적용하여 토큰을 예측

### 트랜스포머의 구성 요소 - 입력과 임베딩

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/b6dff828-fe98-4fa1-b09e-2e3ae32824aa" />

- 원-핫 벡터에서 해당 토큰의 인덱스로 입력받음
- 문장 벡터 전체를 하나의 행렬로 간주하고 행렬연산을 진행하므로 연산시간이 줄어들음
- 학습시 디코더에 문장을 입력할 때도 전체 문장을 한번에 신경망에 입력함
- 하지만, 학습된 트랜스포머를 이용하여 추론할 때 디코더는 순차적으로 토큰을 입력받음
- 실제 번역할 때 출력 문장은 없으므로 출력 문장은 이전에 생성된 토큰을 바탕으로 다음 토큰을 생성해야 하기 때문
- 입력은 임베딩 층에서 $d_model$ 차원의 실수 벡터로 변환됨
- 임베딩 층도 다른 가중치들과 함께 학습됨
- 인코더와 디코더는 같은 임베딩 층을 공유
- 임베딩 층을 통과한 입력값은 위치 인코딩을 거쳐 트랜스포머 블록의 입력으로 사용됨

### 트랜스포머의 구성 요소 - 위치 인코딩

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/98fcb1f8-ac3f-4f98-bb7e-de2f2d248076" />

- 트랜스포머는 그 구조상 입력 시퀀스 전체가 입력됨
- 토큰들 간의 순서를 알 수 없음
- 트랜스포머에서는 모든 토큰이 순서에 관계없이 다른 토큰에 동일한 영향을 미침
- 이를 보완하기 위해 문장에 각 토큰의 위치를 나타내는 정보를 임베딩에 더하는 위치 인코딩을 이용

<p align="center">
<img width="300" height="70" alt="image" src="https://github.com/user-attachments/assets/b5aae515-917f-456e-a5d4-b7b3b292d7ca" />
</p>

### 트랜스포머의 구성 요소 - 셀프 어텐션 

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/44a0aabe-4861-4002-89e3-3a3bd2c80890" />
<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/e3be2b04-dadb-4e46-9fa7-77f3c1a85d8a" />

- 트랜스포머에는 두 가지 형태의 어텐션이 사용됨
  - 병렬 코퍼스가 주어졌을 때 사용되는 일반적인 어텐션
  - 하나의 시퀀스가 주어졌을 때 사용되는 셀프 어텐션
- 두 개의 다른 어텐션을 스케일드 닷-프로덕트 어텐션으로 통합함
- T개의 토큰이 있는 문장: 각각의 토큰은 크기 d인 임베딩 벡터
- X: T X d의 행렬
- X에 서로 다른 변환 행렬을 곱함

<p align="center">
<img width="150" height="70" alt="image" src="https://github.com/user-attachments/assets/f07ab6f1-1b15-4542-9314-7f0423ed891d" />
</p>

- 셀프 어텐션은 같은 문장의 다른 표현인 Q, K, V를 스케일드 닷-프로덕트 어텐션을 통해 결합하여 문장 내 토큰 간 문법 및 의미적 관계를 찾는 것
- 출력 시퀀스의 특정 토큰이 입력 시퀀스 중에 어떤 부분과 연관성이 높은가를 자체문장 속에서 파악할 수 있음
- 'it'은 자기 스스로의 토큰들에게 참조를 요청하는 토큰인 쿼리를 의미
- 왼쪽의 토큰들은 그 역할상 참조되는 대상인 키와 값의 역할을 함

 ### 트랜스포머의 구성 요소 - 스케일드 닷-프로덕트 어텐션

 <p align="center">
 <img width="400" height="100" alt="image" src="https://github.com/user-attachments/assets/d066c056-bbe0-41d9-a4dc-9b88bbc8af35" />
</p>

- Q와 K의 전치행렬을 곱하고 키의 벡터차원인- Seq2Seq 모형에서 순환신경망을 제거하고 어텐션 메커니즘만 이용한 트랜스포머를 제안
- BERT 를 비롯한 문장 임베딩 모형들의 근간이 됨

### Seq2Seq 모형과 트랜스포머
- Seq2Seq 모형의 병목현상: 다양하고 복잡한 문장들을 맥락 벡터에 담아내기에는 은닉상태의 크기가 부족하여 최종 은닉상태만으로 입력 데이터 전체 문장을 대표하기 어려운 장기의존성 문제
- 어텐션 메커니즘을 이용해 출력 문장의 매 토큰마다 입력 문장의 모든 토큰의 정보를 바탕으로 맥락벡터를 계산하여 입력 문장의 특성을 보다 잘 반영할 수 있게 됨
- 어텐션 메커니즘은 순환신경망에 행렬연산이 추가되어 연산의 부담이 커짐
- 어텐션 메커니즘 기반의 Seq2Seq 모형은 순차적으로 연산할 수 밖에 없는 순환신경망이라는 제약이 있었음
- 위 제약을 극복하기 위해 제안 된 모형이 트랜스포머
- 어텐션 가중치가 입력 토큰과 출력 토근의 조합 간 의존성을 모두 담고 있으므로 전후 토큰들의 의존성을 어텐션 메커니즘으로 표현할 수 있었고, 어텐션 메커니즘만으로 순환신경망을 대체할 수 있음

### 트랜스포머의 구조

<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/9789616d-1c94-4fda-a3a8-94aea66aabf1" />

- 왼쪽의 인코다와 오른쪽읜 디코더로 이루어짐
- 인코더
  - 입력 문장을 구성하는 토큰의 시퀀스들을 받아서 임베딩층을 거쳐 정해진 크기의 벡터로 변형
  - 여기에 위치 인코딩을 더해 토큰의 위치 정보를 가미한 후 N개의 인코더 트랜스포머 블록을 거침
  - 트랜스포머 블록의 입력과 출력의 크기 = (문장의 길이) X (임베딩 크기)
  - N개의 인코더 트랜스포머 블록을 거친 최종 출력은 N개의 디코더 트랜스포머 블록을 거칠 때마다 여러개의 어텐션으로 구성된 멀티-헤드 어텐션의 입력으로 사용됨
  - 위 과정을 통해 입력 문장에서 인코딩된 정보는 디코더로 넘어감
- N개의 디코더 트랜스포어 블록을 통과하면 마지막으로 밀집신경망을 거친 후 소프트맥스 함수를 적용하여 토큰을 예측

### 트랜스포머의 구성 요소 - 입력과 임베딩

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/b6dff828-fe98-4fa1-b09e-2e3ae32824aa" />

- 원-핫 벡터에서 해당 토큰의 인덱스로 입력받음
- 문장 벡터 전체를 하나의 행렬로 간주하고 행렬연산을 진행하므로 연산시간이 줄어들음
- 학습시 디코더에 문장을 입력할 때도 전체 문장을 한번에 신경망에 입력함
- 하지만, 학습된 트랜스포머를 이용하여 추론할 때 디코더는 순차적으로 토큰을 입력받음
- 실제 번역할 때 출력 문장은 없으므로 출력 문장은 이전에 생성된 토큰을 바탕으로 다음 토큰을 생성해야 하기 때문
- 입력은 임베딩 층에서 $d_model$ 차원의 실수 벡터로 변환됨
- 임베딩 층도 다른 가중치들과 함께 학습됨
- 인코더와 디코더는 같은 임베딩 층을 공유
- 임베딩 층을 통과한 입력값은 위치 인코딩을 거쳐 트랜스포머 블록의 입력으로 사용됨

### 트랜스포머의 구성 요소 - 위치 인코딩

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/98fcb1f8-ac3f-4f98-bb7e-de2f2d248076" />

- 트랜스포머는 그 구조상 입력 시퀀스 전체가 입력됨
- 토큰들 간의 순서를 알 수 없음
- 트랜스포머에서는 모든 토큰이 순서에 관계없이 다른 토큰에 동일한 영향을 미침
- 이를 보완하기 위해 문장에 각 토큰의 위치를 나타내는 정보를 임베딩에 더하는 위치 인코딩을 이용

<p align="center">
<img width="300" height="70" alt="image" src="https://github.com/user-attachments/assets/b5aae515-917f-456e-a5d4-b7b3b292d7ca" />
</p>

### 트랜스포머의 구성 요소 - 셀프 어텐션 

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/44a0aabe-4861-4002-89e3-3a3bd2c80890" />
<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/e3be2b04-dadb-4e46-9fa7-77f3c1a85d8a" />

- 트랜스포머에는 두 가지 형태의 어텐션이 사용됨
  - 병렬 코퍼스가 주어졌을 때 사용되는 일반적인 어텐션
  - 하나의 시퀀스가 주어졌을 때 사용되는 셀프 어텐션
- 두 개의 다른 어텐션을 스케일드 닷-프로덕트 어텐션으로 통합함
- T개의 토큰이 있는 문장: 각각의 토큰은 크기 d인 임베딩 벡터
- X: T X d의 행렬
- X에 서로 다른 변환 행렬을 곱함

<p align="center">
<img width="150" height="70" alt="image" src="https://github.com/user-attachments/assets/f07ab6f1-1b15-4542-9314-7f0423ed891d" />
</p>

- 셀프 어텐션은 같은 문장의 다른 표현인 Q, K, V를 스케일드 닷-프로덕트 어텐션을 통해 결합하여 문장 내 토큰 간 문법 및 의미적 관계를 찾는 것
- 출력 시퀀스의 특정 토큰이 입력 시퀀스 중에 어떤 부분과 연관성이 높은가를 자체문장 속에서 파악할 수 있음
- 'it'은 자기 스스로의 토큰들에게 참조를 요청하는 토큰인 쿼리를 의미
- 왼쪽의 토큰들은 그 역할상 참조되는 대상인 키와 값의 역할을 함

 ### 트랜스포머의 구성 요소 - 스케일드 닷-프로덕트 어텐션

 <p align="center">
 <img width="400" height="100" alt="image" src="https://github.com/user-attachments/assets/d066c056-bbe0-41d9-a4dc-9b88bbc8af35" />
</p>

- Q와 K의 전치행렬을 곱하고 키의 벡터차원인 $\sqrt{d_k}$로 나눈 후 행렬의 행이 합이 1이 되도록 소프트맥스 함수를 적용한 가중치 행렬을 구하고, 여기에 V 행렬을 곱함으로써 어텐션 행렬을 얻음
- k번째 행 <img width="100" height="48" alt="image" src="https://github.com/user-attachments/assets/0ceb1898-3f2a-4a78-a032-fa78c3a7bc4c" />
   - 출력 시퀀스의 k번째 토큰은 $w_{k1}, ..., w_{kT_1}$를 가중치로 하여 $T_1$의 토큰 임베딩 벡터 $v_1, ..., v_{T_1}$의 선형결합으로 나타낼 수 있음
- 출력 시퀀스의 특정 토큰이 입력 시퀀스 중에 어떤 부분과 연관성이 높은가를 파악 가능

### 트랜스포머의 구성 요소 - 마스킹
- 트랜스포머의 추론 단계에서 출력 시퀀스를 만들어낼 때 순차적으로 입력
- 문장의 시작을 알리는 <SOS> 토큰으로부터 인코더의 정보와 이전 시점에서의 출력을 입력으로 바탕으로 하나씩 만들어 감
- 이 상황을 트랜스포머에 반영하기 위해 디코더에서 이미 생성된 토큰만 참조하는 것과 같은 환경을 만들어야 함
- 디코더에서 실제 문장을 만들어낼 때 마스킹을 적용하면 인코더로부터의 정보와 이전 시점까지의 디코더 출력으로 토큰을 생성함

### 트랜스포머의 구성 요소 - 멀티-헤드 어텐션

<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/e0f23a94-34f1-49bf-ac2a-851ba23e1858" />

- Q, K, V라는 입력을 완전 연결된 밀집층을 통과시키고 난 후의 결과물을 스케일드 닷-프로덕트 노드의 입력으로 사용
- 멀티-헤드란 어텐션 메커니즘을 여러 번 적용시키는 것을 의미
- 입력 차원이 512인 상황에서 h = 8개의 헤드를 사용해서 어텐션을 적용한다면,
- 임베딩 층의 결과물인 T X 512개의 행렬을 8개의 T X 64의 행렬로 쪼갠 후에 각각 어텐션을 적용시키는 것을 의미
- 8개의 어텐션 헤드를 통과한 결과물을 다시 이어 붙여서 T X 512의 행렬을 얻으면,
- 이 행렬을 다시 한번 512 X 512 차원의 완전 연결 밀집 신경망을 적용시켜 멀티-헤드 어텐션의 출력이 완성됨
- i번째 어텐션 헤드: $head_i = Attention(QW_i^Q, KW_i^K, VW_i^V)$
- 최종 출력: $MultiHead(Q,K,V) = Concat(head_1, \ldots, head_h)W^O$

### 트랜스포머의 구성 요소 - 스킵 연결

<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/8a4f2023-33d1-45cc-8e20-f966d6946d6b" />

- 6개의 트랜스포머 블록을 통과하는 아주 큰 모형이기 때문에 경사가 소실되어 신경망의 입력으로 갈수록 학습이 잘 되지 않음
- 경사정보를 멀티-헤드 어텐션을 거치지 않고 바로 이전의 입력값으로 넘김으로써 입력에 가까운 층에 보다 명확한 경사를 전달
- 이를 위해서도 멀티-헤드 어텐션을 통과하기 이전과 이후의 입/출력의 크기는 동일해야 함
- 스킵 연결을 적용시키고 층 정규화를 함

### 트랜스포머의 구성 요소 - 위치 기반 순방향 신경망
- Position-wise Feed-Forward Network
- 수식: $FFN(x) = \max(0, \, xW_1 + b_1)W_2 + b_2$
- 완전 연결 신경망 두 개 층 사이에 ReLU 함수 적용
- x: 멀티-헤드 어텐션에 스킵연결과 층 정규화를 적용한 T X 512 차원 출력의 한 행을 의미
- 두 개의 순방향 신경망은 각각 512차원의 임베딩을 2048차원으로 늘렸다가 다시 512차원으로 줄이는 역할

### 트랜스포머의 구성 요소 - 디코더 신경망

<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/00e3b955-1b0c-4495-a854-028ba382e64c" />

- 디코더 트랜스포머 블록은 구조상 인코더 블록과 유사
- 셀프 어텐션과 더불어 인코더의 정보와 디코더의 정보를 섞기 위한 어텐션도 같이 적용되었다는 점에서 인코더 블록과 차이가 있음
- 마스킹이 모든 셀프 어텐션에 적용됨
- 출력 시퀀스를 생성할 때 추론이 순차적으로 이루어지므로 마스킹을 통해서 인위적으로 추론 환경을 만들어주기 위한 것
- 마스크 멀티-헤드 어텐션을 통과한 정보는 인코더 블록의 결과물을 K, V 쌍으로, 마스크 멀티-헤드 어텐션의 출력을 Q로 하는 또 다른 멀티-헤드 어텐션을 통과함
- 디코더를 통과한 출력값을 밀집층에 통과시킨 후 소프트맥스 층을 적용시켜서 다음 토큰이 나올 확률 출력

### 트랜스포머 - 추론
테스트 단계에는 디코더가 단어를 1개씩 생성해내야 함

<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/bc0d1944-414f-4bc3-92ab-50a4c8d539c7" />
<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/7e6b836c-01d9-4ff4-829f-f2fb40a861ce" />
<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/13b8bf3a-3c48-42b9-8992-68f2020fcf3e" />
<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/af00eb54-2485-44e2-b9bd-c1356f5cc991" />
<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/6689dcbf-754a-4e6a-8588-6532442a262f" />
<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/bf4f2a0a-3876-4b7c-9c08-a382c49fb14f" />
<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/afc751c8-c773-4048-85ac-6872ccc2a09f" />
<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/58231ed2-ac4b-4078-a734-a9c226aa602c" />
<img width="300" height="100" alt="image" src="https://github.com/user-attachments/assets/c54690e2-8072-4de3-879b-97a8d7a98a4c" />

## ELM0

### 엘모 - – Embeddings from Language Model
- 언어 모델로부터 얻는 임베딩
- BiLM(양방향 언어 모델) 사용
- 순방향과 역방향으로 RNN 언어 모델을 따로 학습시킴 - 사전 학습(pre-training) 단계

## Pre-training in NLP

### Pre-training in NLP
- 워드 임베딩은 딥러닝 자연어 처리의 기본
- 사전 훈련된 임베딩(Word2Vec, GloVe)은 대용량 텍스트의 단어들의 동시 등장 통계로부터 훈련시키는 방법

### Contextual Representation
- 문제점: 단어는 문맥에 따라 뜻이 달라질 수 있음
  - 예: 동음이의어(배, 사과 등)
- 해결법: 언어 모델을 사전훈련 시켜서 문맥에 따른 표현(Contextual Representation)을 얻음

### Problem with Previous Methods
- 문제점: 언어 모델은 정방향 또는 역방향으로만 진행됨
  - 하지만 사실 언어의 문맥이라는 것은 양방향을 통해서 결정
- 언어 모델이 단방향이어야 하는 이유
  - 양방향 언어모델을 만들 경우, 미리 예측할 단어에 대한 정보를 보게되기 때문
 
### Masked Language Model
- 해결 방법: 훈련 데이터의 단어 중 k%의 단어를 [MASK]로 변경하여 이를 예측하도록 함
- 15%의 [MASK] token을 만들어 낼 때 몇 가지 추가적인 처리를 해줌
  - (Masked Language Model을 제안한 논문에서는 k = 15로 사용)
   - 80%: token을 [MASK]로 바꿈
   - 10%: token을 random word로 바꿈
   - 10%: token을 원래의 단어 그대로 놔둠
     - 이는 실제 관측된 단어에 대한 정보 또한 남겨주기 위함

