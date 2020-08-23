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