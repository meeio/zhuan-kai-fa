

### 快速排序 C++ 实现

```c++
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