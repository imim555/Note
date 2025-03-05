

# 배경

corpus를 이용해서 <span style="background:rgba(240, 107, 5, 0.2)">학습을 진행</span>하면 그 부산물로 <span style="background:rgba(240, 107, 5, 0.2)">단어의 분산표현</span>을 얻을 수 있다. 분산표현은 단어의 의미가 인코딩된 <span style="background:rgba(240, 107, 5, 0.2)">피쳐맵, 특징벡터, 독립변수</span> 성격을 가지며, 모델구조에서는 <span style="background:rgba(240, 107, 5, 0.2)">embedding층</span>이라 부른다. 여하튼 이런 <span style="background:rgba(240, 107, 5, 0.2)">학습결과</span>를 활용해 결과적으로 <span style="background:#fff88f">단어나열에 확률을 부여</span>할 수 있게 되는데 이를 바로 <span style="background:#fff88f">언어모델</span>이라 한다.  

앞서 본 여러가지 분산표현기법 -추론기반(Word2vec)- 을 이용해서 언어모델을 만들 수 있긴 하나, 문제점이 있다. 
- 맥락정보(window size)를 아무리 늘려도 그 전에 나온 단어는 무시된다.
- 맥락정보에 포함된 단어들의 순서가 무시된다

여기서 순환신경망을 이용하는 RNN이 그 대안이 된다.
- 맥락정보가 순차적으로 입력된다. 과거정보보다 최근정보가 더 중요한 역할을 한다. 즉 순서정보가 반영된다. (이전 word2vec에서는 target의 전후 단어를 맥락정보로 사용하였으나 여기서는 왼쪽의 맥락정보만 사용한다. 첨언; 양방향의 모든 맥락정보를 사용하는 rnn를 양방향rnn이라 한다)
- 학습데이터 가공과정에서 이를 염두한다


> [! note]
> - corpus : 대량 텍스트 데이터
> - 학습 : 손실함수 최소화
> - 단어의 분산표현 : 단어의 의미가 인코딩된 피쳐맵, 특징벡터, 독립변수, 임베딩벡터
> - 언어모델 : 언어모델은 단어 배열에 사후확률을 할당하여 가장 자연스러운 시퀀스를 찾아낸다. 기계번역, 오타교정, 음성인식, 문장생성 등에 응용된다.






# 구조
## forward 과정에서 주요 특징

은닉층에서 활성화 함수를 지난 값은이 오직 출력층 방향으로만 향하는 신경망들을 피드 포워드 신경망(Feed Forward Neural Network)이라고 한다. 이와 달리, <span style="background:rgba(240, 107, 5, 0.2)">RNN(Recurrent Neural Network)은 은닉층의 노드에서 활성화 함수를 통해 나온 결과값을 출력층 방향으로도 보내면서, 다시 은닉층 노드의 다음 계산의 입력으로 보내는 특징</span>을 가진다

recurrent neural network
- RNN은 순환하는 경로가 있어서 데이터가 끊임없이 순환한다. 과거의 정보를 기억하는 동시에 최신 데이터로 갱신된다. 즉 contexts의 길이를 고정하지 않아도 먼 과거의 정보가 계속 남아있게 된다
- 입력시 RNN노드에서는 2개의 정보(현재정보, 과거정보)를 받아야 하며, 출력값(갱신된 과거정보)을 다음 RNN노드로 전달한다
- RNN에서 은닉층의 활성화 함수를 통해 결과를 내보내는 역할을 하는 노드를 셀(cell)이라고 한다. 이 셀은 이전의 값을 기억하려고 하는 일종의 메모리 역할을 수행하므로 이를 **메모리 셀** 또는 **RNN 셀**이라고 표현한다.
- 은닉층의 메모리 셀은 각각의 시점(time step)에서 바로 이전 시점에서의 은닉층의 메모리 셀에서 나온 값을 자신의 입력으로 사용하는 재귀적 활동을 수행한다. 이처럼 메모리 셀이 출력층 방향으로 또는 다음 시점 t+1의 자신에게 보내는 값을 **은닉 상태(hidden state)** 라고 부른다

![[Pasted image 20250302165136.png]]
![[Pasted image 20250302165140.png]]

![[Pasted image 20250303102711.png]]
## 셀의 구조

- 은닉층 (rnn계층)
	- $h_{t} = tanh(W_{x} x_{t} + W_{h}h_{t−1} + b)$
	- 현재정보($x_t$)와 과거정보($h_(t-1)$)가 각 가중치들과 선형결합하고 비선형함수($tanh$)를 통과
- 출력층 (affein계층)
	- $y_{t} = f(W_{y}h_{t} + b)$   
	- 은닉층의 결과값이 가중치와 선형결합하고 비선형함수($f$)를 통과
	- 여기서 $f$는 문제종류에 따라 결정. 문제가 다중분류라면 softmax함수 사용.

- shape (1batch, 1시점인 경우)
	- $x_t : (d × 1)$ ; dense
	- $W_x :(D_{h} × d)$ ; dense x hidden
	- $W_h : (D_{h} × D_{h})$ ; hidden x hidden
	- $h_{(t-1)}: (D_h × 1)$ ; hidden
	- $b : (D_h × 1)$ ; hidden
	각각의 가중치 $W_{x},W_{h},W_{y}$의 값은 모든 시점에서 값을 동일하게 공유한다

![[Pasted image 20250302165152.png]]

![[Pasted image 20250303102446.png]]


- 활성화함수

![[Pasted image 20250303102638.png]]

![[Pasted image 20250303102753.png]]

## backward 과정에서 주요 특징

### BPTT(Back Propagation Through Time)
- 말뭉치는 기다란 시퀀스 데이터이다.
- 시퀀트 데이터를 처음부터 끝까지 forward하고 다시 역순으로 backward를 한다.
- 각 rnn층은 동일한 노드를 펼쳐놓은 것이기 때문에 동일한 가중치를 공유한다.
- 각 노드의 가중치에서 기울기값을 모두 구하고 합하여 가중치를 업데이트한다.
- 여기서 각 노드는 단어1개 = 1시점(time) 의미
- 한번 학습할 때마다 긴 시계열데이터를 소진하기 때문에(마라톤을 하고 다시 돌아오는 것과 비슷) 계산비용이 크다는 문제점이 있어 truncated BPTT를 고려한다. 
![[Pasted image 20250303102400.png]]
![[Pasted image 20250303102418.png]]


### truncated BPTT
- 말뭉치를 일정한 길이의 블록으로 나눈다
- 과거기억을 살리면서 계산비용을 줄이자 => forward는 그대로 길게 진행하되, backward 과정을 블록화하자. 
- 블럭끼리만 backward가 진행되기 때문에 각 블록단위에 속한 rnn 노드만 동일한 가중치를 공유한다
- forward가 진행되면 순서상 첫번째 블록부터 backward가 진행되고 첫번째 블록의 가중치행렬이 업데이트되며 이를 다음블록에 전달한다. 즉 업데이트된 가중치 행렬이 두번째 블록의 가중치행렬이 된다.
- 두번째 블록에 forward와 backward가 진행되고, 두번째 블록의 가중치 행렬이 업데이트되며 이를 다음블록에 전달한다. 즉 업데이트된 가중치 행렬이 세번째 블록의 가중치 행렬이 된다.
- 이후 모든 블록에서 같은 작업이 반복된다.

![[Pasted image 20250303102332.png]]
### 미니배치 truncated BPTT
- 시계열 데이터를 하나씩 입력하여 학습하는 것보다 일정량을 묶어서 배치단위로 학습하는 것이 더 효율적이다.
- 시계열 데이터의 길이를 일정하게 잘라서 위과정을 동시에 병렬적으로 처리한다


![[Pasted image 20250303102252.png]]










# RNN 모델링

말뭉치 -> embedding -> RNN -> affein -> softmax with loss
 http://colah.github.io/posts/2015-08-Understanding-LSTMs/
https://ratsgo.github.io/natural%20language%20processing/2017/03/09/rnnlstm/
https://deepmind.com/blog/article/decoupled-neural-networks-using-synthetic-gradients


![[Pasted image 20250303102042.png]]





## embedding 노드

- 가중치행렬(vocab, dense) * 단어행렬 (batch, time, 1) -> (batch, time, dense)
- 단어행렬의 모든 단어는 정수를 할당받은 상태 (원핫인코딩X)
- 단어인덱스를 이용한 Look-up table 방식의 연산이 이루어지며
- 결과적으로 모든 단어는 각 인덱스에 매칭된 dense 값을 할당받는다


![[Pasted image 20250303102104.png]]

![[Pasted image 20250303102114.png]]






## RNN 노드


- 1시점 t에 대해서,  $X_t$ (batch, dense) @ $W_x$ (dense, hidden) => $X_{out}$ (batch, hidden)
- 모든 시점 t에 대해서,  $XS$ (batch, time, dense) @ $WS_x$ (dense, time, hidden) => $XS_{out}$ (batch, time, hidden)
- $tanh(W_{x} x_{t} + W_{h}h_{t−1} + b)=h_{t}$ =>$tanh(X_t (batch,dense)@W_x(dense,hidden)+H_{t-1} (batch,hidden)@ W_h (hidden,hidden) + b (batch,hidden))$ = $H_t (batch, hidden)$



![[Pasted image 20250303102142.png]]

![[Pasted image 20250303102153.png]]

## Affine 노드

**Affine Transformation (아핀 변환)** 은 선형 변환(linear transformation)과 평행이동(translation, 편향)을 포함하는 변환이다. RNN노드를 나온 값은 이전 메모리값과 선형결합이 진행된다. 
 - $y_{t} = f(W_{y}h_{t} + b)$ 
- 추후 softmax 변환 한다면 한 단어에 대해 모든 vocab의 확률분포가 출력된다. 즉 1개의 샘플은 (1,vocab_size) shape을 가지게 될 것이기 때문에
- 입력값$h_t$을 이 단계에서 미리 형변환 해준다 ; 3D (batch,time,hidden) => 2D (batch x time, hidden)
- $H_t (batch * time, hidden) @ W_y (hidden, vocab$) => $Y_t (batch * time, vocab)$




![[Pasted image 20250303100724.png]]

![[Pasted image 20250303100403.png]]
![[Pasted image 20250303100750.png]]
![[Pasted image 20250303100810.png]]



## Softmax With Loss

- 추정값 $Y_t (batch * time, vocab)$ => $(batch * time, 1)$ <--> label (batch x time, 1)
- softmax 통과시키면 모든 샘플(batch x time)에 대한 확률분포가 나타난다
- 각 샘플에 대해 가장 큰 값의 확률분포를 가진 단어를 채택한다


![[Pasted image 20250303101332.png]]
![[Pasted image 20250303101321.png]]


## 평가

- Perplexity = exp(entropy)
- 불확실성의 대한 지표로 수치가 낮을수록 선택사항의 수가 줄어든다는 것이기 때문에 모델 성능이 좋다고 본다


![[Pasted image 20250303101718.png]]


# 적용

RNN은 입력과 출력의 길이를 다르게 설계 할 수 있으므로 다양한 용도로 사용할 수 있다. 시점별 입출력의 보편적인 단위는 단어벡터이다. 

예를 들어 one-to-many 모델은 하나의 이미지 입력에 대해서 사진의 제목을 출력하는 Image Captioning 작업에 사용할 수 있다. 사진의 제목은 단어들의 나열이므로 시퀀스 출력이다.

many-to-one을 하는 모델은 입력 문서가 긍정적인지 부정적인지를 판별하는 감성 분류(sentiment classification), 또는 메일이 정상 메일인지 스팸 메일인지 판별하는 스팸 메일 분류(spam detection)에 사용

many-to-many의 모델의 경우에는 입력 문장으로 부터 대답 문장을 출력하는 챗봇과 입력 문장으로부터 번역된 문장을 출력하는 번역기, 개체명 인식이나 품사 태깅과 같은 작업에 사용할 수 있다



![[Pasted image 20250302165205.png]]

![[Pasted image 20250302165206.png]]