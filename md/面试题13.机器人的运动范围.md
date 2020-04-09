# 机器人的运动范围 #  
`难度：中等` 

地上有一个m行n列的方格，从坐标 `[0,0]` 到坐标 `[m-1,n-1]` 。一个机器人从坐标 `[0, 0]` 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？  

**示例 1**:  
>**输入**: m = 2, n = 3, k = 1  
>**输出**: 3  

**示例 2**:  
>**输入**: m = 3, n = 1, k = 0  
>**输出**: 1  

**提示**：  
- `1 <= n,m <= 100`
- `0 <= k <= 20`  

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  

---  
>解法一：DFS递归求解  

```C++  
class Solution {
private:
    static constexpr int dx[4] = {0, 0, -1, 1};
    static constexpr int dy[4] = {-1, 1, 0, 0};
public:
    int movingCount(int m, int n, int k) {
        int res = 1;
        vector<vector<bool>> seen(m, vector<bool>(n, false));
        seen[0][0] = true;
        helper(seen, 0, 0, k, res);
        return res;
    }
private:
    void helper(vector<vector<bool>>& seen, int x, int y, int k, int& res)
    {
        for(int i = 0; i < 4; ++i)
        {
            int tx = x + dx[i], ty = y + dy[i];
            if(tx < 0 || tx >= seen.size() || ty < 0 || ty >= seen[0].size()) continue;
            else if(seen[tx][ty]) continue;
            else
            {
                seen[tx][ty] = true;
                int temp = tx % 10 + (tx / 10) % 10 + tx / 100;
                temp += ty % 10 + (ty / 10) % 10 + ty / 100;
                if(temp > k) continue;
                else helper(seen, tx, ty, k, ++res);
            }
        }
    }
};
```  

**执行结果：**  
执行用时 : **8 ms** , 在所有 cpp 提交中击败了 **39.15%** 的用户  
内存消耗 : **7.3 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  
>解法二：根据官方题解一提示，使用队列实现BFS搜索并精简了搜索方向  

```C++  
class Solution {
private:
    // 只保留向左和向右移动方向
    static constexpr int dx[2] = {0, 1};
    static constexpr int dy[2] = {1, 0};
public:
    int movingCount(int m, int n, int k) {
        int res = 1;
        vector<vector<bool>> seen(m, vector<bool>(n, false));
        seen[0][0] = true;
        queue<int> que({0});
        while(!que.empty())
        {
            int x = que.front() / 100, y = que.front() % 100; que.pop();
            for(int i = 0; i < 2; ++i)
            {
                int tx = x + dx[i], ty = y + dy[i];
                if(tx < 0 || tx >= m || ty < 0 || ty >= n || seen[tx][ty]) continue;
                seen[tx][ty] = true;
                int temp = tx % 10 + (tx / 10) % 10 + tx / 100;
                temp += ty % 10 + (ty / 10) % 10 + ty / 100;
                if(temp > k) continue;
                else
                {
                    ++res;
                    que.push(tx * 100 + ty);
                }
            }
        }
        return res;
    }
};
```  

**执行结果：**  
执行用时 : **4 ms** , 在所有 cpp 提交中击败了 **81.36%** 的用户  
内存消耗 : **6.6 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  
>解法三：使用动态规划的思想进行递推 (官方题解二)  

```C++  
class Solution {
public:
    int movingCount(int m, int n, int k) {
        int res = 1;
        vector<vector<bool>> seen(m, vector<bool>(n, false));
        seen[0][0] = true;
        for(int i = 0; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if(seen[i][j]) continue;
                int temp = i % 10 + (i / 10) % 10 + i / 100;
                temp += j % 10 + (j / 10) % 10 + j / 100;
                if(temp > k) continue;
                else if((i > 0 && seen[i - 1][j]) || (j > 0 && seen[i][j - 1]))
                {
                    seen[i][j] = true;
                    ++res;
                }
            }
        }
        return res;
    }
};
```  

**执行结果：**  
执行用时 : **4 ms** , 在所有 cpp 提交中击败了 **81.36%** 的用户  
内存消耗 : **6.3 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  