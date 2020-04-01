# 队列的最大值 #  
`难度：中等` 

请定义一个队列并实现函数 `max_value` 得到队列里的最大值，要求函数`max_value`、`push_back` 和 `pop_front` 的均摊时间复杂度都是O(1)。

若队列为空，`pop_front` 和 `max_value` 需要返回 -1

**示例 1**:  
>**输入**:   
>["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]  
>[[],[1],[2],[],[],[]]  
>**输出**: [null,null,null,2,1,2]  

**示例 2**:  
>**输入**:   
>["MaxQueue","pop_front","max_value"]  
>[[],[],[]]  
>**输出**: [null,-1,-1]  

**限制**：  
- `1 <= push_back,pop_front,max_value的总操作数 <= 10000`  
- `1 <= value <= 10^5`  

来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/  
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  

---  
>解法一：使用链表实现，但是pop_front的操作时间复杂度不是O(1)  

```C++  
class MaxQueue {
private:
    list<int> __que;
    int maxValue;
public:
    MaxQueue() {
        maxValue = INT_MIN;
    }
    
    int max_value() {
        if(__que.empty()) return -1;
        else return maxValue;
    }
    
    void push_back(int value) {
        __que.push_back(value);
        if(value > maxValue) maxValue = value;
    }
    
    int pop_front() {
        if(__que.empty()) return -1;
        int temp = __que.front(); __que.pop_front();
        if(__que.empty()) maxValue = INT_MIN;
        else if(temp == maxValue)
        {
            maxValue = INT_MIN;
            for(int i : __que) maxValue = max(maxValue, i);
        }
        return temp;
    }
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue* obj = new MaxQueue();
 * int param_1 = obj->max_value();
 * obj->push_back(value);
 * int param_3 = obj->pop_front();
 */
```  

**执行结果：**  
执行用时 : **156 ms** , 在所有 cpp 提交中击败了 **58.33%** 的用户  
内存消耗 : **53.1 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  
>解法二：使用队列和一个单调双向队列的组合 (官方题解二)  

```C++  
class MaxQueue {
private:
    queue<int> __queue;
    deque<int> __deque;
public:
    MaxQueue() {
        
    }
    
    int max_value() {
        if(__deque.empty()) return -1;
        else return __deque.front();
    }
    
    void push_back(int value) {
        __queue.push(value);
        while(!__deque.empty() && __deque.back() < value) __deque.pop_back();
        __deque.push_back(value);
    }
    
    int pop_front() {
        if(__queue.empty()) return -1;
        int temp = __queue.front(); __queue.pop();
        if(temp == __deque.front()) __deque.pop_front();
        return temp;
    }
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue* obj = new MaxQueue();
 * int param_1 = obj->max_value();
 * obj->push_back(value);
 * int param_3 = obj->pop_front();
 */
```  

**执行结果：**  
执行用时 : **140 ms** , 在所有 cpp 提交中击败了 **90.18%** 的用户  
内存消耗 : **51.5 MB** , 在所有 cpp 提交中击败了 **100.00%** 的用户  

---  