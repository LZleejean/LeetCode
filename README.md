# LeetCodeInCPP

## List:

* #1    Two Sum
* #2    Add Two Numbers
* #485  Max Consecutive Ones
* #495  Teemo Attacking

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

**Source code:**
<br><https://github.com/LZleejean/LeetCode/blob/master/SourceCode/2_AddTwoNumbers>

**Ideas:**
<br>In my view,i want to get the two numbers of linked list and then add together and finally split the result into a new ListNode.But i forgot the accuracy of number.When it's too long,the way failed.
<br>Just ex:
<br>Input:
<br>[2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,9]
<br>[5,6,4,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,2,4,3,9,9,9,9]
<br>Output:
<br>[]
<br>Expected:
<br>[7,0,8,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,4,8,6,1,4,3,9,1]

<br>This problem is not hard and the solution is O(n),the important issue is how short and simple.
<br>
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        long number1=0,number2=0;
        ListNode *ln=new ListNode(0), *l=ln;
        if(l1->val==0&&!l1->next&&l2->val==0&&!l2->next)
        {
            return ln;
        }
        for(int i=0;l1!=NULL;l1=l1->next,i++)
        {
            number1+=l1->val*pow(10,i);
        }
        for(int i=0;l2!=NULL;l2=l2->next,i++)
        {
            number2+=l2->val*pow(10,i);
        }
        number1+=number2;
        while(number1!=0)
        {
            
            int value=number1%10;
            number1/=10;
            l->next=new ListNode(value);
            l=l->next;
        }
        return ln->next;
    }
};
```
<br>There is a 11-lines solution from @StefanPochmann.
<br>His solution use bitewise operations and the code is so concise.
<br>
```
ListNode *addTwoNumbers(ListNode *l1, ListNode *l2) {
    ListNode preHead(0), *p = &preHead;
    int extra = 0;
    while (l1 || l2 || extra) {
        if (l1) extra += l1->val, l1 = l1->next;
        if (l2) extra += l2->val, l2 = l2->next;
        p->next = new ListNode(extra % 10);
        extra /= 10;
        p = p->next;
    }
    return preHead.next;
}
```

###   #485 Max Consecutive Ones
**LeetCode Link：**
<br>https://leetcode.com/problems/max-consecutive-ones/#/description

**Problem description:**
<br>Given a binary array, find the maximum number of consecutive 1s in this array.
<br>EX:
<br>Input: [1,1,0,1,1,1]
<br>Output: 3
<br>Explanation: The first two digits or the last three digits are consecutive 1s.
<br>    The maximum number of consecutive 1s is 3.

**Source code:**
<br><https://github.com/LZleejean/LeetCode/blob/master/SourceCode/485_MaxConsecutiveOnes>

**Ideas:**
<br>using a iterator traverse the vector.if val is "1",let count plus 1.if val is "0",store the count to a new vector.When it go to end,
<br>i judge the count whether is zero to avoid forgeting to store the last count.Finally,i sort the new vector and return the last value.
<br>this solution depend on the sort's cost best O(nlogn) worst O(n^2) but average O(nlogn) with 66ms.
```
int findMaxConsecutiveOnes(vector<int>& nums) {
        vector<int>::iterator it;
        vector<int> count;
        int item=0;
        for(it=nums.begin();it!=nums.end();it++)
        {
            if(*it==1)
            {
                item++;
            }
            else
            {
                count.push_back(item);
                item=0;
            }
        }
        if(item!=0)
        {
            count.push_back(item);
        }
        sort(count.begin(),count.end());
        return count[count.size()-1];
    }
```

<br> When i eat dinner,i thought a good idea.Don't use a new vector.Directly contrast and assign.When the val is '1',plus one and contrast with count.
<br>This solution just O(n) with 38ms.
<br>
```
int findMaxConsecutiveOnes(vector<int>& nums) {
        vector<int>::iterator it;
        int count=0;
        int item=0;
        for(it=nums.begin();it!=nums.end();it++)
        {
            if(*it==1)
            {
                item++;
                if(item>count)
                {
                    count=item;
                }
            }
            else 
            {
                item=0;
            }
        }
        return count;
    }
```

<br>Coutrasting my second method with others,what better than me is that "if" statements is concise.
<br>
```
int findMaxConsecutiveOnes(vector<int>& nums) 
{
    int max=0,cur=0;
    for(int i=0;i<nums.size();i++)
    {
        if(nums[i]&1)
        {
            max=max>++cur?max:cur;
        }
        else cur=0;
    }
    return max;
}
```
<br>more concise.it's better.
```
if(nums[i]==1) max=max>++cur?max:cur;
else cur=0;
```


<br>OMG, @G513's solution is so simple.By using c++11 "auto" keyword.But with 59ms.O(∩_∩)O.
<br>
```
int findMaxConsecutiveOnes(vector<int>& nums) {
        int max_cnt = 0, cnt = 0;
        for (auto n : nums) {
            if (n == 1) max_cnt = max(++cnt, max_cnt);
            else cnt = 0;
        }
        return max_cnt;
    }
```

### #495 Teemo Attacking
**LeetCode Link：**
<br>https://leetcode.com/problems/teemo-attacking/#/description

**Problem description:**
<br>In LLP world, there is a hero called Teemo and his attacking can make his enemy Ashe be in poisoned condition. Now, given the Teemo's attacking ascending time series towards Ashe and the poisoning time duration per Teemo's attacking, you need to output the total time that Ashe is in poisoned condition.
<br>You may assume that Teemo attacks at the very beginning of a specific time point, and makes Ashe be in poisoned condition immediately.
<br>EX:
<br>Input:[1,4], 2
<br>Output: 4
<br>Input:[1,2], 2
<br>Output: 3

**Source code:**
<br><https://github.com/LZleejean/LeetCode/blob/master/SourceCode/495_TeemoAttacking>

**Ideas:**
<br>This problem just like a game,constantly losing blood and update stuts.So my method is that traversing the vector and contrast current value with next to decide if it needs update.
<br>This O(n)'s solution with 79ms is not hard,but i think the point is how to simple and concise.
<br>
```
int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        vector<int>::iterator it;
        int count=0;
        for(it=timeSeries.begin();it!=timeSeries.end();it++)
        {
            if(it+1!=timeSeries.end())
            {
                if(*(it+1)>=*it+duration)
                {
                    count+=duration;
                }
                else
                {
                    count+=*(it+1)-*it;
                }
            }
        }
        return count=timeSeries.empty()?count:count+duration;
    }
```

<br>There is two solution different from me.more and more simple.
<br>@hl1250 use a value "poisonEnd" to store the last attacking casusing poison time.first he or she get the last time and then when it go to a new attacking,he get the previous time by new total time not duration minus old "poisonEnd".
<br>
```
int findPosisonedDuration(vector<int>& timeSeries, int duration)
{
    int poisonEnd = 0,total=0;
    for(auto it : timeSeries)
    {
        poisonEnd = max(it, poisonEnd);
        total += (it + duration - poisonEnd);
        poisonEnd = it +  duration;
    }
    return total;
}
```
<br>@ kevin36's solution is similar but he or she use duration minus old not total time.this point is similar with mine.
<br>
```
int findPosisonedDuration(vector<int>& ts, int duration) {
        int tp = 0, pe = 0;
        for(int i=0; i<ts.size(); ++i){
            tp += duration - (ts[i] < pe) *(pe - ts[i]);
            pe = ts[i] + duration;
        }
        return tp;
    }
```
<br> In my view,i think the second is better than me and the first.
