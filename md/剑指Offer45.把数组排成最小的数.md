# 把数组排成最小的数 #  
`难度：中等` 

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。  

**示例 1**:  
>**输入**: [10,2]  
>**输出**: "102"  

**示例 2**:  
>**输入**: [3,30,34,5,9]  
>**输出**: "3033459"  

**提示**：  
- `0 < nums.length <= 100`   

**说明**:  
- 输出结果可能非常大，所以你需要返回一个字符串而不是整数  
- 拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0  

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  

---  
>解法一：先将数字转化为字符串，然后按照最高位对齐的方式设计比较函数，最后使用由自设定的比较函数排序的结果组合输出，注意两个数字不同长度的时候使用字符串拼接结果的方式来比较大小 (题解)  

```C++  
class Solution {
public:
    string minNumber(vector<int>& nums) {
        if(nums.empty()) return "";
        vector<string> temp(nums.size());
        for(int i = 0; i < nums.size(); ++i) temp[i] = to_string(nums[i]);
        sort(temp.begin(), temp.end(), less);
        string res = "";
        for(string& str : temp) res += str;
        return res;
    }
private:
    static bool less(const string a, const string b)
    {
        int la = a.length(), lb = b.length();
        for(int i = 0; i < la && i < lb; ++i)
        {
            if(a[i] < b[i]) return true;
            else if(a[i] > b[i]) return false;
        }
        string str_ab = a + b, str_ba = b + a;
        int ab = atoi(str_ab.c_str()), ba = atoi(str_ba.c_str());
        if(ab < ba) return true;
        else return false;
    }
};
```  

**执行结果：**  
执行用时 : **16 ms** , 在所有 cpp 提交中击败了 **83.28%** 的用户  
内存消耗 : **11.6 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  