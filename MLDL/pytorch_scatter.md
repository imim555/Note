


- https://pytorch.org/docs/stable/generated/torch.Tensor.scatter_.html#torch-tensor-scatter
- https://hongl.tistory.com/201

새롭게 구성한 텐서에 원하는 인덱스에 맞게 값을 할당
Pytorch 공식 문서에는 scatter 함수가 다음과 같이 in-place 함수로 정의



### **scatter_(dim, index, src, reduce=None) → Tensor**

파라미터로 주어진 "index" 에 맞게 "src" 의 값을 새로운 "tensor" 로 할당합니다. 예를 들어 3차원 텐서라면 다음과 같이 업데이트 됩니다.


```python
# 2D
tensor[index[i][j]][j] = src[i][j] # if dim == 0 
tensor[i][index[i][j]] = src[i][j] # if dim == 1

# 3D
tensor[index[i][j][k]][j][k] = src[i][j][k] # if dim == 0 
tensor[i][index[i][j][k]][k] = src[i][j][k] # if dim == 1 
tensor[i][j][index[i][j][k]] = src[i][j][k] # if dim == 2
```


- dim (int): 인덱싱의 기준이 되는 축
- index (LongTensor): "src" 구성요소들이 흩어질 기준이 되는 인덱스 텐서로 정수형 텐서로 구성되어야 합니다. 
- src (Tensor or float): 타겟 "tensor" 를 구성할 값들이 담겨있는 텐서입니다. 하나의 실수로 선언되면 그 값만으로 채워집니다.
- reduce (str, optional): 기존의 값을 어떻게 업데이트할 것인지 정의합니다. 'multiply', 'add' 두 가지 방법이 존재하며, 정의되지 않을 경우 기존의 값을 없애고 새로운 값으로 치환합니다.


#### Example
```python
src = torch.arange(1, 11).reshape((2, 5)) 
src 
# tensor([[ 1, 2, 3, 4, 5],
#         [ 6, 7, 8, 9, 10]]) 

index = torch.tensor([[1, 2, 1, 0]]) # (1,4)

```


```python
torch.zeros(3, 5, dtype=src.dtype).scatter_(0, index, src) 
#tensor([[0, 0, 0, 4, 0], 
#        [1, 0, 3, 0, 0], 
#        [0, 2, 0, 0, 0]])

```
"index" 크기 기준으로 "scr"의 값이 추출되고 "tensor"의 자리에 값이 배정된다
즉 "index"는 [0,1],[0,1],[0,2],[0,3] 자리만 있기 때문에 (값 2,1,2,0)
추출할 때는 scr[0,1],scr[0,1],scr[0,2],scr[0,3] 가 순차적으로 나온다
배치할 때는 tensor['index', : ] , 즉 tensor['1',0],['2',1],['1',2],['0',3] 에 값이 채워진다


```python
torch.zeros(3, 5, dtype=src.dtype).scatter_(dim=1, index=index, src=src )
#tensor([[4, 3, 2, 0, 0], 
#        [0, 0, 0, 0, 0], 
#        [0, 0, 0, 0, 0]])
```
"index" 크기 기준으로 "scr"의 값이 추출되고 "tensor"의 자리에 값이 배정된다
즉 "index"는 [0,1],[0,1],[0,2],[0,3] 자리만 있기 때문에 (값 2,1,2,0)
추출할 때는 scr[0,1],scr[0,1],scr[0,2],scr[0,3] 가 순차적으로 나온다
배치할 때는 tensor[ : ,'index'] , 즉 tensor[0, '1'],[0, '2'],[0, 1'],[0, '0'] 에 값이 채워진다

인덱스가 중복된다면, 특별한 연산(reduce인수)를 지정하지 않았을때, 마지막으로 할당된 값으로 갱신된다