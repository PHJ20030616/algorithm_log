# [1. 两数之和](https://leetcode.cn/problems/two-sum/)

**解法一：暴力枚举**

**解法二：使用哈希表**

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        for(int i=0;i<nums.size();i++){
            auto it=hash.find(target-nums[i]);
            if(it!=hash.end()){
                return {it->second,i};
            }else{
                hash[nums[i]]=i;
            }
        }
        return {};
    }
};
```

**注意：unordered_map中的find(x)是指key=x而不是value=x，若查找成功则返回迭代器，查找失败则返回hash.end()**

# [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>>hash;
        for(auto s:strs){
            string sorted_s=s;
            ranges::sort(sorted_s);
            hash[sorted_s].push_back(s);
        }
        vector<vector<string>> res;
        for(auto [key,val]:hash){
            res.push_back(val);
        }
        return res;
    }
};
```

**核心思想：**

利用哈希表，key为排序后的str（因为各个字母移位词排序后必定相同），val为 vector<string>类型，用于存储各个字母移位词

# [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> st(nums.begin(),nums.end());
        int res=0;
        for(int x:st){
            if(st.contains(x-1)){
                continue;
            }
            int y=x+1;
            while(st.contains(y)){
                y++;
            }
            res=max(res,y-x);
        }
        return res;

    }
};
```

**核心思想：**

1. 将数组中所有元素存入unordered_set中
2. 遍历数组int x:nums，同时在unordered_set中查找是否存在x-1
   - 若unordered_set中存在x-1，说明x并不是起始位置，直接continue
   - 若unordered_set中不存在x-1，说明x是起始位置，此时while循环，查出最长连续循环的长度

# [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

思路视频：https://www.bilibili.com/video/BV12E421T7wr/?spm_id_from=333.337.search-card.all.click&vd_source=986859c9b5bf6f76e3c51c9396174117

```
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int left=0,right=0;
        while(right<nums.size()){
            if(nums[right]!=0){
                swap(nums[left],nums[right]);
                left++;
            }
            right++;
        }
    }
};
```

**核心思想：**

核心逻辑在于交换，当left和right所指的元素均不为0时，交换不会产生任何影响

当right指向0时，right++，但是left保持不变

这就会发生，left指向0，并且left右边均为0，right左边均为0

# [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

题目的意思就是计算任何两个垂线之间的面积，找到最大值

```
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left=0;
        int right=height.size()-1;
        int res=0;
        while(left<right){
            int t=(right-left)*min(height[left],height[right]);
            res=max(res,t);
            if(height[left]<=height[right]){
                left++;
            }else{
                right--;
            }
        }
        return res;
    }
};
```

**核心思想**

一左一右两个指针，不断计算大小，并将高度较小的进行移动，直到遍历完所有可能

# [15. 三数之和](https://leetcode.cn/problems/3sum/)

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int n=nums.size();
        vector<vector<int>> res;
        for(int i=0;i<n-2;i++){
            if(i>0&&nums[i]==nums[i-1]){
                continue;
            }
            int left=i+1;
            int right=n-1;
            while(left<right){
                if(nums[i]+nums[left]+nums[right]==0){
                    res.push_back({nums[i],nums[left++],nums[right--]});
                    while(left<right && nums[right]==nums[right+1]){
                        right--;
                    }
                    while(left<right && nums[left]==nums[left-1]){
                        left++;
                    }
                }else if(nums[i]+nums[left]+nums[right]>0){
                    right--;
                }else{
                    left++;
                }
            }
        }
        return res;
    }
};
```

参考教程：

https://www.bilibili.com/video/BV1GW4y127qo/?spm_id_from=333.337.search-card.all.click&vd_source=986859c9b5bf6f76e3c51c9396174117

![image-20260324210754554](https://cdn.jsdelivr.net/gh/PHJ20030616/personal_pic/img/20260324210758200.png)

