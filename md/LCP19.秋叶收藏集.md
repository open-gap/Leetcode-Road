# 秋叶收藏集 #  
`难度：中等` 

小扣出去秋游，途中收集了一些红叶和黄叶，他利用这些叶子初步整理了一份秋叶收藏集 `leaves`， 字符串 `leaves` 仅包含小写字符 `r` 和 `y`， 其中字符 `r` 表示一片红叶，字符 `y` 表示一片黄叶。  
出于美观整齐的考虑，小扣想要将收藏集中树叶的排列调整成「红、黄、红」三部分。每部分树叶数量可以不相等，但均需大于等于 **1**。每次调整操作，小扣可以将一片红叶替换成黄叶或者将一片黄叶替换成红叶。请问小扣最少需要多少次调整操作才能将秋叶收藏集调整完毕。  

**示例 1**:  
>**输入**: leaves = "rrryyyrryyyrr"  
>**输出**: 2  
>**解释**: 调整两次，将中间的两片红叶替换成黄叶，得到 "rrryyyyyyyyrr"  

**示例 2**:  
>**输入**: leaves = "ryr"  
>**输出**: 0  
>**解释**: 已符合要求，不需要额外操作  

**提示**：  
- `3 <= leaves.length <= 10^5`
- `leaves` 中只包含字符 `'r'` 和字符 `'y'`  

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/UlBDOe/  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  

---  
>解法一：动态规划，设置三个动态规划行向量，f[i][j]表示第i个位置树叶属于0、1、2(左、中、右)区域所需要的调整次数 (官方题解一)  

```C++  
class Solution {
public:
    int minimumOperations(string leaves) {
        int n = leaves.size();
        // 初始化动态规划数组，处理边界情况
        vector<vector<int>> f(n, vector<int>(3));
        f[0][0] = (leaves[0] == 'y');
        f[0][1] = f[0][2] = f[1][2] = INT_MAX;//边界条件赋大值
        // 遍历递推动态规划数组的值
        for(int i = 1; i < n; ++i)
        {
            int isRed = (leaves[i] == 'r'), isYellow = 1 - isRed;
            f[i][0] = f[i - 1][0] + isYellow;
            f[i][1] = min(f[i - 1][0], f[i - 1][1]) + isRed;
            if(i >= 2) f[i][2] = min(f[i - 1][1], f[i - 1][2]) + isYellow;
        }
        return f[n - 1][2];
    }
};
```  

**执行结果：**  
执行用时 : **836 ms** , 在所有 cpp 提交中击败了 **5.05%** 的用户  
内存消耗 : **111.5 MB** , 在所有 cpp 提交中击败了 **48.70%** 的用户  

---  
>解法二：前缀和数组+动态规划，主要是通过推导三部分的黄叶数量和红叶数量关系，得到递推公式，最后在O(1)的空间复杂度内解决问题 (官方题解二)  

```C++  
class Solution {
public:
    int minimumOperations(string leaves) {
        int n = leaves.size();
        int g = (leaves[0] == 'y' ? 1 : -1);
        int gmin = g;
        int ans = INT_MAX;
        for(int i = 1; i < n; ++i)
        {
            int isYellow = (leaves[i] == 'y');
            g += 2 * isYellow - 1;
            if(i != n - 1) ans = min(ans, gmin - g);
            gmin = min(gmin, g);
        }
        return ans + (g + n) / 2;
    }
};
```  

**执行结果：**  
执行用时 : **124 ms** , 在所有 cpp 提交中击败了 **93.73%** 的用户  
内存消耗 : **13.1 MB** , 在所有 cpp 提交中击败了 **82.01%** 的用户  

---  