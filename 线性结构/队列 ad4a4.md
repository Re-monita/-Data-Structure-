# 队列

## 一、什么是队列

**队列**是一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作，和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头。

**队列**(Queue)：具有一定操作约束的线性表

- 插入和删除操作：只能在一端插入,而在另一端删除。
- 数据插入：入队列(AddQ)
- 数据删除：出队列(DeleteQ)
- 先来先服务
- 先进先出：FIFO

## 二、****队列的抽象数据类型描述****

**类型名称：**队列(Queue)

**数据对象集：**一个有0个或多个元素的有穷线性表

**操作集：**长度为MaxSize的队列Q$\in$Queue，队列元素item$\in$ElementType

1. `Queue CreatQueue( int MaxSize )`：生成长度为MaxSize的空队列；
2. `int IsFullQ( Queue Q, int MaxSize )`：判断队列Q是否已满；
3. `void AddQ( Queue Q, ElementType item )`：将数据元素item插入队列Q中；
4. `int IsEmptyQ( Queue Q )`：判断队列Q是否为空；
5. `ElementType DeleteQ( Queue Q)`：将队头数据元素从队列中删除并返回。

## 三、队列的顺序存储实现

**队列的顺序存储结构**通常由一个一维数组和一个记录队列头元素位置的变量`front`以及一个记录队列尾元素位置的变量`rear`组成。

```c
#define MaxSize<储存数据元素的最大个数>
struct QNode {
    ElementType Data[ MaxSize ];
    int rear;
    int front;
};
typedef struct QNode *Queue;
```

**例**：一个工作队列

- 队列空：front = -1,rear = -1
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled.png)
    
- AddQ Job1: front = -1, rear = 0
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%201.png)
    
- AddQ Job2: front = -1, rear  = 1
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%202.png)
    
- AddQ job3: front = -1, rear = 2
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%203.png)
    
- DeleteQ Job1: front = 0, rear = 2
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%204.png)
    
- AddQ job4: front = 0, rear = 3
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%205.png)
    
- AddQ job5: front = 0, rear = 4
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%206.png)
    
- AddQ job6: front = 0, rear = 5
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%207.png)
    
- DeleteQ Job2: front = 1, rear = 5
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%208.png)
    
- AddQ job7: front = 1, rear = 6
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%209.png)
    
- AddQ job8: rear已经到数组最大值了，加不进去，但是数组前面还是空的，所以可以使用循环队列来解决这个问题。
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2010.png)
    

循环队列：

- 队列空：front = 0, rear = 0

![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2011.png)

- AddQ Job1: front = 0, rear = 1
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2012.png)
    
- AddQ Job2: front = 0, rear = 2
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2013.png)
    
- AddQ Job3: front = 0, rear = 3
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2014.png)
    
- DeleteQ Job1: front = 1, rear = 3
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2015.png)
    
- AddQ Job4: front = 1, rear = 4
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2016.png)
    
- AddQ Job5: front = 1, rear = 5
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2017.png)
    
- AddQ Job6: front = 1, rear = 0
    
    ![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2018.png)
    
- AddQ Job7: front = rear，队列是空的还是满的？

**思考**：

1. 这种方案，堆栈空和满的判别条件是什么?
2. 为什么会出现空、满无法区分?根本原因?

**front 与rear“差距”有6种情况：**0，1，2，3，4，5

**队列装载元素个数情况有7种：**0（空队列）,1 (一个元素)，2，3，4，5、6

**出现空、满无法区分的根本原因：**数组大小等于n，队列装载元素个数情况有n+1种，我们要判别队列空满根据front 与rear“差距”的n种情况，也就是说，我们要根据n种情况来区分实际上存在的n+1种情况，是不可能的。

**解决方案：**

1. 使用额外标记：Size或者tag域；
2. 仅使用n-1个数组空间。

### 1. 入队列

```c
void AddQ(Queue PtrQ,ElementType item)
{
    if((PtrQ->rear+1) % Maxsize == PtrQ->front )  //front和rear指针的移动采用“加1取余”法,体现了顺序存储的“循环使用”
    {
        cout << "队列满" << endl;
        return;
    }
    PtrQ->rear = (PtrQ->rear+1)% MaxSize;
    PtrQ->Data[PtrQ->rear] = item;
}
```

### 2. 出队列

```c
ElementType DeleteQ(Queue PtrQ)
{
    if (PtrQ->front == PtrQ->rear )
    {
        cout << "队列空" << endl;
        return ERROR;
    }
    else 
    {
        PtrQ->front=(PtrQ->front+1)%MaxSize;
        return PtrQ->Data[PtrQ->front];
    }
}
```

### 3. 队列的顺序存储实现实例

```c
#include<iostream>
using namespace std;
#define MaxSize 100
typedef int ElementType;
typedef struct QNode *Queue;
struct QNode {
	ElementType Data[MaxSize];
	int front;   // 记录队头 
	int rear;    // 记录队尾 
};
Queue Q;

Queue CreateQueue();  // 初始化队列 
void AddQ(Queue Q, ElementType item);  //  入队
int IsFull(Queue Q); // 判断队列是否已满 
ElementType DeleteQ(Queue Q);  // 出队 
int IsEmpty(Queue Q); // 判断队列是否为空 

// 初始化 
Queue CreateQueue() {
	Q = (Queue)malloc(sizeof(struct QNode));
	Q->front = -1;
	Q->rear = -1;
	return Q;
}

// 判断队列是否已满
int IsFull(Queue Q) {
	return ((Q->rear + 1) % MaxSize == Q->front);
}

// 入队 
void AddQ(Queue Q, ElementType item) {
	if (IsFull(Q)) {
		cout << "队列满" << endl;
		return;
	}
	else {
		Q->rear = (Q->rear + 1) % MaxSize;  //front和rear指针的移动采用“加1取余”法,体现了顺序存储的“循环使用”
		Q->Data[Q->rear] = item;
	}
}

//判断队列是否为空
int IsEmpty(Queue Q) {
	return (Q->front == Q->rear);
}

// 出队
ElementType DeleteQ(Queue Q) {
	if (IsEmpty(Q)) {
		cout << "队列空" << endl;
		return 0;
	}
	else {
		Q->front = (Q->front + 1) % MaxSize;
		return Q->Data[Q->front];
	}
}

int main() {
	Q = CreateQueue();

	AddQ(Q, 1);
	cout << "1入队" << endl;

	AddQ(Q, 2);
	cout << "2入队" << endl;

	AddQ(Q, 3);
	cout << "3入队" << endl;

	cout << DeleteQ(Q) << "出队" << endl;
	cout << DeleteQ(Q) << "出队" << endl;

	system("pause");

	return 0;
}
```

## 四、队列的链式存储实现

**队列的链式存储结构**也可以用一个单链表实现。插入和删除操作分别在链表的两头进行。队列指针front和rear应该分别指向链表的哪一头?

队列的链式存储数据结构定义如下所示。

```c
struct Node{
    ElementType Data;
    struct Node *Next;
}
struct QNode{  //链队列结构
    struct Node *rear;  //指向队尾结点
    struct Node *front;  //指向队头结点
};
typedef struct QNode *Queue;
Queue PtrQ;
```

![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2019.png)

不带头结点的链式队列出队操作的一个示例：

```c
ElementType DeleteQ(Queue PtrQ)
{
    struct Node *FrontCell;
    ElementType FrontElem;
    
    if (PtrQ->front == NULL){
        cout << "队列空" << endl;
        return ERROR;
    }
    FrontCell = PtrQ->front;
    if(PtrQ->front == PtrQ->rear)  //若队列只有一个元素
        PtrQ->front = PtrQ->rear = NULL;  //删除后队列置为空
    else
        PtrQ->front = PtrQ->front->Next;
    FrontElem = FrontCell->Data;
    free(FrontCell);  //释放被删除结点空间
    return FrontElem;
}
```

### 队列的链式存储实现实例

```c
#include<iostream>
using namespace std;
typedef int ElementType;
typedef struct QNode *Queue;
struct Node {
	ElementType Data;
	struct Node *Next;
};
struct QNode {
	struct Node *rear;    // 指向队尾结点 
	struct Node *front;   // 指向队头结点 
};
Queue Q;

Queue CreateQueue();  // 初始化队列 
void AddQ(Queue Q, ElementType item);  //  入队
ElementType DeleteQ(Queue Q);  // 出队 
int IsEmpty(Queue Q); // 判断队列是否为空 

// 初始化 
Queue CreateQueue() {
	Q = (Queue)malloc(sizeof(struct QNode));
	Q->front = NULL;
	Q->rear = NULL;
	return Q;
}

// 是否为空 
int IsEmpty(Queue Q) {
	return (Q->front == NULL);
}

// 入队
void AddQ(Queue Q, ElementType item) {
	struct Node *node;
	node = (struct Node *)malloc(sizeof(struct Node));
	node->Data = item;
	node->Next = NULL;
	if (Q->rear == NULL) {  //此时队列空 
		Q->rear = node;
		Q->front = node;
	}
	else { //不为空 
		Q->rear->Next = node;  // 将结点入队 
		Q->rear = node;   // rear 仍然保持最后 
	}
}

// 出队
ElementType DeleteQ(Queue Q) {
	struct Node *FrontCell;
	ElementType FrontElem;
	if (IsEmpty(Q)) {
		cout << "队列空" << endl;
		return 0;
	}
	FrontCell = Q->front;
	if (Q->front == Q->rear) { // 队列中只有一个元素 
		Q->front = Q->rear = NULL;
	}
	else {
		Q->front = Q->front->Next;
	}
	FrontElem = FrontCell->Data;
	free(FrontCell);
	return FrontElem;
}

int main() {
	Q = CreateQueue();

	cout << "入队1" << endl;
	AddQ(Q, 1);

	cout << "入队2" << endl;
	AddQ(Q, 2);

	cout << "入队3" << endl;
	AddQ(Q, 3);

	cout << "出队" << DeleteQ(Q) << endl;
	cout << "出队" << DeleteQ(Q) << endl;
	cout << "出队" << DeleteQ(Q) << endl;
	cout << DeleteQ(Q) << endl;

	system("pause");

	return 0;
}
```

代码运行结果如下图所示。

![Untitled](%E9%98%9F%E5%88%97%20ad4a4/Untitled%2020.png)