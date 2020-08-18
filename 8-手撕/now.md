## 25-K个一组翻转链表
```
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        int count = 0;
        ListNode* move = head;
        while (move) {
            count++;
            move = move->next;
        }
        count = count / k;
        ListNode* prehead = new ListNode(0);
        move = prehead;
        ListNode* last_reverse_node;
        for (int i = 0; i < count; i++) {
            last_reverse_node = head;
            for (int j = 0; j < k; j++) {
                ListNode* tmp = move->next;
                move->next = head;
                head = head->next;
                move->next->next = tmp;
            }
            move = last_reverse_node;
        }
        move->next = head ? head : nullptr;
        return prehead->next;
    }
};
```

## 121-买卖股票的最佳时机
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minprice = INT_MAX, maxprofit = 0;
        for (int price : prices) {
            maxprofit = max(maxprofit, price - minprice);
            minprice = min(price, minprice);
        }
        return maxprofit;
    }
};
```

## 15-三数之和
```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        if (nums.size() < 3) {
            return res;
        }
        if (nums[i] > 0) {
            return res;
        }
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size() - 2; i++) {
            if (i != 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum > 0) {
                    right--;
                }
                else if (sum < 0) {
                    left++;
                }
                else {
                    res.push_back({i, left++, right--});
                }
            }
        }
        return res;
    }
};
```

## 155-最小栈
```
class MinStack {
private:
    stack<int> values;
    stack<int> mins;
public:
    MinStack() {
    }

    void push(int x) {
        if (values.empty()) {
            mins.push(x);
        }
        else {
            if (mins.top() >= x) {
                mins.push(mins.top());
            }
        }
        values.push(x);
    }

    void pop() {
        if (values.empty()) {
            return;
        }
        values.pop();
        mins.pop();
    }

    int top() {
        return values.top();
    }

    int getMin() {
        return mins.top();
    }
};
```

## 124-二叉树中的最大路径和
```
class Solution {
private:
    int ans;
public:
    int maxPathSum(TreeNode* root) {
        ans = INT_MIN;
        oneSideMax(root);
        return ans;
    }

    int oneSideMax(TreeNode* root) {
        if (!root) {
            return 0;
        }
        int left = max(0, oneSideMax(root->left));
        int right = max(0, oneSideMax(root->right));
        ans = max(ans, left + right + root->val);
        return max(left, right) + root->val;
    }
};
```

## 199-二叉树的右视图
```
class Solution {
public:
    vector<int> youshitu(TreeNode* root) {
        vector<int> res;
        if (!root) {
            return res;
        }
        vector<TreeNode*> vec;
        vec.push_back(root);
        res.push_back(root->val);
        int start = 0, end = 1;
        while (start < end) {
            while (start < end) {
                if (vec[start]->left) {
                    vec.push_back(vec[start]->left);
                }
                if (vec[start]->right) {
                    vec.push_back(vec[start]->right);
                }
                start++;
            }
            if (vec.size() > end) {
                res.push_back(vec.back()->val);
                end = vec.size();
            }
        }
        return res;
    }
};
```

## 3-无重复字符的最长子串

## 88-合并两个有序数组
```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int count = m + n - 1;
        while (m - 1 >= 0 && n - 1 >= 0) {
            if (nums2[n - 1] >= nums1[m - 1]) {
                nums1[count--] = nums2[n - 1];
                n--;
            }
            else {
                nums1[count--] = nums1[m - 1];
                m--;
            }
        }
        while (n - 1 >= 0) {
            nums1[count--] = nums2[n - 1];
            n--;
        }
        return;
    }
};
```

## 108-将有序数组转换为二叉搜索树
```
class Solution {
public:
    TreeNode* turnTree(vector<int>& nums) {
        return turnTreeCore(nums, 0, nums.size() - 1);
    }

    TreeNode* turnTreeCore(vector<int>& nums, int start, int end) {
        if (start > end) {
            return nullptr;
        }
        int mid = (start + end) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = turnTreeCore(nums, start, mid - 1);
        root->right = turnTreeCore(nums, mid + 1, end);
        return root;
    }
};
```

## 110-平衡二叉树
```
class Solution {
public:
    bool isTree(TreeNode* root) {
        if (!root) {
            return true;
        }
        int left = isTreeCore(root->left);
        int right = isTreeCore(root->right);
        if (left - right > 1 || right - left > 1) {
            return false;
        }
        return isTree(root->left) && isTree(root->right);
    }
    int isTreeCore(TreeNode* root) {
        if (!root) {
            return 0;
        }
        return 1 + max(isTreeCore(root->left), isTreeCore(root->right));
    }
};
```

## 236-二叉树的最近公共祖先
```
class Solution {
public:
    TreeNode* ancestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (p == root || q == root) {
            return root;
        }
        TreeNode* left = ancestor(root->left, p, q);
        TreeNode* right = ancestor(root->right, p, q);
        return left ? (right ? root : left) : (right ? right : nullptr);
    }
};
```

## 33-搜索旋转排序数组

## 322-零钱兑换
```
class Solution {
public:
    int mincoins(vector<int>& coins, int acount) {
        vector<int> dp(acount + 1, INT_MAX);
        for (int i = 0; i < coins.size(); i++) {
            dp[coins[i]] = 1;
        }
        for (int i = 1; i <= acount; i++) {
            for (int j = 0; j < coins.size(); j++) {
                if (i > coins[j]) {
                    dp[i] = min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[acount];
    } 
};
```

## 83-删除排序链表中的重复元素
```

```

## 206-反转链表
```
class Solution {
public:
    ListNode* reverse(ListNode* head) {
        ListNode* out = nullptr;
        while (head) {
            ListNode* tmp = out;
            out = head;
            head = head->next;
            out->next = tmp;
        }
        return out;
    }
};
```

## 215-数组中的第K个最大元素

## 56-合并区间
```
class Solution {
public:
    vector<vector<int>> hebing(vector<vector<int>>& qujian) {
        if (qujian.size() < 2) {
            return qujian;
        }
        sort(qujian.begin(), qujian.end());
        vector<vector<int>> res;
        res.push_back(qujian[0]);
        for (int i = 1; i < qujian.size(); i++) {
            if (qujian[i][0] > res.back()[1]) {
                res.push_back(qujian[i]);
            }
            else {
                res.back()[1] = max(qujian[i][1], res.back()[1]);
            }
        }
        return res;
    }
};
```

## 146-LRU缓存机制
```
class LRUCache {
private:
    int cap;
    list<pair<int, int>> caches;
    unordered_map<int, list<pair<int, int>>::iterator> maps;
public:
    LRUCache(int x) {
        cap = x;
    }

    void put(int x, int y) {
        auto iter = maps.find(x);
        if (iter != maps.end()) {
            caches.erase(iter->second);
            caches.push_front(make_pair(x, y));
            maps[x] = caches.begin();
            return;
        }
        if (maps.size() == cap) {
            auto last = caches.back();
            auto last2 = maps.find(last.first);
            caches.pop_back();
            maps.erase(last2);
        }
        caches.push_front(make_pair(x, y));
        maps[x] = caches.begin();
    }

    int get(int x) {
        auto iter = maps.find(x);
        if (iter != maps.end()) {
            int y = iter->second->second;
            caches.erase(iter->second);
            caches.push_front(make_pair(x, y));
            maps[x] = caches.begin();
            return y;
        }
        return -1;
    }
};
```

## 102-二叉树的层序遍历
```
class Solution {
public:
    vector<vector<int>> levelTraversal(TreeNode* root) {
        vector<vector<int>> res;
        vector<int> level;
        if (!root) {
            return res;
        }
        vector<TreeNode*> vec;
        vec.push_back(root);
        int start = 0, end = 1;
        while (start < end) {
            level.clear();
            while (start < end) {
                level.push_bakc(vec[start]->val);
                if (vec[start]->left) {
                    vec.push_back(vec[start]->left);
                }
                if (vec[start]->right) {
                    vec.push_back(vec[start]->right);
                }
                start++;
            }
            end = vec.size();
            res.push_back(level);
        }
        return res;
    }
};
```

## 518-零钱兑换II
```
class Solution {
public:
    int change(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, INT_MAX);
        for (int i = 0; i < coins.size(); i++) {
            dp[coins[i]] = 1;
        }
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.size(); j++) {
                if (coins[j] < i && dp[i - coins[j]] != INT_MAX) {
                    dp[i] += dp[i - coins[j]];
                }
            }
        }
        return dp[amount];
    }
};
```

## 54-螺旋矩阵

## 1299-将每个元素替换为右侧最大元素
```
class Solution {
    void replace(vector<int>& nums) {
        int cur = -1;
        for (int i = nums.size() - 1; i >= 0; i--) {
            int tmp = nums[i];
            nums[i] = cur;
            cur = max(cur, tmp);
        }
        return;
    }
};
```

## 42-接雨水
```
```

## 105-从前序与中序遍历序列构造二叉树
```
class Solution {
public:
    TreeNode* Tree(vector<int>& preorder, vector<int>& inorder) {
        return Core(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size());
    }

    TreeNode* Core(vector<int>& preorder, int start1, int end1, vector<int>& inorder, int start2, int end2) {
        if (start1 > end1 || start2 > end2) {
            return nullptr;
        }
        TreeNode* root = new TreeNode(preorder[start1]);
        int mid = 0;
        for (int i = start2; i <= end2; i++) {
            if (inorder[i] == preorder[start1]) {
                mid = i;
                break;
            }
        }
        int half_len = mid - start2;
        root->left = Core(preorder, start1 + 1, start + half_len, inorder, start2, mid - 1);
        root->right = Core(preorder, start + half_len + 1, end1, inorder, mid + 1, end2);
        return root;
    }
};
```

## 160-相交链表
```
class Solution {
public:
    ListNode* node(ListNode* l1, ListNode* l2) {
        
    }
};
```

## 139-单词拆分
```

```

## 67-二进制求和
```
class Solution {
public:
    string addBinary(string a, string b) {
        string sum;
        int count1 = a.size(), count2 = b.size();
        int curry = 0;
        while (count1 && count2) {
            int ans = a[count1 - 1] - '0' + b[count2 - 1] - '0' + curry;
            sum = (char)('0' + ans % 2) + sum;
            curry = ans / 2;
            count1--;
            count2--;
        }
        while (count2) {
            int ans = b[count2 - 1] - '0' + curry;
            sum = (char)('0' + ans % 2) + sum;
            curry = ans / 2;
            count2--;
        }
        while (count1) {
            int ans = a[count1 - 1] - '0' + curry;
            sum = (char)('0' + ans % 2) + sum;
            curry = ans / 2;
            count1--;
        }
        if (curry) {
            sum = '1' + sum;
        }
        return sum;
    }
};
```

## 230-二叉搜索树中第K小的元素
```
class Solution {
public:
    int Kth(TreeNode* root) {
        int count = 0;
        if (!root) {
            return -1;
        }
        stack<TreeNode*> stk;
        TreeNode* cur = root;
        while (cur || !stk.empty()) {
            while (cur) {
                stk.push(cur);
                cur = cur->left;
            }
            cur = stk.top();
            stk.pop();
            if (++count == k) {
                return cur->val;
            }
            cur = cur->right;
        }
        return -1;
    }
};
```

## 70-爬楼梯
```
class Solution {
public:
    int stairs(int n) {
        if (n == 1) {
            return 1;
        }
        if (n == 2) {
            return 2;
        }
        int post = 1;
        int cur = 2;
        while (n-- > 2) {
            int tmp = post + cur;
            post = cur;
            cur = tmp;
        }
        return cur;
    }
};
```

## 543-二叉树的直径
```
class Solution {
private:
    int ans;
public:
    int length(TreeNode* root) {
        ans = 0;
        Core(root);
        return ans;
    }

    int Core(TreeNode* root) {
        if (!root) {
            return 0;
        }
        int left = Core(root->left);
        int right = Core(root->right);
        ans = max(ans, left + right + 1);
        return 1 + max(left, right);
    }
};
```

## 112-路径总和
```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum, int res = 0) {
        if (!root) {
            return false;
        }
        res += root->val;
        if (!root->left && !root->right) {
            return res == sum;
        }
        bool sumleft = false;
        bool sumright = false;
        if (root->left) {
            sumleft = hasPathSum(root->left, sum, res);
        }
        if (root->right) {
            sumleft = hasPathSum(root->right, sum, res);
        }
        return sumleft || sumright;
    }
};
```

## 23-合并K个排序链表
```
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* prehead = new ListNode(0), *move = prehead;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                move->next = l1;
                l1 = l1->next;
            }
            else {
                move->next = l2;
                l2 = l2->next;
            }
            move = move->next;
        }

        move->next = l1 ? l1 : l2;
        ListNode* res = prehead->next;
        delete prehead;
        return res;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.size() == 0) {
            return nullptr;
        }
        if (lists.size() == 1) {
            return lists[0];
        }
        int interval = 1;
        while (interval < list.size()) {
            for (int i = 0; i < list.size(); i += 2 * interval) {
                mergeTwoLists(lists[i], lists[i + interval]);
            }
            interval *= 2;
        }
        return lists[0];
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

## 141-环形链表
```
class Solution {
    bool loop(ListNode* head) {
        if (!head) {
            return false;
        }
        ListNode* slow = head, *fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                return true;
            }
        }
        return false;
    }
};
```

## 2-两数相加
```
class Solution {
public:
    ListNode* addNodes(ListNode* l1, ListNode* l2) {
        ListNode* prehead = new ListNode(0), *move = prehead;
        int curry = 0;
        while (l1 || l2 || curry) {
            int tmp = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + curry;
            move->next = new ListNode(tmp % 10);
            move = move->next;
            curry = tmp / 10;
        }
        ListNode* res = prehead->next;
        delete prehead;
        return res;
    }
};
```

## 31-下一个排列

## 503-下一个更大元素II
```
class Solution {
public:
    vector<int> nextGreaterEle(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n);
        stack<int> s;
        for (int i = 2 * n - 1; i >= 0; i--) {
            while (!s.empty() && s.top() <= nums[i % n]) {
                s.pop();
            }
            res[i % n] = s.empty() ? -1 : s.top();
            s.push(nums[i % n]);
        }
        return res;
    }
};
```

## 221-最大正方形
```
class Solution {
public:
    int square(vector<vector<char>>& board) {
        if (board.size() == 0 || board[0].size() == 0) {
            return 0;
        }
        vector<vector<int>> dp(board.size(), vector<int>(board[0].size()));
        int maxLen = 0;
        for (int i = 0; i < board.size(); i++) {
            dp[i][0] = board[i][0] == '1' ? 1 : 0;
            maxLen = max(dp[i][0], maxLen);
        }
        for (int i = 0; i < board[0].size(); i++) {
            dp[0][i] = board[0][j] == '1' ? 1 : 0;
            maxLen = max(dp[i][0], maxLen);
        }
        for (int i = 1; i < board.size(); i++) {
            for (int j = 1; j < board[0].size(); j++) {
                if (board[i][j] == '1') {
                    dp[i][j] = min(dp[i - 1][j - 1], min(dp[i - 1][j], dp[i][j - 1])) + 1;
                    maxLen = max(maxLen, dp[i][j]);
                }
            }
        }
        return maxLen * maxLen;
    }
};
```

## 128-最长连续序列
```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.size() < 2) {
            return nums.size();
        }
        unordered_set<int> s(nums.begin(), nums.end());
        int res = 1;
        for (int num : s) {
            if (s.count(num - 1) != 0) {
                continue;
            }
            int len = 1;
            while (s.count(num + 1) != 0) {
                len++;
                num++;
            }
            res = max(res, len);
        }
        return res;
    }
};
```