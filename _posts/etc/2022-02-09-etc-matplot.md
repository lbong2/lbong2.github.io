---
layout: post
title: matplotlib 공부1
description: >
    matplotlib 공부1
hide_last_modified: false
categories:
  - etc
---

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8919104066540378"
     crossorigin="anonymous"></script>
# matplotlib 공부1

## 1. matplotlib

~~~python
import matplotlib.pyplot as plt
import numpy as np

plt.plot([1, 2, 3, 4])
plt.show()

# x, y값 매칭 가능
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.show()

# 스타일 지정하기. ro = red + circle('o') marker
plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'ro')
plt.axis([0, 6, 0, 20])
plt.show()

# 200ms 간격으로 균일하게 샘플된 시간
t = np.arange(0., 5., 0.2)

# 빨간 대쉬, 파란 사각형, 녹색 삼각형
# 순서대로 f(t) = t, f(t) = t^2, f(t) = t^3
plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
plt.show()
~~~

## 2. 데이터 입력
~~~python
import matplotlib.pyplot as plt
import numpy as np

plt.plot([2, 3, 5, 10])
# plt.plot(np.array([2,3,5,10])) 이거도 가능
plt.show()

# 순서대로 (1, 2)(2, 3)(3, 5)(4, 10) 좌표 지나는 꺾은선 그래프 나타남
plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.show()

# Label 있는 데이터 사용하기.
data_dict = {'data_x': [1, 2, 3, 4, 5], 'data_y': [2, 3, 5, 10, 8]}

plt.plot('data_x', 'data_y', data=data_dict)
plt.show()
~~~

## 3. 축 라벨
~~~python
import matplotlib.pyplot as plt

# xlabel, ylabel로 라벨 이름 설정 가능. plot에 표시됨.
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.xlabel('X-Label')
plt.ylabel('Y-Label')
plt.show()

# 여백 지정 가능.
plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis', labelpad=5)
plt.ylabel('Y-Axis', labelpad=10)
plt.show()

# 폰트 설정 가능. 차례로 글꼴, 색깔, 굵기, 사이즈// 사이즈는 숫자 혹은 속성 사용 가능.
plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis', labelpad=5, fontdict={'family': 'serif', 'color': 'b', 'weight': 'bold', 'size': 14})
plt.ylabel('Y-Axis', labelpad=10, fontdict={'family': 'fantasy', 'color': 'deeppink', 'weight': 'normal', 'size': 'xx-large'})
plt.show()

# 이렇게 폰트 dictionary 만들어서 사용 가능.
font1 = {'family': 'serif',
         'color': 'b',
         'weight': 'bold',
         'size': 14
         }

font2 = {'family': 'fantasy',
         'color': 'deeppink',
         'weight': 'normal',
         'size': 'xx-large'
         }

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis', labelpad=5, fontdict=font1)
plt.ylabel('Y-Axis', labelpad=10, fontdict=font2)
plt.show()

# 위치 지정. loc속성을 사용해 라벨 위치 변경 가능. {‘left’, ‘center’, ‘right’}, {‘bottom’, ‘center’, ‘top’}
plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis', loc='right')
plt.ylabel('Y-Axis', loc='top')
plt.show()
~~~
## 4. legend
~~~python
import matplotlib.pyplot as plt

# price 라는 범례 생성, plt.legend() 호출 해야함.
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.legend()

plt.show()

# 위치 지정하기, [0.0, 1.0] x [0.0, 1.0] 범위로 loc 속성 조절해서 위치시킬 수 있음.
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.legend(loc=(0.0, 0.0))
# plt.legend(loc=(0.5, 0.5))
plt.legend(loc=(1.0, 1.0))
plt.show()

# 위치 지정하기2, lower right = 오른쪽 아래.
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.legend(loc='lower right')
plt.show()

# 열 갯수 지정하기, ncol 속성을 조절해서 범례의 column 갯수 지정 가능.
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.plot([1, 2, 3, 4], [3, 5, 9, 7], label='Demand (#)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.legend(loc='best')          # ncol = 1
plt.legend(loc='best', ncol=2)    # ncol = 2
plt.show()

# 폰트 크기 지정하기,
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.plot([1, 2, 3, 4], [3, 5, 9, 7], label='Demand (#)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.legend(loc='best')
plt.legend(loc='best', ncol=2, fontsize=14)
plt.show()

# 범례 테두리 꾸미기, frameon <- 테두리 여부, shadow <- 그림자 여부.
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.plot([1, 2, 3, 4], [3, 5, 9, 7], label='Demand (#)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.legend(loc='best')
plt.legend(loc='best', ncol=2, fontsize=14, frameon=True, shadow=True)
plt.show()

# 그 외에도 facecolor, edgecolor, borderpad, labelspacing 과 같은 다양한 파라미터 존재.
~~~
## 5. axis 범위
~~~python
import matplotlib.pyplot as plt

# 범위 지정, xlim(), ylim()
plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.xlim([0, 5])      # X축의 범위: [xmin, xmax]
plt.ylim([0, 20])     # Y축의 범위: [ymin, ymax]
plt.show()

# 범위 지정, axis(), 위의 결과랑 같이 나옴.
plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0, 5, 0, 20])  # X, Y축의 범위: [xmin, xmax, ymin, ymax]
plt.show()

# 옵션 지정하기, 'on' | 'off' | 'equal' | 'scaled' | 'tight' | 'auto' | 'normal' | 'image' | 'square'
plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.axis('square')
plt.axis('scaled')

plt.show()
~~~
## 6. line style 설정
~~~python
import matplotlib.pyplot as plt

# 라인 스타일 지정하기, color, label을 포멧 문자열로 선택가능.
plt.plot([1, 2, 3], [4, 4, 4], '-', color='C0', label='Solid')
plt.plot([1, 2, 3], [3, 3, 3], '--', color='C0', label='Dashed')
plt.plot([1, 2, 3], [2, 2, 2], ':', color='C0', label='Dotted')
plt.plot([1, 2, 3], [1, 1, 1], '-.', color='C0', label='Dash-dot')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0.8, 3.2, 0.5, 5.0])
plt.legend(loc='upper right', ncol=4)
plt.show()

# linestyle 지정하기,
plt.plot([1, 2, 3], [4, 4, 4], linestyle='solid', color='C0', label="'solid'")
plt.plot([1, 2, 3], [3, 3, 3], linestyle='dashed', color='C0', label="'dashed'")
plt.plot([1, 2, 3], [2, 2, 2], linestyle='dotted', color='C0', label="'dotted'")
plt.plot([1, 2, 3], [1, 1, 1], linestyle='dashdot', color='C0', label="'dashdot'")
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0.8, 3.2, 0.5, 5.0])
plt.legend(loc='upper right', ncol=4)
plt.tight_layout()
plt.show()

# 튜플 사용하기, 괄호 안의 숫자로 on, off 조절하는 듯, 주기적인 모양 만들 수 있음.
plt.plot([1, 2, 3], [4, 4, 4], linestyle=(0, (1, 1)), color='C0', label='(0, (1, 1))')
plt.plot([1, 2, 3], [3, 3, 3], linestyle=(0, (1, 5)), color='C0', label='(0, (1, 5))')
plt.plot([1, 2, 3], [2, 2, 2], linestyle=(0, (5, 1)), color='C0', label='(0, (5, 1))')
plt.plot([1, 2, 3], [1, 1, 1], linestyle=(0, (3, 5, 1, 5)), color='C0', label='(0, (3, 5, 1, 5))')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0.8, 3.2, 0.5, 5.0])
plt.legend(loc='upper right', ncol=2)
plt.tight_layout()
plt.show()

# 선 끝 모양 지정하기, linestyle_capstyle 파라미터를 통해 조절 가능. butt = 사각? round = 둥근 모양,
plt.plot([1, 2, 3], [4, 4, 4], linestyle='solid', linewidth=10,
      solid_capstyle='butt', color='C0', label='solid+butt')
plt.plot([1, 2, 3], [3, 3, 3], linestyle='solid', linewidth=10,
      solid_capstyle='round', color='C0', label='solid+round')

plt.plot([1, 2, 3], [2, 2, 2], linestyle='dashed', linewidth=10,
      dash_capstyle='butt', color='C1', label='dashed+butt')
plt.plot([1, 2, 3], [1, 1, 1], linestyle='dashed', linewidth=10,
      dash_capstyle='round', color='C1', label='dashed+round')


plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0.8, 3.2, 0.5, 5.0])
plt.legend(loc='upper right', ncol=2)
plt.tight_layout()
plt.show()
~~~
## 7. marker 설정
~~~python
import matplotlib.pyplot as plt

# bo = blue + circle,
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], 'bo')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.show()

# 선이랑 마커 동시에 나타내기 그냥 같이 쓰면 됨.
# plt.plot([1, 2, 3, 4], [2, 3, 5, 10], 'bo-')    # 파란색 + 마커 + 실선
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], 'bo--')     # 파란색 + 마커 + 점선
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.show()

# marker 파라미터 사용
plt.plot([4, 5, 6], marker="H")
plt.plot([3, 4, 5], marker="d")
plt.plot([2, 3, 4], marker="x")
plt.plot([1, 2, 3], marker=11)
plt.plot([0, 1, 2], marker='$Z$')
plt.show()
~~~
## 8. 컬러 설정
~~~python
import matplotlib.pyplot as plt

# 포멧 문자열 사용.
plt.plot([1, 2, 3, 4], [2.0, 3.0, 5.0, 10.0], 'r')
plt.plot([1, 2, 3, 4], [2.0, 2.8, 4.3, 6.5], 'g')
plt.plot([1, 2, 3, 4], [2.0, 2.5, 3.3, 4.5], 'b')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')

plt.show()

# color 파라미터 사용
plt.plot([1, 2, 3, 4], [2.0, 3.0, 5.0, 10.0], color='limegreen')
plt.plot([1, 2, 3, 4], [2.0, 2.8, 4.3, 6.5], color='violet')
plt.plot([1, 2, 3, 4], [2.0, 2.5, 3.3, 4.5], color='dodgerblue')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')

plt.show()

# color 파라미터에 Hex 문자열 사용
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], color='#e35f62', marker='o', linestyle='--')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')

plt.show()

# Cycler 색상 -> 색을 지정하지 않으면 C0 ~ C9의 색상이 반복적으로 표시됨.
~~~
## 9. resion filling
~~~python
import matplotlib.pyplot as plt

# fill_between() 이용해서 영역 색칠 가능 // x범위, y범위, alpha는 색깔 진함 정도? 조절됨
x = [1, 2, 3, 4]
y = [2, 3, 5, 10]

plt.plot(x, y)
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.fill_between(x[1:3], y[1:3], alpha=0.5)

plt.show()

# fill_betweenx() 사용, 이거는 y축으로 색칠함, 위에거는 x축에 내린 부분 색칠.
x = [1, 2, 3, 4]
y = [2, 3, 5, 10]

plt.plot(x, y)
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.fill_betweenx(y[2:4], x[2:4], alpha=0.5)

plt.show()

# 두 그래프 사이 영역 채우기, color 설정 가능, 범위 파라미터 추가하면 됨.
x = [1, 2, 3, 4]
y1 = [2, 3, 5, 10]
y2 = [1, 2, 4, 8]

plt.plot(x, y1)
plt.plot(x, y2)
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.fill_between(x[1:3], y1[1:3], y2[1:3], color='lightgray', alpha=0.5)

plt.show()

# 그래프 상관없이 fill() 함수로 x, y로 정의되는 다각형 영역 채울 수 있음
x = [1, 2, 3, 4]
y1 = [2, 3, 5, 10]
y2 = [1, 2, 4, 8]

plt.plot(x, y1)
plt.plot(x, y2)
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.fill([1.9, 1.9, 3.1, 3.1], [1.0, 4.0, 6.0, 3.0], color='lightgray', alpha=0.5)

plt.show()
~~~
## 10. scaling
~~~python
import matplotlib.pyplot as plt
import numpy as np

# x축 scale 정하기, symlog = Symmetrical Log Scale
x = np.linspace(-10, 10, 100)
y = x ** 3

plt.plot(x, y)
plt.xscale('symlog')

plt.show()

# y축 scale 정하기,
x = np.linspace(0, 5, 100)
y = np.exp(x)

plt.plot(x, y)
# plt.yscale('linear')
plt.yscale('log')

plt.show()
~~~
