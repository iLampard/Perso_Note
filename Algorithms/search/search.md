# Search algorithms

#### Search Insert Position, [LeetCode35](https://leetcode.com/problems/search-insert-position/)

```c
int searchInsert(vector<int>& nums, int target) {
    if(nums.size() == 0)
        return 0;
    int start = 0;
    int end = nums.size() - 1;
    int mid = 0;
    while(start <= end){ # 注意这里必须有等号
        mid = start + (end - start) / 2;
        if(nums[mid] > target)
            end = mid - 1;
        else if(nums[mid] < target)
            start = mid + 1;
        else
            return mid;
    }
    
    return start;
}

```