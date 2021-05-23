상관분석과 단순선형회귀분석
============================


#가짜 데이터 만들어 보기
가짜 데이터를 만들면서 파이썬을 복습해본다.   
가짜 데이터의 전제 조건은 다음과 같다.
- 독립변수는 흡연강도와 음주강도이며 종속변수는 탈모증상이다.
- 가설 설정
>
> 가설 1. 흡연 강도와 음주 강도는 서로 상관이 있다. (상관분석)   
> 가설 2. 흡연 또는 음주의 강도가 탈모 증상에 영향을 미친다. (회귀분석)

- 강도에 따른 5점 리커트 척도를 사용한다. 
> Q.나는 술, 음주를 1년동안    
> A.1=안했다. 2=조금 했다. 3=보통이다. 4=많이 했다. 5=매우 엄청 많이 했다.)
- 1000명을 대상으로 설문조사를 했다고 가정한다.
- 가짜 데이터의 확률 정보
```python
#피험자가 흡연할 확률은 0.5
#피험자가 음주할 확률은 0.4
#흡연만하는 사람이 탈모에 걸릴 확률은 0.2
#음주만하는 사람이 탈모에 걸릴 확률은 0.4
#흡연-음주 하는사람이 탈모에 걸릴 확률은 0.7
#흡연 or 음주를 많이 할 수록 가중값이 10%씩 증가
```
라이브러리 정보
---------------------------
```python
import pandas as pd #판다스
import numpy as np #넘파이
import random as random #랜덤
from tabulate import tabulate #데이터프레임을 예쁘게 출력하기
```

가짜 데이터를 만드는 클래스 선언하기   
--------------------
```python  
class createDataFrame:   
      def __init__(self):   
        self.hairList = [[],[],[],[]] #데이터 프레임에 넣을 4차원 리스트   
        self.percentage = ([1,2,3,4,5]) #2~5=Y 흡연 or 음주 = 조금=2 ~ 매우 많이=5   
                                        #1=N 흡연 or 음주 안한다.   
                                        
        self.tobacoo_per = 0 #피험자가 흡연할 확률   
        self.tobacooYn_per = 0 #흡연자가 탈모에 걸릴 확률   

        self.drink_per = 0 #피험자가 음주할 확률
        self.drinkYn_per = 0 #음주자가 탈모에 걸릴 확률

        self.tobacooAndDrink_per = 0 #흡연+음주자가 탈모에 걸릴 확률

        #빈 데이터 프레임 생성
        self.df = pd.DataFrame(columns=['피험자번호','흡연강도','음주강도','탈모강도'])
```

클래스 안에 가짜 데이터를 생성하는 함수 만들기
-----------------------------

### 1. 확률을 직접 핸들링 할 수 있도록 set 함수를 별도로 만든다.
```python
    def valuesSetting(self, tobacoo_per, tobacooYn_per,
                        drink_per, drinkYn_per,
                        tobacooAndDrink_per):
        self.tobacoo_per = tobacoo_per #피험자가 흡연자일 확률 
        self.tobacooYn_per = tobacooYn_per #흡연자가 탈모에 걸릴 확률


        self.drink_per = drink_per #피험자가 음주 확률
        self.drinkYn_per = drinkYn_per #음주자가 탈모에 걸릴 확률

        self.tobacooAndDrink_per = tobacooAndDrink_per #흡연+음주자가 탈모에 걸릴 확률
```

### 2. 확률에 가중값을 주는 함수를 만든다.
```python
    def Percentage(self,per,w):

        if w==0.2:
            result = random.choices(self.percentage,weights=[0, per + w, per, per, per])
        elif w==0.3:
            result = random.choices(self.percentage, weights=[0, per, per + w, per, per])
        elif w == 0.4:
            result = random.choices(self.percentage, weights=[0, per, per, per + w, per])
        elif w == 0.5:
            result = random.choices(self.percentage, weights=[0, per, per, per, per + w])
        else:
            w = w - w
            result = random.choices(self.percentage, weights=[1 - per, per, w, w, w])

        return int(result[0])
```

### 3. 데이터 프레임을 만드는 함수를 만든다.
```python
    def create(self, subjectList):
        self.number(subjectList)
        self.Tobacco()
        self.Drinking()
        self.Hair_lost()
```

### 4. 가상의 데이터셋을 만든다.


- 피험자의 식별번호를 생성한다. 안 만들어도 된다.
```python
#피험자 식별번호 생성
    def number(self, subjectList):
        numberList=[]
        zero = '0' #자릿수
        max = 6 #맥스 자릿수

        for num in subjectList:
            text = ''

            for i in range(0, max-len(str(num)), 1):
                text += zero

            text = text+str(num)
            numberList.append(text)

        self.hairList[0].extend(numberList)
```

- 흡연 강도를 설정한다.  
```python
    #흡연 강도 생성
    def Tobacco(self) :
        numberList=[]
        for i in range(0, len(self.hairList[0]), 1):
            if(self.Percentage(self.tobacoo_per, 0) == 2): #확률에 가중값을 주는 함수에서 50%확률로 흡연자인지 아닌지를 설정한 후
                numberList.append(random.randint(2,5)) #흡연자일 경우 랜덤으로 2~5의 값을 준다.
            else:
                numberList.append(1)
        self.hairList[1].extend(numberList)
```


- 음주 강도를 설정한다.
```python
 #음주 강도 생성
    def Drinking(self):
        numberList=[]
        for i in range(0, len(self.hairList[0]), 1):
            if (self.Percentage(self.drink_per, 0) == 2): #확률에 가중값을 주는 함수에서 40%확률로 음주자인지 아닌지를 설정한 후
                numberList.append(random.randint(2, 5)) #음주자일 경우 랜덤으로 2~5의 값을 준다.
            else:
                numberList.append(1)
        self.hairList[2].extend(numberList)
```


- 탈모강도를 흡연, 음주 강도에 따라 다르게 설정되게끔 한다.
```python
#탈모강도 생성
        def Hair_lost(self):
        self.df.loc[:,'피험자번호'] = self.hairList[0]
        self.df.loc[:,'흡연강도'] = self.hairList[1]
        self.df.loc[:,'음주강도'] = self.hairList[2]
        numberList=[]
        for index, data in self.df.iterrows():
            if data[1]==1 and data[2]==1:#흡연+음주 X
                numberList.append(1)
            elif data[1]!=1 and data[2]!=1:#흡연+음주 O
                val = data[1]+data[2]
                val = val / 10
                val = val / 2
                w = val
                numberList.append(self.Percentage(self.tobacooAndDrink_per, w))
            elif data[1]!=1 and data[2]==1:#흡연O 음주X
                w = data[1] / 10
                numberList.append(self.Percentage(self.tobacooYn_per, w))
            elif data[1]==1 and data[2] !=1: #흡연X 음주O
                w = data[2] / 10
                numberList.append(self.Percentage(self.drinkYn_per, w))

        self.hairList[3].extend(numberList)
        self.df.loc[:,'탈모강도'] = self.hairList[3]

```


- 리턴 값 만들기
```python
    def getDataFrame(self):
        return self.df
```


가짜데이터 생성하기
-------------------------
```python
if __name__ == '__main__':
    
    def print_df(data): #데이터프레임을 예쁘게 출력해주는 함수
        print(tabulate(data, headers='keys', tablefmt='psql'))  
    
    margi = createDataFrame() # 변수에 클래스 선언하기

    tobacoo_per = 0.5  # 피험자가 흡연할 확률 (독립1)
    tobacooYn_per = 0.2  # 흡연자가 탈모에 걸릴 확률 (독립1-종속)

    drink_per = 0.4  # 피험자가 음주할 확률 (독립2)
    drinkYn_per = 0.3  # 음주자가 탈모에 걸릴 확률 (독립2-종속)

    tobacooAndDrink_per = 0.5  # 흡연+음주자가 탈모에 걸릴 확률 (종속)

    #margi.valuesSetting( 독립1, 독립1-종속,
    #독립2, 독립2-종속,
    #(독립1+독립2)-종속
    margi.valuesSetting(tobacoo_per, tobacooYn_per,
                        drink_per, drinkYn_per,
                        tobacooAndDrink_per)

    margi.create(list(range(1,1001))) #피험자 1000명 만들기
    print_df(margi.getDataFrame())
"""
+-----+--------------+------------+------------+------------+   
|     |   피험자번호 |   흡연강도 |   음주강도 |   탈모강도 |   
|-----+--------------+------------+------------+------------|   
|   0 |       000001 |          4 |          1 |          2 |   
|   1 |       000002 |          2 |          1 |          2 |   
|   2 |       000003 |          1 |          1 |          1 |   
|   3 |       000004 |          1 |          1 |          1 |   
|   4 |       000005 |          1 |          5 |          1 |   
|   5 |       000006 |          1 |          1 |          1 |   
|   6 |       000007 |          1 |          1 |          1 |   
|   7 |       000008 |          2 |          1 |          2 |   
|   8 |       000009 |          4 |          1 |          3 |   
|   9 |       000010 |          1 |          1 |          1 |   
|  10 |       000011 |          1 |          1 |          1 |   
|  11 |       000012 |          1 |          2 |          4 |   
|  12 |       000013 |          1 |          4 |          2 |   
|  13 |       000014 |          1 |          1 |          1 |   
|  14 |       000015 |          1 |          1 |          1 |   
|  15 |       000016 |          1 |          4 |          4 | 
"""
```

변수 선언하기
--------------------
```python
df = margi.getDataFrame()
    x=df.loc[:,'흡연강도'].values #독립변수
    corY=df.loc[:,'음주강도'].values #흡연-음주의 상관분석용
    y=df.loc[:,'탈모강도'].values # 회귀분석용 종속변수
```

상관분석
----------------
### 공분산 계산 식
```python
    cov = (np.sum(x*y)-len(x)*np.mean(x)*np.mean(y))/len(x)
    print(cov)
    """
    출력 = 0.7074000000000001
    """
```
### numpy 공분산함수
```python
    cov = np.cov(x,y)[0,1]
    print(cov)
    """
    출력 = 0.7081081081081081
    """
```
### 공분산 
- cov > 0 (x가 증가할 때 y도 증가한다. = 양의 상관관계)
- cov < 0 (x가 증가할 때 y는 감소한다. = 음의 상관관계)
- cov == 0 (두 변수간에 아무런 선형관계가 없다.)
### 공분산의 한계
- 공분산은 값으 범위가 정해져 있지 않기 때문에 어느정도의 값을 높냐 낮냐로 정하기 애매하다.
- 이러한 공분산의 값을 정규화하여 특정 범위의 값만 나오게끔 하는 상관계수라는 개념이 존재한다.

### 상관분석의 종류
- 피어슨 상관계수 : 숫자형-숫자형 변수의 정규분포 관계 분석

- 스피어만 순위 상관계수 : 숫자형-숫자형 변수의 정규성이 없는 단조 관계(단순증가/단순감소) 분석   
> 수학-영어 과목의 석차 구하기   

- 캔달의 타우 : 숫자형-숫자형 변수의 변수의 정규성이 없는 단조 관계 분석     
> A가 B보다 키가 크고 몸무게도 더 나간다. = C
> A가 B보다 키는 크지만 B의 몸무게가 더 나간다. =D 
> P = (C-D)/(C+D)   

- 스피어만과 캔달의 차이 : 
> 두 변수의 각 비교대상의 상하 관계가 같을 때 캔달을 사용하며 변수의 순서가 존재할 때 스피어만을 사용한다.

### 상관분석 하기

- 통계분석 라이브러리 중 scipy를 사용한다.
```python
    import scipy.stats as stats
    
    result = stats.pearsonr(x, corY)
    print('statistic = %.3f, pvalue = %.3f'%(result))
    """
    statistic < 상관계수 / pvalue < 해당 통계의 유의성
    출력= statistic = 0.031, pvalue = 0.330
    "흡연강도와 음주강도는 서로 상관이 있다."= 가설 기각
    """
```

### 상관분석 가설검증
- p-value = 0.05<0.330 수준에서 통계적 유의성을 검증하여 기존 가설을 기각
- 상관계수는 0.031으로 흡연강도와 음주강도는 서로 상관이 없다.
- "흡연강도와 음주강도는 서로 상관이 있다."라는 기존 가설을 기각한다.

### 상관계수 기준
- 양의 상관계수 =  x가 증가할 때 y도 증가한다.
- 음의 상관계수 =  x가 증가할 때 y는 감소한다.
- 0.3~0.4 = x변수와 y변수는 약한 상관관계이다.
- 0.5~0.6 = x변수와 y변수는 서로 상관관계이다.
- 0.7~    = x변수와 y변수는 강한 상관관계이다.

### pvalue란
- 해당 통계검증 결과의 유의성을 확인해주는 계수이다.   
- pvalue가 특정 값보다 크거나 작을 경우 가설을 기각하거나 체택한다.
- 학문별로 기준 값이 다르다.
> 심리학과 같은 사회과학 계열은 pvalue가 0.05~0.001 범위에서 가설을 체택한다.(일부 학문 제외)   
> 의약임상 계열은 pvalue의 체택 범위를 0.000000000001 범위에서 체택한다. (정확하지 않다.)     
> 물리학 계열은 pvalue의 체택 범위가 0.00000000000000000001로 알고 있다. (정확하지 않다.)   
> 사회과학 계열의 경우 현실 세계에서 하나의 현상이 가지는 원인에는 무수히 많은 매개변수가 있기 때문에 유의성의 범위가 넓은 걸로 알고 있다. (정확하지 않다.)
> ex) 1+1=1(자연과학)/ 1+1=창문(사회과학)
    
### 회귀분석 하기

-통계분석 라이브러리 중 scipy를 사용한다.

```python
    x=df['흡연강도']
    chis = stats.linregress(x, y)
    print(chis) # 출력 = LinregressResult(slope=0.5492508944543828, intercept=1.1217296511627906, rvalue=0.43949900450681434, pvalue=1.7563188530051353e-48, stderr=0.03553378620945475)
    
    print('설명력 : ', chis.rvalue) # 출력 = 설명력 :  0.43949900450681434
    print('p-valeu : {:.3f} '.format(chis.pvalue)) # 출력 = p-valeu : 0.000 
    #"흡연강도는 탈모증상에 영향을 미친다.(흡연강도는 탈모증상에 대해 약 43%정도 설명할 수 있다.)" = 가설 체택
    
```
### 회귀분석 가설검증
- p-value = 0.05>0.000 *** 수준에서 통계적 유의성을 검증하여 기존 가설을 체택
- R-squared는 0.43으로 흡연강도는 탈모증상에 대해 약 43%정도 설명할 수 있다.
- "흡연강도는 탈모증상에 영향을 미친다."라는 기존 가설을 체택한다.

### 설명력(rvalue 또는 R-squared)
- 독립변수가 종속변수를 얼마만큼 설명할 수 있는지를 보여주는 계수이다.     
- 일반적으로 R-squared 또는 Adjusted R-squared가 0.3일 때 "독립변수가 종속변수의 약 30%정도 설명할 수 있다"고 표현한다.
- 몇 퍼센트정도가 나와야 유의한지는 연구자의 판단에 따라 상이할 수 있다.
> ex) 돈(x)이 행복(y)에 미치는 영향에 있어서 R-squared가 0.17일 때   
> 사람에 따라 R-squared = 0.17이 높은지 낮은지 판단하는 기준이 다를 수 있다.   
> 단 행복의 구성요소는 돈만 있는 것이 아니라, 학문적으로 밝혀진 것은 가족관계, 친구관계, 회사내 동료 관계, 직장 여부, 직장 만족도, 취미 등 다양하다.   
> 행복의 구성요소가 돈만 있다면 연구자는 R-squared = 0.17이 유의하지 않다고 판단할 것이고   
> 행복의 구성요소가 다양하다면 연구자는 R-squared = 0.17이 유의하다고 판단할 수도 있다.   
