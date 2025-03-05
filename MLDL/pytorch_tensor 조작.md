


### PyTorch Tensor Allocation
파이토치로 텐서를 선언하는 것은 numpy와 매우 유사하다



### Broadcasting
연산할 때 행렬의 크기가 맞지 않더라도 자동으로 열/행을 확장


### operation
#### Matrix Multiplication Vs. Multiplication
Matrix Multiplication 
- dot product, 행렬곱셈,내적기반 곱셈
- 벡터간 크기 출력 활용, 가중치 연산, 형변환, 정보차원변환
- .matmul


![[Pasted image 20250302230042.png]]



![[Pasted image 20250302230049.png]]






Multiplication 
- Hadamard product 아다마르 곱셈, point-wise product, 원소별 곱셈
- 필터링에 활용(LSTM의 gate노드 등)
- .mul

![[Pasted image 20250302230101.png]]
![[Pasted image 20250302230107.png]]




```python
# numpy
a = np.array([[1,2],[3,4]]) 
b = np.array([[5,6],[7,8]]) 
print(f"Matrix multiplication = {np.dot(a,b)}") 
print(f"Hadamard product = {np.multiply(a,b)}")

# pytorch

```




#### Aggregate : 평균(Mean), 덧셈(Sum), Max, ArgMax
집계함수를 사용할때 축을 기준으로 연산이 진행

#### reshape : View, Squeeze, 
#### Unsqueeze
원소의 수를 유지하면서 텐서의 크기 변경. numpy의 reshape 동일


#### Type Casting

![[Pasted image 20250302230132.png]]


#### 배열확장 : concatenate, stacking


#### 배열초기화 : ones_like와 zeros_like


#### In-place Operation : __
덮어쓰기 연산

