
Huggingface의 공식 문서 링크 : https://huggingface.co/docs/transformers/index

# BERT


BERT(Bidirectional Encoder Representations from Transformers)는 2018년에 구글이 공개한 사전 훈련된 모델
트랜스포머를 이용하여 구현되었으며, 위키피디아(25억 단어)와 BooksCorpus(8억 단어)와 같은 레이블이 없는 텍스트 데이터로 사전 훈련된 언어 모델입니다.

BERT가 높은 성능을 얻을 수 있었던 것은, 레이블이 없는 방대한 데이터로 사전 훈련된 모델을 가지고, 레이블이 있는 다른 작업(Task)에서 추가 훈련과 함께 하이퍼파라미터를 재조정 과정(파인튜닝)을 진행

![[Pasted image 20250316114046.png]]


BERT의 기본 구조는 트랜스포머의 인코더를 쌓아올린 구조입니다.
트랜스포머 인코더 층의 수를 L, d_model의 크기를 D, 셀프 어텐션 헤드의 수를 A라고 하였을 때 각각의 크기는 다음과 같다
- 초기 트랜스포머 모델 : L=6, D=512, A=8
- BERT-Base : L=12, D=768, A=12 : 110M개의 파라미터 ; Open AI GPT-1 동일
- BERT-Large : L=24, D=1024, A=16 : 340M개의 파라미터 ; 최대성능


## 문맥을 반영한 임베딩(Contextual Embedding)




![[Pasted image 20250316114056.png]]


 d_model을 768로 정의하였으므로, 모든 단어들은 768차원의 임베딩 벡터가 되어 BERT의 입력으로 사용됩니다. BERT는 내부적인 연산을 거친 후, 동일하게 각 단어에 대해서 768차원의 벡터를 출력합니다.
위의 그림에서는 BERT가 각 768차원의 [CLS], I, love, you라는 4개의 벡터를 입력받아서(입력 임베딩) 동일하게 768차원의 4개의 벡터를 출력하는 모습(출력 임베딩)을 보여줍니다.

위의 좌측 그림에서 [CLS]라는 벡터는 BERT의 초기 입력으로 사용되었을 입력 임베딩 당시에는 단순히 임베딩 층(embedding layer)를 지난 임베딩 벡터였지만, BERT를 지나고 나서는 [CLS], I, love, you라는 모든 단어 벡터들을 모두 참고한 후에 문맥 정보를 가진 벡터가 됩니다. 위의 좌측 그림에서는 모든 단어를 참고하고 있다는 것을 점선의 화살표로 표현하였습니다.

![[Pasted image 20250316114102.png]]

BERT는 기본적으로 트랜스포머 인코더를 12번 쌓은 것이므로 내부적으로 각 층마다 멀티 헤드 셀프 어텐션과 포지션 와이즈 피드 포워드 신경망을 수행
하나의 단어가 모든 단어를 참고하는 연산은 사실 BERT의 12개의 층에서 전부 이루어지는 연산입니다.

## 4. BERT의 서브워드 토크나이저 : WordPiece

BERT는 단어보다 더 작은 단위로 쪼개는 서브워드 토크나이저를 사용합니다. BERT가 사용한 토크나이저는 WordPiece 토크나이저로 서브워드 토크나이저 챕터에서 공부한 바이트 페어 인코딩(Byte Pair Encoding, BPE)의 유사 알고리즘입니다.

서브워드 토크나이저는 기본적으로 자주 등장하는 단어는 그대로 단어 집합에 추가하지만, 자주 등장하지 않는 단어의 경우에는 더 작은 단위인 서브워드로 분리되어 서브워드들이 단어 집합에 추가된다는 아이디어를 갖고있습니다.

BERT에서 토큰화를 수행하는 방식

```
준비물 : 이미 훈련 데이터로부터 만들어진 단어 집합

1. 토큰이 단어 집합에 존재한다.
=> 해당 토큰을 분리하지 않는다.

2. 토큰이 단어 집합에 존재하지 않는다.
=> 해당 토큰을 서브워드로 분리한다.
=> 해당 토큰의 첫번째 서브워드를 제외한 나머지 서브워드들은 앞에 "##"를 붙인 것을 토큰으로 한다.
```

예를 들어 embeddings 라는 단어가 입력으로 들어왔을 때, BERT의 단어 집합에 해당 단어가 존재하지 않았다고 하자. 이 경우 일반 토크나이저에서는 OOV 문제가 발생하지만, 서브워드 토크나이저에서는 해당 단어를 더 쪼개어 집합에 추가한다. 만약, BERT의 단어 집합에 em, ##bed, ##ding, # s 라는 서브 워드들이 존재한다면, embeddings는 em, ##bed, ##ding, # s 로 분리됩니다. 여기서 ##은 추후 복원을 위해, 이 서브워드들은 단어의 중간부터 등장하는 서브워드라는 것을 알려주기 위해 단어 집합 생성 시 표시해둔 기호


- ~ 실습 : BERT

## 5. 포지션 임베딩(Position Embedding)

트랜스포머에서는 포지셔널 인코딩(Positional Encoding)이라는 방법을 통해서 단어의 위치 정보를 표현했습니다. 포지셔널 인코딩은 사인 함수와 코사인 함수를 사용하여 위치에 따라 다른 값을 가지는 행렬을 만들어 이를 단어 벡터들과 더하는 방법입니다.

BERT에서는 이와 유사하지만, 위치 정보를 사인 함수와 코사인 함수로 만드는 것이 아닌 학습을 통해서 얻는 포지션 임베딩(Position Embedding)이라는 방법을 사용합니다.


![[Pasted image 20250316114112.png]]


위의 그림에서 WordPiece Embedding은 우리가 이미 알고 있는 단어 임베딩으로 실질적인 입력입니다. Position Embedding은 위치 정보를 위한 임베딩 층이다. 실제 BERT에서는 문장의 최대 길이를 512로 하고 있으므로, 총 512개의 포지션 임베딩 벡터가 학습됩니다.


## 6. BERT의 사전 훈련(Pre-training)

![[Pasted image 20250316114121.png]]
위의 그림은 BERT의 논문에 첨부된 그림으로 ELMo와 GPT-1, 그리고 BERT의 구조적인 차이를 보여줍니다. 가장 우측 그림의 ELMo는 정방향 LSTM과 역방향 LSTM을 각각 훈련시키는 방식으로 양방향 언어 모델을 만들었습니다. 가운데 그림의 GPT-1은 트랜스포머의 디코더를 이전 단어들로부터 다음 단어를 예측하는 방식으로 단방향 언어 모델을 만들었습니다. Trm은 트랜스포머를 의미합니다. 단방향(→)으로 설계된 Open AI GPT와 달리 가장 좌측 그림의 BERT는 화살표가 양방향으로 뻗어나가는 모습을 보여줍니다. 이는 마스크드 언어 모델(Masked Language Model)을 통해 양방향성을 얻었기 때문입니다. BERT의 사전 훈련 방법은 크게 두 가지로 나뉩니다. 첫번째는 마스크드 언어 모델이고, 두번째는 다음 문장 예측(Next sentence prediction, NSP)입니다.

논문에 따르면 BERT는 BookCorpus(8억 단어)와 위키피디아(25억 단어)로 학습되었습니다.

마스크드 언어 모델과 다음 문장 예측은 따로 학습하는 것이 아닌 loss를 합하여 학습이 동시에 이루어집니다.
### 1) 마스크드 언어 모델(Masked Language Model, MLM)
BERT는 사전 훈련을 위해서 인공 신경망의 입력으로 들어가는 입력 텍스트의 15%의 단어를 랜덤으로 마스킹(Masking)합니다. 그리고 인공 신경망에게 이 가려진 단어들을(Masked words) 예측하도록 합니다. 

랜덤으로 선택된 15%의 단어들은 다시 다음과 같은 비율로 규칙이 적용
- 80%의 단어들은 [MASK]로 변경한다.  
    Ex) The man went to the store → The man went to the [MASK]
    
- 10%의 단어들은 랜덤으로 단어가 변경된다.  
    Ex) The man went to the store → The man went to the dog
    
- 10%의 단어들은 동일하게 둔다.  
    Ex) The man went to the store → The man went to the store


![[Pasted image 20250316114127.png]]



마스크드 언어 모델의 학습에 사용되는 단어는 전체 단어의 15%(마스킹 대상 단어)입니다.
![[Pasted image 20250316114134.png]]

- 'dog' 토큰은 [MASK]로 변경되었습니다.
- 'he'는 랜덤 단어 'king'으로 변경되었습니다.
- 'play'는 변경되진 않았지만 예측에 사용됩니다.

데이터가 위와 같이 마스킹 처리 되었을 떄, 위 그림에서 볼 수 있듯, 출력층에 있는 다른 위치의 벡터들은 예측과 학습에 사용되지 않고, 오직 'dog', 'he', 'play' 위치의 벡터만 사용된다다

### 2) 다음 문장 예측(Next Sentence Prediction, NSP)
QA(Question Answering)나 NLI(Natural Language Inference)와 같이 두 문장의 관계를 이해하는 것이 중요한 태스크를 수행하기 위해서 다음 문장 예측이라는 태스크를 학습한다.

BERT는 두 개의 문장을 준 후에 이 문장이 이어지는 문장인지 아닌지를 맞추는 방식으로 연속성을 훈련. 이를 위해서 50:50 비율로 실제 이어지는 두 개의 문장과 랜덤으로 이어붙인 두 개의 문장을 주고 연속성 여부를 이진 훈련.

- 이어지는 문장의 경우  
    Sentence A : The man went to the store.  
    Sentence B : He bought a gallon of milk.  
    Label = IsNextSentence
    
- 이어지는 문장이 아닌 경우 경우  
    Sentence A : The man went to the store.  
    Sentence B : dogs are so cute.  
    Label = NotNextSentence



![[Pasted image 20250316114141.png]]


특별토큰 사용
 [SEP] : 문장을 구분하기 위한 특별 토큰으로 문장의 끝에 사용.
 [CLS] :  분류 문제를 풀기위한 특별 토큰으로 토큰의  위치의 출력층에서 분류 문제를 풀도록 한다. 분류 문제는 2개 문장의 연속성 여부, 한개 문장의 감성 분류 등 태스크에 따라 달라진다. 
[CLS] 토큰이 입력된 문장에 대한 총체적 표현이라고 간주
 







## 7. 세그먼트 임베딩(Segment Embedding)

![[Pasted image 20250316114147.png]]


앞서 언급했듯이 BERT는 QA 등과 같은 두 개의 문장 입력이 필요한 태스크를 풀기도 합니다. 문장 구분을 위해서 BERT는 세그먼트 임베딩이라는 또 다른 임베딩 층(Embedding layer)을 사용합니다. 첫번째 문장에는 Sentence 0 임베딩, 두번째 문장에는 Sentence 1 임베딩을 더해주는 방식이며 임베딩 벡터는 두 개만 사용됩니다.

결론적으로 BERT는 총 3개의 임베딩 층이 사용됩니다.

- WordPiece Embedding : 실질적인 입력이 되는 워드 임베딩. 임베딩 벡터의 종류는 단어 집합의 크기로 30,522개.
- Position Embedding : 위치 정보를 학습하기 위한 임베딩. 임베딩 벡터의 종류는 문장의 최대 길이인 512개.
- Segment Embedding : 두 개의 문장을 구분하기 위한 임베딩. 임베딩 벡터의 종류는 문장의 최대 개수인 2개.


[SEP]와 세그먼트 임베딩으로 구분되는 BERT의 입력에서의 두 개의 문장은 실제로는 두 종류의 텍스트, 두 개의 문서일 수 있습니다.


## 8. BERT를 파인 튜닝(Fine-tuning)하기

사전 학습 된 BERT에 우리가 풀고자 하는 태스크의 데이터를 추가로 학습 시켜서 테스트하는 단계인 파인 튜닝 단계에 대해서 알아보겠습니다. 실질적으로 태스크에 BERT를 사용하는 단계에 해당됩니다.

### 1) 하나의 텍스트에 대한 텍스트 분류 유형(Single Text Classification)

![[Pasted image 20250316114153.png]]

BERT를 사용하는 첫번째 유형은 하나의 문서에 대한 텍스트 분류 유형입니다. 이 유형은 영화 리뷰 감성 분류, 로이터 뉴스 분류 등과 같이 입력된 문서에 대해서 분류를 하는 유형으로 문서의 시작에 [CLS] 라는 토큰을 입력합니다.

텍스트 분류 문제를 풀기 위해서 [CLS] 토큰의 위치의 출력층에서 밀집층(Dense layer) 또는 같은 이름으로는 완전 연결층(fully-connected layer)이라고 불리는 층들을 추가하여 분류에 대한 예측을 하게됩니다.


### 2) 하나의 텍스트에 대한 태깅 작업(Tagging)
![[Pasted image 20250316114156.png]]

 대표적으로 문장의 각 단어에 품사를 태깅하는 품사 태깅 작업과 개체를 태깅하는 개체명 인식 작업이 있습니다. 출력층에서는 입력 텍스트의 각 토큰의 위치에 밀집층을 사용하여 분류에 대한 예측을 하게 됩니다.

### 3) 텍스트의 쌍에 대한 분류 또는 회귀 문제(Text Pair Classification or Regression)

![[Pasted image 20250316114201.png]]

텍스트의 쌍을 입력으로 받는 태스크도 풀 수 있습니다. 텍스트의 쌍을 입력으로 받는 대표적인 태스크로 자연어 추론(Natural language inference)이 있습니다. 자연어 추론 문제란, 두 문장이 주어졌을 때, 하나의 문장이 다른 문장과 논리적으로 어떤 관계에 있는지를 분류하는 것입니다. 유형으로는 모순 관계(contradiction), 함의 관계(entailment), 중립 관계(neutral)가 있습니다.

이러한 태스크의 경우에는 입력 텍스트가 1개가 아니므로, 텍스트 사이에 [SEP] 토큰을 집어넣고, Sentence 0 임베딩과 Sentence 1 임베딩이라는 두 종류의 세그먼트 임베딩을 모두 사용하여 문서를 구분합니다.



### 4) 질의 응답(Question Answering)
![[Pasted image 20250316114205.png]]

QA를 풀기 위해서 질문과 본문이라는 두 개의 텍스트의 쌍을 입력합니다. 이 태스크의 대표적인 데이터셋으로 SQuAD(Stanford Question Answering Dataset) v1.1이 있습니다. 이 데이터셋을 푸는 방법은 질문과 본문을 입력받으면, 본문의 일부분을 추출해서 질문에 답변하는 것입니다.


## 9. 그 외 기타

- 훈련 데이터는 위키피디아(25억 단어)와 BooksCorpus(8억 단어) ≈ 33억 단어
- WordPiece 토크나이저로 토큰화를 수행 후 15% 비율에 대해서 마스크드 언어 모델 학습
- 두 문장 Sentence A와 B의 합한 길이. 즉, 최대 입력의 길이는 512로 제한
- 100만 step 훈련 ≈ (총 합 33억 단어 코퍼스에 대해 40 에포크 학습)
- 옵티마이저 : 아담(Adam)
- 학습률(learning rate) : 10−4
- 가중치 감소(Weight Decay) : L2 정규화로 0.01 적용
- 드롭 아웃 : 모든 레이어에 대해서 0.1 적용
- 활성화 함수 : relu 함수가 아닌 gelu 함수
- 배치 크기(Batch size) : 256


## 10. 어텐션 마스크(Attention Mask)

BERT를 실제로 실습하게 되면 어텐션 마스크라는 시퀀스 입력이 추가로 필요합니다. 어텐션 마스크는 BERT가 어텐션 연산을 할 때, 불필요하게 패딩 토큰에 대해서 어텐션을 하지 않도록 실제 단어와 패딩 토큰을 구분할 수 있도록 알려주는 입력입니다. 이 값은 0(패딩토큰)과 1(실제토큰) 두 가지 값을 가진다.



# SBERT

BERT로부터 문장 임베딩을 얻을 수 있는 센텐스버트(Sentence BERT, SBERT)에 대해 알아보자
SBERT는 기본적으로 BERT의 문장 임베딩의 성능을 우수하게 개선시킨 모델입니다

## 1. BERT의 문장 임베딩


정리하면 사전 학습된 BERT로부터 문장 벡터를 얻는 방법은 다음과 같이 세 가지가 있습니다.
- BERT의 [CLS] 토큰의 출력 벡터를 문장 벡터로 간주한다.
- BERT의 모든 단어의 출력 벡터에 대해서 평균 풀링을 수행한 벡터를 문장 벡터로 간주한다.
- BERT의 모든 단어의 출력 벡터에 대해서 맥스 풀링을 수행한 벡터를 문장 벡터로 간주한다.

이때 평균 풀링을 하느냐와 맥스 풀링을 하느냐에 따라서 해당 문장 벡터가 가지는 의미는 다소 다른데, 평균 풀링을 얻은 문장 벡터의 경우에는 모든 단어의 의미를 반영하는 쪽에 가깝다면, 맥스 풀링을 얻은 문장 벡터의 경우에는 중요한 단어의 의미를 반영하는 쪽에 가깝습니다.


![[Pasted image 20250316114218.png]]
[CLS] 토큰의 출력 벡터만 고려하는 방법



![[Pasted image 20250316114222.png]]
 BERT의 모든 출력 벡터들을 고려하는 방법(pooling)


## 2. SBERT(센텐스버트, Sentence-BERT)


선택에 따라서 1) 문장 쌍 분류 태스크로만 파인 튜닝 할 수도 있고, 2) 문장 쌍 회귀 태스크로만 파인 튜닝 할 수도 있으며 1)을 학습한 후에 2)를 학습하는 전략을 세울 수도 있습니다.

### 1) 문장 쌍 분류 태스크로 파인 튜닝
대표적으로는 NLI(Natural Language Inferencing) 문제를 푸는 것.
NLI는 두 개의 문장이 주어지면 수반(entailment) 관계인지, 모순(contradiction) 관계인지, 중립(neutral) 관계인지를 맞추는 문제입니다. 다음은 NLI 데이터의 예시입니다.

|문장 A|문장 B|레이블|
|---|---|---|
|A lady sits on a bench that is against a shopping mall.|A person sits on the seat.|Entailment|
|A lady sits on a bench that is against a shopping mall.|A woman is sitting against a building.|Entailment|
|A lady sits on a bench that is against a shopping mall.|Nobody is sitting on the bench.|Contradiction|
|Two women are embracing while holding to go packages.|The sisters are hugging goodbye while holding to go packages after just eating lunch.|Neutral|

학습 구조


![[Pasted image 20250316114230.png]]
우선 문장 A와 문장 B 각각을 BERT의 입력으로 넣고, 평균 풀링 또는 맥스 풀링을 통해서 각각에 대한 문장 임베딩 벡터(각각 u와 v)를 얻습니다. 그리고 나서 u벡터와 v벡터의 차이 벡터 |u-v|를 구합니다.  그리고 이 세 가지 벡터를 연결(concatenation)합니다. 세미콜론(;)을 연결 기호
$h = (u; v; |u-v|)$

그리고 이 벡터를 출력층으로 보내 다중 클래스 분류 문제를 풀도록 합니다.
가중치 행렬 $W_y$를 곱한 후에 소프트맥스 함수를 통과시킨다.
$o = softmax(W_{y}h)$

이때 만약 BERT의 문장 임베딩 벡터의 차원이 n이라면 세 개의 벡터를 연결한 벡터 h의 차원은 3n이 되고,  분류하고자 하는 클래스의 개수가 k라면, 가중치 행렬 $W_y$ 는 3n × k의 크기이다
### 2) 문장 쌍 회귀 태스크로 파인 튜닝

대표적으로 STS(Semantic Textual Similarity) 문제를 푸는 경우입니다. STS란 두 개의 문장으로부터 의미적 유사성을 구하는 문제를 말합니다. 다음은 STS 데이터의 예시입니다. 여기서 레이블은 두 문장의 유사도로 범위값은 0~5입니다.

| 문장 A                                          | 문장 B                                              | 레이블  |
| --------------------------------------------- | ------------------------------------------------- | ---- |
| A plane is taking off.                        | An air plane is taking off.                       | 5.00 |
| A man is playing a large flute.               | A man is playing a flute.                         | 3.80 |
| A man is spreading shreded cheese on a pizza. | A man is spreading shredded cheese on an uncoo... | 3.80 |
| Three men are playing chess.                  | Two men are playing chess.                        | 2.60 |
| A man is playing the cello.                   | A man seated is playing the cello.                | 4.25 |


모델 구조

![[Pasted image 20250316114237.png]]

문장 A와 문장 B 각각에 대한 문장 임베딩 벡터를 u와 v라고 하였을 때 이 두 벡터의 코사인 유사도를 구합니다. 그리고 해당 유사도와 레이블 유사도와의 평균 제곱 오차(Mean Squared Error, MSE)를 최소화하는 방식으로 학습합니다. 

코사인 유사도의 값의 범위는 -1과 1사이므로 위 데이터와 같이 레이블 스코어의 범위가 0~5점이라면 학습 전 해당 레이블들의 값들을 5로 나누어 값의 범위를 줄인 후 학습할 수 있습니다.

