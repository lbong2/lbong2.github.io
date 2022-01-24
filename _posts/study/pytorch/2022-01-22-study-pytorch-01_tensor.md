---
layout: post
title: 01. Tensor
description: >
    Pytorch Tutorial 1. Tensor
hide_lase_modified: true
categories:
 - study
 - pytorch
---

# Pytorch Tutorial_01. Tensor

Pytorch는 공식 커뮤니티에 튜토리얼이 나와있다. 순서대로 따라가보려 한다.

환경은 아나콘다 가상환경 python=3.6 버전, Cuda 11.3버전이다.

### import

~~~python
import torch
import numpy as np
~~~

import 해준다.

### 1. 데이터로부터 직접 생성하기

~~~python
print("1. 데이터로부터 직접 생성하기")
data = [[1, 2],[3, 4]]
x_data = torch.tensor(data)
print(data)
print(x_data)
~~~
~~~bash
1. 데이터로부터 직접 생성하기
[[1, 2], [3, 4]]
tensor([[1, 2],
        [3, 4]])
~~~

파이썬 numpy를 한번써본 경험이 있어서 조금은 익숙한 모습이다. data는 생성한 모습 그대로 출력되는 모습이고, x_data는 tensor라는 문구와 함께 출력된다.

### 2. NumPy 배열로부터 생성하기

~~~python
print("\n2. NumPy 배열로부터 생성하기")
np_array = np.array(data)
x_np = torch.from_numpy(np_array)
print(np_array)
print(x_np)
~~~
~~~bash
2. NumPy 배열로부터 생성하기
[[1 2]
 [3 4]]
tensor([[1, 2],
        [3, 4]], dtype=torch.int32)
~~~
위의 코드와는 약간 다른 부분이 tensor가 데이터 타입을 명시하면서 출력된다.

### 3. 다른 텐서로부터 생성하기
~~~python
print("\n3. 다른 텐서로부터 생성하기")
x_ones = torch.ones_like(x_data) # x_data의 속성을 유지합니다.
print(f"Ones Tensor: \n {x_ones} \n")

x_rand = torch.rand_like(x_data, dtype=torch.float) # x_data의 속성을 덮어씁니다.
print(f"Random Tensor: \n {x_rand} \n")
~~~
~~~bash
3. 다른 텐서로부터 생성하기
Ones Tensor:
 tensor([[1, 1],
        [1, 1]])

Random Tensor:
 tensor([[0.1301, 0.0180],
        [0.9418, 0.1468]])
~~~
ones_like()라는 함수를 사용하였다. numpy의 \~~_like와 같은 함수인듯하다. 매개변수의 x_data를 받아서 속성을 유지하는 부분이 numpy와 매우 닮아 있다.

rand_like라는 함수는 C/C++을 주력으로 써왔던 사람으로써 상당히 매력적인것 같다. 한줄로 rand라니.. 거기다 time같은 종속인자도 필요없다니...

### 4. 무작위(random) 또는 상수(constant) 값을 사용하기
~~~python
print("\n4. 무작위(random) 또는 상수(constant) 값을 사용하기")
shape = (2,3,)
rand_tensor = torch.rand(shape)
ones_tensor = torch.ones(shape)
zeros_tensor = torch.zeros(shape)

print(f"Random Tensor: \n {rand_tensor} \n")
print(f"Ones Tensor: \n {ones_tensor} \n")
print(f"Zeros Tensor: \n {zeros_tensor}")
~~~
~~~bash
4. 무작위(random) 또는 상수(constant) 값을 사용하기
Random Tensor:
 tensor([[0.8232, 0.9872, 0.3701],
        [0.2806, 0.6345, 0.4564]])

Ones Tensor:
 tensor([[1., 1., 1.],
        [1., 1., 1.]])

Zeros Tensor:
 tensor([[0., 0., 0.],
        [0., 0., 0.]])
~~~

먼저 shape를 2, 3으로 정한 모습이고 shape형태로 rand, ones, zeros를 실행한 모습이다. 함수 이름과 똑같은 기능을 보여준다.
### 5. 텐서의 속성(Attribute)
~~~python
print("\n5. 텐서의 속성(Attribute)")
tensor = torch.rand(3,4)
print(f"Shape of tensor: {tensor.shape}")
print(f"Datatype of tensor: {tensor.dtype}")
print(f"Device tensor is stored on: {tensor.device}")
~~~
~~~bash
5. 텐서의 속성(Attribute)
Shape of tensor: torch.Size([3, 4])
Datatype of tensor: torch.float32
Device tensor is stored on: cpu
~~~
텐서의 속성을 출력한 부분이다. 현재 Device tensor가 cpu에 있다고 나온다.

### 6. 텐서 연산(Operation)
~~~python
print("\n6. 텐서 연산(Operation)")
if torch.cuda.is_available():
    tensor = tensor.to('cuda')
print(f"Device tensor is stored on: {tensor.device}")
~~~
GPU가 존재한다면 Tensor가 cuda로 이동한다.
~~~bash
Device tensor is stored on: cuda:0
~~~
### 7. NumPy식의 표준 인덱싱과 슬라이싱
~~~python
print("\n7. NumPy식의 표준 인덱싱과 슬라이싱")
tensor = torch.ones(4, 4)
print('First row: ', tensor[0])
print('First column: ', tensor[:, 0])
print('Last column:', tensor[..., -1])
tensor[:,1] = 0 # 1번째 column 0으로 초기화
print(tensor)
~~~
~~~bash
7. NumPy식의 표준 인덱싱과 슬라이싱
First row:  tensor([1., 1., 1., 1.])
First column:  tensor([1., 1., 1., 1.])
Last column: tensor([1., 1., 1., 1.])
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
~~~
또한 파이썬 자료형에서 맨 처음 배우는 인덱싱, 슬라이싱인데 대체로 비슷한 형태를 보인다.
[..., ~] 이거도 파이썬 기본 자료형에서 썼었나? 아닌거 같은데.. 보통 (:)이걸 많이 썼던거 같다.

### 8. 텐서 합치기
~~~python
print("\n8. 텐서 합치기")
t1 = torch.cat([tensor, tensor, tensor], dim=1)
t2 = torch.stack([tensor, tensor, tensor], dim=1)
print(t1)
print(t2)
~~~
~~~bash
8. 텐서 합치기
tensor([[1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.]])
tensor([[[1., 0., 1., 1.],
         [1., 0., 1., 1.],
         [1., 0., 1., 1.]],

        [[1., 0., 1., 1.],
         [1., 0., 1., 1.],
         [1., 0., 1., 1.]],

        [[1., 0., 1., 1.],
         [1., 0., 1., 1.],
         [1., 0., 1., 1.]],

        [[1., 0., 1., 1.],
         [1., 0., 1., 1.],
         [1., 0., 1., 1.]]])
~~~
이것 또한 numpy에서 쓰던 dstack, column_stack, hstack 등등이 생각나게 만드는 함수다.

### 9. 산술 연산(Arithmetic operations)
~~~python
print("\n9. 산술 연산(Arithmetic operations)")

y1 = tensor @ tensor.T
y2 = tensor.matmul(tensor.T)

y3 = torch.rand_like(tensor)
torch.matmul(tensor, tensor.T, out=y3)
print(y3)

z1 = tensor * tensor
z2 = tensor.mul(tensor)

z3 = torch.rand_like(tensor)
torch.mul(tensor, tensor, out=z3)
print(z3)
~~~
~~~bash
9. 산술 연산(Arithmetic operations)
tensor([[3., 3., 3., 3.],
        [3., 3., 3., 3.],
        [3., 3., 3., 3.],
        [3., 3., 3., 3.]])
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
~~~
먼저 tensor.T를 통해 tensor의 transpose를 구할 수 있는듯 하다. 여기서 matmul은 행렬곱을 의미하고 mul은 tensor 요소간 곱을 구할 수 있다.

### 10. 단일-요소(single-element) 텐서
~~~python
print("\n10. 단일-요소(single-element) 텐서")
agg = tensor.sum()
agg_item = agg.item()
print(agg_item, type(agg_item))
~~~
~~~bash
10. 단일-요소(single-element) 텐서
12.0 <class 'float'>
~~~
단일 요소 텐서, 여기서 agg는 12.0이라는 요소 하나만을 가지는 tensor다.
agg.item()은 agg에서 값만 가져온다.

### 11. 바꿔치기(in-place) 연산
~~~python
print("\n11. 바꿔치기(in-place) 연산")
print(tensor, "\n")
tensor.add_(5)
print(tensor)
~~~
~~~bash
11. 바꿔치기(in-place) 연산
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])

tensor([[6., 5., 6., 6.],
        [6., 5., 6., 6.],
        [6., 5., 6., 6.],
        [6., 5., 6., 6.]])
~~~
바꿔치기 연산은 앞으로 연산과정에서 도함수 계산에 별로 안좋을 수 있으므로 권장하지 않는다고 나와있다.

### 12. 텐서를 NumPy 배열로 변환하기
~~~python
print("\n12. 텐서를 NumPy 배열로 변환하기")
t = torch.ones(5)
print(f"t: {t}")
n = t.numpy()
print(f"n: {n}")

t.add_(1)
print(f"t: {t}")
print(f"n: {n}")
~~~
### 13. NumPy 배열을 텐서로 변환하기
~~~python
print("\n13. NumPy 배열을 텐서로 변환하기")
n = np.ones(5)
t = torch.from_numpy(n)
np.add(n, 1, out=n)
print(f"t: {t}")
print(f"n: {n}")
~~~
~~~bash
12. 텐서를 NumPy 배열로 변환하기
t: tensor([1., 1., 1., 1., 1.])
n: [1. 1. 1. 1. 1.]
t: tensor([2., 2., 2., 2., 2.])
n: [2. 2. 2. 2. 2.]

13. NumPy 배열을 텐서로 변환하기
t: tensor([2., 2., 2., 2., 2.], dtype=torch.float64)
n: [2. 2. 2. 2. 2.]
~~~

해당 부분은 깊은 복사가 아니므로 서로서로 변경사항이 서로의 변수에 반영된다.
