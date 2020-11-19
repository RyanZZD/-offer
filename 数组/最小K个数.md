## 题目描述

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。

示例

输入:[4,5,1,6,2,7,3,8],4

返回值:[1,2,3,4]

1.全排序  时间复杂度O（nlogn） 

```C++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        if(input.empty()||k>input.size()) return res;
        
        sort(input.begin(),input.end());
        
        for(int i=0;i<k;i++)
            res.push_back(input[i]);
        
        return res;
        
    }
};
```
2.Partiton思想 时间复杂度O(n)
```C++
class Solution {
public:
    int Partition(vector<int>& input, int begin, int end)
    {
    	int low=begin;
        int high=end;
        
        int pivot=input[low];
        while(low<high)
        {
            while(low<high&&pivot<=input[high])
                high--;
            input[low]=input[high];
            while(low<high&&pivot>=input[low])
                low++;
            input[high]=input[low];
        }
        input[low]=pivot;
        return low;
    }
    
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        
        int len=input.size();
        if(len==0||k>len) return vector<int>();
        if(len==k) return input;
        
        int start=0;
        int end=len-1;
        int index=Partition(input,start,end);
        while(index!=(k-1))
        {
            if(index>k-1)
            {
                end=index-1;
                index=Partition(input,start,end);
            }
            else
            {
                start=index+1;
                index=Partition(input,start,end);
            }
        }
        
        vector<int> res(input.begin(), input.begin() + k);
        
        return res;
    }

};

```

3.最大堆 时间复杂度O（nlogk）
```C++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) { 
        int len=input.size();
        if(len<=0||k>len) return vector<int>();
        
        vector<int> res(input.begin(),input.begin()+k);
        //建堆
        make_heap(res.begin(),res.end());
        
        for(int i=k;i<len;i++)
        {
            if(input[i]<res[0])
            {
                //先pop,然后在容器中删除
                pop_heap(res.begin(),res.end());
                res.pop_back();
                //先在容器中加入，再push
                res.push_back(input[i]);
                push_heap(res.begin(),res.end());
            }
        }
        //使其从小到大输出
        sort_heap(res.begin(),res.end());
        
        return res;
        
    }
};
```
4.红黑树：multiset集合  利用仿函数改变排序顺序 时间复杂度O（nlogk） 
```C++
class Solution {
public:
    typedef multiset<int, greater<int> > inSet;
    typedef multiset<int, greater<int> >::iterator setIterator;
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.empty()||input.size()<k)
            return {};
        if(input.size()==k)
            return input;
        inSet leastNum;
        for(int i=0;i<input.size();i++){
            if(i<k)
                leastNum.insert(input[i]);
            else{
                setIterator iter=leastNum.begin();
                if(*iter>input[i]){
                    leastNum.erase(iter);
                    leastNum.insert(input[i]);
                }
            }
        }
        vector<int> res(leastNum.begin(), leastNum.end());
        return res;
    }
};
```
