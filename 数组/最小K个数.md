## 题目描述

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。

示例

输入:[4,5,1,6,2,7,3,8],4

返回值:[1,2,3,4]
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
