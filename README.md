# LeetCodeInCPP

## List:

* #1 Two Sum

## Detail

* ### #1 Two Sum
**LeetCode Link：**
<br><https://leetcode.com/problems/two-sum/>

**Problem description:**
<br>Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution.

**Source code:**
<https://github.com/LZleejean/LeetCode/blob/master/SourceCode/1_TwoSum>

**Ideas:**
First,an idea came into my mind that I can use a sort function and find the index of value equals half of target,then I can esaily get the indices.But I made a mistake that what I need is the index before sorting.So I had to renew a vector to costing more time.
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> n2=nums;
        sort(nums.begin(),nums.end());
        vector<int> result;
        int i=0;
        while(nums[i]<=target/2){
            ++i;
        }
        for(;i>=0;--i){
            for(int j=i;nums[i-1]+nums[j]<=target;++j){
                if(nums[i-1]+nums[j]==target){
                    for(int k=0;result.size()<2;++k){
                        if(n2[k]==nums[i-1]||n2[k]==nums[j]){
                            result.push_back(k);
                        }
                    }
                    break;
                }
            }
        }
        return result;
    }
};
```
