# 技巧型：
1、多多鸡有N个魔术盒子（编号1～N），其中编号为i的盒子里有i个球。多多鸡让皮皮虾每次选择一个数字X（1 <= X <= N），多多鸡就会把球数量大于等于X个的盒子里的球减少X个。通过观察，皮皮虾已经掌握了其中的奥秘，并且发现只要通过一定的操作顺序，可以用最少的次数将所有盒子里的球变没。那么请问聪明的你，是否已经知道了应该如何操作呢？  
```
/* 
要用最少的次数把所有盒子减到0，第一次必然是减少中间盒子的球数
比如 1，2，3，4，5， 第一次减3 得到1，2，0，1，2 ,这时我们可以看到左右两边相等的，分冶求解
*/
int cal(int n) {
    if (n == 1) {
        return 1;
    }
    if (n == 2) {
        return 2;
    }
    else return 1 + cal(n / 2);
}
```
极坐标解法：https://blog.csdn.net/qq_41855420/article/details/88978033  

2、给你一个圆的圆心（0，0）半径为R，实现给出一个在圆内的一个随机点  
```
class Solution {
public:
    double rad, xc, yc;
    
    Solution(double radius, double x_center, double y_centor) {
        rad = radius;
        xc = x_center;
        yc = y_center;
    }

    vector<double> randPoint() {
        while (true) {
            double x = 2 * (double)rand()/RAND_MAX - 1；
            double y = 2 * (double)rand()/RAND_MAX - 1;
            if (x * x + y * y <= 1) {
                return {xc + x * rad, yc + y * rad};
            }
        }
    }
};
```

# 归并排序
1、对100TB的数据进行排序？（拆分多个数据段进行排序，然后归并）需要归并多少次？分配给多个机器并行处理，应该怎么做？  


# 字典树
1、多多鸡打算造一本自己的电子字典，里面的所有单词都只由a和b组成。每个单词的组成里a的数量不能超过N个且b的数量不能超过M个。多多鸡的幸运数字是K，它打算把所有满足条件的单词里的字典序第K小的单词找出来，作为字典的封面。  
https://www.nowcoder.com/test/question/done?tid=36035419&qid=967827#summary  
```
/*
先使用字典树，然后先序遍历
*/
#include<iostream>
#include<string>
#include<map>
using namespace std;
map<pair<int, int>, unsigned long long> ma;

unsigned long long f(int m, int n) {
    if (ma.count({m, n})) {
        return ma[{m, n}];
    }
    if (!m) {
        ma[{m, n}] = n;
    }
    else if (!n) {
        ma[{m, n}] = m;
    }
    else {
        ma[{m, n}] = f(m - 1, n) + f(m, n - 1) + 2;
        return ma[{m, n}];
    }
}

int main() {
    long long k;
    int n, m;
    cin >> n >> m >> k;
    string cur = "a";
    n--;
    k--;
    while (k > 0 && (m | n)) {

    }
}
```

# 二分查找
https://www.nowcoder.com/test/question/done?tid=36035419&qid=967829#summary  
在一块长为n，宽为m的场地上，有n✖️m个1✖️1的单元格。每个单元格上的数字就是按照从1到n和1到m中的数的乘积。具体如下

n = 3, m = 3
1   2   3
2   4   6
3   6   9

给出一个查询的值k，求出按照这个方式列举的的数中第k大的值v。
例如上面的例子里，
从大到小为(9, 6, 6, 4, 3, 3, 2, 2, 1)
k = 1, v = 9
k = 2, v = 6
k = 3, v = 6
...
k = 8, v = 2
k = 9, v = 1

# DP
1、钱老板去国外度了个假，刚回到公司就收到了 n 封催促工作完成的邮件。每项工作都有完成截止日期 deadline，钱老板做每项工作都会花去cost天，而且不能中断。请你帮钱老板安排一下完成工作的顺序，以减少总的工作推迟时间。  
状态压缩DP  
https://www.nowcoder.com/test/question/done?tid=36036371&qid=956469#summary  


2、给定一个数组，每个元素范围是0~K（K < 整数最大值2^32），将该数组分成两部分，使得 |S1- S2|最小，其中S1和S2分别是数组两部分的元素之和。  
https://www.nowcoder.com/test/question/done?tid=36041245&qid=907635#summary  
```
// 01背包
#include<iostream>
#include<numeric>
using namespace std;

int main() {
    int n;
    cin >> n;
    long num[n];
    for (int i = 0; i < n; i++) {
        cin >> nums[i];
    }
    long sum = accumulate(num, num + n, 0);
    long half = sum / 2;

    long dp[half + 1] = {0};

    for (int i = 0; i < n; i++) {
        for (long j = half; j >= num[i]; j--) {
            dp[j] = max(dp[j], dp[j - num[i]] + num[i]);
        }
    }
    cout << sum - 2 * dp[half] << endl;
    return 0;
}
```

3、给定一个未排序数组,找出其中最长的等差数列(无需保证数字顺序)。  


4、给定一组石头，每个石头有一个正数的重量。每一轮开始的时候，选择两个石头一起碰撞，假定两个石头的重量为x，y，x<=y,碰撞结果为
1. 如果x==y，碰撞结果为两个石头消失
2. 如果x != y，碰撞结果两个石头消失，生成一个新的石头，新石头重量为y-x

最终最多剩下一个石头为结束。求解最小的剩余石头质量的可能性是多少。  
```
// 01背包问题 和问题2一模一样
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
class Solution {
public:
    int test(int n, vector<int> weights) {
        int sum = 0;
        for (int weight : weights) {
            sum += weight;
        }
        int maxCapcity = sum / 2;
        vector<int> dp(maxCapcity + 1, 0);
        for (int i = 0; i < n; i++) {
            for (int j = maxCapcity; j >= weights[i]; j--) {
                dp[j] = max(dp[j], dp[j - weights[i]] + weights[i]);
            }
        }
        return sum - 2 * dp[maxCapcity];
    }
};
```

5、小A很喜欢字母N，他认为连续的N串是他的幸运串。有一天小A看到了一个全部由大写字母组成的字符串，他被允许改变最多2个大写字母（也允许不改变或者只改变1个大写字母），使得字符串中所包含的最长的连续的N串的长度最长。你能帮助他吗？  
https://www.nowcoder.com/test/question/done?tid=36043984&qid=810018#summary  