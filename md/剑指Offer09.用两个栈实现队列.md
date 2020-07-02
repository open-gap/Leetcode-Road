# 用两个栈实现队列 #  
`难度：简单` 

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

**示例 1**:  
>**输入**:   
>["CQueue","appendTail","deleteHead","deleteHead"]  
>[[],[3],[],[]]  
>**输出**: [null,null,3,-1]  

**示例 2**:  
>**输入**:   
>["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]  
>[[],[],[5],[2],[],[]]  
>**输出**: [null,-1,null,null,5,2]  

**提示**：  
- `1 <= values <= 10000`  
- `最多会对 appendTail、deleteHead 进行 10000 次调用`  

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  

---  
>解法一：设置两个栈，一个用于接收插入数据，另一个用于输出  

```C++  
class CQueue {
private:
    stack<int> f1, f2;
public:
    CQueue() {
        
    }
    
    void appendTail(int value) {
        f2.push(value);
    }
    
    int deleteHead() {
        if(f1.empty())
        {
            while(!f2.empty())
            {
                f1.push(f2.top());
                f2.pop();
            }
        }
        if(f1.empty()) return -1;
        int res = f1.top();
        f1.pop();
        return res;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```  

**执行结果：**  
执行用时 : **620 ms** , 在所有 cpp 提交中击败了 **97.19%** 的用户  
内存消耗 : **103.5 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  