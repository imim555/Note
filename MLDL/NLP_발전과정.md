#NLP #gen #transformer




# 자연어 처리 발전과정

transformer의 등장 이후, RNN 계열 신경망은 트랜스포머로 대체되어가고 있다. 이에 따라  다양한 트랜스포머 계열의 BERT, GPT, T5 등 다양한 사전 훈련된 언어 모델들이 계속해서 등장하고 있다.

> BERT의 파생 모델
> - BERT(Bidirectional Encoder Representations from Transformers)
> - ALBERT : A Lite BERT for Self-supervised Learning of Language Representations
> - RoBERTa : A Robustly Optimized BERT Pretraining Approach
> - ELECTRA : Efficiently Learning an Encoder that Classifies Token Replacements Accurately


다시말해, NLP의 주요 트렌드는 사전 훈련된 언어 모델을 만들고 이를 특정 태스크에 추가 학습시켜 해당 태스크에서 높은 성능을 얻는 것으로 접어들었고, 언어 모델의 학습 방법에 변화를 주는 모델들이 등장하고 있다. 

> "사전 훈련된 단어 임베딩이 모든 NLP 실무자의 도구 상자에서 사전 훈련된 언어 모델로 대체되는 것은 시간 문제이다." (2018년 딥 러닝 연구원 세바스찬 루더)

- 사전훈련된 워드 임베딩
	- 사전훈련된 워드 임베딩을 태스크에 따라 추가학습 진행
	- Word2Vec, FastText, GloVe 같은 임베딩 방법론 이용
	- 한 단어가 한 벡터에 연결되므로 다의어 구분 불가 문제 발생
- 'Semi-supervised Sequence Learning', 2015, 구글 : 
	- LSTM 사전 훈련된 언어 모델을 파인튜닝
-  'Deep Contextual Word Embedding, AI2 & University of Washington', 2017 : 
	- 순방향 언어 모델과 역방향 언어 모델을 각각 따로 학습시킨 후에, 이렇게 사전 훈련된 언어 모델로부터 임베딩 값을 얻는 아이디어
	- 문맥에 따라 임베딩 값이 달라지므로 기존의 다의어 문제 해결
	-  ELMo
- 'Improving Language Understanding by Generative Pre-training, OpenAI, 2018 : 
	- 트랜스포머+LSTM(디코더) 사전 훈련된 언어 모델을 파인튜닝
	- GPT-1
- Masked Language Model, 2018, 구글 :  
	- 양방향 구조를 도입하기 위해 입력 텍스트의 단어 집합의 15%를 랜덤으로 Masking 하고 예측하도록 설계 + 트랜스포머 구조
	- 이전 단어들로부터 다음 단어를 예측하는 언어 모델의 특성으로 인해 양방향 언어 모델을 사용할 수 없는 문제를 극복
	- BERT(Bidirectional Encoder Representations from Transformers)





