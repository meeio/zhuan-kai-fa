

### 快速排序 C++ 实现

> Ref: https://github.com/TheAlgorithms/C-Plus-Plus/blob/master/sorting/quick_sort.cpp

```cpp
/**
 *  This function takes last element as pivot, places
 *  the pivot element at its correct position in sorted
 *  array, and places all smaller (smaller than pivot)
 *  to left of pivot and all greater elements to right
 *  of pivot
 *
 */

int partition (int nums[], int low, int high){
    int pivot = nums[high];
    int small = low - 1;

    for (int pos = low; pos < high; pos++){
        if (nums[pos] <= pivot){
            small++;
            int temp = nums[pos];
            nums[pos] = nums[small];
            nums[small] = temp;
        }
    }

    nums[high] = nums[++small];
    nums[small] = pivot;

    return small;
}

/**
 *  The main function that implements QuickSort
 *  nums[] --> Array to be sorted,
 *  low --> Starting index,
 *  high --> Ending index
 */
void quick_sort(int nums[], int low, int high){
    if (low == high) return;

    int pivot_pos = partition(nums, low, high);

    quick_sort(nums, low, pivot_pos - 1);
    quick_sort(nums, pivot_pos + 1, high);
}

```

### 归并排序 C++ 实现

> Ref: https://github.com/TheAlgorithms/C-Plus-Plus/blob/master/sorting/merge_sort.cpp

```cpp
/**
 *
 * The merge() function is used for merging two halves.
 * The merge(arr, l, m, r) is key process that assumes that
 * arr[l..m] and arr[m+1..r] are sorted and merges the two
 * sorted sub-arrays into one.
 * 
 */
void merge(int *arr, int l, int m, int r) {
    int n1 = m - l + 1;
    int n2 = r - m;

    int* L = new int[n1];
    int* R = new int[n2];

    for (int i = 0; i < n1; i++) L[i] = arr[l + i];
    for (int i = 0; i < n2; i++) R[i] = arr[m + 1 + i];


    int i = 0;
    int j = 0;
    int k = 0;
    while (i < n1 || j < n2) {
        if (j >= n2 || (i < n1 && L[i] <= R[j]))
            arr[k++] = L[i++];
        else
            arr[k++] = L[j++];
        k++;
    }

    delete[] L;
    delete[] R;
}

/**
 * Merge sort is a divide and conquer algorithm, it divides the
 * input array into two halves and calls itself for the two halves
 * and then calls merge() to merge the two halves
 *
 */
void mergeSort(int *arr, int l, int r) {
    if (l < r) {
        int m = (l + r ) >> 1;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}
```