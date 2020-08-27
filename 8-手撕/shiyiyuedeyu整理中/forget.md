42-接雨水  
```
int trap(vector<int>& height) {
    int l = 0, r = height.size() - 1, level = 0, water = 0;
    while (l < r) {
        lower = height[height[l] < height[r] ? l++ : r--];
        level = max(level, lower);
        water += level - lower;
    }
    return water;
}
```

503-下一个更大元素II
```
class Solution {
public:
    vector<int> nextGreaterElements2(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n);
        stack<int> s;
        for (int i = 2 * n - 1; i >= 0; i--) {
            while (!s.epmty() && s.top() <= nums[i % n]) {
                s.pop();
            }
            res[i % n] = s.empty() ? -1 : s.top();
            s.push(nums[i % n]);
        }
        return res;
    }
};
```

31-下一个排列
```
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n);
        stack<int> s;
        for (int i = n - 1; i >= 0; i--) {
            while (!s.empty() && nums[i] > s.top()) {
                s.pop();
            }
            res[i] = s.empty() ? -1 : s.top();
            s.push(nums[i]);
        }
        return res;
    }
};
```

46-全排列
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
        for (int i = first, i < len; i++) {
            swap(output[i], output[first]);
            backtrack(res, output, first + 1, len);
            swap(output[i], output[first]);
        }
    }
};
```

## 32-最长有效括号
```
class Solution {
public:
    int longestValidParentheses(string s) {
        int maxans = 0, n = s.size();
        vector<int> dp(n, 0);
        for (int i = 1; i < n; i++) {
            if (s[i] == ')') {
                if (s[i - 1] == '(') {
                    dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
                } else if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] == '(') {
                    dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
                }
                maxans = max(maxans, dp[i]);
            }
        }
        return maxans;
    }
};
```

## 215-数组中的第K个最大元素
```
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        srand(time(0));
        return quickSelect(nums, 0 ,nums.size() - 1, nums.size() - k);
    }
    int quickSelect(vector<int>& a, int l, int r, int index) {
        int q = randomPartition(a, l ,r);
        if (q = index) {
            return a[q];
        }
        else {
            return q < index ? quickSelect(a, q + 1, r, index) : quickSelect(a, l, q - 1, index);
        }
    }
    int randomPartition(vector<int>& a, int l, int r) {
        int i = rand() % (r - l + 1) + 1;
        swap(a[i], a[r]);
        return partition(a, l, r);
    }

    int partition(vector<int>& a, int l, int r) {
        int x = a[r], i = l - 1;
        for (int j = 1; j < r; ++j) {
            if (a[j] <= x) {
                swap(a[++i], a[j]);
            }
        }
        swap(a[i + 1], a[r]);
        return i + 1;
    }
};
```

## 110-平衡二叉树
// 自底向上
class Solution {
public:
    int height(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        int leftHeight = height(root->left);
        int rightHeight = height(root->right);
        if (leftHeight == -1 || rightHeight == -1 || abs(leftHeight - rightHeight) > 1) {
            return -1;
        } else {
            return max(leftHeight, rightHeight) + 1;
        }
    }

    bool isBalanced(TreeNode* root) {
        return height(root) >= 0;
    }
};


## 99-恢复二叉搜索树
class Solution {
public:
    void inorder(TreeNode* root, vector<int>& nums) {
        if (root == nullptr) {
            return;
        }
        inorder(root->left, nums);
        nums.push_back(root->val);
        inorder(root->right, nums);
    }

    pair<int, int> findTwoSwapped(vector<int>& nums) {
        int n = nums.size();
        int x = -1, y = -1;
        for (int i = 0; i < n - 1; ++i) {
            if (nums[i + 1] < nums[i]) {
                y = nums[i + 1];
                if (x == -1) {
                    x = nums[i];
                }
                else break;
            }
        }
        return {}
    }

    void recoverTree(TreeNode* root) {
        vector<int> nums;
        inorder(root, nums);
        pair<int, int> swapped = findTwoSwapped(nums);
    }
};

// 判断二叉排序树
class Solution {
public:
    bool Tree(TreeNode* root) {
        return TreeCore(root, INT_MIN, INT_MAX);
    }

    bool TreeCore(TreeNode* root, int left, int right) {
        if (!root) {
            return true;
        }
        if (root->val > right || root->val < left) {
            return false;
        } 
        return TreeCore(root->left, left, root->val) && TreeCore(root->right, root->val, right);
    }
};

