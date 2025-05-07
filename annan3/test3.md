# 软件实践项目实验报告
## 一、基本信息
1. **课程**：软件实践项目
2. **实验名称**：Jupyter Notebook实践
3. **专业**：软件工程
4. **班级**：软工一班
5. **学号**：121052022037
6. **姓名**：陈安楠
7. **实验日期**：2025年5月7日

## 二、实验目的
1. 进一步熟悉Python语法，强化对Python语言基本结构、数据类型、函数定义与使用等方面的掌握。
2. 熟练掌握Notebook开发的基本流程，包括创建、编辑、运行单元格，以及管理Notebook文件等操作。
3. 熟悉Python中常用库的用法，如NumPy、Pandas、Matplotlib等，能够运用这些库进行数据处理、分析与可视化。

## 三、实验内容
### （一）安装Jupyter Notebook和相关Python环境
采用Anaconda安装方式，Anaconda集成了众多科学计算相关的Python库和工具，为Jupyter Notebook的运行提供了便利的环境。安装过程严格按照官方教程进行，确保环境配置正确，Jupyter Notebook能够正常启动。

### （二）Notebook基本概念学习
1. **熟悉快捷键**：通过查阅官方文档和实际操作练习，掌握了常用快捷键。例如，运行单元格的快捷键`Shift+Enter`，可快速执行代码并显示结果；进入命令模式的快捷键`Esc`，方便进行单元格的选择、复制、删除等操作；进入编辑模式的快捷键`Enter`，用于对单元格内容进行编辑。这些快捷键的熟练使用大大提高了Notebook的操作效率。
2. **掌握Cell的两种模式（Edit和Command）**：理解了Edit模式用于编辑单元格内容，在此模式下可以编写代码、文本等；Command模式用于对单元格进行各种操作，如选择、移动、删除单元格等。通过`Esc`和`Enter`键可以在两种模式之间灵活切换，在实际操作中能够根据需求快速切换模式，提升了操作的便捷性。
3. **理解Kernel的概念**：Kernel是Notebook中运行代码的计算引擎，不同的编程语言需要不同的Kernel支持。在本次实验中，主要使用Python的Kernel来运行Python代码。了解到可以根据项目需求切换Kernel，以适应不同语言环境的开发需求，这为后续多语言开发项目奠定了基础。

### （三）熟悉基本的Python语法并编写选择排序算法
1. **定义selection_sort函数执行选择排序功能**：在Notebook中创建代码单元格，编写Python代码定义`selection_sort`函数。该函数接受一个列表作为参数，通过选择排序算法对列表进行排序。选择排序的基本思想是在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。具体代码如下：
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_index = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_index]:
                min_index = j
        arr[i], arr[min_index] = arr[min_index], arr[i]
    return arr
```
2. **定义test函数进行测试**：编写`test`函数用于测试`selection_sort`函数的正确性。在`test`函数中，首先生成一个随机整数列表，然后调用`selection_sort`函数对列表进行排序，并输出排序前后的结果。通过这种方式可以直观地验证`selection_sort`函数的功能是否正确。具体代码如下：
```python
import numpy as np

def test():
    arr = np.random.randint(1, 10, 5)
    print("Original array:", arr)
    sorted_arr = selection_sort(arr)
    print("Sorted array:", sorted_arr)
```
在Notebook中运行`test`函数，多次测试后，结果均符合预期，表明`selection_sort`函数实现正确。

### （四）数据分析
1. **数据读取与初步查看**：使用Pandas库对财富500强排名数据集进行分析。首先，通过`pd.read_csv`函数读取数据集文件，将数据加载到DataFrame中。然后，使用`head()`和`tail()`函数分别查看数据集的前几行和后几行数据，对数据的整体结构和内容有了初步了解。代码如下：
```python
import pandas as pd

df = pd.read_csv(r'D:\迅雷下载\fortune500.csv')
df.head()
df.tail()
```
2. **数据预处理**：对数据进行预处理操作，包括重命名列名、检查数据类型、处理异常值等。使用`columns`属性重命名列名，使列名更简洁直观；使用`dtypes`属性查看各列的数据类型，发现`profit`列的数据类型为对象，存在非数字值。通过`str.contains`方法筛选出`profit`列中包含非数字值的行，并使用`len`函数统计异常值的数量。最后，通过布尔索引删除包含异常值的行，并将`profit`列的数据类型转换为数值类型。代码如下：
```python
df.columns = ['year', 'rank', 'company','revenue', 'profit']
non_numberic_profits = df.profit.str.contains('[^0-9.-]')
len(df.profit[non_numberic_profits])
df = df.loc[~non_numberic_profits]
df.profit = df.profit.apply(pd.to_numeric)
```
3. **分组计算与分析**：使用`groupby`方法按照`year`对数据进行分组，并计算每年的`revenue`和`profit`的平均值。通过分组计算，可以更清晰地观察不同年份的财富500强公司的营收和利润变化趋势。代码如下：
```python
group_by_year = df.loc[:, ['year','revenue', 'profit']].groupby('year')
avgs = group_by_year.mean()
x = avgs.index
y1 = avgs.profit
y2 = avgs.revenue
```

### （五）数据图形绘制
使用Matplotlib库进行数据图形绘制。首先，定义`plot`函数用于绘制折线图，该函数接受横坐标数据、纵坐标数据、坐标轴对象、图表标题和纵坐标标签作为参数。然后，分别调用`plot`函数绘制利润和收入随年份变化的折线图，直观展示了1955年至2005年财富500强公司的平均利润和平均收入的变化趋势。代码如下：
```python
import matplotlib.pyplot as plt

def plot(x, y, ax, title, y_label):
    ax.set_title(title)
    ax.set_ylabel(y_label)
    ax.plot(x, y)
    ax.margins(x=0, y=0)

fig, ax = plt.subplots()
plot(x, y1, ax, 'Increase in mean Fortune 500 company profits from 1955 to 2005', 'Profit (millions)')

fig, ax = plt.subplots()
plot(x, y2, ax, 'Increase in mean Fortune 500 company revenues from 1955 to 2005', 'Revenue (millions)')
```

### （六）将完成的Jupyter Notebook在Github上进行共享
在本地完成Jupyter Notebook的编写和测试后，将该文件上传至Github仓库进行共享。首先，在Github上创建一个新的仓库；然后，通过Git命令将本地文件添加到仓库中，并进行提交和推送操作。这样，其他开发者可以通过访问Github仓库获取该Notebook文件，实现代码的共享和交流。

## 四、实验总结
1. **实验收获**：通过本次实验，对Python语法有了更深入的理解和掌握，能够熟练运用函数、列表、循环等基本语法结构进行编程。同时，熟悉了Jupyter Notebook的开发流程和常用操作，掌握了快捷键、Cell模式、Kernel等概念，提高了使用Notebook进行开发的效率。此外，还学会了使用Pandas库进行数据处理和分析，以及使用Matplotlib库进行数据可视化，能够从数据中提取有价值的信息并以直观的方式展示出来。
2. **遇到的问题及解决方法**：在数据预处理阶段，处理`profit`列的异常值时，最初对`str.contains`方法的使用不够熟练，导致筛选异常值的逻辑出现错误。通过查阅Pandas官方文档，深入理解了该方法的参数和用法，最终成功筛选出异常值并进行了处理。在数据图形绘制过程中，对Matplotlib库的一些绘图参数设置不够熟悉，导致绘制的图表样式不符合预期。通过参考Matplotlib的官方教程和示例代码，逐步调整参数，使图表更加美观和清晰。
3. **实验改进方向**：在后续的实验中，可以进一步优化代码，提高代码的可读性和可维护性。例如，在编写选择排序算法时，可以添加更多的注释，解释代码的实现逻辑；在数据处理和分析过程中，可以封装一些常用的函数，减少重复代码。此外，可以尝试使用更多的Python库和工具，如Seaborn、Plotly等，提升数据可视化的效果和交互性。同时，加强对数据挖掘和机器学习算法的学习，将其应用到实际的数据处理项目中，挖掘数据更深层次的价值。 