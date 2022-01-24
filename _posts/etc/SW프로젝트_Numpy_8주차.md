```python
import numpy as np
a = np.arange(12)**2
i = np.array([1, 1, 3, 8, 5])
a, a[i] # 위와 같은 방법으로 정의한 index 행렬을 통해 a의 요소에 접근 가능 
```




    (array([  0,   1,   4,   9,  16,  25,  36,  49,  64,  81, 100, 121],
           dtype=int32),
     array([ 1,  1,  9, 64, 25], dtype=int32))




```python
J = np.array([[3,4], [9,7]])
a[J] # 2차원의 행렬도 각 요소에 2차원 행렬 형태로 접근가능하다. 
```




    array([[ 9, 16],
           [81, 49]], dtype=int32)




```python
palette = np.array( [ [0, 0, 0], # black
[0, 0, 255], # red
[0, 255, 0], # green
[255, 0, 0],# blue
[255, 255, 255] ] ) # white
image = np.array( [ [0, 1, 2, 0], [0, 3, 4, 0] ] )
palette[image] # 이것은 BGR 값을 통해 image 형성에 활용 가능하다. 
```




    array([[[  0,   0,   0],
            [  0,   0, 255],
            [  0, 255,   0],
            [  0,   0,   0]],
    
           [[  0,   0,   0],
            [255,   0,   0],
            [255, 255, 255],
            [  0,   0,   0]]])




```python
A = np.arange(12).reshape(3, 4)
idx1 = np.array( [ [0, 1], [1, 2] ] )
idx2 = np.array( [ [2, 1], [3, 3] ] )
A, A[idx1, idx2] # idx1과 idx2의 순서대로 0,2 1,1 1,3 2,3 index의 요소가 2차원 형태로 출력되었다. 
```




    (array([[ 0,  1,  2,  3],
            [ 4,  5,  6,  7],
            [ 8,  9, 10, 11]]),
     array([[ 2,  5],
            [ 7, 11]]))




```python
A[idx1, 2] # idx1, 2의 모든 요소가 2차원 형태로 출력
```




    array([[ 2,  6],
           [ 6, 10]])




```python
A[:, idx2] # :을 쓰면 row의 모든 차원에 대한 idx2의 요소가 출력된다. 
```




    array([[[ 2,  1],
            [ 3,  3]],
    
           [[ 6,  5],
            [ 7,  7]],
    
           [[10,  9],
            [11, 11]]])




```python
IDX = (idx1, idx2)
A[IDX] # 해당 형태로 두 index를 묶어 접근 가능하다. 
```




    array([[ 2,  5],
           [ 7, 11]])




```python
S = np.array( [idx1, idx2] )
A[S] # 해당 형태는 3차원의 tensor이므로 접근이 불가하다 따라서 
```


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_13124/197338414.py in <module>
          1 S = np.array( [idx1, idx2] )
    ----> 2 A[S]
    

    IndexError: index 3 is out of bounds for axis 0 with size 3



```python
A[tuple(S)] # tuple() 함수로 2차원의 행렬으로 접근 가능하다. 
```




    array([[ 2,  5],
           [ 7, 11]])




```python
time = np.linspace(20, 145, 5) # 20부터 145까지 5개의 요소 
Data = np.sin(np.arange(20)).reshape(5, 4)
idx = Data.argmax(axis=0) # Data의 axis 0 방향으로의 max값의 index를 반환한다. 
time, Data, idx # 순서대로 2, 0, 3, 1번째 원소가 axis 0 방향의 max값이다. 
```




    (array([ 20.  ,  51.25,  82.5 , 113.75, 145.  ]),
     array([[ 0.        ,  0.84147098,  0.90929743,  0.14112001],
            [-0.7568025 , -0.95892427, -0.2794155 ,  0.6569866 ],
            [ 0.98935825,  0.41211849, -0.54402111, -0.99999021],
            [-0.53657292,  0.42016704,  0.99060736,  0.65028784],
            [-0.28790332, -0.96139749, -0.75098725,  0.14987721]]),
     array([2, 0, 3, 1], dtype=int64))




```python
time_max = time[idx] # 해당 index를 time_max로 초기화
Data_max = Data[idx, range(Data.shape[1])] # idx, col 방향 range 의 값을 max에 저장 
time_max, Data_max, np.all(Data_max == Data.max(axis=0)) # Data.max와 동일한 것을 확인할 수 있다. 
```




    (array([ 82.5 ,  20.  , 113.75,  51.25]),
     array([0.98935825, 0.84147098, 0.99060736, 0.6569866 ]),
     True)




```python
a = np.arange(5)
```


```python
a[ [1, 3, 4] ] = 0 # 1, 3, 4번째 index의 요소 0으로 저장 
a
```




    array([0, 0, 2, 0, 0])




```python
a = np.arange(5)
a
```




    array([0, 1, 2, 3, 4])




```python
a[ [0, 0, 2] ] = [1, 2, 3] # 0번째, 2번째의 값 해당 행렬의 요소로 변경 
a
```




    array([2, 1, 3, 3, 4])




```python
a = np.arange(5)
a[ [0, 0, 2] ] += 1 # 얼핏 보면 1이  2번 더해질거 같지만 임시적인 연산으로 1만 더해짐 
a
```




    array([1, 1, 3, 3, 4])




```python
A = np.arange(12).reshape(3, 4)
B = A > 4 # B는 A의 요소 중 4보다 큰 값들의 index
A, A[B]
```




    (array([[ 0,  1,  2,  3],
            [ 4,  5,  6,  7],
            [ 8,  9, 10, 11]]),
     array([ 5,  6,  7,  8,  9, 10, 11]))




```python
A[B] = 0 # 4보다 큰값들의 요소를 0으로 초기화 
A
```




    array([[0, 1, 2, 3],
           [4, 0, 0, 0],
           [0, 0, 0, 0]])




```python
A = np.arange(12).reshape(3, 4)
b1 = np.array([False, True, True])
b2 = np.array([1, 0, 1, 0], dtype = bool) # 해당 형태로 선언하면서 dtype을 bool로 선언하면 boolean 행렬이 됨
A[b1, :] # 총 3개의 row중 F T T이므로 1번 2번 row와 모든 cols의 경우의 수 행렬 출력 
```




    array([[ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
A[b1], A[:, b2], A[b1, b2] # A[b1]은 위와 같고, A[:, b2]는 0번째 2번째 column과 모든 rows, A[b1, b2]는 두 index의 교집합이다. 
```




    (array([[ 4,  5,  6,  7],
            [ 8,  9, 10, 11]]),
     array([[ 0,  2],
            [ 4,  6],
            [ 8, 10]]),
     array([ 4, 10]))




```python
a = np.array( [2, 3, 4, 5] )
b = np.array( [8, 5, 4] )
c = np.array( [5, 4, 6, 8, 3] )
ax, bx, cx = np.ix_(a, b, c) # a ,b, c의 행렬을 조합 가능한 형태로 combine한다. 
ax # 따라서 크기가 다른 행렬 a, b, c에 대해 a + b * c와 같은 연산 적용 가능 .
```




    array([[[2]],
    
           [[3]],
    
           [[4]],
    
           [[5]]])




```python
ax.shape, bx.shape, cx.shape # 각각 다른 차원으로 정리되었음을 볼 수 있다. 따라서 
```




    ((4, 1, 1), (1, 3, 1), (1, 1, 5))




```python
result = ax + bx*cx # 4x3x5행렬이 형성될 수 있다. 
result
```




    array([[[42, 34, 50, 66, 26],
            [27, 22, 32, 42, 17],
            [22, 18, 26, 34, 14]],
    
           [[43, 35, 51, 67, 27],
            [28, 23, 33, 43, 18],
            [23, 19, 27, 35, 15]],
    
           [[44, 36, 52, 68, 28],
            [29, 24, 34, 44, 19],
            [24, 20, 28, 36, 16]],
    
           [[45, 37, 53, 69, 29],
            [30, 25, 35, 45, 20],
            [25, 21, 29, 37, 17]]])




```python
a = np.array( [2, 3, 4, 5] )
b = np.array( [8, 5, 4] )
c = np.array( [5, 4, 6, 8, 3] )
def ufunc_reduce(ufct, *vectors):
    vs = np.ix_(*vectors) # 다수의 행렬을 각기 다른차원의 다수의 벡터로 변환 
    r = ufct.identity # 
    for v in vs: # vs내의 모든 벡터에 대해 
        r = ufct(r, v) # r + 벡터 를 r에 저장 
    return r
ufunc_reduce(np.add, a, b, c)
```




    array([[[15, 14, 16, 18, 13],
            [12, 11, 13, 15, 10],
            [11, 10, 12, 14,  9]],
    
           [[16, 15, 17, 19, 14],
            [13, 12, 14, 16, 11],
            [12, 11, 13, 15, 10]],
    
           [[17, 16, 18, 20, 15],
            [14, 13, 15, 17, 12],
            [13, 12, 14, 16, 11]],
    
           [[18, 17, 19, 21, 16],
            [15, 14, 16, 18, 13],
            [14, 13, 15, 17, 12]]])




```python
A = np.array( [[1, -2, 1], [0, 2, -8], [5, 0, -5]])
b = np.array( [0, 8, 10] ).reshape(3, 1)
np.linalg.solve(A, b) # 해당 함수를 사용하면 Ax = b의 해를 쉽게 구할 수 있다. 
```




    array([[ 1.],
           [ 0.],
           [-1.]])




```python
invA = np.linalg.inv(A) # A의 역행렬 구할 수 있음. 

from fractions import Fraction
(row, col) = invA.shape
for i in range(row):
    for j in range(col):
        print(Fraction.from_float(invA[i,j]).limit_denominator(100), end=' ')
    print('\n')
# 분수의 형태로 나타내주는 반복문이다. 
invA @ b # 소수점이 연산과정에 포함되어 정확하진 않지만 해당 방법으로 해를 구할 수 있다. 
```

    -1/6 -1/6 7/30 
    
    -2/3 -1/6 2/15 
    
    -1/6 -1/6 1/30 
    
    




    array([[ 1.00000000e+00],
           [ 2.22044605e-16],
           [-1.00000000e+00]])




```python
I = np.eye(2) # 2x2크기의 단위행렬 
U = np.array( [[0., -1.], [1., 0.]] )
I * U # 요소끼리의 곱셈
```




    array([[ 0., -0.],
           [ 0.,  0.]])




```python
I @ U # 행렬 곱셈
```




    array([[ 0., -1.],
           [ 1.,  0.]])




```python
np.trace(I) # 요소들의 합
```




    2.0




```python
np.linalg.eig(U) # 행렬의 eigen value, vector 쌍 얻음
```




    (array([0.+1.j, 0.-1.j]),
     array([[0.70710678+0.j        , 0.70710678-0.j        ],
            [0.        -0.70710678j, 0.        +0.70710678j]]))




```python
a = np.arange(30)
B = a.reshape( (2, -1, 3) ) # -1은 컴퓨터 알아서 해당 값 계산 
B.shape
```




    (2, 5, 3)




```python
x = np.arange(0, 10, 2)
y = np.arange(5)
A = np.vstack( [x, y] ) # 세로 방향으로 쌓는 개념
xy = np.hstack( [x, y] ) # 가로 방향으로 연결하는 개념 
A, xy
```




    (array([[0, 2, 4, 6, 8],
            [0, 1, 2, 3, 4]]),
     array([0, 2, 4, 6, 8, 0, 1, 2, 3, 4]))




```python
import numpy as np
rg = np.random.default_rng(1)
import matplotlib.pyplot as plt
mu, sigma = 2, 0.5
v = rg.normal(mu, sigma, 10000) # 정규분포로부터 임의의 샘플들을 그린다. mu = center value sigma = scale 10000 = size 
plt.hist(v, bins = 50, density = 1)

(n, bins) = np.histogram(v, bins = 50, density = True) # v: 입력 데이터 bins = 값의 개수 density = True이면 확률 False이면 빈도 계산 
plt.plot(.5*(bins[1:]+bins[:-1]), n)

plt.show( )
```

    Generator(PCG64)
    


    
![png](output_32_1.png)
    



```python

```
