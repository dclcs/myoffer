## Q1: B+树，B树和红黑树区别
- B树
  - M阶B树结点最多含有m棵子树，m-1个关键字（存数据，空间）
  - 除根节点外，其他每个结点至少有m/2个子结点
  - 若根节点不是叶结点，则至少有两颗子树
- B+树
  - 叶子节点用指针连接在一起形成双向链表
  - 非叶子节点仅用作索引，叶子节点存储数据，非叶子节点和叶子节点有重复数据
  - B+树中只有叶子节点会带有指向记录的指针（ROWID），而B树则所有节点都带有，在内部节点出现的索引项不会再出现在叶子节点中。
  - B+树中所有叶子节点都是通过指针连接在一起，而B树不会。
- 红黑树
  - 


## Q2: 快排
```c++
int partition(vector<int>& nums, int l, int r){
    int pivot = nums[r];
    int i = l - 1;
    for(int j = l; j <= r - 1; j ++){
        if(nums[j] <= pivot){
            i = i + 1;
            swap(nums[i], nums[j]);
        }
    }
    swap(nums[i + 1], nums[r]);
    return i + 1;
}


int randomized_partition(vector<int>& nums, int l , int r){
    int i = rand() % (r - l + 1) + l; // i [l, r]
    swap(nums[r], nums[i]);
    return partition(nums, l, r);
}

void randomized_quicksort(vector<int>& nums, int l , int r){
    if(l < r){
        int pos = randomized_partition(nums, l, r);
        randomized_quicksort(nums, 1, pos - 1);
        randomized_quicksort(nums, pos + 1, r);
    }
}

/**
 * @attention randomized_quicksort(nums, l, r)对数组里的[l,r]进行排序，
 *          每次首先调用randomized_partition对nums进行划分，返回分界下标
 */
vector<int> quick_sort(vector<int>& arr){
    srand((unsigned) time(NULL));
    randomized_quicksort(arr, 0 , arr.size() - 1);
    return arr;
}
```


## Q3: 堆排

```c++
void max_heapify(vector<int>& nums, int i , int len){
    for (; (i << 1) + 1 <= len;) {
        int lson = (i << 1) + 1;
        int rson = (i << 1) + 2;
        int large;
        if (lson <= len && nums[lson] > nums[i]) {
            large = lson;
        }
        else {
            large = i;
        }
        if (rson <= len && nums[rson] > nums[large]) {
            large = rson;
        }
        if (large != i) {
            swap(nums[i], nums[large]);
            i = large;
        }
        else break;
    }
}

void build_max_heap(vector<int>& nums, int len){

    for (int i = len / 2; i >= 0; --i) {
        max_heapify(nums, i, len);
    }
}

void heap_sort(vector<int>& nums) {

    int len = (int)nums.size() - 1;
    build_max_heap(nums, len);
    for (int i = len; i >= 1; --i) {
        swap(nums[i], nums[0]);
        len -= 1;
        max_heapify(nums, 0, len);
    }
}
```
## Q4: 各种排序方法比较


|排序方法	|平均时间|最好时间	|最坏时间|
|:------:|-------|--------- |------|
|桶排序(不稳定)|	O(n)|	O(n)|	O(n)|
|基数排序(稳定)|	O(n)|	O(n)|	O(n)|
|归并排序(稳定)|	O(nlogn)|	O(nlogn)|	O(nlogn)|
|快速排序(不稳定)|	O(nlogn)|	O(nlogn)|	O(n^2) |
|堆排序(不稳定)|	O(nlogn)|	O(nlogn)|	O(nlogn)|
|希尔排序(不稳定)|	O(n^1.25)|||	 	 
|冒泡排序(稳定)|	O(n^2)|	O(n)|	O(n^2)|
|选择排序(不稳定)|	O(n^2)|	O(n^2)|	O(n^2)|
|直接插入排序(稳定)|	O(n^2)|	O(n)|	O(n^2)|
- 大文件排序
- [0,5]生成随机数相等，求[0,7]生成随机数
- 哈希表，引申出红黑树，二叉搜索树


## Q5: 判断完全二叉树
[LINK](https://blog.csdn.net/Number_0_0/article/details/76177479)

## Q6: 旋转的排序数组
[LINK](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/sou-suo-xuan-zhuan-pai-xu-shu-zu-by-leetcode-solut/)


## Q7 : 100亿数据怎么找到最大的k个值(k小顶堆)；
-  先拿10000个数建堆，然后一次添加剩余元素，如果大于堆顶的数（10000中最小的），将这个数替换堆顶，并调整结构使之仍然是一个最小堆，这样，遍历完后，堆中的10000个数就是所需的最大的10000个。建堆时间复杂度是O（mlogm），算法的时间复杂度为O（nmlogm）（n为10亿，m为10000）。
  
[LINK](https://blog.csdn.net/zyq522376829/article/details/47686867)

## Q8: 实现hash map(包括插入，查找，排序)；

## Q9: 合并k个有序数组。（只有第三个问题写了代码）
[LINK](https://leetcode-cn.com/problems/merge-k-sorted-lists/solution/he-bing-kge-pai-xu-lian-biao-by-leetcode-solutio-2/)

## Q10: 反转链表
[LINK](https://leetcode-cn.com/problems/reverse-linked-list/solution/206-fan-zhuan-lian-biao-shuang-zhi-zhen-fa-di-gui-/)

## Q11: 最大子串leecode3 
[LINK](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

## Q12: leetcode上面那道三角形中等题
[LINK](https://leetcode-cn.com/problemset/all/?search=%E4%B8%89%E8%A7%92%E5%BD%A2)

## Q13: 给定升序数组，返回第一个大于或者等于value元素的下标
[LINK](https://leetcode-cn.com/problems/search-insert-position/)


## Q14: 给定一棵二叉树，判断其是否是对称的
[LINK](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

## Q15: 判断数组中是否有重复的数


## Q16：无序数组求中位数, 最长回文子串
[LINK](https://zhuanlan.zhihu.com/p/65632898)


## Q17: 数组循环左(右)移
[LINK](https://zhuanlan.zhihu.com/p/54357266)

## Q18: 有序数组中的缺失元素
[LINK](https://leetcode-cn.com/problems/missing-element-in-sorted-array/solution/c-er-fen-cha-zhao-1060-you-xu-shu-zu-zhong-de-que-/)

## Q19: 乱序数组1-1000少一个数，找出来，少两个数，找出来。


## Q20: 设计一个能得到最大值的栈
[LINK](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/solution/mian-shi-ti-30-bao-han-minhan-shu-de-zhan-fu-zhu-z/)

一串很长的二进制数据，不能全部放在内存中，也不能转为10进制数，算 %5的值，想到了动态规划的算法，但是提醒说可以用状态机（求大佬告知）
定义一个链表的结构体
不用C++的STL库，实现一个栈的push函数和pop函数，用定义好的push和pop函数，两个栈实现一个队列（力扣原题啊，我做过，但是还是用python写的，C++不会。。。）
给你一个大小为一千万的乱序数组，找出最大的10个数（top K问题吧，只知道用小根堆，说不出来原理）

请问说一下什么是平衡二叉树，满二叉树，完全二叉树的特点？深度为n的平衡二叉树，他有多少个节点（包括根节点），每一层的节点数为多少？
图和树的区别是啥




（实现）
算法：rand7()实现rand12()

40亿不同数据，找到某个是否存在，内存多大

打家劫舍3
最大连续子列积
链表倒数第k个节点
自己构造有环链表，判断链表是否有环
非递归先序遍历，遍历的时间和空间复杂度
跳台阶
数组只有0-3的数字，O（n）时间排序，空间随便用
序列化、反序列化一个二叉搜索树
二叉树的层次遍历
稳定排序以及堆插入
