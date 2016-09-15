---
layout:     post
title:      "<>Leetcode - Two Sum"
subtitle:   "Leetcode Note"
date:       2016-09-15
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Leetcode
    - Easy
---



## Two Sum

Given an array of integers, find two numbers such that they add up to a specific target number. The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based. You may assume that each input would have exactly one solution. Input: numbers={2, 7, 11, 15}, target=9 Output: index1=1, index2=2

> Java solution 1: using hashmap

```java
public class Solution {
    /*
     * @param numbers : An array of Integer
     * @param target : target = numbers[index1] + numbers[index2]
     * @return : [index1 + 1, index2 + 1] (index1 < index2)
         numbers=[2, 7, 11, 15],  target=9
         return [1, 2]
     */
    public int[] twoSum(int[] numbers, int target) {
        HashMap<Integer,Integer> map = new HashMap<>();

        for (int i = 0; i < numbers.length; i++) {
            if (map.get(numbers[i]) != null) {
                int[] result = {map.get(numbers[i]) + 1, i + 1};
                return result;
            }
            map.put(target - numbers[i], i);
        }
        
        int[] result = {};
        return result;
    }
}
```
> Java solution 2: using sort and array (no duplicate number)

```java
    public int[] twoSum(int[] nums, int target) {				
		int n = nums.length;
		
		ArrayList<Integer> original = new ArrayList<Integer>(5);
		for(int i=0; i<n; ++i){
			original.add(nums[i]);
		}

		Arrays.sort(nums); //O = nlog(n)
		System.out.println(Arrays.toString(nums));
        int[] result = {0,0};
        if(nums==null||n<2){
            return result;
        }
        int p = 0;
        int q = n-1;
        while(p<q){
            if(nums[p]+nums[q]==target){
                result[0]=p;
                result[1]=q;
                result[0]=original.indexOf(nums[result[0]]);
                result[1]=original.indexOf(nums[result[1]]);
                break;
            }
            if(nums[p]+nums[q]<target){
                p++;
            }
            if(nums[p]+nums[q]>target){
                q--;
            }
        } 
        return result;
	}
```