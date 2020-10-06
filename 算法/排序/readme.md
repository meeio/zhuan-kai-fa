

### 快速排序 C++ 实现

```c++
int partition(int arr[], int low, int high) {
    int pivot = arr[high];  // taking the last element as pivot
    int i = (low - 1);      // Index of smaller element

    for (int j = low; j < high; j++) {
        // If current element is smaller than or
        // equal to pivot
        if (arr[j] <= pivot) {
            i++;  // increment index of smaller element
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    return (i + 1);
}

/**
 *      The main function that implements QuickSort
 *      arr[] --> Array to be sorted,
 *      low --> Starting index,
 *      high --> Ending index
 */
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int p = partition(arr, low, high);
        quickSort(arr, low, p - 1);
        quickSort(arr, p + 1, high);
    }
}

```