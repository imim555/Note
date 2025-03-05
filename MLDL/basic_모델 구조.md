### 모델링 과정
1. 데이터정의
	- 수집, 전처리
	- 모델훈련 데이터셋 구성 : 피쳐/타겟 정의
	- 모델평가 데이터셋 구성 : 학습/검증용 분할

2. 학습준비
	- 하이퍼파라미터 : EPOCH, BATCH, ITER, LR
	- 데이터로더  **torch.utils.data**

3. 모델정의   
	- 가설 : 선형회귀, 앙상블, 신경망구조 등 레이어별 파라미터 초기화 **torch.nn**
		* 파라미터들의 복합체를 노드, 노드들의 복합체를 레이어, 레이어들의 복합체를 모델로 이해; parameters -> node -> layers -> model
	* 모델 관련 인스턴스 생성 : model, optimizer, loss   **torch.optim**

4. 학습진행
	- forward
	- loss
	- backward **torch.autograd**
		- gradient 초기화 *.zero_grad()*   
		- gradient 계산  *.backward()*
		- parameter 갱신  *.step()*   
5. 성능평가
6. 모델배포    **torch.onnx**

![[Pasted image 20250303103720.png]]