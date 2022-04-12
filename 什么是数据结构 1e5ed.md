# 什么是数据结构

## 一、关于数据结构的一些定义

- “**数据结构**是数据对象，以及存在于该对象的实例和组成实例的数据元素之间的各种联系。这些联系可以通过定义相关的函数来给出。”   ——Sartaj Sahni，《数据结构、算法与应用》
- “**数据结构**是ADT（抽象数据类型Abstract Data Type）的物理实现。”  ——Clifford A.Shaffer，《数据结构与算法分析》
- “**数据结构**（Data Structure）是计算机中存储、组织数据的方式。通常情况下，精心选择的数据结构可以带来最优效率的算法。”  ——中文维基百科

## 二、解决问题方法的效率

1. 跟数据的组织方式有关；
2. 跟空间的利用效率有关；
3. 跟算法的巧妙程度有关。

### 1. 跟数据的组织方式有关

**例1：如何在书架上摆放图书？**

- 随便放
    - 新书怎么插入？ 哪里有空放哪里，一步到位！
    - 怎么找到某本指定的书？ 一本一本挨着找，累死！
- 按照书名的拼音字母顺序查找
    - 新书怎么插入？新进一本书《阿Q正传》，a开头，需要把后面的所有书往后挪
    - 怎么找到某本指定的书？ 二分查找！
- 把书架划分为几块区域，每块区域指定摆放某种类别的图书，在每种类别内，按照书名的拼音字母顺序摆放
    - 新书怎么插入？先定类别，二分查找确定位置，移出空位
    - 怎么找到某本指定的书？ 先定类别，再二分查找
    
    问题：空间如何分配？类别应该分多细？
    

### 2. 跟空间的来利用效率有关

**例2：写程序实现一个函数PrintN，使得传入一个正整数为n的参数后，能顺序打印从1到n的全部正整数。**

用循环和递归两种方法分别实现，实现代码如下所示。

```c
#include<iostream>
using namespace std;

//循环实现
void PrintN_Iteration(int n)
{
	for (int i = 1; i <= n; i++)
	{
		cout << i << endl;
	}
}

//递归实现
void PrintN_Recursion(int n)
{
	if(n)
	{
		PrintN_Recursion(n - 1);
		cout << n << endl;
	}
}

int main()
{
	PrintN_Iteration(10000);
	//PrintN_Recursion(10000);

	system("pause");

	return 0;
}
```

一次测试n=10、100、1000、10000...，循环实现和递归实现的结果如下图所示。

 

- 循环实现：可以正常打印
    
    ![Untitled](%E4%BB%80%E4%B9%88%E6%98%AF%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%201e5ed/Untitled.png)
    
- 递归实现：出现异常，无法正常打印（将自己所有可食用的空间用完，还不够，所以出现异常）
    
    ![Untitled](%E4%BB%80%E4%B9%88%E6%98%AF%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%201e5ed/Untitled%201.png)
    

### 3. 跟算法的巧妙程度有关

**例3：写程序计算给定多项式f ( x ) = a 0 + a 1 x + . . . + a n − 1 x n − 1 + a n x n f(x)=a_{0}+a_{1}x+...+a_{n-1}x^{n-1}+a_{n}x^{n}*f*(*x*)=*a*0+*a*1*x*+...+*an*−1*xn*−1+*an**xn*在给定点x处的值。**

最直接的方法实现代码如下所示。

```c
#include<iostream>
using namespace std;

double f1(double a[], int n, double x)
{
	double p = a[0];
	for (int i = 1; i <= n; i++)
	{
		p += (a[i] * pow(x, i));  //f(x)=a[0]+a[1]*x+...+a[n−1]*x^(n−1)+a[n]*x^(n)
	}
	return p;
}

int main()
{
	double a[] = { 0,1,2,3,4,5,6,7,8,9 };
	cout << "f1(1.1) = " << f1(9, a, 1.1) << endl;

	system("pause");

	return 0;
}
```

f1(1.1) = 84.0626

但通常情况下，我们不会使用上述方法，简化f ( x ) = a 0 + a 1 x + . . . + a n − 1 x n − 1 + a n x n = a 0 + x ( a 1 + x ( . . . ( a n − 1 + x ( a n ) ) . . . ) ) f(x)=a_{0}+a_{1}x+...+a_{n-1}x^{n-1}+a_{n}x^{n}=a_{0}+x(a_{1}+x(...(a_{n-1}+x(a_{n}))...))*f*(*x*)=*a*0+*a*1*x*+...+*an*−1*xn*−1+*anxn*=*a*0+*x*(*a*1+*x*(...(*an*−1+*x*(*an*))...))，编写代码如下所示。

```c
#include<iostream>
using namespace std;

double f2(double a[], int n, double x)
{
	double p = a[n];
	for (int i = n; i > 0; i--)
	{
		p = a[i - 1] + x * p;    //f(x)=a[0]+x*(a[1]+x*(...(a[n-1]+x*(a[n]))...))
	}
	return p;
}

int main()
{
	double a[] = { 0,1,2,3,4,5,6,7,8,9 };
	cout << "f2(1.1) = " << f2(9, a, 1.1) << endl;

	system("pause");

	return 0;
}
```

f2(1.1) = 84.0626

常用第二种方法计算给定多项式f ( x ) = a 0 + a 1 x + . . . + a n − 1 x n − 1 + a n x n f(x)=a_{0}+a_{1}x+...+a_{n-1}x^{n-1}+a_{n}x^{n}*f*(*x*)=*a*0+*a*1*x*+...+*an*−1*xn*−1+*an**xn*在给定点x处的值，是因为第二种方法相比于第一种方法运行更快。

clock():捕捉从程序开始运行到clock()被调用时所耗费的时间。这个时间单位是clock tick，即“时钟打点”。计算上述两种方法编写的程序运行时间的代码如下所示。

```c
#include<iostream>
using namespace std;
#include<ctime>

double f1(double a[], int n, double x)
{
	double p = a[0];
	for (int i = 1; i <= n; i++)
	{
		p += (a[i] * pow(x, i));  //f(x)=a[0]+a[1]*x+...+a[n−1]*x^(n−1)+a[n]*x^(n)
	}
	return p;
}

double f2(double a[], int n, double x)
{
	double p = a[n];
	for (int i = n; i > 0; i--)
	{
		p = a[i - 1] + x * p;    //f(x)=a[0]+x*(a[1]+x*(...(a[n-1]+x*(a[n]))...))
	}
	return p;
}

int main()
{
	double a[] = { 0,1,2,3,4,5,6,7,8,9 };

	clock_t startTime, endTime;
	startTime = clock();//计时开始
	f1(a, 9, 1.1);
	endTime = clock();//计时结束
	cout << "The run time of f1 is: " << (double)(endTime - startTime) / CLOCKS_PER_SEC  << "s" << endl;  //常数CLOCKS_PER_SEC为机器时钟每秒所走的时钟打点数
	cout << "f1(1.1) = " << f1(a, 9, 1.1) << endl;

	startTime = clock();//计时开始
    f2(a, 9, 1.1);
	endTime = clock();//计时结束
	cout << "The run time of f2 is: " << (double)(endTime - startTime) / CLOCKS_PER_SEC  << "s" << endl;
	cout << "f2(1.1) = " << f2(a, 9, 1.1) << endl;

	system("pause");

	return 0;
}
```

运行结果如下图所示

![Untitled](%E4%BB%80%E4%B9%88%E6%98%AF%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%201e5ed/Untitled%202.png)

```c
int main()
{
	double a[] = { 0,1,2,3,4,5,6,7,8,9 };

	clock_t startTime, endTime;
	startTime = clock();//计时开始
	for (int i = 0; i < 1e7; i++)
	{
		f1(a, 9, 1.1);
	}
	//f1(a, 9, 1.1);
	endTime = clock();//计时结束
	//cout << "The run time of f1 is: " << (double)(endTime - startTime) / CLOCKS_PER_SEC  << "s" << endl;  //常数CLOCKS_PER_SEC为机器时钟每秒所走的时钟打点数
	cout << "The run time of f1 is: " << (double)(endTime - startTime) / CLOCKS_PER_SEC / 1e7 << "s" << endl;
	cout << "f1(1.1) = " << f1(a, 9, 1.1) << endl;

	startTime = clock();//计时开始
	for (int i = 0; i < 1e7; i++)  //重复调用函数以获得充分多的时钟打点数
	{
		f2(a, 9, 1.1);
	}
	//f2(a, 9, 1.1);
	endTime = clock();//计时结束
	//cout << "The run time of f2 is: " << (double)(endTime - startTime) / CLOCKS_PER_SEC  << "s" << endl;
	cout << "The run time of f2 is: " << (double)(endTime - startTime) / CLOCKS_PER_SEC / 1e7 << "s" << endl;  //计算函数单次运行时间
	cout << "f2(1.1) = " << f2(a, 9, 1.1) << endl;

	system("pause");

	return 0;
}
```

因为运行速度太快，所以结果均显示为0，无法区分快慢。**解决方案：**让被测函数重复运行充分多次，使得测出的总的时钟打点间隔充分长，最后计算被测函数平均次数运行的时间即可。修改main函数部分的代码为如下所示。

运行结果如下图所示

![Untitled](%E4%BB%80%E4%B9%88%E6%98%AF%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%201e5ed/Untitled%203.png)

由上述结果可以看出，第二种方法编写的程序运行时间更快。

## 三、****到底什么是数据结构****

- 数据结构是关于**数据对象**在计算机中的组织方式
    - 逻辑结构
    - 物理存储结构
- 数据对象必定与一系列加在其上的**操作**相关联
- 完成这些操作所用的方法就是**算法**

## 四、抽象数据类型

**抽象数据类型**（Abstract Data Type，ADT）是计算机科学中具有类似行为的特定类别的数据结构的数学模型；或者具有类似语义的一种或多种程序设计语言的数据类型。抽象数据类型是描述数据结构的一种理论工具，其目的是使人们能够独立于程序的实现细节来理解数据结构的特性。抽象数据类型的定义取决于它的一组逻辑特性，而与计算机内部如何表示无关。

- **数据类型**
    - 数据对象集
    - 数据集合相关联的操作集
- **抽象：**描述数据类型的方法不依赖于具体实现
    - 与存放数据的机器无关
    - 与数据存储的物理结构无关
    - 与实现操作的算法和编程语言均无关

只描述数据对象集和相关操作集“**是什么**”，并不涉及“**如何做到**”的问题。

**例4：“矩阵”的抽象数据类型定义。**

- **类型名称：** 矩阵(Matrix)
- **数据对象集：**一个M × N M\times N*M*×*N*的矩阵A M × N = ( a i j ) ( i = 1 , . . . , M ; j = 1 , . . . , N )A_{M\times N}/math=(a_{ij})(i=1,...,M; j=1,...,N )*AM*×*N*=(*aij*)(*i*=1,...,*M*;*j*=1,...,*N*)由M × N M\times N*M*×*N*个三元组< a , i , j > <a, i, j ><*a*,*i*,*j*>构成，其中a a*a*是矩阵元素的值，i i*i*是元素所在的行号，j j*j*是元素所在的列号。
- **操作集：**
 对于任意矩阵A、B、C∈Matrix，以及整数i、j、M、N
    - `Matrix Create( int M, int N )`：返回一个M × N的空矩阵；
    - `int GetMaxRow ( Matrix A )`：返回矩阵A的总行数；

**抽象**表示在：

1. 在数据对象集中，a只是矩阵元素的值，不关心其具体的数据类型；
2. 在操作集中，`ElementType GetEntry (Matrix A, int i, int j)`返回的也是通用的数据类型`ElementType`，并不是某个具体的数据类型。