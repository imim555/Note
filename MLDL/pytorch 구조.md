### pytorch api

- torch : 메인 네임스페이스, 텐서조작
- torch.nn : 신경망구축 관련 레이어, 활성화함수, 손실함수
	- torch.nn.functional
- torch.utils.data : 데이터샘플링
	- torch.utils.data.Dataset
	- torch.utils.data.DataLoader
- torch.autograd : 자동미분
- torch.optim : 최적화 알고리즘
- torch.onnx : open neural network exchange 포맷으로 모델 export

```python
# 기본 모듈로딩
import torch            # 텐서조작

import torch.nn as nn   # 신경망구축 관련
import torch.nn.functional as F # 활성화함수

import torch.optim as optim     # 최적화

from torch.utils.data import Dataset    # dataset custom
from torch.utils.data import DataLoader # dataloader
```


![[Pasted image 20250303103915.png]]