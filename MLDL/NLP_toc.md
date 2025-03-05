

- Pytorch로 시작하는 딥 러닝 입문 https://wikidocs.net/book/2788
- 밑바닥부터시작하는딥러닝 & 한경훈교수강의 https://sites.google.com/site/kyunghoonhan/deep-learning-i?authuser=0

### NLP 주요 이론
- <span style="background:#d2cbff">텍스트 데이터</span>
	- [[basic_데이터 구조| 데이터 구조]] : batch, timesteps, dense
	- 전처리: 토큰화, 정제, 불용어, 정규표현식 *(chap.9)*

- <span style="background:#d2cbff">통계기반기법</span> *(chap.10-11)*
	- 발전과정
		- 시소러스 : rule-based, 트리체계, WordNet
		- 분산표현 : data-based, 분포가설기초(희소>통계>추론>융합)
	- counted based : 전체문서의 통계정보에 집중, 단어간 유사성 반영 부족
	- DTM, PMI, PPMI, TF-IDF, LSA
	- 차원축소 PCA, SVD
	- 평가 : PPL, 유사도

- <span style="background:#d2cbff">추론기반기법</span> *(chap.12)*
	- predicted based : window 크기에 의존, 전체문서의 정보반영 부족
	- One-hot encoding : sparse vector
	- Embedding : dense vector, lookup-table, nagative sampling
	- 방법론 : Word2Vec(CBOW, skip-gram), GloVE, FastText, ELMo, 전이학습

 - <span style="background:#d2cbff">순환신경망</span>  *(chap.7,13,14)*
	 순환네트워크로 메모리 시스템 구축
	 -  [[NLP_RNN_Vanilla|RNN]]
	 -  [[NLP_RNN_LSTM#LSTM|LSTM]] 
	 -  [[NLP_RNN_LSTM#GRU|GRU]]
	 - [[NLP_RNN_LSTM#모델 개선방법| 모델개선]]


- <span style="background:#d2cbff">생성형모델</span> *(chap.16-22)*
	- [[NLP_gen_seq2seq|seq2seq]] 
	- attention, Transformer, 
	- BERT, GPT 


- <span style="background:#d2cbff">전처리모델</span> *(chap.15)*
	- subword Tokenizer
	- byte pair encoding
	- sentence piece
	- huggingface 

- <span style="background:#d2cbff">적용</span> 
	-  번역, 챗봇, 이미지캡셔닝, TTS
	
- <span style="background:#d2cbff">평가</span>
	- [[NPL_evaluation| PPL, MUP, BLUE]]

- <span style="background:#d2cbff">LLM엔지니어링</span>
