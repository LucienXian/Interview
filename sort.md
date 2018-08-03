# Sort algorithms

## 冒泡排序

每次将最大的放在最后，代码：

```C++
void bubbleSort(vector<int>& arr)
{
    for (int i = 0; i < arr.size()-1; i++) {
        for (int j = 0; j < arr.size()-1-i; j++) {
            if (arr[j] > arr[j+1])
                swap(arr[j], arr[j+1]);
        }
    }
}
```

## 选择排序

整体比较，相对于冒泡排序，选择排序需要比较选择最小的放在前面，减少了交换次数。代码：

```C++
void selection_sort(vector<int>& arr)
{
    for (int i = 0; i < arr.size(); i++) {
        int min_idx = i;
        for (int j = i+1; j < arr.size(); j++) {
            if (arr[min_idx] > arr[j])
                min_idx = j;
        }
        swap(arr[min_idx], arr[i]);
    }
}
```

## 插入排序

插入排序类似于玩扑克牌，拿到一张牌，就把它插入到大小合适的位置，并保证其前面的牌已经按顺序排列好。代码：

```C++
void insertion_sort(vector<int>& arr)
{
    int j;
    for (int i = 1; i < arr.size(); i++){
        int tmp = arr[i];
        j = i-1;
        while (j>=0&&tmp<arr[j]) {
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = tmp; 
    }
}
```

## 快速排序

来自于冒泡排序的思想，通过与基准比较，将小数排到一边，大数排到另外一遍。代码：

```C++
int partion(vector<int>& arr, int low, int high) 
{
    int pivot = arr[low];
    int pivotind = low;
    while (low < high) {
        while (low < high && arr[high] >= pivot) high--;
        while (low < high && arr[low] <= pivot) low++;
        swap(arr[low], arr[high]);
    }
    swap(arr[low], arr[pivotind]);
    return low;
}

void quick_sort(vector<int>& arr, int low, int high)
{
    if (low >= high) return;
    int pi = partion(arr, low, high);
    quick_sort(arr, low, pi-1);
    quick_sort(arr, pi+1, high);

}
```

##　归并排序

基于分而治之的思想，先递归划分成子问题，然后合并结果。简单来说就是先两两合并有序序列，然后再四四合并。。。

```C++
void merge(vector<int>& arr, int l, int m, int r)
{
    int i, j, k;
    int n1 = m - l + 1;
    int n2 = r - m;
    int lv[n1], rv[n2];
    for (i = 0; i < n1; i++)
        lv[i] = arr[l+i];
    for (j = 0; j < n2; j++)
        rv[j] = arr[m+1+j];
    
    k = l;
    i = j = 0;
    while (i < n1 && j < n2) {
        if (lv[i]<=rv[j])
            arr[k] = lv[i++];
        else
            arr[k] = rv[j++];
        k++;
    }

    while (i < n1) {
        arr[k] = lv[i++];
        k++;
    }
    while (j < n2) {
        arr[k] = rv[j++];
        k++;
    }
}

void merge_sort(vector<int>& arr, int l, int r)
{
    int m = l + (r - l) / 2;
    if (l < r) {
        merge_sort(arr, l, m);
        merge_sort(arr, m+1, r);
        merge(arr, l, m, r);
    }
}
```

## 堆排序

借助堆来实现选择排序，升序就用大顶堆，降序就用小顶堆。将有序数列建成堆，从第一个非叶元素开始依次建堆；调整成堆，每次将堆顶元素和最后一个元素交换，并调整堆。

```C++
void heapify(vector<int>& arr, int n, int root)
{
    int largest = root;
    int l = root * 2 + 1;
    int r = root * 2 + 2;
    if (l < n && arr[l]>arr[largest]) largest = l;
    if (r < n && arr[r]>arr[largest]) largest = r;

    if (largest != root) {
        swap(arr[largest], arr[root]);
        heapify(arr, n, largest);
    }
}

void heap_sort(vector<int>& arr, int n)
{
    for (int i = n / 2 - 1; i >=0; i--) {
        heapify(arr, n, i);
    }

    for (int i = n-1; i >= 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```