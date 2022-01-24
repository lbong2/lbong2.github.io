```python
import numpy as np
rg = np.random.default_rng(1) # 0 ~ 1사이의 랜덤 실수
A = np.floor( 10*rg.random( (3, 4) ) ) # floor: 내림, 랜덤 실수에 *10 한 뒤, 3x4 행렬로 생성 
A
```




    array([[5., 9., 1., 9.],
           [3., 4., 8., 4.],
           [5., 0., 7., 5.]])




```python
A.shape # 3x4행렬
```




    (3, 4)




```python
A.ravel( ) # 가로로 펼친다. flatten은 깊은 복사 ravel은 얕은 복사
```




    array([5., 9., 1., 9., 3., 4., 8., 4., 5., 0., 7., 5.])




```python
A.reshape(6, 2) # 6x2 행렬로 리턴한다. 
```




    array([[5., 9.],
           [1., 9.],
           [3., 4.],
           [8., 4.],
           [5., 0.],
           [7., 5.]])




```python
A.T # Transpose형태를 리턴한다. 
```




    array([[5., 3., 5.],
           [9., 4., 0.],
           [1., 8., 7.],
           [9., 4., 5.]])




```python
A.T.shape
```




    (4, 3)




```python
A.shape # Transpose형태와 반대인 모습
```




    (3, 4)




```python
A.resize( (2, 6) ) # A자체를 2x6형태로 바꾼다. 
A
```




    array([[5., 9., 1., 9., 3., 4.],
           [8., 4., 5., 0., 7., 5.]])




```python
 A.reshape(3, -1) # 3x? 형태로 알아서 알맞게 바꾼다. 
```




    array([[5., 9., 1., 9.],
           [3., 4., 8., 4.],
           [5., 0., 7., 5.]])




```python
rg = np.random.default_rng(1)
A = np.floor(10*rg.random( (2,2) ) )
A
```




    array([[5., 9.],
           [1., 9.]])




```python
B = np.floor(10*rg.random( (2,2) ) )
B
```




    array([[3., 4.],
           [8., 4.]])




```python
np.vstack( (A, B) ) # 세로로 쌓은 형태를 리턴한다. 
```




    array([[5., 9.],
           [1., 9.],
           [3., 4.],
           [8., 4.]])




```python
np.hstack( (A, B) ) # 가로로 쌓은 형태를 리턴한다. 
```




    array([[5., 9., 3., 4.],
           [1., 9., 8., 4.]])




```python
from numpy import newaxis
np.column_stack( (A,B) ) # 현재는 hstack과 같은 기능을 보인다. 
```




    array([[5., 9., 3., 4.],
           [1., 9., 8., 4.]])




```python
a = np.array( [4., 2.] ) # shape를 확인할 시 (2, ) 라는 값이 나옴. 언어의 유동성
b = np.array( [3., 8.] ) # 따라서 colomn_stack 시 column으로 stack하는 모습을 보임.
np.column_stack( (a,b) )
```




    array([[4., 3.],
           [2., 8.]])




```python
np.hstack( (a,b) ) # 하지만 hstack은 가로로 이어진 형태를 반환 
```




    array([4., 2., 3., 8.])




```python
a[:, newaxis] # = 벡터의 모든 요소를 세운다. 
```




    array([[4.],
           [2.]])




```python
np.column_stack( (a[:,newaxis], b[:,newaxis]) ) # 확실하게 column형태로 확인
```




    array([[4., 3.],
           [2., 8.]])




```python
np.hstack( (a[:,newaxis], b[:,newaxis]) )
```




    array([[4., 3.],
           [2., 8.]])




```python
rg = np.random.default_rng(1)
A = np.floor(10*rg.random( (2, 12) ) )
A
```




    array([[5., 9., 1., 9., 3., 4., 8., 4., 5., 0., 7., 5.],
           [3., 7., 3., 4., 1., 4., 2., 2., 7., 2., 4., 9.]])




```python
np.hsplit(A, 3) # 3등분으로 나뉜 배열을 리턴 
```




    [array([[5., 9., 1., 9.],
            [3., 7., 3., 4.]]),
     array([[3., 4., 8., 4.],
            [1., 4., 2., 2.]]),
     array([[5., 0., 7., 5.],
            [7., 2., 4., 9.]])]




```python
np.hsplit(A, (3, 4) ) # 3번째, 4번째 에서 slice 한 값을 리턴 
```




    [array([[5., 9., 1.],
            [3., 7., 3.]]),
     array([[9.],
            [4.]]),
     array([[3., 4., 8., 4., 5., 0., 7., 5.],
            [1., 4., 2., 2., 7., 2., 4., 9.]])]




```python
import numpy as np
A = np.arange(12).reshape(3,4) # 0~11까지의 수를 3x4행렬로 생성 
A
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
B = A # 참조
B
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
B is A
```




    True




```python
id(A)
```




    2399044210480




```python
id(B) # B가 A를 참조하므로 주소값이 같음 
```




    2399044210480




```python
 def f(x): print(id(x))
>>> f(A)
```

    2399044210480
    


```python
A = np.arange(12).reshape(3,4)
A
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
B = A.view( ) # 얕은 복사 
B
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
id(A), id(B) # 주소 값이 다름, 하지만 A의 변화를 반영함
```




    (2399166236400, 2399166235824)




```python
A[0,0] = 1234
A
```




    array([[1234,    1,    2,    3],
           [   4,    5,    6,    7],
           [   8,    9,   10,   11]])




```python
B
```




    array([[1234,    1,    2,    3],
           [   4,    5,    6,    7],
           [   8,    9,   10,   11]])




```python
B = B.reshape( (2,6) ) # B의 형태 변화는 A에 반영되지 않음 
B
```




    array([[1234,    1,    2,    3,    4,    5],
           [   6,    7,    8,    9,   10,   11]])




```python
A
```




    array([[1234,    1,    2,    3],
           [   4,    5,    6,    7],
           [   8,    9,   10,   11]])




```python
A.shape, B.shape
```




    ((3, 4), (2, 6))




```python
S = A[:, 1:3] # S는 A의 모든 row, 1번째 column에서 2번째 column을 가리킴 
S[:] = 10 # 그 모든 부분을 10으로 변경 
A
```




    array([[1234,   10,   10,    3],
           [   4,   10,   10,    7],
           [   8,   10,   10,   11]])




```python
A = np.arange(12).reshape(3,4)
A
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
B = A.copy( ) # copy는 깊은 복사, 값자체를 복사함 
id(A), id(B)
```




    (2399166292880, 2399166292016)




```python
B
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
B[0,0] = 1234 # 당연히 B의 값 변화가 A에 영향을 주지 않음 
B
```




    array([[1234,    1,    2,    3],
           [   4,    5,    6,    7],
           [   8,    9,   10,   11]])




```python
A
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
A = np.arange(int(1e8))
B = A[:100].copy( ) # 이미 복사된 시점에서 A가 삭제되도 B는 독립적인 행렬임. 
del A
A
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_7788/3854793520.py in <module>
          2 B = A[:100].copy( )
          3 del A
    ----> 4 A
    

    NameError: name 'A' is not defined

