// 快排
```
class Solution {
public:
    void quickSort(vector<int>& nums) {
        if (nums.size() < 2) {
            return;
        }
        quickSortCore(nums, 0, nums.size() - 1);
    }

    void quickSortCore(vector<int>& nums, int start, int end) {
        if (start >= end) {
            return;
        }
        int j = partition(nums, start, end);
        quickSortCore(nums, start, j - 1);
        quickSortCore(nums, j + 1, end);
    }

    void partition(vector<int>& nums, int start, int end) {
        int cur = nums[start];
        int left = start + 1;
        int right = end;
        while (left < right) {
            while (nums[left] <= cur) {
                left++;
            }
            while (nums[right] >= cur) {
                right--;
            }
            if (left < right) {
                swap(nums[left], nums[right]);
            }
        }
        swap(nums[start], nums[right]);
        return right;
    }
};
```

// 归并排序
```
class Solution {
public:
    void MergeSort(vector<int>& nums) {
        if (nums.size() < 2) {
            return;
        }
        MergeSortCore(nums, 0, nums.size() - 1);
    }

    void MergeSortCore(vector<int>& nums, int start, int end) {
        if (start >= end) {
            return;
        }
        int j = (start + end) / 2;
        MergeSort(nums, start, j);
        MergeSort(arr, j + 1, end);
        Merge(arr, start, j, end);
    }

    void Merge(vector<int>& nums, int start, int j, int end) {
        int n = end - start + 1;
        vector<int> tmp(n);
        int i = 0;
        int left = start;
        int right = j + 1;
        while (left <= j && right <= end) {
            tmp[i++] = arr[left] <= arr[right] ? arr[left++] : arr[right++];
        }
        while (left <= j) {
            tmp[i++] = arr[left++];
        }
        while (right <= r) {
            tmp[i++] = arr[right++];
        }
        for (int t = 0; t < n; t++) {
            arr[start + t] = tmp[t];
        }
    }
};
```

// 堆排序
```
void HeapSort(vector<int>& nums) {
    int n = nums.size();
    for (int i = n / 2 - 1; i >= 0; i--) {
        MaxSort(nums, i, n);
    }
    for (int i = n - 1; i >= 1; i--) {
        swap(nums[i], nums[0]);
        MaxSort(nums, 0, i);
    }
}

void MaxSort(vector<int>& nums, int i, int n) {
    int j = 2 * i + 1; // 找到当前节点的左孩子
    int temp = nums[j]; // 把当前节点的数赋给temp变量
    while (j < n) {
        if (j + 1 < n && nums[j] < nums[j + 1]) {
            ++j;
        }
        if (temp > nums[j]) {
            break;
        }
        else {
            nums[i] = nums[j]; // 判断过程，把最大的孩子结点的数赋给父结点。并利用递归思想找出子节点的子节点。
            i = j;
            j = 2 * i + 1;
        }
    }
    a[i] = temp; //i已经成为了孩子结点的下标，赋值temp，也就是原本父结点的值，达成交换。
}
```