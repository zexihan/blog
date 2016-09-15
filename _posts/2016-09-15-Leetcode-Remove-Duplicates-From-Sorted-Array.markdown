---
layout:     post
title:      "<>Leetcode - Remove Duplicates from Sorted Array"
subtitle:   "Leetcode Note"
date:       2016-09-15
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Leetcode
    - Easy
---



## Remove Duplicates from Sorted Array

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.
Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array nums = [1,1,2],
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

> Java solution: using pointer

```java
    public int removeDuplicates(int[] nums) {
        int length = nums.length;
        int last = 0;
        Arrays.sort(nums);
        for(int curr=1; curr<length; ++curr){
        	if(nums[curr] != nums[curr-1]){
        		nums[++last]=nums[curr];
        	}
        }
        return last+1;
    }
```
