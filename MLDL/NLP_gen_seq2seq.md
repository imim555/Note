

# text generator model

RNN 언어모델에서는 신경망이 출력한 확률분포에 기초하여 다음 단어를 샘플링하는데, 이 원리를 이용해 언어생성 모델로 발전시킨다.


![[Pasted image 20250304161221.png]]





# seq2seq

seq2seq는 <span style="background:rgba(240, 107, 5, 0.2)">시계열를 입력</span>받으면 또다른 <span style="background:rgba(240, 107, 5, 0.2)">시계열을 출력</span>하자는 개념이다. 인풋과 아웃풋의 시계열을 처리하기 위해 2개의 RNN신경망으로 구성되며 각각<span style="background:rgba(240, 107, 5, 0.2)"> Encoder</span>, <span style="background:rgba(240, 107, 5, 0.2)">Decoder</span> 역할을 수행한다.


![[Pasted image 20250303120119.png]]

![[Pasted image 20250304162554.png]]


![[Pasted image 20250303120122.png]]



### Encoder
- Embedding층과 LSTM층으로 구성된다. 
- LSTM의 2개 hidden state와 1개 cell state 중에서, 마지막 시점의 hidden state만 Decoder로 전달된다. 여기에는 문맥의 정보가 내재되어 있다. 
- 입력되는 문장길이에 상관없이 차원은 동일하게 유지될 수 있다. 왜냐면 데이터가 시간블록(time length) 단위로 처리되기 때문이다. 시간블록의 차원이 곧 정보를 담는 그릇 사이즈인 셈이다.



### Decoder
- Embedding-LSTM-Affine-Softmax 계층으로 구성된다. 
- Encoder가 출력한 hidden state가 첫번째 단어의 LSTM층에 입력된다.


### 응용
내용 요약(Text Summarization), STT(Speech to Text), 기계 번역(Machine Translation),메일자동응답, 챗봇, 이미지캡셔닝, 수학연산 등 목적에 따라 Encoder, Decoder 에 적절한 신경망을 배치하여 연결한다. 이미지캡셔닝의 경우 (인코더)CNN-(디코더)LSTM 신경망을 연결하면 된다. 


# 개선

- reverse : 입력데이터의 순서를 반전

![[Pasted image 20250304162817.png]]

- peeky : Encoder의 결과 hidden state를 Decoder에서 다른 여러계층에 공유

![[Pasted image 20250304162843.png]]

-  Peeky Decoder 순전파
![[Pasted image 20250304162947.png]]

![[Pasted image 20250304163003.png]]


- Peeky Decoder 역전파
![[Pasted image 20250304163018.png]]

![[Pasted image 20250304163032.png]]

![[Pasted image 20250304163044.png]]