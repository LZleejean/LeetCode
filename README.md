# LeetCodeInCPP

## List:

* #1 Two Sum
* #2 Add Two Numbers

## Detail

###   #1 Two Sum
**LeetCode Link：**
<br><https://leetcode.com/problems/two-sum/>

**Problem description:**
<br>Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution.
<br>Ex:
<br>Given nums = [2, 7, 11, 15], target = 9,
<br>Because nums[0] + nums[1] = 2 + 7 = 9,
<br>return [0, 1].

**Source code:**
<br><https://github.com/LZleejean/LeetCode/blob/master/SourceCode/1_TwoSum>

**Ideas:**
<br>First,an idea came into my mind that I can use a sort function and find the index of value equals half of target,then I can esaily get the indices.But I made a mistake that what I need is the index before sorting.So I had to renew a vector to costing more time.
<br>The most simple solution but best O(1) worst O(n^3) with 223 ms.
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
            for(int j=i;nums[i-1]+nums[j]<=target&&j<nums.size();++j){
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
<br> The O(n) solution(15 ms) from naveed.zafar.
<br>By using a unordered_map, we can add the value-index link and find the target value.Because the hash find() function is O(1).
<br>
```
vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        vector<int> result;
        for(int i=0;i<nums.size();++i){
            //key
            int valueFind = target - nums[i];
            //if find it in the map
            if(hash.find(valueFind)!=hash.end()){
                result.push_back(hash[valueFind]);
                result.push_back(i);
                return result;
            }
            //if not find,add link
            hash[nums[i]]=i;
        }
        return result;
}
```

### #2 Add Two Numbers
**LeetCode Link：**
<br>https://leetcode.com/problems/add-two-numbers/#/description

**Problem description:**
<br>You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
<br>You may assume the two numbers do not contain any leading zero, except the number 0 itself.
<br>EX:
<br>Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
<br>Output: 7 -> 0 -> 8
