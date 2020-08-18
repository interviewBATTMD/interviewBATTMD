# 二分查找
## 33-搜索旋转排序数组

# 快排 

## 4-寻找两个正序数组的中位数
https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xun-zhao-liang-ge-you-xu-shu-zu-de-zhong-wei-s-114/  
```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int totalLength = nums1.size() + nums2.size();
        if (totalLength % 2 == 1) {
            return getKthElement(nums1, nums2, (totalLength + 1) / 2);
        }
        else {
            return (getKthElement(nums1, nums2, totalLength / 2) + getKthElement(nums1, nums2, totalLength / 2 + 1)) / 2.0;
        }
    }

    int getKthElement(const vector<int>& nums1, cosnt vector<int>& nums2, int k) {
        int m = nums1.size();
        int n = nums2.size();
        int index1 = 0, index2 = 0;

        while (true) {
            // 边界情况
            if (index1 == m) {
                return nums2[index2 + k - 1];
            }
            if (index2 == n) {
                return nums1[index1 + k - 1];
            }
            if (k == 1) {
                return min(nums1[index], nums[index2]);
            }

            // 正常情况
            int newIndex1 = min(index1 + k / 2 - 1, m - 1);
            int newIndex2 = min(index2 + k / 2 - 1, n - 1);
            int pivot1 = nums1[newIndex1];
            int pivot2 = nums2[newIndex2];
            if (pivot1 <= pivot2) {
                k -= newIndex1 - index1 + 1;
                index1 = newIndex1 + 1;
            }
            else {
                k -= newIndex2 - index2 + 1;
                index2 = newIndex2 + 1;
            }
        }
    }
};
```

## 数组中的第k个最大元素
这个题用优先队列（堆排序也可以，时间复杂度O(nlogK)）  
快排的时间复杂度O(n)  
```
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        int result = 0;
        int numsSize = int(nums.size());
        if (numsSize == 0 || k > numsSize) {
            return 0;
        }
        int kMin = numsSize - k + 1;
        result = select(nums, 0, numsSize - 1, kMin);
        return result;
    }

    int select(vector<int>& nums, int left, int right, int target) {
        if (left == right) {
            return nums[left];
        }
        int cur = partition(nums, left, right);
        int currentResult = cut - left + 1;
        if (target == currentResult) {
            return nums[cut];
        }
        else if (target < currentResult) {
            return select(nums, cut + 1, right, target - currentResult);
        }
        return 0;
    }

    int partition(vector<int>& nums, int left, int right) {
        int cut = nums[right];
        int i = left;
        for (int j = left; j < right; j++) {
            if (nums[j] <= cut) {
                exchange(nums[i], nums[j]);
                i++;
            }
        }
        exchange(nums[i], nums[right]);
        return i;
    }

    void exchange(int& a, int& b) {
        int tmpInt = a;
        a = b;
        b = tmpInt;
        return;
    }
};
```

# dp
## 486-预测赢家
https://leetcode-cn.com/problems/predict-the-winner/solution/java-dong-tai-gui-hua-by-zxy0917-2/  
```
class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int i = 0; i < n; i++) {
            dp[i][i] = nums[i];
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
            }
        }
        return dp[0][n - 1] >= 0;
    }
};
```

## 72-编辑距离
https://leetcode-cn.com/problems/predict-the-winner/solution/java-dong-tai-gui-hua-by-zxy0917-2/  
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.length();
        int m = word2.length();

        if (n * m == 0) {
            return n + m;
        }

        int dp[n + 1][m + 1];

        for (int i = 0; i < n + 1; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j < m + 1; j++) {
            dp[0][j] = j;
        }

        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < m + 1; j++) {
                int left = dp[i - 1][j] + 1;
                int down = dp[i][j - 1] + 1;
                int left_down = dp[i - 1][j - 1];
                if (word1[i - 1] != word2[j - 1]) left_down += 1;
                dp[i][j] = min(left, min(down, left_down));
            }
        }
        return dp[n][m];
    }
};
```

## 1143-最长公共子序列
```
class Solution {
public:
    int longestCommonSubsequence(string text1,string text2) {
        int len1 = text1.size(), len2 = text2.size();
        vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1));
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                }
                else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[len1][len2];
    }
};
```

# 回溯
## 46-全排列
```
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        backtrack(res, nums, 0, nums.size());
        return res;
    }

    void backtrack(vector<vector<int>>& res, vector<int>& output, int first, int len) {
        if (first == len) {
            res.emplace_back(output);
            return;
        }
        for (int i = first, i < len; ++i) {
            swap(output[i], output[first]);
            backtrack(res, output, first + 1, len);
            swap(output[i], output[first]);
        }
    }
};
```

# 字典树
## 208-实现Trie
```
class Trie {
private:
    bool isEnd;
    Trie* next[26];
public:
    Trie() {
        isEnd = false;
        memset(next, 0, sizeof(next));
    }

    void insert(string word) {
        Trie* node = this;
        for (char c : word) {
            if (node->next[c - 'a'] == nullptr) {
                node->next[c - 'a'] = new Trie();
            }
            node = node->next[c - 'a'];
        }
        node->isEnd = true;
    }

    bool search(string word) {
        Trie* node = this;
        for (char c : word) {
            if (node->next[c - 'a'] == nullptr) {
                return false;
            }
            node = node->next[c - 'a'];
        }
        return node->isEnd;
    }

    bool startsWith(string prefix) {
        Trie* node = this;
        for (char c : prefix) {
            node = node->next[c - 'a'];
            if (node == nullptr) {
                return false;
            }
        }
        return true;
    }
};
```

