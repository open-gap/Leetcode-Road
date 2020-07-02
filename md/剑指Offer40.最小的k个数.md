# 最小的k个数 #  
`难度：简单` 

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。  

**示例 1**:  
>**输入**: arr = [3,2,1], k = 2  
>**输出**: [1,2] 或者 [2,1]  

**示例 2**:  
>**输入**: arr = [0,1,2,1], k = 1  
>**输出**: [0]  

**限制**：  
- `0 <= k <= arr.length <= 10000`  
- `0 <= arr[i] <= 10000`  

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  

---  
>解法一：排序法  

```C++  
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if(arr.empty()) return {};
        sort(arr.begin(), arr.end());
        return vector<int>(arr.begin(), arr.begin() + k);
    }
};
```  

**执行结果：**  
执行用时 : **36 ms** , 在所有 cpp 提交中击败了 **80.33%** 的用户  
内存消耗 : **20.6 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  
>解法二：使用优先队列实现的堆排序  

```C++  
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if(arr.empty() || k == 0) return {};
        priority_queue<int> que;
        for(int i = 0; i < k; ++i) que.push(arr[i]);
        for(int i = k; i < arr.size(); ++i)
        {
            if(arr[i] <= que.top())
            {
                que.pop();
                que.push(arr[i]);
            }
        }
        vector<int> res(k);
        for(int i = 0; i < k; ++i)
        {
            res[i] = que.top();
            que.pop();
        }
        return res;
    }
};
```  

**执行结果：**  
执行用时 : **84 ms** , 在所有 cpp 提交中击败了 **15.54%** 的用户  
内存消耗 : **21 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  
>解法三：计数排序法  

```C++  
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        vector<int> res(k);
        int a[10001] = {0}, index = 0;
        for(int i : arr) ++a[i];
        for(int i = 0; i <= 10000 && index < k; ++i)
        {
            while(a[i] > 0)
            {
                if(index == k) break;
                res[index++] = i;
                --a[i];
            }
        }
        return res;
    }
};
```  

**执行结果：**  
执行用时 : **52 ms** , 在所有 cpp 提交中击败了 **43.81%** 的用户  
内存消耗 : **18.8 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  