# 数组中数字出现的次数 #  
`难度：中等` 

一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。    

**示例 1**:  
>**输入**: nums = [4,1,4,6]  
>**输出**: [1,6] 或 [6,1] 

**示例 2**:  
>**输入**: nums = [1,2,10,4,1,4,3,3]  
>**输出**: [2,10] 或 [10,2] 

**限制**：  
- `2 <= nums <= 10000`  

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  

---  
>解法一：先通过挨个异或找到唯一的两个数，然后选择最低的区分位将数组分成两组分别异或，得到最终结果 (官方题解一)  

```C++  
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int ret = 0;
        for(int n : nums) ret ^= n;//先来一次异或求两个唯一数的异或结果
        int div = 1;
        while((div & ret) == 0) div <<= 1;//然后找区别两个数的最低位
        int a = 0, b = 0;
        for(int n : nums)
        {
            if(div & n)a ^= n;//如果div位为一，异或到a组
            else b ^= n;//否则，异或到b组
        }
        return {a, b};//返回结果
    }
};
```  

**执行结果：**  
执行用时 : **40 ms** , 在所有 cpp 提交中击败了 **56.80%** 的用户  
内存消耗 : **16.3 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  