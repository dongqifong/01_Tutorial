## 物件導向介紹

1. [繼承(Inheritance)](#1)

2. [覆寫(Override)](#2)

3. [多重繼承(Multiple Inheritance)](#3)

4. [多層繼承(Multilevel Inheritance)](#4)

5. [多載(Overloading)](#5)

6. [多型(Polymorphism)](#6)

7. [封裝(Encapsulation)](#7)

<div id=1> </div>
    
# 繼承(Inheritance)

子類別透過繼承的動作，擁有父類別結構的行為


```python
#建立一個父類別叫做Transportation
class Transportation:
    # constructor
    def __init__(self):
        self.name="Transportation"
        self.color="Null"
        
    # member function
    def call_name(self):
        print("call_name: ",self.name)
        
    # member function
    def get_color(self):
        print("color = ",self.color)
        
    # member function
    def power(self):
        print("Not define")
```


```python
T=Transportation()
T.call_name()
```

    call_name:  Transportation



```python
# 建立兩個子類別為 Bus
# 同時繼承Transportation的屬性和方法

# 但同時想新增自己專有的屬性n_tire --> 
# 靠super()呼叫父類別方法＋再在__init__下面新增自己的專有屬性

#新增自己專有的方法accelerate

class Bus(Transportation):
    # constructor
    def __init__(self,n_tire):
        # 因為想要繼承父類別__init__()的屬性和方法，
        # 但又想要再額外新增自己特有的屬性tire
        
        # 所以透過super來同時執行父類別的方法，其後因為名稱同父類別，
        # 再進行改寫得到新增自己屬性的目的
        super().__init__()
        self.n_tire=n_tire
        
    def set_color(self,color):
        self.color=color
        
    def accelerate(self):
        print("Bus accelerate")
        
    def get_n_tire(self):
        print("輪胎數目 = ",self.n_tire)
```


```python

# 繼承Transportation後，Bus也同時繼承了屬性和方法，可以不需要在新建一次__init_
print("第一次呼叫Bus的call_name")
b=Bus(4)
b.call_name()# call_name:  Transportation

# 因為有繼承到屬性和方法，所以可以直接更改繼承的屬性，
# 呼叫繼承的方法也可以看到改變後的狀態
print()
print("更改了屬性name和color後，在呼叫一次繼承的call_name方法")
b.name="Bus"
b.call_name()# call_name:  Bus

# 呼叫Bus自己獨有的方法
print()
print("呼叫Bus自己獨有的方法")
b.accelerate()# Bus accelerate


# 呼叫Bus類別中的set_color來更改color屬性
print()
print("呼叫Bus類別中的set_color來更改color屬性")
b.set_color("Red")
# 呼叫父類別的get_color來得到剛剛設定的顏色
b.get_color()

# 呼叫Bus類別中的get_n_tire獲得輪胎數
print()
print("呼叫Bus類別中的get_n_tire獲得輪胎數")
# b.get_n_tire()
```

    第一次呼叫Bus的call_name
    call_name:  Transportation
    
    更改了屬性name和color後，在呼叫一次繼承的call_name方法
    call_name:  Bus
    
    呼叫Bus自己獨有的方法
    Bus accelerate
    
    呼叫Bus類別中的set_color來更改color屬性
    color =  Red
    
    呼叫Bus類別中的get_n_tire獲得輪胎數


<div STYLE="page-break-after: always;"></div>

<div id=2> </div>

# 覆寫(Override)
以相同的方法名稱撰寫不同的內容可以達到覆寫的作用


```python
# 同上面，在Car class中，若想呼叫父類別的方法，可以使用super
class Car(Transportation):
    def power(self):
        self.name="Car"#因為繼承了name屬性，可以直接更改
        super().call_name()#呼叫父類別的call_name方法
        print("I use gasoline as Power")
c=Car()
c.power()
```

    call_name:  Car
    I use gasoline as Power


<div STYLE="page-break-after: always;"></div>

<div id=3> </div>

# 多重繼承(Multiple Inheritance)
子類別繼承超過一種以上的類別


```python
# 建立父類別一
class Tool:
    def __init__(self):
        self.material="metal"
    def Tool_method(self):
        print("Tool method")
        
# 建立父類別二
class Function:
    def __init__(self):
        self.func=None
    def set_func(self,func):
        self.func=func
        
# 子類別Scissor繼承父類別Tool和Function
class Scissor(Tool,Function):
    def get_func(self):
        #因為從Function繼承了func屬性，可以直接呼叫
        print("I'm Scissor, my function is ",self.func)
s=Scissor()
s.set_func("cut")
s.get_func()
```

    I'm Scissor, my function is  cut


<div STYLE="page-break-after: always;"></div>

<div id=4> </div>

# 多層繼承(Multilevel Inheritance)
父類別也是繼承其他類別而來，層數超過一層的繼承方式


```python
class Transp:
    def __init__(self):
        self.name=None
        self.color=None
        self.material=None
    def build(self):
        # pass 表示這個方法不做任何回傳值，只是供給未來繼承者覆寫使用
        print("Do nothing")
        pass
    def go(self):
        # pass 表示這個方法不做任何回傳值，只是供給未來繼承者覆寫使用
        print("Do nothing")
        pass
    
class A_Car(Transp):
    def __init__(self):
        super().__init__()
        self.n_tire=None
        
    def set_material(self,m):
        self.material=m
        
    def go(self):
        if self.name and self.color and self.material and self.n_tire:
            print("| All properties are already settle down")
            print("| Let's accecelarate")
        else:
            print("| some properties are missing")
            print("| But Let's Go")
        
class Taxi(A_Car):
    def build(self):
        print("Before building, do 'go' ")
        print("-"*20)
        
        super().go()
        
        
        print()
        print("After building, do 'go' ")
        print("-"*20)
        
        self.name="Taxi"
        self.color="Yellow"
        self.n_tire=4
        super().set_material("Metal")
    
        super().go()
```


```python
T=Taxi()
T.build()
print()
print("當然，也可以讓T呼叫go()方法:")
T.go()
```

    Before building, do 'go' 
    --------------------
    | some properties are missing
    | But Let's Go
    
    After building, do 'go' 
    --------------------
    | All properties are already settle down
    | Let's accecelarate
    
    當然，也可以讓T呼叫go()方法:
    | All properties are already settle down
    | Let's accecelarate


<div STYLE="page-break-after: always;"></div>

<div id=5> </div>

# 多載(Overloading)
一個類別中有相同名稱但不同引數數量的方法，會以引數的數量決定呼叫哪一個方法

但是Python並**不提供**多載特性，如果真的寫了兩個相同名稱的方法，會以後面的為主


```python
class Overloading_class:
    def method(self,x):
        print("呼叫引數數目為一個的method")
    def method(self,x,y):
        print("呼叫引數數目為兩個的method")
O=Overloading_class()
print("執行引數數目為二的方法")
O.method(1,2)

print()
print("執行引數數目為一的方法")
try:
    O.method(1)
except:
    print("Use 'O.method(1)' fail")
```

    執行引數數目為二的方法
    呼叫引數數目為兩個的method
    
    執行引數數目為一的方法
    Use 'O.method(1)' fail


<div STYLE="page-break-after: always;"></div>

<div id=6> </div>

# 多型(Polymorphism)
不同類別繼承同一個父類別，對父類別某個共同方法的表現方法各自發展各自的內容


```python
class Animal:
    def shout(self):
        print("Not define")
        
class Cat(Animal):
    def shout(self):
        print("meow~")
        
class Dog(Animal):
    def shout(self):
        print("woof~")
        
cat=Cat()
dog=Dog()
print("貓叫：")
cat.shout()
print()
print("狗叫")
dog.shout()
```

    貓叫：
    meow~
    
    狗叫
    woof~


<div STYLE="page-break-after: always;"></div>

<div id=7> </div>

# 封裝(Encapsulation)
在類別內使用私有變數，只留"接口"給使用者，也就是使用者不需要知道內部到底如何運做，只需要知道要**接口**和**回傳值**是什麼

在python中，只要在變數前面加入兩個底線(__)就可以設為私有變數


```python
class Calculate:
    def __init__(self):
        self.x=0
        self.y=0
        self.__w=-1 # private variable
        
    def __Add(self,x,y):# private method
        return x+y
    
    def subtract(self,x,y):
        return x-y
    
    def combination(self):
        self.x=0
        self.y=10
        self.__w=2
        self.__z=3
        res1=self.__Add(self.x,self.y) # 10
        res2=self.subtract(self.x,self.__w) # -2
        return res1+res2 #12
cal=Calculate()
print("call self.x = ",cal.x)

y1=cal.subtract(7,5)#2
print("y1 = ",y1)

y2=cal.combination()#8
print("y2 = ",y2)
```

    call self.x =  0
    y1 =  2
    y2 =  8



```python

# 以下方法是不行的
# y3=cal.Add(1,2)
# x=cal.x
print("Try get cal.__w:")
try:
    print(cal.__w)
except:
    print("\tGet cal.__w fail")
    
print()
print("Try get cal.w:")
try:
    print(cal.w)
except:
    print("\tGet cal.w fail")
    
print()
print("Try get cal.__Add(1,2):")
try:
    print(cal.__Add(1,2))
except:
    print("\tGet cal.__Add(1,2) fail")
    
print()
print("Try get cal.Add(1,2):")
try:
    print(cal.Add(1,2))
except:
    print("\tGet cal.Add(1,2) fail")
    
```

    Try get cal.__w:
    	Get cal.__w fail
    
    Try get cal.w:
    	Get cal.w fail
    
    Try get cal.__Add(1,2):
    	Get cal.__Add(1,2) fail
    
    Try get cal.Add(1,2):
    	Get cal.Add(1,2) fail


# 封裝(Encapsulation)-2
1. Python 有提供另一個方法，還是可以存取得到私有方法或屬性
2. python會更改私有屬性的名稱來分別是否為私有屬性，所以我們用這個更改過的名稱來存取就可以了
3. 透過底線＋類別名稱就可以辦得到


```python
# Python 有提供另一個方法，還是可以存取得到私有方法或屬性
# 透過底線＋類別名稱就可以辦得到

#先以__dict__來看全部的屬性
dic=cal.__dict__
print("私有屬性__dic = ",dic)

print()
# 原來Python會更改私有屬性的名稱來分別是否為私有屬性，
# 所以我們用這個更改過的名稱來存取就可以了
private_w=cal._Calculate__z
print("私有屬性__w = ",private_w)

#也可以更改私有屬性
cal._Calculate__z=10
private_w=cal._Calculate__z
print("更改私有屬性__w =",private_w)

print()
#也可以調用私有方法
res=cal._Calculate__Add(100,200)
print("私有方法__Add(100,200)結果 = ", res)
```

    私有屬性__dic =  {'x': 0, 'y': 10, '_Calculate__w': 2, '_Calculate__z': 3}
    
    私有屬性__w =  3
    更改私有屬性__w = 10
    
    私有方法__Add(100,200)結果 =  300

