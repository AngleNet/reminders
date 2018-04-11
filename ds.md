
1. Implements `list`, `queue`, `stack` via backing array.
    
    对于`list`: 在list中维护一个当前元素的索引,插入表示插入当前位置,将当前位置后的元素往后移动一个位置,然后插入.删除表示删除当前位置,将当前位置后的元素往前移动一个位置. 插入和删除操作都是**O(n)**. 我们可以让`list`在容量不足时扩展,每次扩展一倍,从链表为空开始,插入到N个元素的时间复杂度是O(N)*(利用摊还分析)*

    对于`queue`: 用`front`和`rear`维护队列的头部和尾部,入队从尾部入队,即`front = (front+1) % queue_length`,出队从头部出队,即`rear = (rear+1) % queue_length`. `push_back`和`pop_front`都是常数级. 有一个问题就是当队列中已经有了N个元素时会有`front==rear`, 这时我们如何分辨是否是空的队列? 可以限制队列中最多有`N-1`个元素.还有队列的长度可以由`(N-front+rear) % N`来计算得到. **缺点时队列的长度有一个固定的上限**. 应用有`Josephus problem` 和 `Round Robin Scheduler`.

    对于`stack`: 用`top`记录栈顶位置.入栈`top++`,出栈`top--`. `push`和`pop`操作都是常数级. **缺点是栈有一个固定的上限, 如果栈的长度可以事先预估, 则可以适用**

2. Implements `list`, `queue`, `stack` via pointer.

    对于`list`: 有双向链表和单向链表之分.双向链表可以快速访问头部和尾部. 一般也有循环列表比较常用.

    对于`stack`: 可以利用单链表实现

    对于`queue`: 利用双向链表实现双边队列.

3. `skiplist`

    主要的定义如下:
    ```cpp
    class SkipListNode{
        int val;
        vector<SkipListNode*> neighbors;
    };

    class SkipList{
        int count;
        SkipListNode head;

        bool search(int key);
        bool add(int key);
        bool remove(int key);
        void buildUpdatesTalbe(vector<SkipListNode*> &updates);
    };
    ```

    `skiplist`是一种随机数据结构,其插入,删除,搜索都可以达到**O(log n)**. 三种操作的重点在于总是从上到下,从左到右的搜索方式.
    * 插入: 先创建一个列表用于记录搜索路径上的上一跳节点,然后插入节点(即修改指针)
    * 删除: 创建`updates`数组,然后从底到顶更新节点指针
    * 搜索:从上到下,从左到右(`current[i].value < key`)

    *skiplist*和*tree*的区别: *skiplist*对于并发的访问更容易去维护,但是*tree*并不那么容易去维护,因为重分布的过程涉及的节点太多.在实际的并发编程中,*locking skip list*非常快,而*non-locking skip list*比加了锁的要更快.但是*locking red-black tree*随着并发数的增加性能线性减少.而且*skiplist*是一种随机数据结构.

    参考列表:
    * [An extensive examination of data structure using C# by Scott Mitchell](https://msdn.microsoft.com/en-us/library/ms379573(v=vs.80).aspx#datastructures20_4_topic4)

4. Sorting list

    可以利用插入排序和冒泡排序或者归并排序

5. Binary tree via array or pointer
    
    * 二叉树的各种遍历方式

        I. 先序遍历: 栈中保存的是以访问过的结点.压栈规则是将刚访问过的结点压栈,出栈规则是当向左已经遍历完时开始出栈.node一直指向即将被访问到的结点. 遍历过程是:如果下一个向左遍历的结点不空,访问该结点并继续向左遍历,如果下一个向左遍历的结点为空,则出栈并从出栈结点开始向左遍历

        II. 中序遍历: 栈中保存的是未访问过的结点.压栈规则是将向左遍历的路径上的结点压栈,出栈规则时向左已经遍历完. 当向左已经遍历完开始出栈并访问. node一直指向下一个向左遍历的结点. 遍历过程是:如果下一个向左遍历的结点不空,将该结点入栈并继续向左遍历,如果下一个向左遍历的结点为空,则出栈并访问结点,从出栈结点开始向左遍历.

        III. 后序遍历: 栈中保存的是未访问过的结点.每次将当前结点的左右孩子入栈.出栈是当确定栈顶结点是下一个要访问的结点时.遍历过程:每次检验栈顶结点,如果栈顶结点的左右子树都已经被访问了或者左子树或者右子树是上一个访问结点,那么出栈并访问,否则先将右子树入栈,然后将左子树入栈.

        IV. 层序遍历: 每一轮将本层上结点的左右孩子均入队.

6. Priority queue

7. Heap

8. Hash table

9. Dictionary

10. Binary search tree: AVL, Splay, (2,4) tree, red-black tree

    * Binary search tree: 缺点是当插入的元素基本是有序的时候,会退化成链表. 但是`self-balancing tree`就不会有这些问题.

        I. `find()`: 如果key比当前结点小,向左遍历,如果key比当前结点大,向右遍历,否则当前结点就是key

        II. `min() max()`: `min()`是最左边的元素, `max()`是最右边的元素

        III. `insert()`: 按照`find()`找到的索引为空的结点就是要插入的位置.

        IV. `remove()`: 利用`find()`找到这个结点N,如果N没有孩子,直接删掉,如果N有一个子节点,将子节点上移就可以了.如果N有两个子结点,将右子节点的最小结点拿过来替代N.
    
    * AVL Tree: 对于任意结点左子树和右子树的高度最多只能相差1

    * Splay tree

    * Red-black tree

    * 2-3-4 tree

11. Sorting: merge sort, quick sort

12. Set

13. 字符串操作

14. Tries

15. Graph: graph traversal, representation, shortest path, minimum spanning tree

16.  B-trees

figlet
cowsay