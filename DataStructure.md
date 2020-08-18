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