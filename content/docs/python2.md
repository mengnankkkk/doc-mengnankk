---
title: python期末复习
categories:
  - python
  
abbrlink: 2701
date: 2024.12.10
tags: 
   - python
---

# 正则表达式

Python 的正则表达式通过 `re` 模块实现，用于处理复杂的字符串匹配和提取操作。

- 常用函数：
  - `re.match(pattern, string)`
    从字符串的开头匹配，返回匹配对象或 `None`。
  - `re.search(pattern, string)`
    搜索整个字符串，找到第一个匹配。
  - `re.findall(pattern, string)`
    返回所有匹配的结果（列表形式）。
  - `re.finditer(pattern, string)`
    返回所有匹配结果的迭代器。
  - `re.sub(pattern, repl, string)`
    替换匹配的内容。
  - `re.split(pattern, string)`
    根据模式分割字符串。

**. 常用正则表达式语法**

| 符号    | 含义                                                  |
| ------- | ----------------------------------------------------- |
| `.`     | 匹配任意单个字符（除换行符）。                        |
| `^`     | 匹配字符串的开头。                                    |
| `$`     | 匹配字符串的结尾。                                    |
| `*`     | 匹配前面的字符 0 次或多次。                           |
| `+`     | 匹配前面的字符 1 次或多次。                           |
| `?`     | 匹配前面的字符 0 次或 1 次。                          |
| `{n}`   | 匹配前面的字符恰好 n 次。                             |
| `{n,}`  | 匹配前面的字符至少 n 次。                             |
| `{n,m}` | 匹配前面的字符至少 n 次，至多 m 次。                  |
| `[]`    | 匹配字符集合中的任意一个，例如 `[a-z]` 匹配小写字母。 |
| `       | `                                                     |
| `()`    | 分组，用于提取子模式。                                |
| `\`     | 转义字符，用于匹配特殊字符。                          |

实例：

```
import re

email = "example@domain.com"
pattern = r"^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$"
if re.match(pattern, email):
    print("有效的邮箱地址")
else:
    print("无效的邮箱地址")

```

| `\d` | 匹配任意数字，等价于 `[0-9]`。                      |
| ---- | --------------------------------------------------- |
| `\D` | 匹配任意非数字。                                    |
| `\w` | 匹配任意字母、数字、下划线，等价于 `[a-zA-Z0-9_]`。 |
| `\W` | 匹配任意非字母、数字、下划线。                      |
| `\s` | 匹配任意空白字符（包括空格、制表符等）。            |
| `\S` | 匹配任意非空白字符。                                |

对于需要多次使用的正则表达式，可以通过 `re.compile()` 提前编译：

```
python复制代码import re

pattern = re.compile(r"\d+")
result = pattern.findall("订单号123，总金额456元")
print(result)  # 输出 ['123', '456']
```

编译时可以指定标志（flags）：

- `re.IGNORECASE` (`re.I`)：忽略大小写。
- `re.MULTILINE` (`re.M`)：多行模式。
- `re.DOTALL` (`re.S`)：使 `.` 匹配换行符。

```
pattern = re.compile(r"hello", re.IGNORECASE)
print(pattern.findall("Hello, hello, HELLO"))  # 输出 ['Hello', 'hello', 'HELLO']
```



# 概念理解

1.编写程序的目的是“使用计算机解决问题”，写出使用计算机解决问题的常见步骤

**问题定义与理解**：明确问题及需求，确定输入输出。

**问题分析**：分析问题细节，识别关键点和约束。

**设计算法**：设计解决问题的算法，选择合适的数据结构。

**选择工具**：选择适合的编程语言和开发环境。

**编写程序**：根据算法编写清晰的代码，确保正确性。

**调试与测试**：检测和修复错误，进行功能和边界测试。

**优化与改进**：提高程序性能和可维护性。

**文档化与维护**：编写使用文档，定期维护和更新程序。

2.python语言时一种被广泛使用的高级通用脚本编程语言，请结合本学期的学习，列举不少于5个Python语言的特点。

**简洁易读**：代码清晰，使用缩进结构。

**跨平台**：可在不同操作系统上运行。

**动态类型**：变量类型在运行时确定。

**解释型语言**：无需编译，直接运行。

**丰富的库**：大量标准库和第三方库支持。

3.根据自己的理解，说明为什么应尽量从列表的尾部进行元素的添加与删除操作。

**时间复杂度为O(1)**：在列表尾部添加或删除元素，不需要移动其他元素，操作时间固定。

**避免元素移动**：从头部或中间添加/删除元素时，需要移动大量元素，时间复杂度为O(n)，效率低。

4.阐述说明Python语言编程时为什么要导入扩展库？导入的途径有哪些？

1. **功能增强**：扩展库提供了丰富的功能，避免重复造轮子，节省开发时间。
2. **简化代码**：扩展库封装了复杂的功能，使用者可以直接调用简洁的接口，减少代码量。
3. **提高效率**：许多扩展库经过优化，能提高程序的性能。

**导入途径**：

1. **`import`**：导入整个模块，使用时需要加模块名，例如 `import math`。
2. **`from ... import ...`**：只导入模块中的某个部分，例如 `from math import sqrt`。
3. **`import ... as ...`**：为模块指定别名，简化代码，例如 `import numpy as np`。

5.**变量**：用于存储数据的命名位置。Python 是动态类型语言，变量不需要事先声明类型，类型由赋值时自动推导。

**数据类型**：Python 支持多种数据类型，包括 `int`（整数）、`float`（浮点数）、`str`（字符串）、`list`（列表）、`tuple`（元组）、`dict`（字典）、`set`（集合）等。

**List（列表）**：有序，可变，可以包含重复元素。{}

**Tuple（元组）**：有序，不可变，通常用于表示不希望修改的数据。（）

**Dict（字典）**：无序，由键值对组成，键是唯一的，适用于映射关系。{}

**Set（集合）**：无序，不能包含重复元素，常用于去重或集合运算。{}

6.**控制结构**

- **条件语句**：`if`、`elif`、`else` 用于根据条件执行不同的代码块。
- **循环语句**：`for` 和 `while` 循环用来重复执行代码。`for` 用于遍历序列（如列表、字典等），`while` 用于基于条件的循环。
- **循环控制**：`break`、`continue` 和 `pass` 用于控制循环的执行。

7.**定义函数**：通过 `def` 关键字定义函数，函数可以带参数、返回值。

**内置函数**：Python 提供了许多常用的内置函数，如 `len()`、`max()`、`min()`、`range()` 等。

8.面向对象

**类与对象**：Python 是面向对象的语言，可以通过 `class` 定义类。类是对象的蓝图，而对象是类的实例。

**继承**：子类继承父类的属性和方法，能够扩展和重写父类的功能。

**多态与封装**：多态使得不同的类可以以相同的方式调用，封装则通过访问控制保护数据。

**构造函数 `__init__`**：每次创建对象时调用，用于初始化对象的状态。

9.异常处理

- **`try`、`except`、`else`、`finally`**：用于捕获和处理异常，避免程序崩溃。`finally` 块中的代码会始终执行，通常用于资源清理。
- **自定义异常**：可以通过继承 `Exception` 类自定义异常类型。

10.字典推导式

**列表推导式**：一种简洁的创建列表的方式，通常包含条件和表达式。

**字典推导式**：类似于列表推导式，用于创建字典。

11.生成器和迭代器

**生成器**：使用 `yield` 关键字创建的迭代器。生成器可以按需生成数据，避免一次性生成所有数据，节省内存。

**迭代器**：Python 中的迭代器协议（`__iter__()` 和 `__next__()`），用于按序访问容器中的元素。



# 真题解析

1. 表达式[3] not in [1，3，4，5]的值为（  **false**    ）。

2. 你认为“在编写Python程序时，不管一条语句有多长，都必须写在同一行中，不能换行。”这种观点对吗？（  **不对**    ）。

3. 假设已导入模块re，那么表达式re.findall('\d{1,3}','a12b345ccc567890')的结果为（ **12,345,567，890**   ）

4. 已知vec=[[1，2]，[3，4]]，则表达式[col for row in vec for col in row]的值为（  **[1, 2, 3, 4]**    ）。

5. 表达式max（[1，2，3]，[2，3，5]，[5，3，7，6]，[5，1，3，8，9]，key=len）的值为（   **[5，1，3，8，9]**    ）。

6. 在导入模块re后，设s='a s d e'，则re.sub('a|s|d|e','well',s)的结果为（**well well well well**     ）。

7. s=’qlu，upc，jlu，pear，qinghua’，则s.split(‘，’)的输出结果是（ [**'qlu,upc,jlu,pear,qinghua']**        ）。

8. 对于文本文件，使用Python的内置函数open（）以读模式成功打开后返回的文件对象（  **可以**  ）使用for循环直接迭代。

9. 设f=lambda x，y，z：x+y*z，则print（f（1，2，3））的输出结果为（ **7**）

10. 导入re模块后，若ch=’aaa   bb c d e  fff   ’则执行语句：’ ’.join（ch.split( )）后的输出结果为（ **'aaa bb c d e fff'**   ）。

11.下面代码的运行后的结果是

```
def demo(a, b, c=3, d=100):
    return sum((a, b, c, d))

print(demo(1, 2, 3, 4))  # (1)
print(demo(1, 2, d=3))   # (2)

```

```
10，9
```

解析：

因为**如果函数的参数给了值的话，就用给的值。没给的话，就要用默认值**

```
list2 = [5, 6, 7, 8, 9]
print(list2[])       # (1)
print(list2[2:])     # (2)
print(list2[:-2])    # (3)
print(list2[1:3])    # (4)
print(list2[1] <= 3 or list2[3] > 5)  # (5)

```

```
# (1) 错误：SyntaxError
# (2) [7, 8, 9]
# (3) [5, 6, 7]
# (4) [6, 7]
# (5) True

```

解析：

因为在列表中没有index的话，是输出不了东西的

然后从前面开始分别是为0123...

从后面开始分别是-1-2-3....

然后【1:3】是不包括后面的，和for循环中的range一样

```
import types

class Car:
    price = 100000                               # 定义类属性
    def __init__(self, c):
        self.color = c                           # 定义实例属性

car1 = Car("Red")                                # 实例化对象
car2 = Car("Blue")
print(car1.color, Car.price)                     # 查看实例属性和类属性的值
Car.price = 110000                               # 修改类属性
Car.name = 'QQ'                                  # 动态增加类属性
car1.color = "Yellow"                            # 修改实例属性
print(car2.color, Car.price, Car.name)
print(car1.color, Car.price, Car.name)

```

```
Red 100000
Blue 110000 QQ
Yellow 110000 QQ

```

这个就不解释了，直接能看懂哈哈哈☺

```
Dcountry = {
    "中国": "北京",
    "美国": "华盛顿",
    "法国": "巴黎"
}

print(list(Dcountry.values()))  # (1)
print(list(Dcountry.keys()))    # (2)
print(Dcountry.get("德国", "伦敦"))  # (3)
print(Dcountry.get("美国", "伦敦"))  # (4)

for key in Dcountry:  # (5)
    print(key)

```

```
['北京', '华盛顿', '巴黎']  # (1)
['中国', '美国', '法国']  # (2)
伦敦  # (3)
华盛顿  # (4)
中国
美国
法国  # (5)

```

前面的不解释，主要就是键值对，前面的为key 后面的为value

`get` 方法用于获取字典中指定键的值。如果键不存在，则返回默认值

so,print(Dcountry.get("德国", "伦敦"))找不到key=德国所以return默认值伦敦

print(Dcountry.get("美国", "伦敦"))找到key=美国，返回他的value华盛顿

# 编程题

1.在软件设计和实现时，用户登录模块必不可少。请使用python语言编程实现该功能，要求：每次登录提供3次机会，只有账户名和密码完全匹配才“登录成功！”，否则提示“请注意剩余尝试次数为X次”，超过3次，则提示“3次用户名或者密码均有误，请退出程序！”。（提示：用户名和密码可以提前自行设定好。）

```
# 设定用户名和密码
username = "admin"
password = "123456"

# 最大尝试次数
max_attempts = 3

# 登录验证
for attempt in range(max_attempts):
    input_username = input("请输入用户名: ")
    input_password = input("请输入密码: ")

    if input_username == username and input_password == password:
        print("登录成功！")
        break
    else:
        remaining_attempts = max_attempts - attempt - 1
        if remaining_attempts > 0:
            print(f"请注意，剩余尝试次数为{remaining_attempts} 次")
        else:
            print("3次用户名或者密码均有误，请退出程序！")

```

2.编程实现对键盘接受的整数进行奇偶数判断。

要求：包括主函数和is­_odd()子函数，其中is­_odd（）函数，用于实现数值奇偶数判断，参数为整数，如果参数为奇数，返回true，否则返回false。

```
# 判断奇偶数的子函数
def is_odd(num):
    return num % 2 != 0

# 主函数
def main():
    # 从键盘接受整数输入
    try:
        num = int(input("请输入一个整数: "))
        
        # 判断奇偶数
        if is_odd(num):
            print(f"{num} 是奇数")
        else:
            print(f"{num} 是偶数")
    except ValueError:
        print("请输入有效的整数")

# 调用主函数
if __name__ == "__main__":
    main()

```

3.中文里,有回文诗句、对联,如:"灵山大佛,佛大山灵","客上天然居,居然天上客"等等,都是美妙的符合正念倒念都一样的回文句；设n是一个任意自然数，如果n的各位数字反向排列所得自然数与n相等，则n称为回文数。从键盘输入5位数字，请编写程序判断这个数字是不是回文数（自行设置退出条件）。

```
# 判断回文数的函数
def is_palindrome(num):
    return str(num) == str(num)[::-1]

# 主函数
def main():
    while True:
        # 从键盘输入5位数字
        try:
            num = int(input("请输入一个5位数字（输入0退出程序）: "))
            
            if num == 0:
                print("程序退出")
                break
            
            if 10000 <= num <= 99999:  # 判断是否为5位数字
                if is_palindrome(num):
                    print(f"{num} 是回文数")
                else:
                    print(f"{num} 不是回文数")
            else:
                print("请输入一个有效的5位数字！")
        except ValueError:
            print("输入无效，请输入一个数字！")

# 调用主函数
if __name__ == "__main__":
    main()

```

4.编写程序，在D盘根目录下创建一个文本文件test.txt，并向其中写入字符串“Beautiful is better than ugly.Explicit is better than implicit.”

```
# 文件路径
file_path = "D:/test.txt"

# 要写入的字符串
text = "Beautiful is better than ugly.\nExplicit is better than implicit."

# 打开文件并写入
with open(file_path, "w") as file:
    file.write(text)

print("文件已成功创建并写入内容。")

```

5.\1. IT时代信息安全更为重要，无论是国家安全领域、商业领域还是个人信息都要有一定的保护措施。利用python课程中所学知识，批量产生50个人的信息，每个人的信息内容由以下字段组成：

| 姓名（XM） | 性别（XB） | 身份证号（ID） | 电话号码（TEL) | 家庭住址（ADRESS） |
| ---------- | ---------- | -------------- | -------------- | ------------------ |
| XXX        | 男         | 3709......     | 136......      | aaaaaaaaaaaaaaaa   |
| YYY        | 女         | 3701......     | 184......      | bbbbbbbbbbbbbbbb   |

身份证号和电话号码长度按实际长度产生，其它字段信息长度自行设定，编写程序完成以下要求：

（1）按照凯撒加密算法对产生的每项信息进行加密。

（2）把加密后每个人的信息写入到excel文件中，每一行存储一个人的信息。

（3）把加密后每个人的信息写入到写入到SQLite数据库中，每条记录存储一个人的信息。

```
import random
import sqlite3
import pandas as pd

# 凯撒加密函数
def caesar_encrypt(text, shift=3):
    return ''.join([chr((ord(char) - ord('a') + shift) % 26 + ord('a')) if char.isalpha() else char for char in text])

# 生成随机信息
def generate_info():
    name = random.choice(['张伟', '李娜', '王磊', '刘敏', '陈静'])
    gender = random.choice(['男', '女'])
    id_number = ''.join([str(random.randint(0, 9)) for _ in range(18)])
    tel = '1' + str(random.randint(3, 9)) + ''.join([str(random.randint(0, 9)) for _ in range(9)])
    address = ''.join(random.choices('abcdefghijklmnopqrstuvwxyz0123456789', k=15))
    return [name, gender, id_number, tel, address]

# 生成50条数据
people_info = [generate_info() for _ in range(50)]

# 对数据进行凯撒加密
shift = 3
encrypted_info = [[caesar_encrypt(field, shift) for field in person] for person in people_info]

# 写入Excel
def write_to_excel(data):
    df = pd.DataFrame(data, columns=['姓名', '性别', '身份证号', '电话号码', '家庭住址'])
    df.to_excel('encrypted_people_info.xlsx', index=False)

# 写入SQLite
def write_to_sqlite(data):
    conn = sqlite3.connect('people_info.db')
    cursor = conn.cursor()
    cursor.execute('''
    CREATE TABLE IF NOT EXISTS people (
        name TEXT, gender TEXT, id_number TEXT, phone_number TEXT, address TEXT)
    ''')
    cursor.executemany('''
    INSERT INTO people (name, gender, id_number, phone_number, address)
    VALUES (?, ?, ?, ?, ?)
    ''', data)
    conn.commit()
    conn.close()

# 执行写入操作
write_to_excel(encrypted_info)
write_to_sqlite(encrypted_info)

print("数据已成功写入 Excel 和 SQLite 数据库。")

```

6.π（圆周率）是一个无理数，即无限不循环小数。精确求解圆周率π是几何学、物理学和很多工程学科的关键。对π的精确求解曾经是数学历史上一直难以解决的问题之一，因为π无法用任何精确公式表示，在电子计算机出现以前，π只能通过一些近似公式的求解得到，如BBP公式。结合自己了解的数学方法和Python编程技术，设计一种近似计算π的程序，并给出设计步骤和主要思想。

```
import random

# 1. 使用莱布尼茨级数法近似计算 π
def leibniz_formula(n_terms):
    pi_approx = 0
    for i in range(n_terms):
        pi_approx += ((-1) ** i) / (2 * i + 1)
    return 4 * pi_approx

# 2. 使用蒙特卡罗方法近似计算 π
def monte_carlo_pi(n_points):
    inside_circle = 0
    for _ in range(n_points):
        x = random.random()
        y = random.random()
        # 计算点是否在单位圆内
        if x**2 + y**2 <= 1:
            inside_circle += 1
    return 4 * inside_circle / n_points

# 主函数
def main():
    n_terms = 1000000  # 莱布尼茨级数法迭代次数
    n_points = 1000000  # 蒙特卡罗方法随机点数

    # 使用莱布尼茨级数法计算 π
    pi_leibniz = leibniz_formula(n_terms)
    print(f"莱布尼茨级数法计算 π：{pi_leibniz}")

    # 使用蒙特卡罗方法计算 π
    pi_monte_carlo = monte_carlo_pi(n_points)
    print(f"蒙特卡罗方法计算 π：{pi_monte_carlo}")

if __name__ == "__main__":
    main()

```


$$
\pi = 4 \times \left(1 - \frac{1}{3} + \frac{1}{5} - \frac{1}{7} + \frac{1}{9} - \dots \right)
$$
7.个滴滴快车是怎么计费的？请你用所学Python语言编写一个计费程序。根据实际情况，回答问题。

（一）收集、分析数据，运用数理思维建模

登录滴滴出行官网，得到了如下信息，即“滴滴快车（普通型）计价规则”：

![img](C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

小萌同学19：33从“济南泉城广场”到“千佛山景区”乘坐滴滴快车（普通车型），里程4.1公里，时长约21分钟，按照表中的计费规则，此次出行应该支付的车费是：车费=8+（4.1-3.3）×1.35+（21-9）×0.2=9.68。

假设Tot1表示时长费，Tot2表示里程费，S表示实际里程，T表示实际时长，Cost表示应支付费用。运用数学解析式归纳出计费公式。

答:

 

 

（二）运用算法描述方法将问题解决步骤化

明晰了滴滴快车车费的计算方法之后，设计求解滴滴快车普通时段车费的算法，并用IPO算法设计法和流程图的方式表述出来。

![img](C:/Users/ikeife/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg) 1、IPO（输入-处理-输出）算法描述：

I：             

P：            

O：            

2、流程图描述：如图所示，请添加序号（1）  

~（5）处的语句。

（1）：            

（2）：            

（3）：            

（4）：            

（5）：            

（三）编写、调试、运行程序解决问题。



根据滴滴快车计价规则，可以推导出车费计算的公式：

1. **基本费用**：8 元
2. **里程费用**：如果实际行驶里程超过 3.3 公里，则超出的部分按每公里 1.35 元计费。即，实际里程 S 大于 3.3 时，超出部分按 `(S - 3.3) * 1.35` 计算。
3. **时长费用**：如果实际时长超过 9 分钟，则超出的部分按每分钟 0.2 元计费。即，实际时长 T 大于 9 分钟时，超出部分按 `(T - 9) * 0.2` 计算。

根据这些规则，最终的车费可以通过以下公式计算：
$$
Cost=8+(S−3.3)×1.35+(T−9)×0.2\
$$


1. **IPO（输入-处理-输出）算法描述**：

   - **I（输入）**：输入实际的行驶里程 `S` 和实际的时长 `T`。
   - **P（处理）**：根据上述的计费规则，计算里程费用和时长费用，并求出总车费。
   - **O（输出）**：输出应支付的车费。

2. **流程图描述**：

   以下是步骤化的描述：

   - **(1)**：输入实际里程 `S` 和时长 `T`。
   - **(2)**：计算里程费用：如果实际里程 `S` 大于 3.3 公里，则计算 `(S - 3.3) * 1.35`；否则里程费用为 0。
   - **(3)**：计算时长费用：如果实际时长 `T` 大于 9 分钟，则计算 `(T - 9) * 0.2`；否则时长费用为 0。
   - **(4)**：计算总费用：`Cost = 8 + 里程费用 + 时长费用`。
   - **(5)**：输出车费 `Cost`。



根据上面的计费公式和流程，下面是一个用 Python 编写的滴滴快车车费计算程序：

```
 	calculate_fare(distance, duration):
    # 基本费用
    base_fare = 8.0
    
    # 计算里程费用
    if distance > 3.3:
        distance_fare = (distance - 3.3) * 1.35
    else:
        distance_fare = 0
    
    # 计算时长费用
    if duration > 9:
        duration_fare = (duration - 9) * 0.2
    else:
        duration_fare = 0
    
    # 总车费
    total_fare = base_fare + distance_fare + duration_fare
    return total_fare

# 获取用户输入的实际里程和时长
distance = float(input("请输入实际行驶里程（公里）："))
duration = float(input("请输入实际行驶时长（分钟）："))

# 计算车费
fare = calculate_fare(distance, duration)

# 输出结果
print(f"此次出行的车费为：{fare:.2f} 元")
```



假设用户输入：

- 实际里程：`4.1` 公里
- 实际时长：`21` 分钟

程序输出：

```
复制代码请输入实际行驶里程（公里）：4.1
请输入实际行驶时长（分钟）：21
此次出行的车费为：9.68 元
```

# 基础知识补充

1.eval()会执行字符串中的表达式

例如：

```
eval(500/10)
输出
50.0
```

2.在列表中，空格也是占一个字符位置的，然后[3:8]前面的闭口的，后面是开口的，即3-7

3.Python 提供的一个元素全为字符串的列表写入文件的函数是

`writelines()` 方法将一个列表的所有字符串写入文件

`readlines()`是读取的方法，把

4.字典的键必须是不可变类型（例如，元组、字符串），而列表是可变的

5.jieba库应用

利用 Python 内置函数及 jieba 库中已有函数，计算字符串 s 的中文字符个数及中文词语 个数。

 

import jieba s = “中国举办冬奥会”

 

n = __________

 

m = __________

 

print("中文字符数为{}，中文词语数为{}。".format(n, m))

 

```
import jieba
s = "中国举办冬奥会"

n = len([ch for ch in s if '\u4e00' <= ch <= '\u9fa5'])  # 统计中文字符数
m = len(list(jieba.cut(s)))  # 统计中文词语数

print("中文字符数为{}，中文词语数为{}。".format(n, m))

```

jieba.lcut()返回一个列表，而不是生成器

```
import jieba
text = "我来到北京清华大学"
words = jieba.lcut(text)
print(words)  # 返回列表
```

jieba.posseg.cut()

```
import jieba.posseg as pseg
text = "我来到北京清华大学"
words = pseg.cut(text)
for word, flag in words:
    print(f"{word} {flag}")
```

jieba.analyse.extract_tags()

```
import jieba.analyse
text = "我爱北京天安门，天安门是北京的标志性建筑。"
keywords = jieba.analyse.extract_tags(text, topK=5, withWeight=False)
print(keywords)  # 返回前5个关键词

```

**`jieba.cut()`**: 精确模式分词。

**`jieba.lcut()`**: 返回列表形式的精确模式分词。

**`jieba.cut_for_search()`**: 搜索引擎模式分词。

**`jieba.posseg.cut()`**: 词性标注分词。

**`jieba.load_userdict()`**: 加载自定义词典。

**`jieba.analyse.extract_tags()`**: 提取关键词。

**`jieba.add_word()`**: 动态添加新词。

**`jieba.del_word()`**: 删除词典中的词。

6.type函数

type函数获取里面数据的类型

`val = (3)` 定义的是一个包含单一元素 `3` 的元组，但元组中的单个元素没有使用逗号，Python 会将其视为整数 `3`，因此它的类型是 `int`。

如果是多个元素，Python 会正确地识别为元组。举个例子：

```
val = (3, 4)
```

此时，`val` 是一个包含两个元素 `3` 和 `4` 的元组，类型是：

```
type(val)  # 输出 <class 'tuple'>
```

**解释**：当定义元组时，至少需要一个逗号来表示这是一个元组。即使只有一个元素，但只要加上逗号，它就会被识别为元组。例如：

```
val = (3,)  # 正确的单元素元组
```

在这种情况下，`val` 仍然是一个元组，类型是：

```
type(val)  # 输出 <class 'tuple'>
```

如果没有逗号，`val = (3)` 会被当作普通的括号表达式，`val` 只是一个整数 `3`。

7.

对于 `ls = list(range(5))`，`print(ls)` 的输出是：

```
[0, 1, 2, 3, 4]
```

输入列表为xxxx

8.

```
for s in "python":
    if s == "h":
        continue
    print(s, end="")

```

代码等于在遇到`h`的时候跳过，然后print

```
pyhon
```

9.

```
n = 1

def func(a, b):
    n = b
    return a * b

s = func("Hello~", 2)

print(s, n)
```

```
Hello~Hello~ 1

```

**因为n=2是局部变量，n=1是全局变量**

 10.

```
def func(s, i, j):
    if i < j:
        func(s, i+1, j-1)  # 递归调用
        s[i], s[j] = s[j], s[i]  # 交换元素

def main():
    a = [10, 6, 23, -90, 0, 3]
    func(a, 0, len(a)-1)  # 调用函数进行交换
    print(a)

main()

```

主要是找到规律

```
[3, 0, -90, 23, 6, 10]

```

1. **递归**：每次递归将 `i` 和 `j` 向中间靠拢，直到 `i >= j`。
2. **交换**：在递归返回时，交换 `i` 和 `j` 位置的元素，直到列表被反转。

- `func` 函数使用递归和回溯来实现反转。
- 最终输出的结果是反转后的列表。
