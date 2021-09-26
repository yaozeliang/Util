## Typing

-   1.类型检查，防止运行时出现参数和返回值类型不符合。
-   2.作为开发文档附加说明，方便使用者调用时传入和返回参数类型。
-   3.该模块加入后并不会影响程序的运行，不会报正式的错误，只有提醒。
```py
from typing import List, Tuple, Dict
def test(a:int, s:str, f:float, b:bool) -> Tuple[int, Tuple, Dict, bool]:
    l = a
    tup = tuple(s)
    di = {'key': f}
    bo = b
    return l, tup, di, bo

print(test(12, 'test', 1.00, 1)) # 输入正确类型的参数

# 结果
(12, ('t', 'e', 's', 't'), {'key': 1.0}, 1)

print(test('sss', 'test', 1, 'h')) # 输入错误的参数类型

# 结果
ValueError: invalid literal for int() with base 10: 'sss'
# 注意：报错并不是因为typing检查到了类型错误，而是里面的int方法不支持对str进行操作
```

## Common Type

-   **int,long,float: 整型,长整形,浮点型;**
-   **bool,str: 布尔型，字符串类型；**
-   **List, Tuple, Dict, Set:列表，元组，字典, 集合;**
-   **Iterable,Iterator:可迭代类型，迭代器类型；**
-   **Generator：生成器类型；**
    
  ```py
from typing import List, Union

def func(a: int, string: str) -> List[int or str]:
	   list1 = []
	   list1.append(a)
	   list1.append(string)
	   return list1

def get_next_id() -> Union[int, None]:
	   return 1
	   return None
```
  
  ## Use
  ```py
from typing import List, Tuple, Dict

names: List[str] = ['lily', 'tom']
version: Tuple[int, int, int] = (6, 6, 6)
operations: Dict[str, bool] = {'sad': False, 'happy': True}
```

>### Variable

```py
# 声明变量类型的类型的方式，python 3.6 +
age: int = 1
# 在 Python 3.5 及更低版本中，您可以改用类型注释
# 同上效果
age = 1  # type: int
# 无需初始化变量即可对其进行注释
a: int  # ok（但不能调用，name 'a' is not defined，直到被赋值）
# 在条件分支中很有用
child: bool
if age < 18:
    child = True
else:
    child = False
```

>### Built-in Type

```PY
from typing import List, Set, Dict, Tuple, Optional

# 对于简单的 Python 内置类型，只需使用类型的名称
x: int = 1
x: float = 1.0
x: bool = True
x: str = "test"
x: bytes = b"test"
# 对于 collections ，类型名称用大写字母表示，并且
# collections 内类型的名称在方括号中
x: List[int] = [1]
x: Set[int] = {6, 7}
# 与上述相同，但具有类型注释语法
x = [1]  # type: List[int]
# 对于映射，需要键和值的类型
x: Dict[str, float] = {'field': 2.0}
# 对于固定大小的元组，指定所有元素的类型
x: Tuple[int, str, float] = (3, "yes", 7.5)
# 对于可变大小的元组，使用一种类型和省略号
x: Tuple[int, ...] = (1, 2, 3)
# 使用 Optional[] 表示可能为 None 的值
x: Optional[str] = some_function()
# Mypy 理解 if 语句中的值不能为 None
if x is not None:
    print(x.upper())
# 如果由于某些不变量而使值永远不能为 None，请使用断言
assert x is not None
print(x.upper())
```

>### Function

```py
from typing import Callable, Iterator, Union, Optional, List

# 注释函数定义的方式
def stringify(num: int) -> str:
    return str(num)

# 指定多个参数的方式
def plus(num1: int, num2: int) -> int:
    return num1 + num2

# 在类型注释后为参数添加默认值
def f(num1: int, my_float: float = 3.5) -> float:
    return num1 + my_float

# 注释可调用（函数）值的方式, lambda 可以此方法
x: Callable[[int, float], float] = f

# 产生整数的生成器函数安全地返回只是一个
# 整数迭代器的函数，因此这就是我们对其进行注释的方式
def g(n: int) -> Iterator[int]:
    i = 0
    while i < n:
        yield i
        i += 1

# 可以将功能注释分成多行
def send_email(address: Union[str, List[str]],
               sender: str,
               cc: Optional[List[str]],
               bcc: Optional[List[str]],
               subject='',
               body: Optional[List[str]] = None
               ) -> bool:
```

>### Mixed Structure

```py
from typing import Union, Any, List, Optional, cast

# Union 表示可能是以下几种类型
x: List[Union[int, str]] = [3, 5, "test", "fun"]

# 不知道类型或它太动态而无法为它编写类型，请使用 Any
x: Any = mystery_function()

# 如果使用空容器或“无”初始化变量
# 类型注解帮助 mypy 获知类型信息
x: List[str] = []
x: Optional[str] = None

# 每个位置 arg 和每个关键字 arg 均为 str
def call(self, *args: str, **kwargs: str) -> str:
    request = make_request(*args, **kwargs)
    return self.do_api_query(request)

# cast 可以转换类型
a = [4]
b = cast(List[int], a)  # 正常通过
c = cast(List[str], a)  # 正常通过 (运行是不做检查，无影响)

# 如果要在类上使用动态属性，请使其覆盖 “ __setattr__”
# 或 “ __getattr__”。
#
# "__setattr__" 允许动态分配名称
# "__getattr__" 允许动态访问名称
class A:
    # 如果 x 与“值”属于同一类型，则这将允许分配给任何 A.x
    # （使用“value: Any”以允许任意类型）
    def __setattr__(self, name: str, value: int) -> None: ...

    # 如果 x 与返回类型兼容，则将允许访问任何 A.x
    def __getattr__(self, name: str) -> int: ...

a.foo = 42  # Works
a.bar = 'Ex-parrot'  # Fails type checking
```


> ### lambda 的类型标注
```py
from typing import Callable

# is_even 传入 int 返回布尔
is_even: Callable[[int], bool] = lambda x: (x % 2 == 0)
# func 传入两个字符串，返回 int
func: Callable[[str, str], int] = lambda var1, var2: var1.index(var2)
```

> ### 鸭子类型

在程序设计中，鸭子类型（英语：duck typing）是动态类型的一种风格。在这种风格中，一个对象有效的语义，不是由继承自特定的类或实现特定的接口，而是由"当前方法和属性的集合"决定。

在典型的 Python 代码中，许多可以将列表或 dict 作为参数的函数只需要将其参数设为“类似于列表”（list-like）或“类似于 dict”（dict-like）即可。 “类似列表”或“类似字典”（或类似其他的东西）的特定含义被称为「鸭子类型」，并且标准化了在惯用Python中常见的几种鸭子类型。

```py
from typing import Mapping, MutableMapping, Sequence, Iterable, List, Set

# 将 Iterable 用于一般可迭代对象（for 中可用的任何东西）
# 以及需要序列（支持 len 和  __getitem__ 的序列）
def f(ints: Iterable[int]) -> List[str]:
    return [str(x) for x in ints]

f(range(1, 3))

# Mapping 映射描述了一个我们不会经常变化的
# 类似 dict 的对象（带有  __getitem__）
# 而 MutableMapping 则描述了一个对象（带有 __setitem__）
def f(my_mapping: Mapping[int, str]) -> List[int]:
    my_mapping[5] = 'maybe'  # mypy 会引发错误
    return list(my_mapping.keys())

f({3: 'yes', 4: 'no'})

def f(my_mapping: MutableMapping[int, str]) -> Set[str]:
    my_mapping[5] = 'maybe'  # mypy 正常执行
    return set(my_mapping.values())

f({3: 'yes', 4: 'no'})
```

>### Class
```py
class MyClass:
    # 在类主体中声明实例变量
    attr: int
    # 具有默认值的实例变量
    charge_percent: int = 100

    # __init__ 方法不返回任何内容，因此返回 None
    def __init__(self) -> None:
        ...

    # 对于实例方法，省略 self 的类型
    def my_method(self, num: int, str1: str) -> str:
        return num * str1

# 用户定义的类作为注释中的类型有效
x: MyClass = MyClass()

# 可以使用 ClassVar 批注来声明类变量
class Car:
    seats: ClassVar[int] = 4
    passengers: ClassVar[List[str]]

# 可以在  __init__ 中声明属性的类型
class Box:
    def __init__(self) -> None:
        self.items: List[str] = []
```

> ### NoReturn

```py
def fun() -> NoReturn:
    print('Hi!')
```
>### Dict、Mapping、MutableMapping
```py

def size(rect: Mapping[str, int]) -> Dict[str, int]:  
 return {'width': rect['width'] + 100, 'height': rect['width'] + 100}
```

> ### Sequence
Sequence，是 collections.abc.Sequence 的泛型，在某些情况下，我们可能并不需要严格区分一个变量或参数到底是列表 list 类型还是元组 tuple 类型，我们可以使用一个更为泛化的类型，叫做 Sequence，其用法类似于 List，如：
```py


def square(elements: Sequence[float]) -> List[float]:  
 return [x ** 2 for x in elements]

```

> ### Any
Any，是一种特殊的类型，它可以代表所有类型，静态类型检查器的所有类型都与 Any 类型兼容，所有的无参数类型注解和返回类型注解的都会默认使用 Any 类型，也就是说，下面两个方法的声明是完全等价的：

```py
def add(a):  
 return a + 1  
  
def add(a: Any) -> Any:  
 return a + 1
```

>  ### TypeVar
TypeVar，我们可以借助它来自定义兼容特定类型的变量，比如有的变量声明为 int、float、None 都是符合要求的，实际就是代表任意的数字或者空内容都可以，其他的类型则不可以，比如列表 list、字典 dict 等等，像这样的情况，我们可以使用 TypeVar 来表示。 例如一个人的身高，便可以使用 int 或 float 或 None 来表示，但不能用 dict 来表示，所以可以这么声明：
```py
height = 1.75  
Height = TypeVar('Height', int, float, None)  
def get_height() -> Height:  
 return height

```

>  ### NewType
NewType，我们可以借助于它来声明一些具有特殊含义的类型，例如像 Tuple 的例子一样，我们需要将它表示为 Person，即一个人的含义，但但从表面上声明为 Tuple 并不直观，所以我们可以使用 NewType 为其声明一个类型，如：
```py
Person = NewType('Person', Tuple[str, int, float])  
person = Person(('Mike', 22, 1.75))

```

> ### Union
Union，联合类型，`Union[X, Y]`  代表要么是 X 类型，要么是 Y 类型。 联合类型的联合类型等价于展平后的类型：

```py
Union[int, str, int] == Union[int, str]
```

>### Optional
Optional，意思是说这个参数可以为空或已经声明的类型，即 `Optional[X]` 等价于 `Union[X, None]`。 但值得注意的是，这个并不等价于可选参数，当它作为参数类型注解的时候，不代表这个参数可以不传递了，而是说这个参数可以传为 None。 如当一个方法执行结果，如果执行完毕就不返回错误信息， 如果发生问题就返回错误信息，则可以这么声明：

```py

def judge(result: bool) -> Optional[str]:  
	 if result: return 'Error Occurred'
```


