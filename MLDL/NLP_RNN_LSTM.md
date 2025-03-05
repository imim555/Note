
Long Short-Term Memory
게이트가 추가된 RNN
# 배경

### 문제
바닐라 RNN의 장기의존 관계 모델링 실패 (the problem of Long-Term Dependencies)
시계열길이가 길어길수록 기울기 소실/폭등 문제 발생 (tanh, matmul 연산의 backward과정)
1.0보다 작거나 큰 숫자를 무한히 곱한다고 생각하면 된다


![[Pasted image 20250303104100.png]]
### 해결
- Gradient Clipping : 기울기를 일정수준(임계치) 이내로 유지하여 기울기 폭등 방지
- LSTM : 기존 RNN노드에 장기기억관리시스템 추가


# LSTM

### 장기기억 보관 시스템  
- 1 cell state -> 3 gates -> 1 hidden state
-  cell state는 장기상태로 순환만 하며, hidden state는 단기상태로 순환+출력 진행
- <span style="background:rgba(240, 107, 5, 0.2)">gate는 cell state와 hidden state 간의 정보량를 관리</span>하는 역할을 한다
- forget gate : <span style="background:rgba(240, 107, 5, 0.2)">cell state의 기억 제거</span>
	- $X_{t}$ 와 $H_{t-1}$ 를 affine변환 + sigmoid변환+ Hadamard곱
	- $f=\sigma(x_{t}W_{xf}+h_{t-1}W_{hf}+b_{f})$ => $c_{t-1}=f∘c_{t-1}$
	- affine변환 => 정보 간 선형결합
	- sigmoid 변환 => 중요정도에 따라 원소값이 0~1 사이로 스케일링
	- Hadamard dot =>기존 cell state에 반영

- input gate : <span style="background:rgba(240, 107, 5, 0.2)">cell state의 정보 추가</span> => cell state 갱신
	- $X_{t}$ 와 $H_{t-1}$ 를 affine변환+tanh변환 정보(RNN처리) + sigmoid변환 + Hadamard곱
	- $g=tanh(x_{t}W_{xg}+h_{t-1}W_{hg}+b_{g})$ => 정보갱신
	- $i=\sigma(x_{t}W_{xi}+h_{t-1}W_{hi}+b_{i})$ => 정보스케일링
	- $c_t = f∘c_{t-1}+g∘i$ => 중요정보만 cell_state로 전송해서 갱신
- output gate : 갱신된 cell state의 정보를 <span style="background:rgba(240, 107, 5, 0.2)">hidden state로 출력</span>해서 RNN정보에 반영
	- $o=\sigma(x_{t}W_{xo}+h_{t-1}W_{ho}+b_{o})$ => 정보스케일링
	- $h_t = o∘tanh(c_{t})$ => cell_state를 활성화하여 출력값에 반영


![[Pasted image 20250303105433.png]]

LSTM
![[Pasted image 20250303105935.png]]

바닐라RNN
![[Pasted image 20250303111400.png]]

- 파라미터
	-  $Wxi,Wxg,Wxf,Wxo$는 $x_t$와 함께 각 게이트에서 사용되는 4개의 가중치입니다.
	- $Whi,Whg,Whf,Who$는 $h_{t−1}$와 함께 각 게이트에서 사용되는 4개의 가중치입니다.
	- $bi,bg,bf,bo$는 각 게이트에서 사용되는 4개의 편향입니다.
 
 - 계산식
	 - input gate : $i_{t}=σ(W_{xi}x_{t}+W_{hi}h_{t-1}+b_{i})$
	- rnn :  $g_{t}=tanh(W_{xg}x_{t}+W_{hg}h_{t-1}+b_{g})$
	- forget gate : $f_{t}=σ(W_{xf}x_{t}+W_{hf}h_{t-1}+b_{f})$
	- cell state : $C_{t}=f_{t}∘C_{t-1}+i_{t}∘g_{t}$
	- output gate  : $o_{t}=σ(W_{xo}x_{t}+W_{ho}h_{t-1}+b_{o})$
	- hidden state : $h_{t}=o_{t}∘tanh(c_{t})$
![[Pasted image 20250303114009.png]]
### 주요 연산의 역할
- tanh : 정보갱신
- sigmoid : 필요성에 따른 정보 스케일링
- hadamard product : point-wise dot 으로 정보필터링


### 데이터처리 형태
- 공통된 작업(affine)을 묶어서 병렬처리


![[Pasted image 20250303104127.png]]






# 모델 개선방법

RNN 층 -> LSTM층으로 교체
multi-layer
dropout : overfitting 방지, 시간축(시계열방향) 말고 상하축(깊이방향)으로 삽입해야 효과 있음
weight tying : Embedding층과 Affine층의 가중치를 공유 ; dense_size = hidden_size 같게 설정


![[Pasted image 20250303114511.png]]


![[Pasted image 20250303114747.png]]

# GRU 

Gated Recurrent Unit
GRU는 은닉 상태를 업데이트하는 계산 줄여서 LSTM의 구조를 간단화
GRU에서는 업데이트 게이트와 리셋 게이트 두 가지 게이트만이 존재한다. 

$r_{t}=σ(W_{xr}x_{t}+W_{hr}h_{t-1}+b_{r})$
$z_{t}=σ(W_{xz}x_{t}+W_{hz}h_{t-1}+b_{z})$
$g_{t}=tanh(W_{hg}(r_{t}∘h_{t-1})+W_{xg}x_{t}+b_{g})$
$h_{t}=(1-z_{t})∘g_{t}+z_{t}∘h_{t-1}$

![[Pasted image 20250303104139.png]]


GRU는 LSTM보다 학습 속도가 빠르다고 알려져있지만 여러 평가에서 GRU는 LSTM과 비슷한 성능을 보인다고 알려져 있습니다. 경험적으로 데이터 양이 적을 때는 매개 변수의 양이 적은 GRU가 조금 더 낫고, 데이터 양이 더 많으면 LSTM이 더 낫다고도 한다.