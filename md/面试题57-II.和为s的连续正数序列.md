# 和为s的连续正数序列 #  
`难度：简单` 

输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。  

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。  

**示例 1**:  
>**输入**: target = 9  
>**输出**: [[2,3,4],[4,5]]  

**示例 2**:  
>**输入**: target = 15  
>**输出**: [[1,2,3,4,5],[4,5,6],[7,8]]  

**限制**：  
- `1 <= target <= 10^5`  

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  

---  
>解法一：暴力搜索法

```C++  
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        if(target <= 2) return {{}};
        vector<vector<int> > res;
        vector<int> que({1, 2});
        int temp = 3, next = 3;
        while(que.size() > 1)
        {
            if(temp > target)
            {
                temp -= que.front();
                que.erase(que.begin());
            }
            else
            {
                if(temp == target) res.push_back(que);
                temp += next;
                que.push_back(next++);
            }
        }
        return res;
    }
};
```  

**执行结果：**  
执行用时 : **32 ms** , 在所有 cpp 提交中击败了 **15.09%** 的用户  
内存消耗 : **9.1 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  
>解法二：双指针夹逼法

```C++  
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        if(target <= 2) return {{}};
        vector<vector<int> > res;
        int temp = 3, begin = 1, end = 3;
        while(end - begin > 1)
        {
            if(temp > target) temp -= begin++;
            else
            {
                if(temp == target)
                {
                    res.push_back(vector<int>(end - begin));
                    int tmp = begin;
                    for(int &i : res.back()) i = tmp++;
                }
                temp += end++;
            }
        }
        return res;
    }
};
```  

**执行结果：**  
执行用时 : **8 ms** , 在所有 cpp 提交中击败了 **52.41%** 的用户  
内存消耗 : **9 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  
>解法三：数学法，将求解问题转化为求根公式 (官方题解二)  

```C++  
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<int> res;
        vector<vector<int>> vec;
        int sum = 0, limit = (target - 1) / 2;// (target - 1)/2等效于target/2下取整
        for(int x = 1; x <= limit; ++x)
        {
            long long delta = 1 - 4 * (x - 1ll * x * x - 2 * target);
            if(delta < 0) continue;
            int delta_sqrt = (int)sqrt(delta + 0.5);
            if(1ll * delta_sqrt * delta_sqrt == delta && (delta_sqrt - 1) % 2 == 0)
            {
                int y = (-1 + delta_sqrt) / 2;//另一个解(-1-delta_sqrt)/2必然小于0
                if(x < y)
                {
                    res.clear();
                    for(int i = x; i <= y; ++i) res.emplace_back(i);
                    vec.emplace_back(res);
                }
            }
        }
        return vec;
    }
};
```  

**执行结果：**  
执行用时 : **4 ms** , 在所有 cpp 提交中击败了 **86.02%** 的用户  
内存消耗 : **7.1 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  